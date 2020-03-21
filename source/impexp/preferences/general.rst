General preferences
~~~~~~~~~~~~~~~~~~~

In addition to the preference settings that influence the behavior of a
particular import or export operation, the General node on the Preferences
tab offers application-wide settings.

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

.. figure:: /media/impexp_preferences_general_cache_fig.png
   :name: impexp_preferences_general_cache_fig

   General preferences – Cache.

By default, temporary tables are created in the *3D City Database
instance* itself. The tables are populated during the import and export
operation and are automatically dropped after the operation has
finished. Alternatively, the user can choose to store the temporary
information in the *local file system* instead. An absolute path where
to create the file-based storage has to be provided. Either type the
location manually into the input field or use the *Browse* button to
open a file selection dialog. The file-based storage is also automatically
removed after the operation has finished.

Some reasons for using a file-based storage are:

-  The 3D City Database instance is kept clean from additional
   tables holding temporary process information.
-  If the Importer/Exporter runs on a different machine than the 3D City
   Database instance, sending temporary information over the network
   might be slow. In such cases, using a local storage might help to
   increase performance, especially if fast disk drives are used.

.. _file-path:

Import and export path
^^^^^^^^^^^^^^^^^^^^^^

This preference dialog allows for setting a default path for import and
export operations.

.. figure:: /media/impexp_preferences_general_path_fig.png
   :name: impexp_preferences_general_path_fig

   General preferences – Import and export path.

Simply choose between the last used import/export path (default) or
browse for a specific folder in your local file system. The selected
folder will then be used as default path in all dialogs that require an
input/output file.


.. _impexp_preferences_general_proxy_chapter:

Network proxies
^^^^^^^^^^^^^^^

Some of the functionalities offered by the Importer/Exporter require
internet access. This applies, for instance, to the XML validation when
accessing XML Schema documents on the web, to the map window for the
graphical selection of bounding boxes, or to
the automatic calculation of height offsets during KML/COLLADA/glTF
exports based on the *Google* *Elevation Service*.

Most computers in corporate environments have no direct internet access
but must use a proxy server. The preference dialog shown below lets you
configure network proxies.

.. figure:: /media/impexp_preferences_general_network_proxies_fig.png
   :name: impexp_preferences_general_network_proxies_fig

   General preferences – Network proxies.

The Importer/Exporter supports *Web (HTTP)*, *Secure web (HTTPS)* and
*SOCKS* proxies.

To provide proxy information for one of the protocols, simply select
the corresponding entry from the list and enter the proxy settings in
the input fields below: *Server*, *Port*, and if the proxy requires login credentials
*Username* and *Password*. Default *Port* values for each protocol are
automatically set (HTTP: 80; HTTPS: 443; SOCKS: 1080) and only
need to be changed if required.

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

.. _impexp_preferences_general_apiKeys_chapter:

API Keys
^^^^^^^^

The Importer/Exporter uses external web services offered by third party
providers for different tasks and functionalities. Some of these
services are open and free to use, whereas others are more restrictive
and require passing an API key to use the service. In the API Keys
preference dialog, you can provide your API keys for different services.

.. figure:: /media/impexp_preferences_general_apikeys_fig.png
   :name: impexp_preferences_general_apikeys_fig

   General preferences – API keys.

The *Google Maps API* services can be used by the Importer/Exporter for
two different tasks: 1) the *Geocoding API* is used for geocoding
addresses and address lookups in the map window (cf. :numref:`impexp_preferences_map_window_chapter`), and
2) the *Maps Elevation API* is used in KML/COLLADA/glTF exports for
retrieving height values from the Google Earth terrain model (cf.
:numref:`impexp_kml_export_terrain_preferences_chapter`).
If you want to use one of these services, then you
must enter your corresponding API key in the above dialog. Otherwise the
services will respond with an error message that will be displayed by
the Importer/Exporter. Please visit the Google Maps API website if you
do not have an API key yet.

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

.. figure:: /media/impexp_preferences_general_logging_fig.png
   :name: impexp_preferences_general_logging_fig

   General preferences – Logging.

The following four log levels are distinguished (from highest to lowest severity):

.. list-table:: Log levels and their meaning.
   :name: impexp_preferences_general_logging_table

   * - | **Log level**
     - | **Description**
   * - | ERROR
     - | An error has occurred (usually an exception). This comprises internal and unexpected
       | failures. Moreover, invalid XML content of CityGML instance documents is reported
       | via this log level. Fatal errors will cause the operation in progress to abort.
   * - | WARN
     - | An unusual condition has been detected. The operation in progress continues to work
       | but the user should check the warning and take appropriate actions.
   * - | INFO
     - | An interesting piece of information about the current operation that helps to give
       | context to the log, often when processes are starting or stopping.
   * - | DEBUG
     - | Additional messages reporting the internal state of the application.

The log level for messages in the console window can be chosen
from a drop-down list in the Console dialog [1]. The log will include
all events of the indicated severity as well as events of greater
severity (default: *INFO*). *Word wrapping* can be optionally enabled
for long message texts that otherwise exceed the width of the console
window. In addition, the *color scheme* for console log messages can be
customized by assigning text colors to each log level.

.. note::
   The log output in the *console window* is truncated after 10,000
   log messages in order to prevent the UI from getting unresponsive.

If log messages shall additionally be stored in a log file, simply
activate the option *Write messages to log file*. The log file is named
``log_3dcitydb_impexp_{date}.log`` by default, with the ``{date}`` token
being replaced with the current date at program startup. The Importer/Exporter creates
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
   ``3dcitydb/importer-exporter/log``. However, the location of the home
   directory differs for different operating systems. Using environment
   variables, the location can be identified dynamically:

   - ``%HOMEDRIVE%%HOMEPATH%\3dcitydb\importer-exporter\log`` (Windows 7
     and higher)
   - ``$HOME/3dcitydb/importer-exporter/log`` (UNIX/Linux, Mac OS
     families)

.. _language:

Language selection
^^^^^^^^^^^^^^^^^^

The Importer/Exporter GUI has support for different languages. Use the
Language selection preference dialog shown below to pick your favourite
language.

.. figure:: /media/impexp_preferences_general_language_fig.png
   :name: impexp_preferences_general_language_fig

   General preferences – Language selection.
