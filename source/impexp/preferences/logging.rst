.. _impexp_general_preferences_logging:

Logging
^^^^^^^

The Importer/Exporter logs information about events such as activities
or failures, for instance, during database imports and exports. Each log
entry consists of a timestamp when the event occurred, a log level
indicating the severity of the event and a human-readable message text.
Log messages are always printed to the *console window* and may
additionally be forwarded to a log file on your local computer. The
*Logging* preference dialog is shown below.

.. figure:: /media/impexp_preferences_general_logging_fig.png
   :name: impexp_preferences_general_logging_fig
   :align: center

   General preferences – Logging.

The following four log levels are distinguished (from highest to lowest severity):

.. list-table:: Log levels and their meaning.
   :name: impexp_preferences_general_logging_table
   :widths: 20 80

   * - | **Log level**
     - | **Description**
   * - | ERROR
     - | An error has occurred (usually an exception). This comprises internal and unexpected  failures. Moreover, invalid content of CityGML/CityJSON input files is reported via this log level. Fatal errors will cause the operation in progress to abort.
   * - | WARN
     - | An unusual condition has been detected. The operation in progress continues to work but the user should check the warning and take appropriate actions.
   * - | INFO
     - | An interesting piece of information about the current operation that helps to give context to the log, often when processes are starting or stopping.
   * - | DEBUG
     - | Additional messages reporting the internal state of the application.

The log level for messages in the console window can be chosen
from a drop-down list in the *Console* dialog [1]. The log will include
all events of the indicated severity as well as events of greater
severity (default: *INFO*). *Word wrapping* can be optionally enabled
for long message texts that otherwise exceed the width of the console
window. In addition, the *color scheme* for console log messages can be
customized by assigning text colors to each log level.

.. note::
   The log output in the *console window* is truncated after 10,000
   log messages in order to prevent the UI from getting unresponsive.

If log messages shall additionally be stored in a log file, simply
activate the option *Write log messages to log file*. The log file is named
``impexp_{date}.log`` by default, with the ``{date}`` token
being replaced with the current date at program start. By default,
the Importer/Exporter appends log messages to the end of the file
in case the log file already exists. To change this behaviour, enable the
*Truncate log file at program start* option which will clear the log file
with every start of the Importer/Exporter.

Log files are by default stored in the *home directory* of the
*operating system user* running the Importer/Exporter. Precisely, you
will find the log files in the subfolder ``3dcitydb/importer-exporter/log``.
The location of the home
directory differs for different operating systems. Using environment
variables, the location can be identified dynamically:

- ``%HOMEDRIVE%%HOMEPATH%\3dcitydb\importer-exporter\log`` (Windows 7
  and higher)
- ``$HOME/3dcitydb/importer-exporter/log`` (UNIX/Linux, Mac OS
  families)

You can also choose a different directory or even a different
target file through the option *Use alternative log file* [2]. Either enter the directory
or log file manually or click on *Browse* to open a file selection dialog. The log
level can be chosen independently from the console window through the
corresponding drop-down list [2] (default: *INFO*).