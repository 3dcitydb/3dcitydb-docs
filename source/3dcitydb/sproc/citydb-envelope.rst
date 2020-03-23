CITYDB_ENVELOPE
---------------

The package ``CITYDB_ENVELOPE`` provides functions that allow a user to
calculate the maximum 3D bounding volume of a *CityObject* identified by
its ID. For each feature type, a corresponding function is provided
starting with ``env_`` prefix. In PostgreSQL, they are part of an instance
schema like ‘citydb’ and not ‘citydb_pkg’ due to unforeseen schema
changes by adding CityGML ADEs.

The bounding volume is calculated by evaluating all geometries of the
city object in all LoDs including implicit geometries. In PostGIS, they
are first collected and then fed to the ``ST_3DExtent`` aggregate function
which returns a ``BOX3D`` object. In Oracle the aggregate function
``SDO_AGGR_MBR`` is used which produces a 3D optimized rectangle with only
two points. The ``box2envelope`` function turns this output into a diagonal
cutting plane through the calculated bounding volume. This surface
representation follows the definition of the ENVELOPE column of the
CITYOBJECT table as discussed in :numref:`citydb_schema_core_model_chapter`
(see also :numref:`citydb_envelope_definition`).
All functions in this package return such a geometry.

The ``CITYDB_ENVELOPE`` API also allows for updating the ENVELOPE column of
the city objects with the calculated value (by simply setting the
``set_envelope`` argument that is available for all functions to *1*).
This is useful, for instance, whenever one of the geometry
representations of the city object has been changed or if the ENVELOPE
column could not be (correctly) filled during import and, for example,
is ``NULL``.

To calculate and update the ENVELOPE of all city objects of a given
feature type, use the ``get_envelope_cityobjects`` function and provide the
``OBJECTCLASS_ID`` as parameter. If *0* is passed as ``OBJECTCLASS_ID``, then
the ENVELOPE columns of all city objects are updated. To update only
those ENVELOPE columns having ``NULL`` as value, set the ``only_if_null``
parameter to *1*.

.. list-table:: API of the CITYDB_ENVELOPE package for PostgreSQL
   :name: citydb_envelope_api_postgresql_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **box2envelope** (BOX3D)
     - | GEOMETRY
     - | Takes a BOX3D and returns a 3D polygon that
       | represents a diagonal cutting plane through this
       | box. Under Oracle the input is an optimized 3D
       | rectangle (SDO_INTERPRETATION = 3)
   * - | **env_cityobject** (cityobject_id,
       | set_envelope)
     - | GEOMETRY
     - | Returns the current envelope representation of
       | the given CityObject and optionally updates the
       | ENVELOPE column
   * - | **get_envelope_cityobjects**
       | (objectclass_id, set_envelope,
       | only_if_null)
     - | GEOMETRY
     - | Returns the current envelope representation of
       | all CityObjects of given object class and
       | optionally updates the ENVELOPE column with
       | the individual bounding boxes
   * - | **get_envelope_implicit_geometry**
       | (implicit_rep_id, reference_point,
       | transformation_matrix)
     - | GEOMETRY
     - | Returns the envelope of an implicit geometry
       | which has been transformed based on the
       | passed reference point and transformation
       | matrix
   * - | **update_bounds** (old_box,
       | new_box)
     - | GEOMETRY
     - | Takes two GEOMETRY objects to call
       | box2envelope and returns the result. If one
       | side is NULL, the non-empty input is
       | returned.