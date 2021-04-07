Installation
------------

The *Spreadsheet Generator* plugin is packaged with the Importer/Exporter installer.
When using the GUI-based setup wizard for installation (see :numref:`impexp_gui_installation_chapter`),
you can simply select the plugin from the list of available software
packages as shown below.

.. figure:: /media/impexp_plugin_spshg_installation_fig.PNG
   :name: pic_plugin_spreadsheet_installation
   :align: center

   Installation wizard of the Importer/Exporter tool.

If you have not installed the plugin together with the Importer/Exporter,
it is also possible to install it at any later time with the following steps:

1.  Download the Spreadsheet Generator plugin as ZIP file from the official
    3D City Database website at https://www.3dcitydb.org
    or from the releases section of the GitHub repository used for maintaining the plugin
    at https://github.com/3dcitydb/plugin-spreadsheet-generator.

    .. caution::
       Make sure the version of the Spreadsheet Generator plugin that you
       want to download can be used together with the version of your
       Importer/Exporter installation.

2.  Open the ``plugins`` folder within the installation directory of your
    Importer/Exporter and unzip the ZIP file of the Spreadsheet Generator plugin
    there. If the ``plugins`` folder does not exist, then create it first.
    After unzipping, a new subfolder ``plugin-spreadsheet-generator`` should
    have been created containing all files required by the plugin.

3.  Run the *Importer/Exporter*. The Spreadsheet Generator plugin should
    be automatically detected and loaded.

If you have successfully installed the plugin, the *Table Export* operation tab
is available on the operations window of the Importer/Exporter as illustrated
below. In addition, the command ``export-table`` is added to the command-line
interface of the Importer/Exporter.

.. figure:: /media/plugin_spreadsheet_main_gui.png
   :name: pic_plugin_spreadsheet_main_gui
   :align: center

   The "Table Export" operation tab of the Spreadsheet Generator plugin.