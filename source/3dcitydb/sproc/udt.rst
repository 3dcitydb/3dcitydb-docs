User-defined data types
-----------------------

The Oracle version defines a set of user-defined data types that are
used by functions from the PL/SQL packages. They are not necessary in
PostgreSQL, because of how it deals with arrays and returns of multiple
variables.

-  STRARRAY, a nested table of the data type VARCHAR2

-  ID_ARRAY, a nested table of the data type NUMBER

-  DB_VERSION_OBJ, an object that bundles version information of the
   installed 3D City Database instance

-  DB_VERSION_TABLE, a nested table of DB_VERSION_OBJ

-  DB_INFO_OBJ, an object that bundles metadata of the used reference
   system

-  DB_INFO_TABLE, a nested table of DB_INFO_OBJ

The definition of the data types can be found in the SQL file for the
CITYDB_UTIL package.
