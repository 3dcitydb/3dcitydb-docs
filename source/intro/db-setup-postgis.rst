.. _first_step_3dcitydb_installation_postgis:

Installation steps on PostgreSQL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Step 1 - Create an empty PostgreSQL database**

Choose a superuser or a user with the CREATEDB privilege to create a new
database on the PostgreSQL server (e.g. 'citydb_v4'). As owner of this
new database, choose or create a user who will later set up the 3D City
Database instance. Otherwise, more permissions have to be granted. In
the following steps, this user is called 'citydb_user'.

Connect to the database and type:

.. code:: sql

    CREATE DATABASE citydb_v4 OWNER citydb_user;

or use a graphical database client such as *pgAdmin* that is shipped
with PostgreSQL. Please check the *pgAdmin* documentation for more
details.

**Step 2 – Add the PostGIS extension**

The 3D City Database requires the PostGIS extension to be added to the
database. This can **only be done as superuser**. The extension is added
with the following command (or, alternatively, using *pgAdmin*):

.. code:: sql

    CREATE EXTENSION postgis;

Some 3D operations such as extrusion or volume calculation are only
available through the PostGIS **SFCGAL** extension. **The installed
PostGIS add-on should at least be on version 2.2** to execute the DDL
command:

.. code:: sql

    CREATE EXTENSION postgis_sfcgal;

**Step 3 – Edit the CONNECTION_DETAILS[.sh \| .bat] script**

Go to the 3dcitydb/postgresql/ShellScrpts directory, choose the folder
corresponding to your operating system and open the file named
CONNECTION_DETAILS within a text editor. There are five variables that
will be used to connect to the DBMS. If **psql** is already registered
in your system path, you do not have to set the directory for the PGBIN
variable. The other parameters should be obvious to PostgreSQL users.
Here is an example how the complete CONNECTION_DETAILS can look like:

.. code:: bash

    PGBIN= C:\PostgreSQL\9.6\bin
    PGHOST=localhost
    PGPORT=5432
    CITYDB=citydb_v4
    PGUSER=citydb_user

**Step 4 - Execute the CREATE_DB script**

As soon as the database credentials are defined run the CREATE_DB script
– located in the same folder as CONNECTION_DETAILS (see also :numref:`3dcitydb_shell_scripts`).

**Step 5 – Specify the coordinate reference system**

Like with the Oracle version, the user is prompted to enter the SRID
used for the geometry columns, the SRID of the height system and the URN
encoding of the coordinate reference system to be used (see :numref:`3dcitydb_crs_definition` for more information).

.. note::
   The setup process will terminate immediately if an error occurs.
   Reasons might be:

-  The user executing CREATE_DB.sql is neither a superuser nor the owner
   of the specified database (or does not own privileges to create
   objects in that database);

-  The PostGIS extension has not been installed; or

-  Parts of the 3D City Database do already exist because of a previous
   setup attempt. Therefore, make sure that the schemas ‘citydb’ and
   ‘citydb_pkg’ do not exist in the database when setting up the 3D City
   Database.

After a series of log messages reporting the creation of database
objects, the chosen reference system is applied to the spatial columns
(expect for those that will store data with local coordinate systems).
This takes some seconds and is finished when the word ‘Done’ is
displayed.

**Step 5 – Check if the setup is correct**

The 3D City Database is stored in a separate PostgreSQL schema called
‘citydb’. The stored procedures are written to a separate PostgreSQL
schema called ‘citydb_pkg’. Usually different schemas have to be
addressed in every query via dot notation, e.g.

.. code:: sql

    SELECT * FROM citydb.building;

Fortunately, this can be avoided when the corresponding schemas are on
the database **search path**. The search path is **automatically
adapted** during the setup. Execute the command

.. code:: sql

    SHOW search_path;

to check if the schemas citydb, citydb_pkg and public (for PostGIS
elements) are contained.

.. note::
   When using the created 3D City Database as a template database
   for new databases, the search path information is not transferred and
   thus has to be set again, e.g.:

   .. code:: sql

       ALTER DATABASE new_citydb_v4 SET search_path TO citydb, citydb_pkg, public;

   The search path will be updated upon the next login, not within the
   same session.

To drop the 3D City Database with all data, execute the DROP_DB.sql
script in the same way like CREATE_DB.sql. Simply dropping the schemas
‘citydb’ and ‘citydb_pkg’ in a cascading way will also do the job.
