.. _ade_manager_plugin_registration_chapter:

ADE registration
----------------

The *ADE registration* operation of the *ADE Manager* plugin allows you to register
a CityGML ADE with your 3D City Database. During the ADE registration process,
new and ADE-specific database objects such as feature tables, functions, sequences,
and indexes are added to the existing 3DCityDB database schema. Also, the 3DCityDB
metadata tables (see :numref:`chapter_citydb_schema_metadata`) are populated with the
information about the ADE.

.. figure:: /media/ade_manager_plugin_ade_registration.png
   :name: ade_manager_plugin_ade_registration
   :align: center

   The ADE Registration dialog of the ADE Manager plugin.

**ADE extension package**

The registration operation requires an *ADE extension package* as input.
An ADE extension package is a collection of database scripts together with
a schema mapping file organized in a predefined folder structure. This
folder structure is illustrated below.

.. figure:: /media/ade_manager_plugin_input_folder_structure.png
   :name: ade_manager_plugin_input_folder_structure
   :width: 231px
   :align: center

   Structure of an ADE extension package required for registering an ADE.

The root folder of the package must contain the two mandatory subfolders
*3dcitydb* and *schema-mapping*. The first subfolder *3dcitydb*
again contains two subfolders called *oracle* and *postgreSQL* that contain
database-specific SQL scripts for creating and dropping the ADE schema.
These files must be named *CREATE_ADE_DB.sql* and *DROP_ADE_DB.sql*.

The *CREATE_ADE_DB.sql* script will be executed by the ADE
Manager plugin for creating the 3DCityDB compliant ADE database schema
according to the database type (PostgreSQL or Oracle) being used. The
SQL file *DROP_ADE_DB.sql* must contain the SQL statements for removing the
corresponding ADE database schema. These statements are imported
into the *ADE* metadata table of the 3DCityDB schema
(see :numref:`chapter_citydb_schema_metadata`) during the ADE registration process and
hence are persistently stored in the database. When removing
an ADE using the *ADE Operations* dialog (see :numref:`impexp_plugin_ade_manager_ade_operations`),
the statements will be read from the ADE table and executed by the ADE Manager plugin.

The second subfolder *schema-mapping* shall contain an XML file called
*schema-mapping.xml*. This file defines the relevant metadata about the
ADE extension (e.g., name, description, XML namespace, value range of
object class IDs) as well as the explicit mapping of elements of
the XML Schema definition of the CityGML ADE to tables and columns in
the relational database schema. This schema mapping file is not only
used in the ADE registration process, but also required by most operations
of the Importer/Exporter, for instance, to automatically build SQL
queries against the 3DCityDB and ADE tables when exporting data.

All files and folder in an ADE extension package **must adhere to the structure
and naming conventions** explained above to be accepted by the ADE Manager plugin.
The name of the root folder of the package can be chosen freely though.

.. note::
   The *ADE transformation* operation of the ADE Manager plugin automatically creates
   a valid ADE extension package with all required files from the
   XML Schema definition of a CityGML ADE. More information is provided in
   :numref:`impexp_plugin_ade_manager_ade_transformation`.

.. note::
   A schema mapping file is also used for the predefined
   database schema of the 3DCityDB. It can be found
   `here <https://github.com/3dcitydb/importer-exporter/tree/master/impexp-core/src/main/resources/org/citydb/database/schema>`_
   together with a corresponding XML schema definition. The
   ``impexp-core`` Java library of the Importer/Exporter provides
   an API for parsing, creating and writing valid schema mapping
   file (see `here <https://github.com/3dcitydb/importer-exporter/tree/master/impexp-core/src/main/java/org/citydb/database/schema>`_).

**Starting the registration process**

Provide the path to the root folder of the ADE extension package in the
corresponding input field of the user dialog (see :numref:`ade_manager_plugin_ade_registration`).
You can either enter the folder manually or open a file selection dialog
via the *Browse* button. Afterwards, click the *Register ADE* button to
start the process. If a database connection has not been established manually
beforehand, the currently selected entry on the *Database* tab is used to
connect to the 3D City Database. The separate steps of the registration process
as well as all errors and warnings that might occur during the registration
are reported to the console window.

.. note::
   The registration process cannot be aborted by the user as this might
   lead to an inconsistent database state. However, you can remove a registered
   ADE at any time using the *ADE Operations* dialog
   (see :numref:`impexp_plugin_ade_manager_ade_operations`).

.. caution::
   The registration operation only creates the database tables,
   objects and functions that are required for storing and managing ADE data in the 3DCityDB.
   You can already use this database schema to load and export ADE data with your own tools.
   However, if you want to use the Importer/Exporter for this purpose, you need an additional
   Java library that adds support for the CityGML ADE to the Importer/Exporter.
   This Java library is not automatically created by the ADE Manager plugin but
   must be developed manually.

**Example**

If you want to test the ADE registration process, you can use the
open source *Test ADE* for this purpose. The TestADE is an artificial CityGML ADE for testing and
demonstrating the ADE support of the 3D City Database. Download
the Test ADE extension as ZIP file from the releases section of the
GitHub repository at https://github.com/3dcitydb/extension-test-ade.
Make sure you download a version that can be used together with
the version of your Importer/Exporter.

The ZIP file contains the ADE extension package of the Test ADE.
Unzip the file to a folder of your choice in your local file system and make sure that
the folder has the required content as discussed above.
Afterwards, simply provide this folder as input for the *ADE extension package*
field in the user dialog and click the *Register ADE* button.

.. figure:: /media/ade_manager_plugin_gui_ade_registration.png
   :name: ade_manager_plugin_gui_ade_registration
   :align: center

   Registering the Test ADE extension package.

During the ADE registration process, the database schema of the Test ADE
will be created and the metadata about the ADE will be written to
the 3DCityDB metadata tables. In addition, the 3DCityDB database
functions for deleting city objects (see :numref:`citydb_sproc_delete_chapter`)
and calculating the envelope of city objects (see :numref:`citydb_sproc_envelope_chapter`)
will be automatically regenerated by the ADE Manager plugin
to account for the new ADE tables and features.

After the Test ADE has been successfully registered, the list of all ADEs registered
in the 3DCityDB instance is updated and displayed in the ADE table
of the *ADE Operations* dialog (see :numref:`impexp_plugin_ade_manager_ade_operations`).
Make sure the Test ADE is listed here.

.. figure:: /media/ade_manager_plugin_ade_operations_result.png
   :name: ade_manager_plugin_ade_operations_result
   :align: center

   The Test ADE is listed as registered ADE after the registration operation.

You may also use a database tool like pgAdmin (PostgreSQL) or
SQLDeveloper (Oracle) to check whether the ADE database
schema has been correctly created. For the Test ADE, the 3DCityDB
schema should now contain additional tables starting with the prefix *"test_"*
that are used to store Test ADE data. In addition, there should be new
database functions to delete Test ADE features (with prefix *"del_test_"*)
and to calculate their envelope (with prefix *"env_test_"*).

.. figure:: /media/ade_manager_plugin_tables_pgadmin.png
   :name: ade_manager_plugin_tables_pgadmin
   :width: 3.5in
   :align: center

   The Test ADE tables starting with the prefix "test_" shown in pgAdmin.