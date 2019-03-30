3D City Database SQL Scripts
----------------------------

The required scripts for setting up the 3D City Database are in the
installation directory of the Importer/Exporter within the
*3dcitydb/oracle/* or *3dcitydb/postgresql/* subfolders.

Shell Scripts
~~~~~~~~~~~~~

In previous versions of the 3D City Database the setup was managed
through user prompts in SQL scripts. To facilitate continuous
integration workflows these inputs have been moved to batch (Windows)
and shell scripts (UNIX/Linux/macOS). The following table provides an
overview of the different shell scripts:

========================== ====== ===== ==========================================================================================================
File                       Oracle PgSQL Explanation
========================== ====== ===== ==========================================================================================================
CONNECTION_DETAILS         x      x     Sets database credentials
CREATE_DB                  x      x     Runs all scripts for creating the relational schema of the 3DCityDB incl. user-defined types and functions
CREATE_SCHEMA                     x     Creates an additional 3DCityDB instance in a separate schema within the same database
DROP_DB                    x      x     Deletes all elements of the 3DCityDB
MIGRATION/ DROP_DB_V2             x     Deletes all deprecated elements of a 3DCityDB v2 after a successful migration towards v4
DROP_SCHEMA                       x     Removes a given database schema that contains a 3DCityDB instance
MIGRATION/ GRANT_ACCESS_V2 x            Grants access on a 3DCityDB v2 to a v4 user (only relevant for migration)
GRANT_ACCESS               x      x     Grants read-only of read-write access on the 3DCityDB for a given user
MIGRATION/ MIGRATE_DB      x      x     Starts the migration process from an older version
REVOKE_ACCESS              x      x     Revokes access rights for a given user
========================== ====== ===== ==========================================================================================================

Table 19: Overview of all shell scripts within 3dcitydb/oracle or
3dcitydb/postgresql folder.

The batch/shell scripts can be executed on double click. On some
UNIX/Linux distributions though, you will have to run the .sh scripts
from within a shell environment. Please open your favorite shell and
check whether execution permission is set for the starter script. Change
to the installation folder and enter the following to make the starter
script executable for the owner of the file, e.g.:

chmod u+x CREATE_DB.sh

Afterwards, simply run the shell script by typing:

./CREATE_DB.sh

.. note::
   The **database connection details need to be set** in the
   CONNECTION_DETAILS script prior to executing the batch/shell scripts.

SQL Scripts
~~~~~~~~~~~

The SQLScripts directory contains four subfolders:

**SCHEMA**

Includes SQL files about the logical (tables, constraints) and physical
(datatypes, indexes) database schema of the 3D City Database exported
from the schema modelling tools JDeveloper (Oracle) or pgModeler
(PostgreSQL) (with minor changes). INSERT statements for the prefilled
lookup tables OBJECTCLASS and AGGREGATION_INFO as well as converter
functions between table names and objectclass IDs can be found in the
OBJECTCLASS subfolder. Because of PostgreSQL’s way to handle database
schemas the SCHEMA folder contains a few more scripts with stored
procedures. See next chapter for more details.

**CITYDB_PKG**

Contains scripts that create database objects and stored procedures
mainly to be used by the Importer/Exporter application. They are written
in PL/SQL (Oracle) or PL/pgSQL (PostgreSQL) and grouped by the type of
operation (data manipulation, maintenance etc.). The APIs are introduced
in chapter 4.

**UTIL**

This folder assembles different database management utilities:

-  Grant and revoke read rights to and from the 3D City Database.

-  Create additional database schemas with a 3D City Database layout
   (PostgreSQL-only)

-  Enable or disable versioning (execution can be time-consuming)
   (Oracle-only)

-  Update table statistics for spatial columns (PostgreSQL-only)

**MIGRATION**

Provides a migration path from previous releases to the newest version.
See chapter 3.4 for more details. This folder will also include upgrade
scripts for upcoming minor releases.
