Introduction to the plugin architecture
---------------------------------------

Plugins are extensions that add additional functionality to the
Importer/Exporter. They are self-contained in that one plugin cannot extend the
functionality of another plugin. Therefore, plugins can be added
separately to the Importer/Exporter without interdependencies.

A plugin may extend the GUI of the Importer/Exporter by providing its
own user dialog that will be rendered in a separate tab on the
operations window. In addition, a plugin may add new entries to the main
menu bar and the preferences dialog. To remember the preference settings
at program startup, a plugin can choose to serialize the settings to the
main config file or a plugin-specific config file. Please refer to the
plugin documentation of your vendor for more information.

Plugin installation is simple. Just get the plugin from your vendor and
put all plugin files into the ``plugins`` subfolder of the Importer/Exporter
installation directory. To keep multiple plugins independent from each
other, it is recommended to create a separate subfolder below ``plugins``
for each plugin. When running the Importer/Exporter, the installed
plugins are automatically detected and loaded with the application.

The current version of the Importer/Exporter is shipped with two free
and open-source plugins that can be installed during the setup process
(see :numref:`first_steps_importer_exporter_installation`).
The *Spreadsheet Generator* *Plugin* allows for
exporting attributes of city objects as spreadsheets with user-defined
formatting, either to a CSV or a Microsoft Excel file (see :numref:`impexp_plugin_spshg_chapter`).
The *ADE Manager Plugin* automatically transforms CityGML ADEs to
relational schemas extending the 3DCityDB schema and un-/registers such
ADE schemas with existing 3DCityDB instances.

You can also develop your own plugins. For this purpose, the
Importer/Exporter comes with a *Plugin API* that is available as
separate JAR file ``impexp-plugin-api-{version}.jar``. Simply put the JAR file
on your classpath to start plugin development. A comprehensive *Plugin
API* guide will be offered on the
`www.3dcitydb.org <http://www.3dcitydb.org>`__ website soon. Moreover,
the source codes of the *Spreadsheet Generator* *Plugin* and *ADE
Manager Plugin* can be used as templates for your own developments.
