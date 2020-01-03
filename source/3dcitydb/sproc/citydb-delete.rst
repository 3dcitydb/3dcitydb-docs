.. _citydb_sproc_delete_chapter:

CITYDB_DELETE
-------------

The package CITYDB_DELETE consists of several functions that facilitate
to delete single and multiple city objects. Each function automatically
takes care of integrity constraints between relations in the database.
The package is meant as low-level API providing a delete function for
each relation (except for linking tables) – from a single polygon in the
table SURFACE_GEOMETRY (del_surface_geometry) up to a complete
*CityObject* (del_cityobject) or even a whole *CityObjectGroup*
(del_cityobjectgroup). This should help users to develop more complex
delete operations on top of these low-level functions without
re-implementing their functionality.

Most of the stored procedures take the **primary key ID value** of the
entry to be deleted as input parameter and return the ID value if the
entry has been successfully removed. So, if NULL is returned, the entry
is either already gone or the deletion did not work due to an error.
Nearly every delete function comes with a pendant to delete multiple
entries at once. These alternative functions take an **array of ID
values** as input and return an array of successfully deleted entries.
For PostgreSQL, the array is unrolled inside the functions as PL/pgSQL
can return a SET OF INTEGER values.

In order to illustrate the low-level approach of this package, assume a
user wants to delete a building feature together with all its nested sub
features. For this purpose, the user calls the del_building (or
del_cityobject) function, which internally leads to subsequent calls to
the following stored procedures:

-  del_building for the building and its dependent building parts
   (recursive call)

-  del_thematic_surface for dependent boundary surfaces of the building
   (nested call of del_opening for dependent openings of the boundary
   surfaces)

-  del_building_installation for dependent outer installations of the
   building (nested call of del_thematic_surface for boundary surfaces
   of the installations)

-  del_room for dependent rooms of the building (nested call of
   del_thematic_surface for interior boundary surfaces,
   del_building_installation for interior installation and
   del_building_furniture for furniture within the room)

-  del_address for dependent addresses that are not referenced by other
   buildings and bridges

-  del_implicit_geometry for each prototype geometry of a nested
   feature, e.g. *Openings*, *BuildingInstallation*

-  del_surface_geometry for deleting the geometry of the building and
   its nested features

-  del_cityobject to remove the entry in the CITYOBJECT table that
   corresponds to the deleted building and the deleted child features
   (also deletes generic attributes, external references, appearances,
   etc.)

Note, that global *Appearances* with no direct reference to a
*CityObject* are not deleted during such a deletion process. Therefore,
the method cleanup_appearances should be executed afterwards, to remove
all *Appearance* information (incl. entries in tables
APPEAR_TO_SURFACE_DATA, SURFACE_DATA and TEX_IMAGE). Like with the
stored procedures from the CITYDB_OBJCLASS package, the delete functions
are part of the ‘citydb’ schema and not ‘citydb_pkg’. This is not only
because of a better performance without dynamic SQL. It is mandatory as
**the code for the** **delete functions is generated automatically based
on the foreign keys**.

The del\_ prefix is used to not exceed 30 characters in Oracle. As
explained in :numref:`citydb_multiple_database_schemas_chapter`,
managing different CityGML ADEs in different
schema would require different delete scripts for each schema. A simple
code block to delete objects based on a query result can look like this:

**Oracle:**

.. code:: sql

    -- single version
    DECLARE
      deleted_id NUMBER;
      dummy_ids ID_ARRAY := ID_ARRAY();
    BEGIN
      FOR rec IN (SELECT * FROM cityobject WHERE ...) LOOP
        deleted_id := citydb_delete.del_cityobject(rec.id);
      END LOOP;
      dummy_ids := citydb_delete.cleanup_appearances;
    END;
    -- array version
    DECLARE
      pids ID_ARRAY := ID_ARRAY();
      deleted_ids ID_ARRAY := ID_ARRAY();
      dummy_ids ID_ARRAY := ID_ARRAY();
    BEGIN
      SELECT id BULK COLLECT INTO pids
        FROM cityobject WHERE ...;

      deleted_ids := citydb_delete.del_cityobject(pids);
      dummy_ids := citydb_delete.cleanup_appearances;
    END;

**PostgreSQL:**

.. code:: sql

    -- single version
    SELECT citydb.del_cityobject(id) FROM cityobject WHERE ... ;
    SELECT citydb.cleanup_appearances();

    -- array version
    SELECT citydb.del_cityobject(array_agg(id))
      FROM cityobject WHERE ... ;
    SELECT citydb.cleanup_appearances();

Which delete function to use depends on the ratio between the number of
entries to be deleted and the total count of objects in the database.
One array delete executes each necessary query only once compared to
numerous single deletes and can be faster. However, if the array is huge
and covers a great portion of the table (say 20% of all rows) it might
be faster to go for the single version instead or batches of smaller
arrays. Nested features are deleted with arrays anyway.

The previously available CITYDB_DELETE_BY_LINEAGE package has been
included into the CITYDB_DELETE package and reduced to only one
function. It allows to delete multiple city objects that share a common
value in the LINEAGE column of the CITYOBJECT table. The procedure
cleanup_schema provides a convenient way to reset an entire 3DCityDB
instance under both Oracle and PostgreSQL. After invoking this
procedure, all entries from all tables are deleted and all sequences are
reset.

The following table only lists functions that differ from each other
where del_cityobject stands for the general layout of a delete function:

.. list-table:: API of CITYDB_DELETE package for Oracle
   :name: citydb_delete_api_oracle_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **cleanup_appearances**
       | (only_global)
     - | ID_ARRAY
     - | Removes unreferenced Appearences incl.
       | SurfaceData and textures and returns an array of
       | their IDs. Pass 1 (default) to only delete global
       | appearances, or 0 to include local appearances
   * - | **cleanup_schema**
       | (schema_name)
     - | void
     - | Truncates most tables and resets sequences in a
       | given 3D City Database schema
   * - | **cleanup_table** (table_name)
     - | ID_ARRAY
     - | Removes entries in given table which are not
       | referenced by any other entities
   * - | **del_cityobject** (NUMBER)
     - | NUMBER
     - | Removes the CityObject with the given ID incl.
       | all references to other tables. The ID value
       | is returned on success
   * - | **del_cityobject** (ID_ARRAY)
     - | ID_ARRAY
     - | Removes CityObjects with the given IDs incl.
       | all references to other tables. An array of
       | IDs of successfully deleted objects is returned
   * - | **del_cityobjects_by_lineage**
       | (lineage_value)
     - | ID_ARRAY
     - | Removes all CityObjects on behalf of a LINEAGE
       | value and returns an array of their IDs

.. list-table:: API of CITYDB_DELETE package for PostgreSQL
   :name: citydb_delete_api_postgresql_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **cleanup_appearances**
       | (only_global)
     - | SET OF INTEGER
     - | Removes unreferenced Appearences incl.
       | SurfaceData and textures and returns an array of
       | their IDs. Pass 1 (default) to only delete global
       | appearances, or 0 to include local appearances
   * - | **cleanup_schema**
       | (schema_name)
     - | void
     - | Truncates most tables and resets sequences in a
       | given 3D City Database schema
   * - | **cleanup_table** (table_name)
     - | SET OF INTEGER
     - | Removes entries in given table which are not
       | referenced by any other entities
   * - | **del_cityobject** (INTEGER)
     - | INTEGER
     - | Removes the CityObject with the given ID incl.
       | all references to other tables. The ID value
       | is returned on success
   * - | **del_cityobject** ((INTEGER[ ])
     - | SET OF INTEGER
     - | Removes CityObjects with the given IDs incl.
       | all references to other tables. An array of
       | IDs of successfully deleted objects is returned
   * - | **del_cityobjects_by_lineage**
       | (lineage_value)
     - | SET OF INTEGER
     - | Removes all CityObjects on behalf of a LINEAGE
       | value and returns an array of their IDs

