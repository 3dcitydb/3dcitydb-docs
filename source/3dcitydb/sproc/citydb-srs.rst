.. _citydb_sproc_srs_chapter:

CITYDB_SRS
----------

The package CITYDB_SRS provides functions and procedures dealing with
the coordinate reference system used for an 3D City Database instance.
The most essential procedure is change_schema_srid to change the
reference system for all spatial columns within a database schema. If a
coordinate transformation is needed because an alternative reference
system shall be used, the value ‘1’ should be passed to the procedure as
the third parameter. If a wrong SRID had been chosen by mistake during
setup, a coordinate transformation might not be necessary in case the
coordinate values of the city objects are already matching the new
reference system. Thus, the value 0 should be provided to the procedure,
which then only changes the spatial metadata to reflect the new
reference system. It can also be omitted, as 0 is the default value for
the procedure. Either way, changing the CRS will drop and recreate the
spatial index for the affected column. Therefore, this operation can
take a lot of time depending on the size of the table. Note that in
Oracle, the reference system cannot be changed for another user schema.
So, there is no *schema_name* parameter. The is also an additional
function called get_dim(column_name, table_name, schema_name) to fetch
the dimension of the spatial column which is either 2 or 3.

.. list-table:: API of CITYDB_SRS package for PostgreSQL
   :name: citydb_srs_api_postgresql_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **change_column_srid**
       | (table_name, column_name,
       | dimension, srid, do_transform,
       | geometry_type, schema_name)
     - | void
     - | Changes the reference system for a
       | given geometry column. Spatial metadata
       | is needed to recreate the spatial index.
   * - | **change_schema_srid** (srid,
       | gml_srs_name, do_transform,
       | schema_name)
     - | void
     - | Changes the reference system for all
       | spatial columns inside a database schema.
       | The second parameter needs to be a
       | GML-compliant URN to the CRS
       | (see :numref:`citydb_crs_definition_chapter`)
   * - | **check_srid** (srid)
     - | TEXT
     - | Returns the message 'SRID ok' if the CRS
       | with the given EPSG code exists in the
       | database. Returns 'SRID not ok' if not.
   * - | **is_coord_ref_sys_3d** (srid)
     - | INTEGER
     - | Tests if CRS with given EPSG code is a
       | 3D CRS. Returns 1 if yes and 0 if not.
   * - | **is_db_coord_ref_sys_3d**
       | (schema_name)
     - | INTEGER
     - | Tests if the current CRS of a given schema
       | is a 3D one. Returns 1 if yes and 0 if not.
   * - | **transform_or_null**
       | (GEOMETRY, srid)
     - | GEOMETRY
     - | Applies a coordinate transformation on the
       | input geometry with the given CRS. Returns
       | NULL, if the input geometry is not set.