Migration from previous major releases
--------------------------------------

**Scripts are located in the folder
3dcitydb/[oracle/postgresql]/MIGRATION within the installation directory
of the Importer/Exporter tool. A migration path is provided for 3D City
Databases of version 2.1 and of version 3.3.**

*Hint:* Another **safe and simple migration approach is to** export the
database content from the v2.x/v3.x instance as CityGML with the
previous version of the Importer/Exporter and to re-import the data into
the new 3D City Database version by using the new Importer/Exporter
shipped with this release. This approach might take more time though,
depending on the amount of data stored in the database.

.. note::
   The migration scripts do not handle version-enabled tables under
   Oracle. Therefore, if you are using Oracle and have enabled versioning,
   then exporting and re-importing the data is the recommended way to
   migrate to the new 3DCityDB version.

To start the migration process run the MIGRATE_DB shell script. Make
sure, the database credentials taken from the CONNECTION_DETAILS file
are correct. With the first input you need to enter the major version
number of the currently installed 3D City Database instance – either 2
or 3. **To identify the actual version of your 3D City Database you can
use the Importer/Exporter tool to connect to the 3D City Database
instance that you want to upgrade. Starting from v3.0.0 the version
string is printed to the console window after the connection has been
successfully established as shown below (see chapter** **5.2.1 for
details).**

|image59|

Figure 53: Version information of a 3D City Database.

**If the version string does not show up, you are running a v2.x
instance. Alternatively, the version information can also be queried
using database-side functions. For** Oracle the command is:

SQL> select MAJOR_VERSION from table(CITYDB_UTIL.CITYDB_VERSION);

For PostgreSQL it is:

psql> SELECT major_version FROM citydb_pkg.citydb_version();

If the function is not known to the system, you are probably running a
v2.x instance. For Oracle Database, migrating from v2 to v4 has some
prerequisites which will be explained in detail in the next chapter.


V3 to V4 Migration
~~~~~~~~~~~~~~~~~~

**The migration process from v3 to v4 does not require any user inputs
after entering the value 3 in the MIGRATE_DB script (except for choosing
the license under Oracle). Please note, that schema changes on existing
tables are applied with ALTER TABLE statements which can lock these
tables for a longer period if they contain millions of rows.**

Upgrade between minor releases
------------------------------

**Every minor release of the 3D City Database is shipped with an upgrade
script if necessary. Starting from version 4.x.x it can be found in the
MIGRATION folder. Like with other database DDL tasks a shell script will
be provided as well to ease the upgrade process. Make sure to first
check the current version of your 3D City Database installation before
performing an upgrade, as mentioned in the migration chapter** **3.5.**
During an upgrade check the output messages of the script for errors and
warnings. The process should finish the message “3D City Database
upgrade complete”.
