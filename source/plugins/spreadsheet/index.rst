.. _impexp_plugin_spshg_chapter:

Spreadsheet Generator Plugin
----------------------------

The *Spreadsheet Generator* plugin allows you to export attribute data
of the city objects stored in the 3D City Database in tabular form as
comma-separated values (CSV) or Microsoft Excel (XLSX) file.
The tabular data can be loaded with typical spreadsheet applications
like Microsoft Excel or Open Office Calc for further processing or
uploaded to online spreadsheet services such as Google Docs. In the latter
case, the attributes can be accessed,
for instance, from web-based viewers to show or even modify them
when interacting with the city objects. As for example, the 3D Web Map Client shipped
with the 3D City Database can be easily linked to online spreadsheets
that have been created with this plugin
(see :numref:`webmap_chapter` for more information).

The Spreadsheet Generator plugin provides a graphical user interface (GUI)
that is added as *Table Export* operation tab to the GUI of the Importer/Exporter.
The plugin can also be used on the command line. For this purpose, it adds the
command ``export-table`` to the command-line interface (CLI) of the Importer/Exporter.
The following sections cover the installation and use of the plugin.

.. toctree::
   :maxdepth: 1

   installation
   export
   cli

.. note::
   The *Spreadsheet Generator* plugin also supports exporting **attributes
   of features defined in a CityGML ADE**. For this purpose, a
   corresponding ADE extension must have been registered with the
   3D City Database and the Importer/Exporter.