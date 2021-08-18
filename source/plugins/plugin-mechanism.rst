.. _impexp_plugin_mechanism_chapter:

Introduction to the plugin mechanism
------------------------------------

Plugins are extensions that add additional functionality to the
Importer/Exporter. They are self-contained in that one plugin cannot extend the
functionality of another plugin. Therefore, plugins can be added
separately to the Importer/Exporter without interdependencies.

A plugin may extend the GUI of the Importer/Exporter by providing its
own user dialog that will be rendered in a separate tab on the
operations window. In addition, a plugin may add new entries to the main
menu bar and the preferences dialog. To restore plugin-specific settings
at program startup, a plugin can choose to serialize the settings to the
main config file or a plugin-specific config file. Besides GUI extensions,
plugins can also provide functionality that is hooked into the main
operations of the Importer/Exporter, for instance, to postprocess
exported top-level features. And they can add their own commands to
the CLI of the Importer/Exporter to run the plugin from the command line.

Plugin installation is simple. Just get the plugin from your vendor and
put all plugin files into the ``plugins`` subfolder of the Importer/Exporter
installation directory. To keep multiple plugins independent from each
other, it is recommended to create a separate subfolder below ``plugins``
for each plugin. When running the Importer/Exporter, the installed
plugins are automatically detected and loaded with the application. Uninstalling
a plugin just requires to delete the folder containing all the plugin files
from the ``plugins`` subfolder. Note that changes to the ``plugins`` subfolder
require a restart of the Importer/Exporter to become effective.

Installed plugins can be enabled or disabled at runtime both using the
graphical user interface of the Importer/Exporter and on the command line.
Please refer to :numref:`impexp_preferences_plugins` and see :numref:`impexp_cli_chapter`
for more details.

The Importer/Exporter is shipped with two free
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
