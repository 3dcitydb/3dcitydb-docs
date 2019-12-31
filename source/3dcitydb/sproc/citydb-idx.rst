CITYDB_IDX
----------

The package CITYDB_IDX provides functions to create, drop, and check
both spatial and non-spatial indexes on tables of the 3D City Database
by using a user-defined data type called INDEX_OBJ. In the Oracle
version, the data type offers three member functions to construct an
INDEX_OBJ. In the PostgreSQL version, these are just separate functions
within the ‘citydb_pkg’ schema:

-  construct_spatial_3d for a 3-dimensional spatial index

-  construct_spatial_2d for a 2-dimensional spatial index

-  construct_normal for a normal B-tree index

The easiest way to take use of this package is by using the
Importer/Exporter (see :numref:`impexp_executing_database_operations_chapter`),
which provides an interface for
enabling and disabling indexes (ON and OFF). Disabling spatial indexes
can accelerate some operations such as bulk imports, deletion of many
objects, and migration of data from a 3D City Database v2.1.0 instance
to version 4.0. The methods used by the Importer/Exporter iterate over
the entries in the INDEX_TABLE table which is part of the database
schema. In order to include more indexes the user need to insert their
metadata into INDEX_TABLE. The differences between Oracle and PostgreSQL
only apply to different data types. Instead of STRARRAY an array of TEXT
is used as return type.

.. list-table::  API of CITYDB_IDX package for Oracle
   :name: citydb_inx_api_oracle_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **create_index** (INDEX_OBJ,
       | is_versioned, schema_name)
     - | VARCHAR2
     - | Creates a new index based on the metadata of the
       | input INDEX_OBJ. Returns a text status.
   * - | **create_normal_indexes**
       | (schema_name)
     - | STRARRAY
     - | Creates indexes for all normal indexes to be
       | found in INDEX_TABLE. Returns an array of
       | status reports.
   * - | **create_spatial_indexes**
       | (schema_name)
     - | STRARRAY
     - | Creates indexes for all spatial indexes to be
       | found in INDEX_TABLE. Returns an array of
       | status reports.
   * - | **drop_index** (INDEX_OBJ,
       | is_versioned, schema_name)
     - | VARCHAR2
     - | Drops an index that matches the metadata of
       | the input INDEX_OBJ. Returns a text status.
   * - | **drop_normal_indexes**
       | (schema_name)
     - | STRARRAY
     - | Drops indexes that match all normal indexes
       | to be found in  INDEX_TABLE. Returns an array
       | of status reports.
   * - | **drop_spatial_indexes**
       | (schema_name)
     - | STRARRAY
     - | Drops indexes that match all spatial indexes
       | to be found in  INDEX_TABLE. Returns an array
       | of status reports.
   * - | **get_index** (table_name,
       | column_name,
       | schema_name)
     - | INDEX_OBJ
     - | Returns an INDEX_OBJ from INDEX_TABLE
       | based on the inputs
   * - | **index_status** (INDEX_OBJ,
       | schema_name)
     - | VARCHAR2
     - | Returns a text status for an index that matches
       | the metadata of the input INDEX_OBJ
   * - | **index_status** (table_name,
       | column_name,
       | schema_name)
     - | VARCHAR2
     - | Returns a text status for an index that matches
       | the input argument
   * - | **status_normal_indexes**
       | (schema_name)
     - | STRARRAY
     - | Returns an array of status reports for all normal
       | indexes to be found in INDEX_TABLE
   * - | **status_spatial_indexes**
       | (schema_name)
     - | STRARRAY
     - | Returns an array of status reports for all spatial
       | indexes to be found in INDEX_TABLE