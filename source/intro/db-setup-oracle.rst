.. _first_step_3dcitydb_installation_oracle:

Installation steps on Oracle Databases
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Step 1 - Define a user for the 3D City Database**

A dedicated database user should be created for your work with the 3D
City Database. This user must have the roles CONNECT and RESOURCE
assigned and must own the privileges CREATE SEQUENCE and CREATE TABLE.

.. note::
   The privileges CREATE SEQUENCE and CREATE TABLE are required for
   enabling and disabling spatial indexes. It is *not sufficient* to
   inherit these privileges through a role.

**Step 2 – Edit the CONNECTION_DETAILS[.sh \| .bat] script**

Go to the 3dcitydb/oracle/ShellScrpts directory, choose the folder
corresponding to your operating system and open the file named
CONNECTION_DETAILS within a text editor. There are five variables that
will be used to connect to the DBMS. If **SQL*Plus** is already
registered in your system path, you do not have to set the directory for
the SQLPLUSBIN variable. The other parameters should be obvious to
Oracle users. Here is an example how the complete CONNECTION_DETAILS can
look like:

.. code:: bash

    SQLPLUSBIN= C:\\Oracle\\instantclient_11_2
    HOST=localhost
    PORT=1521
    SID=orcl
    USERNAME=citydb_v4

.. note::
    The scripts to grant or revoke read access require SYSDBA
    privileges. You can specify a SYSDBA user in the CONNECTION_DETAILS
    script under an additional parameter called SYSDBA_USERNAME.

**Step 3 - Execute the CREATE_DB script:**

As soon as the database credentials are defined run the CREATE_DB script
– located in the same folder as CONNECTION_DETAILS (see also :numref:`3dcitydb_shell_scripts`).

**Step 4 - Define the coordinate reference system**

When executing the CREATE_DB script, the user is prompted for the
coordinate reference system (CRS) to be used in the 3D City Database.
You have to enter the Oracle-specific SRID (spatial reference ID) of the
CRS which – in most cases – resembles the EPSG code of the CRS. There
are three prompts in total to define the spatial reference:

-  First, specify the SRID to be used for the geometry columns of the
   database. Unlike previous version of the 3D City Database there is no
   default CRS defined.

-  Second, specify the SRID of the height system if no true 3D CRS is
   used for the data. This can be regarded as metadata and has no effect
   on the geometry columns in the database. The default value is 0 –
   which means “not set”.

-  Third, provide the GML-conformant uniform resource name (URN)
   encoding of the CRS. The default value uses the OGC namespace and
   comprises of the first two user inputs:
   ``urn:ogc:def:crs,crs:EPSG::<crs1>[,crs:EPSG::<crs2>]``.

More information about the SRID and the URN encoding can be found in
:numref:`3dcitydb_crs_definition`.

**Step 5 – Enable or disable versioning**

After providing the CRS information, the user is asked whether or not
the database should be versioned-enabled. Versioning is realized based
on Oracle’s *Workspace Manager* functionality (see the Oracle
documentation for more information). Please enter ‘yes’ or ‘no’. The
default value ‘no’ is confirmed by simply pressing *Enter*. Note that,
in general, insert, update, delete and index operations on
version-enabled tables *take considerably more time* than on tables
without versioning support.

**Step 6 – Choose Spatial or Locator license option**

You can set up a 3D City Database instance on an Oracle database with
*Spatial* or *Locator* support. Since *Locator* differs from *Spatial*
with respect to the available spatial data types, you need to specify
which license option is valid for your Oracle installation. Simply enter
‘L’ for *Locator* or ‘S’ for *Spatial* (default value) to make your
choice.

.. note::
   Since *Locator* lacks the GeoRaster data type, the 3D City
   Database tables for storing raster reliefs (RASTER_RELIEF,
   GRID_COVERAGE, GRID_COVERAGE_RDT) are not created when choosing Locator.

.. note::
   Several spatial operations and functionalities that are
   available in Oracle *Spatial* are not covered by the *Locator* license
   even though they might be available from your Oracle installation. It
   is the **responsibility of the database user** to observe the Oracle
   license option. Choosing *Locator* or *Spatial* when setting up the 3D
   City Database does neither affect the license option nor the users’
   responsibility.

The following figure exemplifies the required user input during steps 4
to 6.

.. figure:: ../media/first_step_CREATE_DB_cli.png
   :name: first_step_CREATE_DB_cli
   :width: 5.5in

   Example user input when executing CREATE_DB on an Oracle database.

**Step 7 – Check if the setup is correct**

After successful completion of the setup procedure, the tables,
sequences and packages (that contain stored procedures) should appear in
the user schema.

Versioning of the database can also be switched on and off at any time.
The corresponding scripts are ENABLE_VERSIONING.sql and
DISABLE_VERSIONING.sql. These scripts invoke routines of the Oracle
Workspace Manager and will take some time for execution depending on the
amount of data stored in the 3D City Database instance.

Last but not least, the schema and stored procedures of the 3D City
Database can be dropped with the DROP_DB script, which is executed like
CREATE_DB. Similar to CREATE_DB, you need to provide the license option
(*Locator* or *Spatial*). Note that the script will **delete all data**
stored in the 3D City Database schema. The database user will, however,
not be deleted.