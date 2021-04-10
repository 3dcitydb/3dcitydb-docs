Using the graphical user interface
----------------------------------

When starting the Importer/Exporter, the user interface of the
*ADE Manager* plugin is available as additional tab called
*ADE Manager* on the operations window.

.. figure:: /media/ade_manager_plugin_gui.png
   :name: ade_manager_plugin_gui
   :align: center

   The user interface of the ADE Manager plugin.

The user interface is organized into three sections offering
different functionalities and operations for working with CityGML ADEs.

.. list-table:: Functionalities offered by the ADE Manager plugin
   :name: ade_manager_plugin_gui_functionalities
   :widths: 30 70

   * - | :ref:`impexp_plugin_ade_manager_ade_operations`
     - | Management operations for ADE extensions that are already registered with the 3DCityDB.
   * - | :ref:`ade_manager_plugin_registration_chapter`
     - | This dialog allows you to register an ADE extension package with the 3DCityDB.
   * - | :ref:`impexp_plugin_ade_manager_ade_transformation`
     - | With this dialog, you can automatically transform the XML Schema definition of a CityGML ADE to a relational database schema that seamlessly integrates with the 3DCityDB schema.

The separate operations are discussed in more detail in the following sections.
Which operation to use depends on whether an ADE extension package already
exists for the CityGML ADE you want to use.

If an ADE extension package exists,
you simply have to *register this ADE package* [2] with your 3DCityDB instance. This
process will create the required database tables, objects and functions for storing
and managing ADE data in your 3DCityDB. In addition, you have to install
the ADE extension package for the Importer/Exporter (see :numref:`ade_manager_plugin_impexp_extension_chapter`).
Afterwards, you can import and export ADE datasets using the general
operations of the Importer/Exporter. And you can execute *ADE operations* [1]
like retrieving metadata about an ADE or entirely removing an ADE extension package.

The 3D City Database project offers three open source and free to use ADE extension
packages:

- `Energy ADE <https://github.com/3dcitydb/energy-ade-citydb>`_: The Energy ADE extends
  CityGML by features and properties necessary to perform energy simulations and to store
  and exchange the corresponding results.
- `i-UR ADE <https://github.com/3dcitydb/iur-ade-citydb>`_: The i-UR ADE is an information
  infrastructure for urban revitalization and planning.
- `Test ADE <https://github.com/3dcitydb/extension-test-ade>`_: This is an artificial ADE
  and is only meant for testing and demonstrating the ADE support of the 3DCityDB.

If you do not have a ready-to-use ADE extension package for your CityGML ADE,
the first step will rather be to *transform the XML Schema* [3] of the ADE to a
relational schema representation that can be used with the 3DCityDB. This
mapping can be done fully automatically with the ADE Manager plugin. The output
of the transformation process is an ADE extension package that you can
directly *register* [2] with your 3DCityDB. Afterwards, you can use the
*ADE operations* [1] with your ADE extension if required.

.. caution::
   The ADE Manager plugin only creates the database tables, objects and
   functions that are required for storing and managing ADE data in the
   3DCityDB. You can already use this database schema to load and export ADE
   data with your own tools. However, if you want to use the Importer/Exporter
   for this purpose, you need an additional Java library that adds support for
   the CityGML ADE to the Importer/Exporter. This Java library
   **is not automatically created by the ADE Manager plugin but must
   be developed manually**.