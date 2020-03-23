CITYDB_OBJCLASS
---------------

The ``CITYDB_OBJCLASS`` package only provides two convenience functions to
cast between table names and ID values of the OBJECTCLASS table. In
contrast to the previously introduced packages these functions cannot be
applied against different database schemas as this would require dynamic
SQL. While it would not be problem when converting single values, the
performance with dynamic SQL could be a lot worse when these functions
are integrated in a full table scan. Therefore, for PostgreSQL they are
now part of the ‘citydb’ schema as pure SQL functions. In Oracle, they
make up another PL/SQL package.

.. list-table::  API of CITYDB_OBJCLASS package for Oracle
   :name: citydb_objclass_api_oracle_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **objectclass_id_to_table_name**
       | (objectclass_id)
     - | VARCHAR2
     - | Returns the corresponding table name to a given
       | object class ID
   * - | **table_name_to_objectclass_ids**
       | (table_name)
     - | ID_ARRAY
     - | Returns an array of object class IDs that a are
       | managed in the given table
