CITYDB_UTIL
-----------

The CITYDB_UTIL package can be seen as a container for various single
utility functions. If further releases will bring more stored procedures
with similar functionality some of them will probably be outsourced in
their own package (like CITYDB_CONSTRAINT in v4.0). Nearly all functions
take the schema name as the last input argument (“schema-aware”).
Therefore, they can be executed against another user schema in Oracle or
database schema in PostgreSQL. Note, for the function get_seq_values the
schema name must be part of the first argument – the sequence name, e.g.
'my_schema.cityobject_seq'.

Here is overview on API of the CITYDB_UTIL package in Oracle:

.. list-table::  API of CITYDB_UTIL package for Oracle
   :name: citydb_util_api_oracle_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **citydb_version** ()
     - | DB_VERSION_TABLE
     - | Returns version information of the
       | currently installed 3DCityDB
   * - | **construct_solid** (geom_root_id)
     - | SDO_GEOMETRY
     - | Tries to construct a solid geometry
       | based on a given root_id value in
       | SURFACE_GEOMETRY table
   * - | **db_info** (schema_name)
     - | 3 OUT variables
     - | Returns three columns: schema_srid
       | INTEGER, schema_gml_srs_name
       | VARCHAR2, versioning VARCHAR2
   * - | **db_metadata** (schema_name)
     - | DB_INFO_TABLE
     - | Returns a set of 3DCityDB metadata
   * - | **drop_tmp_tables** (schema_name)
     - | void
     - | Drop existing temporal tables
   * - | **get_id_array_size** (ID_ARRAY)
     - | NUMBER
     - | Returns the size of an ID_ARRAY
       | nested table
   * - | **get_seq_values** (seq_name,
       | seq_count)
     - | ID_ARRAY
     - | Returns the next k values of a
       | given sequence
   * - | **min** (NUMBER, NUMBER)
     - | NUMBER
     - | Returns the smaller of two given
       | numbers
   * - | **sdo2geojson3d**
       | (SDO_GEOMETRY,
       | decimal_places, compress_tags,
       | relative2mbr)
     - | CLOB
     - | Returns a given geometry into a
       | 3D GeoJSON character object
   * - | **split** (VARCHAR2, delimiter)
     - | STRARRAY
     - | Splits a String based on a given
       | delimiter into a STRARRAY object
   * - | **ST_Affine** (SDO_GEOMETRY,
       | row1col1, row1col2, row1col3,
       | row2col1, row2col2, row2col3,
       | row3col1, row3col2, row3col3,
       | row1col4, row2col4, row3col4)
     - | SDO_GEOMETRY
     - | Performs an affine transformation
       | on a given geometry a given 3x3
       | matrix plus 3 offset values
   * - | **string2id_array** (VARCHAR2,
       | delimiter)
     - | ID_ARRAY
     - | Transforms a String into an
       | ID_ARRAY with a given delimiter
   * - | **to_2d** (SDO_GEOMETRY, srid)
     - | SDO_GEOMETRY
     - | Returns a geometry without Z values
   * - | **versioning_db** (schema_name)
     - | VARCHAR2
     - | Returns either ‘ON’ or ‘OFF’
   * - | **versioning_table** (table_name,
       | schema_name)
     - | VARCHAR2
     - | Returns either ‘ON’ or ‘OFF’

The PostgreSQL API includes less functions, as some functionality is
provided by the PostGIS extension, such as ST_AsGeoJSON, ST_Affine and
ST_Force2D. Returning multiple variables is always performed with OUT
variables.

.. list-table::  API of CITYDB_UTIL package for PostgreSQL
   :name: citydb_util_api_postgresql_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **citydb_version** ()
     - | 4 OUT variables
     - | Returns version information of the
       | currently installed 3DCityDB
   * - | **db_info** (schema_name)
     - | 3 OUT variables
     - | Returns three columns: schema_srid
       | INTEGER, schema_gml_srs_name
       | TEXT, versioning TEXT
   * - | **db_metadata** (schema_name)
     - | 6 OUT variables
     - | Returns six variables: schema_srid
       | INTEGER, schema_gml_srs_name TEXT,
       | coord_ref_sys_name TEXT,
       | coord_ref_sys_kind TEXT,
       | wktext TEXT, versioning TEXT
   * - | **drop_tmp_tables** (schema_name)
     - | void
     - | Drop existing temporal tables
   * - | **get_seq_values** (seq_name,
       | seq_count)
     - | SETOF INTEGER
     - | Returns the next k values of a
       | given sequence
   * - | **Min** (NUMERIC, NUMERIC)
     - | NUMERIC
     - | Returns the smaller of two given
       | numbers
   * - | **versioning_db** (schema_name)
     - | TEXT
     - | Returns ‘OFF’
   * - | **versioning_table** (table_name,
       | schema_name)
     - | TEXT
     - | Returns ‘OFF’
