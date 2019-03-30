V2 to V4 Migration on PostgreSQL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Step 1 – Run MIGRATE_DB**

For PostgreSQL, setting up a new v4.0 instance is not necessary.
Simply execute the MIGRATE_DB shell script and choose the value 2 as
first input.

**Step 2 – Be sure of using unique texture URIs**

Like with the Oracle version, you are requested to guarantee that no
texture URI is used for different images. See Step 5 in the workflow
explanation of the Oracle version for further details.

**Step 3 – Check if the setup is correct**

After a series of log messages reporting the selection of data from the
v2.x schema, updates of references and the creation of database objects,
the script is finished with the message '3DCityDB migration complete!'.
If the old database schema is not dropped during the migration (see
last step), both versions of the 3D City Database will remain in one
database. This is actually a good thing, because you can further compare
if everything has been transferred correctly.

**Idempotent migration**

If the migration process has been interrupted by the user or by severe
software errors, the migration script can simply be executed again (only
if the old v2.x schema still exists) without manually dropping already
created parts of the v4.0 schema because the script does it for you.

**Step 4 – Drop** **the deprecated v2.x schema**

To remove the deprecated parts of your 3D City Database invoke the
DROP_DB_V2 shell script.

.. warning::
   DO NOT execute the DROP_DB script as the old and new instance of
   the 3D City Database are both stored inside the same database
   (new = citydb schema, old = public schema). DROP_DB drops all
   database schemas where it finds a DATABASE_SRS table, so all you data
   would be lost. Be careful!
