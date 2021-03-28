Preferences
-----------

.. toctree::
   :hidden:

   preferences/manage-crs
   preferences/cache
   preferences/paths
   preferences/network-proxies
   preferences/api-keys
   preferences/logging
   preferences/language

In addition to the settings on the *Import*, *Export*, *VIS Export* and
*Database* tabs, more preferences affecting the separate operations
of the Importer/Exporter as well as general application settings
are available on the *Preferences* tab shown below.

.. figure:: /media/preferences_main_gui.png
   :name: pic_preferences_main_gui
   :align: center

   The preferences dialog.

The preferences are organized into separate topics using a tree view [1]
on the left side of the dialog. Simply navigate through this tree view
by selecting, expanding and collapsing the separate tree nodes.
When selecting a node, the associated
settings dialog is displayed on the right side [2]. Changes made to the
settings of the selected node are applied through the *Apply* button
[3]. The buttons *Restore* and *Default* allow for resetting the
preferences to their previous state or to their default values.

The preference settings offered by the *Import*, *Export*, and *VIS Export* nodes
are not discussed in this chapter but in the context of the
corresponding operations. Use the links below to directly jump to these chapters.

- :numref:`%s <impexp_citygml_import_preferences_chapter>` :ref:`Import preferences <impexp_citygml_import_preferences_chapter>`
- :numref:`%s <impexp_citygml_export_preferences_chapter>` :ref:`Export preferences <impexp_citygml_export_preferences_chapter>`
- :numref:`%s <impexp_export_vis_preferences_chapter>` :ref:`VIS Export preferences <impexp_export_vis_preferences_chapter>`

This chapter focuses on the *Database* and *General* preferences.
The following table provides a summary overview of the available settings.

.. list-table::  Summery overview of the database and general preferences
   :name: citygml_export_preferences_summary_table
   :widths: 30 70

   * - | **Preference name**
     - | **Description**
   * - | :ref:`impexp_crs_management_chapter`
     - | Management of user-defined coordinate reference systems.
   * - | :ref:`impexp_general_preferences_cache`
     - | Defines cache settings for storing temporary information during import and export operations.
   * - | :ref:`impexp_general_preferences_paths`
     - | Default paths for import and export operations.
   * - | :ref:`impexp_preferences_general_proxy_chapter`
     - | Allows for defining network proxies for different network protocols.
   * - | :ref:`impexp_preferences_general_apiKeys_chapter`
     - | Lets you manage your API keys for different web services.
   * - | :ref:`impexp_general_preferences_logging`
     - | Controls the logging of messages to the console and to files.
   * - | :ref:`impexp_general_preferences_language`
     - | Defines the language of the Importer/Exporter user interface.

All preferences (including the settings on the separate operation tabs)
are automatically stored in the configuration file of the Importer/Exporter and
are restored from this file upon program start. Using the *File* menu [4]
available from the menu bar of the Importer/Exporter, the preferences
can optionally be stored in or loaded from user-defined configuration files
(cf. :numref:`impexp_gui_chapter`).

.. note::
   In case plugins have been registered with the Importer/Exporter,
   each plugin might add another node to the preferences tree.
   Please refer to the manual of the plugin for a documentation of the
   preference settings.