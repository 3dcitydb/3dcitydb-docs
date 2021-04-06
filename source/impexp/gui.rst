.. _impexp_gui_chapter:

Using the graphical user interface (GUI)
----------------------------------------

The graphical user interface of the Importer/Exporter is organized into
four main components as shown in :numref:`impexp_gui_organization_fig`.
A *menu bar* [1] is anchored to
the top of the window (Windows, some Linux distributions) or to the top of the screen (Mac, some
Linux distributions). The main application window is
divided into an *operations window* [2] that renders the user dialogs of
the separate operations of the Importer/Exporter and a *console window*
[4] that displays log messages. Via the View entry in the menu bar, the
console window can be detached from the main window and rendered in a
separate window. At the bottom of the operations window, a *status bar*
[3] provides information about running processes and database
connections.

.. figure:: ../media/impexp_gui_organization_fig.png
   :name: impexp_gui_organization_fig
   :align: center

   Organization of the Importer/Exporter GUI.

The tab menu on top of the operations window lets you switch between the
main operations of the Importer/Exporter and their user dialogs. The
following tabs are available:

- **Import**: Import CityGML or CityJSON datasets into the database
- **Export**: Export city model data in CityGML or CityJSON format
- **VIS Export**: Export city model data in KML, COLLADA or glTF format for visualization
- **Database**: Database connection settings and operations
- **Preferences**: Preference settings for each operation

.. note::
   If you have installed plugins, the tab menu may contain
   additional entries. Please refer to the documentation of your plugin in
   this case.

The main menu bar [1] offers the entries File, View, and Help. The **File menu** lets a user
store and load application settings from a config file and close the application.

.. list-table::  *File menu*
   :widths: 30 70

   * - | **Open Settings...**
     - | Load a config file and recover all settings from this file.
   * - | **Save Settings**
     - | Save all settings made in the GUI to the default config file.
   * - | **Save Settings As...**
     - | Save all settings made in the GUI to a separate config file.
   * - | **Restore Default Settings**
     - | Set all settings to default values.
   * - | **Save Settings XSD As...**
     - | Save the XML Schema defining the XML structure of config files to a
       | separate file. The XML Schema is helpful in case a user wants to
       | manually edit the config file. Only config files conforming to the XML Schema definition will be successfully loaded by the Importer/Exporter.
   * - | **Recently Used Settings**
     - | List of recently loaded config files.
   * - | **Exit**
     - | Close the Importer/Exporter application.

The Importer/Exporter uses one *default config file* per operating
system user running the Importer/Exporter. All settings made in the GUI
are automatically stored in this default config file when the
Importer/Exporter is closed and are loaded from this file upon program
start. The default config file is named ``project.xml`` and is stored in the
*home directory* of the user. Precisely, you will find the config file
in the subfolder ``3dcitydb/importer-exporter/config``. Note that the
location of the home directory differs for different operating systems.
Using environment variables, the location can be identified dynamically:

- ``%HOMEDRIVE%%HOMEPATH%\3dcitydb\importer-exporter\config`` (Windows 7 and higher)
- ``$HOME/3dcitydb/importer-exporter/config`` (UNIX/Linux, Mac OS families)

The **View menu** affects the GUI elements of the Importer/Exporter and
offers the following entries:

.. list-table::  *View menu*
   :widths: 30 70

   * - | **Open Map Window**
     - | Open a 2D map window for bounding box selection (cf. :numref:`impexp_preferences_map_window_chapter`).
   * - | **Detach Console**
     - | Render the console window in a separate application window.
   * - | **Light Mode**
       | **Dark Mode**
     - | Switch between a light and a dark interface style.
   * - | **Restore default perspective**
     - | Restore the GUI to its default settings.

Finally, the **Help menu** provides a link to the user manual and further information about
the Importer/Exporter.

.. list-table::  *Help menu*
   :widths: 30 70

   * - | **Online-Documentation...**
     - | Open this user manual in a web browser.
   * - | **Read Me**
     - | Open README.txt file shipped with the Importer/Exporter.
   * - | **About**
     - | Display general information like the  official *version number* of the Importer/Exporter and development partners.