.. _ade_manager_plugin_impexp_extension_chapter:

Using an ADE with the Importer/Exporter
---------------------------------------

As mentioned in the previous sections, the *ADE Manager* plugin
automatically transforms the XML Schema definition of a CityGML ADE
to a compact relational database schema that seamlessly integrates
with the 3DCityDB schema. And you can use the plugin to automatically
deploy the created ADE schema in your 3DCityDB and use it for storing and
managing ADE datasets with your own tools.

However, the Importer/Exporter tool cannot automatically process CityGML ADE
data in a generic way. It offers a simple Java *ADE API* though that allows
developers to implement a so-called *ADE extension* as Java library
that will be automatically loaded by the Importer/Exporter when starting
the tool. If the Importer/Exporter comes across ADE data in import or export
operations, it will check whether an extension is available for this ADE.
If so, the work will be delegated to this extension through
corresponding method calls of the *ADE API*. Otherwise, the
ADE data will be ignored. The *ADE API* of the Importer/Exporter therefore
gives developers full control over how ADE data should be be managed
in the ADE database schema created with the ADE Manager plugin.

.. note::
   The ADE Manager plugin does not automatically create an
   *ADE extension* library for the Importer/Exporter. You have
   to manually develop this library for the ADE you want to use
   or download an existing *ADE extension* package if available.
   The open source `Test ADE <https://github.com/3dcitydb/extension-test-ade>`_
   serves as example for how to implement an *ADE extension* against
   the *ADE API* of the Importer/Exporter. The TestADE is an artificial
   CityGML ADE for testing and demonstrating the ADE support of the
   3D City Database.

The following brief guide explains the required steps for using the
`Test ADE <https://github.com/3dcitydb/extension-test-ade>`_ with
the Importer/Exporter.

1. Download the latest version of the Test ADE as ZIP package
   from the releases section of the GitHub repository
   at https://github.com/3dcitydb/extension-test-ade. This ZIP package contains
   the Java extension library for the Importer/Exporter as well as the
   ADE database schema and schema mapping file for the 3DCityDB.

2. Open the ``ade-extensions`` folder within the installation directory
   of your Importer/Exporter and unzip the ZIP file of the Test ADE there.
   If the ``ade-extensions`` folder does not exist, then create it first.
   After unzipping, a new subfolder ``extension-test-ade-{version}`` should have been
   created. Of course, you can freely choose another name for the folder.

3. Check that the unzipped folder contains all files required by the Test ADE
   extension, namely the three subfolders ``3dcitydb``, ``schema-mapping`` and
   ``lib``. Whereas the ``3dcitydb`` and ``schema-mapping`` folders contain
   the files to register the ADE with the 3DCityDB (see :numref:`ade_manager_plugin_registration_chapter`),
   the ``lib`` folder contains the JAR files that implement the
   *ADE API* of the Importer/Exporter. The folder structure should look like below.

   .. figure:: /media/ade_manager_plugin_impexp_folder_structure.png
      :name: ade_manager_plugin_impexp_folder_structure
      :align: center

4. Launch the Importer/Exporter. The JAR files in the
   ``lib`` folder along with the schema mapping file in the ``schema-mapping``
   folder will be automatically loaded by the Importer/Exporter.

5. Create a new 3DCityDB instance to be used in the following steps.
   The database should be set up with the *SRID* 31468 as coordinate
   reference system. If you need assistance in setting up a 3DCityDB,
   check the guide provided in :numref:`3dcitydb_setup_schema_chapter`.
   The test database is called *TestADE* in the following.

6. Connect to your new and empty 3DCityDB instance with the Importer/Exporter.
   Go to the *Database* tab and check that the *Test ADE* extension
   has been successfully loaded. The result should look similar to the
   screenshot below. The check sign in the *Importer/Exporter* column
   of the ADE table indicates that the Importer/Exporter now supports
   the Test ADE, while the cross sign in the *Database column* means
   the the Test ADE has not been registered with the 3DCityDB yet.

   .. figure:: /media/ade_manager_plugin_impexp_support_status_no.png
      :name: ade_manager_plugin_impexp_support_status_no
      :align: center

7. Go to the *ADE Manager* tab and register the Test ADE using the
   ADE registration operation described in :numref:`ade_manager_plugin_registration_chapter`.
   Use the ``extension-test-ade-{version}`` folder of step 2 as
   input for the registration operation.

8. Reconnect the *TestADE* database. The ADE table on the *Database* tab should now
   show check signs for both the database and the Importer/Exporter
   as illustrated below.

   .. figure:: /media/ade_manager_plugin_impexp_support_status_yes.png
      :name: ade_manager_plugin_impexp_support_status_yes
      :align: center

9. Now test the Importer/Exporter ADE support by importing Test ADE
   datasets. You can download two free datasets at
   https://github.com/3dcitydb/extension-test-ade/tree/master/resources/datasets.
   Go to the *Import* tab of the Importer/Export to load both datasets
   into the *TestADE* test database. You can optionally use the *feature type filter*
   like shown below to only import top-level features defined by the Test ADE.

   .. figure:: /media/ade_manager_plugin_citygml_import_filter.png
      :name: ade_manager_plugin_citygml_import_filter
      :width: 518px
      :align: center

   A summary of the import process is printed to the console window.
   The log messages should look similar to the following figure.

   .. figure:: /media/ade_manager_plugin_citygml_import_summary.png
      :name: ade_manager_plugin_citygml_import_summary
      :align: center

10. Go back to the *Database* tab and create a *Database report*
    for the *TestADE* database (see :numref:`impexp-db-report`).
    Again, you should get similar numbers for the 3DCityDB abd ADE tables
    as shown below.

    .. figure:: /media/ade_manager_plugin_database_report.png
       :name: ade_manager_plugin_database_report
       :align: center

11. Finally, export your Test ADE data again. For this purpose, go to the
    *Export* tab of the Importer/Exporter and specify an output CityGML file.
    Again, you can use a *feature type filter* to restrict the export
    to top-level features defined by the Test ADE. Click the *Export* button
    to start the export operation. A summary of the export process is printed
    to the console window and should look similar to the following figure.

    .. figure:: /media/ade_manager_plugin_citygml_export_summary.png
       :name: ade_manager_plugin_citygml_export_summary
       :align: center