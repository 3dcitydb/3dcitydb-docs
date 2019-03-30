CITYDB_CONSTRAINT
-----------------

The CITYDB_CONSTRAINT packages includes stored procedures to define
constraints or change their behavior. A user can temporarily disable
certain foreign key relationships between tables, e.g. the numerous
references to the SURFACE_GEOMETRY table. The constraints are not
dropped. While it comes at the risk of data inconsistency it can improve
the performance for bulk write operations such as huge imports or the
deletion of thousands of city objects.

It is also possible to change the delete rule of foreign keys from ON
DELETE NO ACTION (use 'a' as input) to ON DELETE SET NULL ('n') or ON
DELETE CASCADE ('c'). Switching the delete rule will remove and recreate
the foreign key constraint. The delete rule does affect the layout of
automatically generated delete scripts as no explicit code is necessary
in case of cascading deletes. However, we do not recommend to change the
behavior of existing foreign key relationships because some delete
operations might not work properly anymore. For Oracle databases, there
is an additional procedure to define spatial metadata for single
geometry column. All functions are schema-aware and their return type is
void.

================================================================================================================================ ===================================================================================================================
Function                                                                                                                         Explanation
================================================================================================================================ ===================================================================================================================
**set_column_sdo_metadata** (*geom_column_name*, *dimension*, *srid*, *table_name*, *schema_name*)                               Inserts a new entry in the USER_SDO_GEOM_METADATA view for a given geometry column
**set_enabled_fkey** (*fkey_name*, *table_name*, BOOLEAN, *schema_name*)                                                         Disables / enables a given foreign key constraint
**set_enabled_geom_fkeys** (BOOLEAN, *schema_name*)                                                                              Disables / enables all foreign key constraints that reference the SURFACE_GEOMETRY table
**set_enabled_schema_fkeys** (BOOLEAN, *schema_name*)                                                                            Disables / enables all foreign key constraints within a given user schema
**set_fkey_delete_rule** (*fkey_name*, *table_name*, *column_name*, *ref_table*, *ref_column*, *on_delete_param*, *schema_name*) Changes the delete rule of a given foreign key constraint
**set_schema_fkey_delete_rule** (*on_delete_param*, *schema_name*)                                                               Changes the delete rule of all foreign key constraint within a given user schema
**set_schema_sdo_metadata** (*schema_name*)                                                                                      Inserts new entries in the USER_SDO_GEOM_METADATA view for all geometry columns of a given schema (some expections)
================================================================================================================================ ===================================================================================================================

Table 22: API of CITYDB_CONSTRAINT package for Oracle

There is only one significant difference in the API in PostgreSQL.
Instead of specifying the name, table and schema of a foreign key, the
OID of the corresponding integrity trigger is enough. This is because
there is no ALTER TABLE command in PostgreSQL to disable foreign keys.

================================================== =========================================================
Function                                           Explanation
================================================== =========================================================
**set_enabled_fkey** (*fkey_trigger_oid*, BOOLEAN) Disables / enables a given foreign key constraint trigger
================================================== =========================================================

Table 23: Notable difference in the API of CITYDB_CONSTRAINT package for
PostgreSQL
