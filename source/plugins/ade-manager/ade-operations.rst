.. _impexp_plugin_ade_manager_ade_operations:

ADE operations
--------------

The *ADE Operations* dialog offers the possibility to list all CityGML ADEs
that have been successfully registered with a 3D City Database instance
and to perform management operations for these ADE extensions.

.. figure:: /media/ade_manager_plugin_ade_operations.png
   :name: ade_manager_plugin_ade_operations
   :align: center

   The ADE Operations dialog of the ADE Manager plugin.

**Retrieving registered ADEs from the 3DCityDB**

Simply click on the *Fetch ADEs* button below the ADE table to get the
list of all ADEs registered with your 3DCityDB instance.
If a database connection has not been established manually beforehand,
the currently selected entry on the *Database* tab (see :numref:`impexp_database_connection_management_chapter`)
is used to connect to the 3DCityDB. The ADE Manager plugin
queries the ADE metadata tables of the 3DCityDB schema in order
to retrieve the available and supported ADEs (see :numref:`chapter_citydb_schema_metadata`).

For example, assume that you have successfully registered the
`Test ADE <https://github.com/3dcitydb/extension-test-ade>`_. The
*Fetch ADEs* operation would therefore give you the following result.

.. figure:: /media/ade_manager_plugin_ade_operations_result.png
   :name: ade_manager_plugin_ade_operations_result
   :align: center

   Example result of the Fetch ADEs operation.

Each ADE is displayed as separate entry in the above ADE table
with its *Name*, *Description* and *Version*. The *DB Prefix* column
shows the name prefix that is used for every table in the 3DCityDB that
belongs to this ADE. The *Creation Date* reflects the date and time when
the ADE was added to the 3DCityDB. Whenever an ADE is registered, a unique identifier is automatically
generated that is displayed in the *ADE ID* column. This identifier is used
internally, for instance, to check whether an ADE extension for
the Importer/Exporter matches an ADE schema registered in the database.

When double-clicking on an entry in the ADE table, more metadata
about the ADE will be displayed in a separate window as shown below.
The *Status* field shows whether the ADE is fully supported or some user
action is required. The information dialog is identical to the one
that can be requested from the *Database* tab (see :numref:`impexp-db-citygml-ade`).

.. figure:: /media/ade_manager_plugin_ade_operations_metadata.png
   :name: ade_manager_plugin_ade_operations_metadata
   :align: center

   ADE metadata dialog.

**Removing a registered ADE**

The *ADE operations* dialog also allows you to remove a previously
registered ADE from the 3DCityDB. Simply fetch the list of available
ADEs from the database, select the ADE you want to delete from this list
and click the *Remove ADE* button. After a confirmation prompt, the ADE
is removed and the operation progress is logged to the console window.

.. caution::
   When removing an ADE, the **entire ADE database schema together with all
   data** stored in the ADE tables will be deleted from the database. There
   is no way to undo this operation, so use it with care.

**Generate delete and envelope scripts**

The 3DCityDB is shipped with database functions to delete city objects
(see :numref:`citydb_sproc_delete_chapter`) and to calculate the envelope
of city objects (see :numref:`citydb_sproc_envelope_chapter`). The default
versions of these scripts are implemented against the predefined tables of the
3DCityDB schema and, thus, do not consider ADE tables and features. For this reason,
the scripts are automatically regenerated and installed in the database by
the ADE Manager plugin whenever an ADE is registered with the 3DCityDB to
ensure that they also work with the newly added ADE schema.

With the *Generate delete script* and *Generate envelope script* buttons
you can manually trigger the process of generating the scripts. The operation
will always consider all ADEs that are registered with the 3DCityDB.
It is therefore independent of whether you have selected a
specific ADE in the ADE table but rather ignores this selection.
The separate steps of the process are logged to the console window.
The resulting script is presented in a new window like the one shown below.

.. figure:: /media/ade_manager_plugin_show_install_scripts.png
   :name: ade_manager_plugin_show_install_scripts
   :align: center

   Dialog window for showing and installing newly generated database scripts.

At the bottom of this window you can choose to save the
script to a local file. This gives developers the possibility to
modify the script and to install their modified version using a
database tool such as pgAdmin or SQLDeveloper.
Alternatively, you can let the ADE Manager plugin install the script
into the 3DCityDB instance by clicking on the *Install* button. This
can be useful, for instance, if you have installed a modified version
previously and want to restore the original version.