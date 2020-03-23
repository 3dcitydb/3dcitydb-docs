CITYDB_STAT
-----------

The package ``CITYDB_STAT`` currently only serves a single purpose: To count
all entries in all tables and generate a report as an array of string
values (``STRARRAY`` data type in Oracle, ``text[]`` in PostgreSQL). The
tabulator escape sequence ``\t`` is used to generate a nice looking report
for the Importer/Exporter.

.. list-table:: API of CITYDB_STAT package for Oracle
   :name: citydb_stat_api_oracle_table

   * - | **Function**
     - | **Return Type**
     - | **Explanation**
   * - | **table_content** (table_name,
       | schema_name)
     - | NUMBER
     - | Returns the count result obtained from a query
       | against the given table
   * - | **table_contents** (schema_name)
     - | STRARRAY
     - | Returns a text array with row count results
       | for most tables in 3D City Database (excluding
       | metadata tables and system tables)

