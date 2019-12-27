V2 to V4 Migration on Oracle
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Step 1 – Upgrade an existing installation**

The migration to v4.0 **must be carried out on a version 2.1.0
instance** of the 3D City Database. Versions prior to version 2.1.0 must
first be upgraded to 2.1.0 since the internal storage of envelopes of
city objects changed substantially. Corresponding upgrade scripts are
shipped with the v2.1.0 release. Upgrades to 2.1.0 can be carried out
from any older version 2.0.0 to 2.0.6. A more detailed description of
the upgrade procedure can be found in the document “Documentation of the
3D City Database v2.1.0 and the Importer/Exporter v1.6.0”.

Before upgrading your 3D City Database, a database backup is highly
recommended to secure all data. The latter can be easily done using the
Importer/Exporter tool or by tools provided by Oracle.

**Important:** Please note that the last step in the upgrade process is
a lengthy one. Altering the internal storage of the envelopes of all
city objects in a large and/or versioned database may take hours.
Depending on their initial state, spatial indexes may be disabled and
re-enabled in the process, adding to the duration as a whole. This
process MUST NOT be interrupted since it could lead to an inconsistent
state. Please be patient and remember that backing up all of your data
before starting any database upgrade is the commonly recommended
practice.

**Step 2 – Creating a new installation**

The migration script transfers data from a user schema with the v2.1.0
installation to another user schema that has to contain the 3D City
Database schema v4.0. Install the new version like it is described in
:numref:`first_step_3dcitydb_installation_oracle` and :numref:`first_step_3dcitydb_installation_postgis`
if not done so yet.

**Step 3 – Grant select on v2.1.0 schema to v4.0 schema**

The migration process requires that the user with the v4.0 schema can
access the user schema with the v2.1.0 version. Therefore, run the
GRANT_ACCESS_V2 shell script (see :numref:`3dcitydb_shell_scripts`) as the V2 user.
When executed the user is requested to type in the schema name for the
3D City Database v4.0 instance.

**Step 4 – Run MIGRATE_DB**

Now, start the MIGRATE_DB script located in the same folder like
GRANT_ACCESS_V2 as the V4 user. Choose the value 2 as first input and
specify the name of the schema with the v2.1.0 instance.

**Step 5 – Be sure of using unique texture URIs**

Starting from v3.0.0 of the 3D City Database, textures that are
referenced to more than one geometry are no longer stored redundantly in
the SURFACE_DATA table but only once in the TEX_IMAGE table. This
optimization can also be done during the migration process, if it is
guaranteed that texture URIs are unique and not used for different
texture files. Otherwise, some textures would get lost during the
migration and remaining images would be referenced to wrong surfaces.
Therefore, if you can assure the non-existence of duplicate texture
URIs, verify with ‘y’ or ‘yes’. In case you know that textures in the
database are named equally (or if you do not know) you can still run the
script by entering ‘n’ or nothing (because it is the default). Entries
in the TEX_IMAGE column of the SURFACE_DATA table from version 2.1 are
then further mapped 1:1 to the TEX_IMAGE table of version 4.0.

.. note::
   A simple unification of texture URIs in advance of the migration
   will not help to store the textures only once, because same textures
   with different URIs are regarded as different image files and would all
   end up in the new TEX_IMAGE table. You would have to compare the binary
   data itself.

**Step 6 – Choose Spatial or Locator license option**

With the last input parameter you specify the database license running
on your Oracle server, like you have done when setting up the v4.0
instance of the 3D City Database. Choose ‘S’ for Spatial (which will
additionally migrate raster data) and ‘L’ for Locator.

**Step 7 – Check if the setup is correct**

The script temporary disables databases indexes and foreign key
constraints and creates an additional package with migration procedures
(CITYDB_MIGRATE). The package is removed again when the migration
progress is completed and the message "DB migration is completed
successfully." is displayed on the console. It is recommended to
generate a database report of the new user schema and compare it with a
report of the schema that contains the 2.1 instance of the 3D City
Database (done with the previous version of the Import/Export tool).
**Verify that**

-  no city objects are missing (do a database report),

-  indexes and foreign keys got activated again,

-  relations between features and attributes are correct, and

-  exports look correct inside a viewer application.

**Step 8 – Drop the deprecated v2.x schema**

If the migration was successful, the v2.x user simply has to invoke
the DROP_DB (of version 2.x) to drop the deprecated schema. Deleting the
v2.x user works as well.
