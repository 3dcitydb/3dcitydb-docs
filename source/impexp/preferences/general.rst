General preferences
~~~~~~~~~~~~~~~~~~~

In addition to the preference settings that influence the behavior of a
particular import or export operation (cf. previous sections), the
General node on the Preferences tab offers application-wide settings.


.. _cache:

Cache
^^^^^

Both during CityGML imports at exports, the Importer/Exporter has to
keep track of various temporary information. For instance, when
resolving XLinks, the gml:id values as well as additional information
about the related features and geometries must be available. Since the
Importer/Exporter is designed to be able to process arbitrarily large
CityGML input files, keeping this information in main memory only is not
a promising strategy. For this reason, the information is written to
*temporary tables* in the database as soon as user-defined memory limits
are reached.

|image133|

Figure 118: General preferences – Cache.

Per default, temporary tables are created in the *3D City Database
instance* itself. The tables are populated during the import and export
operation and are automatically dropped after the operation has
finished. Alternatively, the user can choose to store the temporary
information in the *local file system* instead. An absolute path where
to create the file-based storage has to be provided. Either type the
location manually into the input field or use the *Browse* button to
open a file selection dialog. A subfolder of the local *temp folder* of
the operating system user running the Importer/Exporter is proposed as
default location (depends on the operating system in use). Like with
temporary database tables, the file-based storage is automatically
removed after the operation has finished.

Some reasons for using a file-based storage are:

-  The 3D City Database instance is kept clean from any additional
   (temporary) table.

-  If the Importer/Exporter runs on a different machine than the 3D City
   Database instance, sending temporary information over the network
   might be slow. In such cases, using a local storage might help to
   increase performance.


.. _file-path:

Import and export path
^^^^^^^^^^^^^^^^^^^^^^

This preference dialog allows for setting a default path for import and
export operations.

|image134|

Figure 119: General preferences – Import and export path.

Simply choose between the last used import/export path (default) or
browse for a specific folder in your local file system. The selected
folder will then be used as default path in all dialogs that require an
input/output file.


.. _proxy:

Network proxies
^^^^^^^^^^^^^^^

Some of the functionalities offered by the Importer/Exporter require
internet access. This applies, for instance, to the XML validation when
accessing XML Schema documents on the web, to the map window for the
graphical selection of bounding boxes (uses *OpenStreetMap* data), or to
the automated calculation of height offsets during KML/COLLADA/glTF
exports (based on the *Google* *Elevation Service*).

Most computers in corporate environments have no direct internet access
but must use a proxy server. The preference dialog shown below let you
configure network proxies.

|image135|

Figure 120: General preferences – Network proxies.

The Importer/Exporter supports *Web (HTTP)*, *Secure web (HTTPS)* and
*SOCKS* proxies. Usually, configuring a *Web proxy (HTTP)* is enough for
most tasks, like those mentioned above. However, more sophisticated use
cases, like uploading cloud documents via an Importer/Exporter extension
plugin (cf. chapter 6.2) may require *Secure web proxy (HTTPS)* support.
*SOCKS proxy* support should currently only be needed when the
Importer/Exporter and the database system running the 3D City Database
reside in different networks.

Whenever one of the protocols to be handled by a proxy is selected in
the choice list at the top of the dialog, the corresponding settings
must be provided in the fields below: *Server*, *Port*, and if the proxy
requires login credentials *Username* and *Password*. Default *Port*
values for each protocol are automatically filled in (HTTP: 80; HTTPS:
443; SOCKS: 1080) and only need to be changed if required.

It is also possible to define one single proxy for all protocols by
simply selecting the corresponding checkbox under the protocol list.
Just make sure the proxy server supports all protocols and that they can
all be routed through the given *Port*.

Proxies are only used if the checkbox next to the protocol type is
enabled. Otherwise, the proxy configuration will be stored but remains
inactive. When the proxy for a given protocol is enabled, every outgoing
connection by the Importer/Exporter that uses the protocol will be
routed through this proxy.

In case the computer running the Importer/Exporter is directly connected
to the internet no proxies need to be configured.


.. _api-keys:

API Keys
^^^^^^^^

The Importer/Exporter uses external web services offered by third party
providers for different tasks and functionalities. Some of these
services are open and free to use, whereas others are more restrictive
and require passing an API key to use the service. In the API Keys
preference dialog, you can provide your API keys for different services.

|image136|

Figure 121: General preferences – API keys.

The *Google Maps API* services can be used by the Importer/Exporter for
two different tasks: 1) the *Geocoding API* is used for geocoding
addresses and address lookups in the map window (cf. chapter 5.7), and
2) the *Maps Elevation API* is used in KML/COLLADA exports for
retrieving height values from the Google Earth terrain model (cf.
chapter 5.6.3.4). If you want to use one of these services, then you
must enter the corresponding API key in the above dialog. Otherwise the
services will respond with an error message that will be displayed by
the Importer/Exporter. Please visit the Google Maps API website if you
do not have an API key yet but intent to get one.

.. note::
   Google has changed the usage and pricing policies for the
   above-mentioned services starting from July 16, 2018. Thus, in previous
   versions of the Importer/Exporter, the services could be used without
   entering an API key.


.. _logging:

Logging
^^^^^^^

The Importer/Exporter logs information about events such as activities
or failures, for instance during database imports and exports. Each log
entry consists of a timestamp when the event occurred, a log level
indicating the severity of the event and a human-readable message text.
Log messages are always printed to the *console window* and may
additionally be forwarded to a log file on your local computer. The
Logging preference dialog is shown below.

|image137|

Figure 122: General preferences – Logging.

The following four log levels are distinguished (from highest to lowest
severity):

========== ==================================================================================================================================================================================================================================================
-  *ERROR* An error has occurred (usually an exception). This comprises internal and unexpected failures. Moreover, invalid XML content of CityGML instance documents is reported via this log level. Fatal errors will cause the running operation to abort.
========== ==================================================================================================================================================================================================================================================
-  *WARN*  An unusual condition has been detected. The operation in progress continues to work but the user should check the warning and take appropriate actions.
-  *INFO*  An interesting piece of information about the current operation that helps to give context to the log, often when processes are starting or stopping.
-  *DEBUG* Additional messages reporting the internal state of the application.
========== ==================================================================================================================================================================================================================================================

The log level for messages printed to the console window can be chosen
from a drop-down list in the Console dialog [1]. The log will include
all events of the indicated severity as well as events of greater
severity (default: *INFO*). *Word wrapping* can be optionally enabled
for long message texts that otherwise exceed the width of the console
window. In addition, the *color scheme* for console log messages can be
customized by assigning text colors to each log level.

.. note::
   The log output in the *console window* is truncated after 10,000
   log messages in order to prevent high main memory consumption.

If log messages shall additionally be stored in a log file, simply
activate the option *Write messages to log file*. The log file is named
log_3dcitydb_impexp_<date>.log per default, where <date> is replaced
with the current date at program startup. The Importer/Exporter creates
the log file if it does not exist. Otherwise, log messages are appended
to the existing log file. The user can choose a location where to store
the log file by enabling the option *Use alternative path for log files*
and by providing a corresponding path [2]. Either enter the path
manually or click on *Browse* to open a file selection dialog. The log
level can be chosen independent from the console window through the
corresponding drop-down list [2] (default: *INFO*).

.. note::
   Log files are per default stored in the *home directory* of the
   *operating system user* running the Importer/Exporter. Precisely, you
   will find the log files in the subfolder
   3dcitydb/importer-exporter-3.0/log. However, the location of the home
   directory differs for different operating systems. Using environment
   variables, the location can be identified dynamically:

-  %HOMEDRIVE%%HOMEPATH%\3dcitydb\importer-exporter-3.0\log (Windows 7
   and higher)

-  $HOME/3dcitydb/importer-exporter-3.0/log (UNIX/Linux, Mac OS
   families)


.. _language:

Language selection
^^^^^^^^^^^^^^^^^^

The Importer/Exporter GUI has support for different languages. Use the
Language selection preference dialog shown below to pick your favourite
language.

|image138|

Figure 123: General preferences – Language selection.


.. _map-window:

Map window for bounding box selections
--------------------------------------

The Importer/Exporter GUI offers a 2D map window that allows the user to
display the overall bounding box calculated from the city model content
stored in each 3D City Database instance and to graphically select a
bounding box filter for data imports and exports.

There are two ways to open the map windows:

1. Choose the entry View Open map window from the menu bar at the top of
   the application window.

|image139|

2. Click the map button
   |C:\devel\java\impexp-oss\resources\jar\resources\img\common\map_select.png|
   on the bounding box dialog available on the Import, Export,
   KML/COLLADA/glTF Export and Database tabs of the operations window.

|image140|

The 2D map is rendered in a separate application window shown below.

|image141|

Figure 124: 2D map window for bounding box selections.

The map content is provided by the *OpenStreetMap* (OSM) service and is
subject to the OSM usage and license terms. Make sure your computer has
internet access to load the map. This might require setting up *network
proxies* (see chapter 5.6.5.3). Please consult your network
administrator.

The map offers default mouse controls for panning and zooming. For
convenience, a geocoding service is included in the map window [1].
Simply type in an address or a geo location (given by geographic lat/lon
coordinates separated by a comma) and click the *Go* button. The map
will automatically zoom to the first match. Further matches are
available from the drop-down list [1]. The geocoding service uses the
free *OSM Nominatim* service per default. You can pick the *Goolge
Geocoding API* as alternative service from the drop-down list in [5].
Note that the *Goolge Geocoding API* is not free but requires an API key
that must be entered in the global preferences of the Importer/Exporter
(cf. chapter 5.6.5.4). Otherwise the service will respond with an error
message. Independent of the service you choose, make sure that you
adhere to its terms of use.

To display the result of the geocoding query on Google Maps in your
default internet browser, simply click the *Show in Google Maps* button
[6].

A list of usage hints is available at the right top of the map window
[7]. Please click on the *Show usage hints* link to display this list.
The map controls are also described in the following.

-  *Select bounding box*: Move the mouse while pressing the ALT key and
   the left mouse button to select a bounding box. The bounding box is
   displayed in a light magenta color. Once the left mouse button is
   released, the coordinates of the bounding box are automatically
   filled in the Bounding Box dialog on left of the map [3]. If you have
   opened the map window from a bounding box filter dialog, then
   clicking the *Apply* button on the upper right corner of the window
   [2] closes the map window and carries the bounding box values to the
   filter dialog. In addition, the values are copied to the clipboard.

-  *Lookup address*: Right-click on the map to bring up a context menu
   for the geo location at the mouse pointer. From the context menu,
   choose *Lookup address here*. This will trigger a reverse geocoding
   query using the geocoding service selected in [5]. The resulting
   address will be displayed on the left of the window [4]. The
   |C:\devel\java\impexp\resources\jar\resources\img\map\waypoint_precise.png|
   icon denotes which location on the map is associated with the
   address, whereas the
   |C:\devel\java\impexp\resources\jar\resources\img\map\waypoint_reverse.png|
   icon shows where you clicked on the map (see Figure 125).

-  *Zoom in/out:* Use the mouse wheel or the context menu (right-click).

-  *Zoom into selected area:* Move the mouse while pressing the SHIFT
   key and the left mouse button to select an area. The selected area is
   displayed in a light grey color. Once the left mouse button is
   released, the map zooms into the selected area. If the maximum zoom
   level is reached this action has no further effect.

-  *Move map:* Keep the left mouse button pressed to move the map.

-  *Center map and zoom in:* Double click the left mouse button to
   center the map at that position and to increase the current zoom
   level by one step.

-  *Use popup menu for further actions:* Right-click on the map to bring
   up a context menu offering additional functions such as *Zoom in*,
   *Zoom out*, *Center map here* and *Lookup address here* (see above).
   The *Get map bounds* function is equivalent to selecting the visible
   map content as bounding box. Thus, the map will be shown in light
   magenta and the map bounds are transferred to the Bounding Box dialog
   on the left [3].

To close the map, simply click the *Cancel* button in the upper right
corner [2].

|image144|

Figure 125: Address lookup in the map window.

The coordinates in the map window and of the selected bounding box are
always given in WGS 84 regardless of the coordinate reference system of
the 3D City Database instance.

When opening the map window from a bounding box dialog that already
contains coordinate values (e.g., from a filter dialog on the Import,
Export or KML/COLLADA/glTF Export tabs or after having calculated the
entire area of the database content on the Database tab), the map window
will automatically display this bounding box. If the coordinate values
of the provided bounding box are not in WGS 84, a transformation to WGS
84 is required. Since the Importer/Exporter uses functionality of the
underlying spatial database system for coordinate transformations, a
connection to the database must have been established beforehand. In
case there is no active database connection, the following pop-up window
asks the user for permission to connect to the database.

|image145|

Figure 126: Asking for permission before connecting to a database for
coordinate transformation.

The *Apply* button on the upper right corner of the map window [2] is a
shortcut for copying the coordinate values to the clipboard and pasting
them in the bounding box fields of the calling tab on the operations
window. Furthermore, coordinate values can now be easily copied from one
tab to another by simply clicking on the copy button
|C:\devel\java\impexp\resources\jar\resources\img\common\bbox_copy.png|
in one of them, say Import tab, with filled *bounding box* values,
changing to another, say KML/COLLADA/glTF Export tab and clicking on the
|C:\devel\java\impexp\resources\jar\resources\img\common\bbox_paste.png|
button there. Previously existing values in the bounding box fields of
the KML/COLLADA/glTF Export tab (if any) will be overwritten.


.. _cli:

Using the command line interface (CLI)
--------------------------------------

In addition to the graphical user interface, the Importer/Exporter also
offers a command line interface (CLI). The CLI allows a user to run the
Importer/Exporter from the command line (or a shell script) and to
easily embed it in batch processing workflows and third-party
applications.

To use the CLI, you first need to start a shell environment offered by
the operating system of your choice. The general command to run the
Importer/Exporter from a shell environment (or a shell script) is shown
below. If required, please replace the version number in the file name
with your current version.

java -jar lib/impexp-client-4.1.0.jar [-options]

This command consists of two parts. The first part executes the *Java
Virtual Machine* (JVM) through the java command. The -jar argument of
the JVM is used to denote the path to the Importer/Exporter JAR file
impexp-client-4.1.0.jar to be executed. After the JAR filename, you must
provide additional program arguments to trigger a specific operation of
the Importer/Exporter.

.. note::
   The above command assumes that you have first changed directory
   to the directory where the Importer/Exporter is installed. Otherwise,
   you must provide the full path to the impexp-client-4.1.0.jar file.

You may add any further JVM arguments to the above command that you
think are required in your environment. *It is recommended* to at least
start the JVM with a *minimum amount of main memory* using the -Xms
argument. For instance, use java -Xms 1g to use 1 GB of your main memory
for the Importer/Exporter.

To get a list of program arguments offered by the Importer/Exporter, use
the -help flag and issue the following command:

java -jar lib/impexp-client-4.1.0.jar -help

This will produce an output like shown below.

|image148|

Figure 127: Help text of the command line interface.

The available program arguments are:

==================== ================================================================================================================================================================================================================================================
   *-shell*          This argument is mandatory to start the shell version of the Importer/Exporter. If this argument is not provided, then the GUI version is launched per default.
==================== ================================================================================================================================================================================================================================================
   *-config*         Provides the path and filename of the config file to be used. If this argument is omitted, the config file in the default path is used instead. Using environment variables, the default path can be identified dynamically (cf. chapter 5.1):
                    
                     -  | %HOMEDRIVE%%HOMEPATH%\3dcitydb\\
                        | importer-exporter\config
                        | (Windows 7 and higher)
                    
                     -  $HOME/3dcitydb/importer-exporter/config (UNIX/Linux, Mac OS families)
   *-import*         Triggers a CityGML import process. Provide a list of one or more input files separated by semicolons (;) in addition. The list may also contain folders. A folder and all its nested subfolders are recursively scanned for CityGML input files.
   *-validate*       Triggers a XML Schema validation on the provided list of input files (see import argument).
   *-export*         Triggers a CityGML export process. Provide the path and name of the output file.
   *-kmlExport*      Triggers a KML/COLLADA/glTF export process. Provide the path and name of the output file.
   *-testConnection* Connects to the database using the connection details provided in the config file and exits afterwards. Evaluate the exit code (and optionally the log messages on the console) to check whether the connection was established successfully.
==================== ================================================================================================================================================================================================================================================

The full range of preferences and settings affecting the different
import and export operations of the Importer/Exporter **are not
offered** as separate program arguments. Instead, it is assumed that the
config file (either the default one or the one provided through the
-config argument) contains all the settings that should be used in a
specific operation (e.g., the database connection details, filter
settings for imports and exports, etc.). The config file is encoded as
XML and hence can be edited by a user manually. However, the recommended
way to provide valid settings is as follows:

1. Run the Importer/Exporter with the graphical user interface (GUI).

2. Make all your settings in the GUI.

3. Save your settings to a local config file via the Project Save
   Project As… dialog from the main menu bar.

4. Feed this config file to the command line interface using the -config
   argument.

.. note::
   You can also create a config file programmatically in Java. The
   JAR file impexp-config-4.1.0.jar in the installation directory of the
   Importer/Exporter contains all the classes required for reading and
   writing a config file. Once you have the JAR file on your classpath, use
   the class org.citydb.config.ConfigUtil as starting point.
