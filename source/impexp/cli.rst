.. _impexp_cli_chapter:

Using the command-line interface (CLI)
--------------------------------------

.. only:: html

.. toctree::
   :hidden:

   cli/help
   cli/import
   cli/export
   cli/export-vis
   cli/delete
   cli/validate
   cli/gui
   cli/environment-variables
   cli/general

In addition to the graphical user interface, the Importer/Exporter also
comes with a command-line interface (CLI) letting you execute the
main operations of the Importer/Exporter on your command prompt.
The CLI also allows you to easily embed the Importer/Exporter
in batch processing workflows and third-party applications, and to
run in from within a Docker container.

To launch the CLI, simply execute the ``impexp`` start script on the command line. The script
is located in the ``bin`` folder within the installation directory of your Importer/Exporter.
Depending on your platform, it comes in two flavors:

- ``impexp.bat`` (Microsoft Windows family)
- ``impexp`` (UNIX/Linux/Mac OS family)

The CLI is launched with default options for the Java Virtual Machine (JVM) that
runs the application. You can override these default options in the launch process by
using the environment variable ``JAVA_OPTS``. More information is available in
:numref:`impexp_launching_chapter`.

.. note::
   For convenience, it is recommended to add the ``impexp`` executable to your path.
   Use one of the following commands to do so.

   Windows: ``set PATH=C:\path\to\3DCityDB-Importer-Exporter\bin;%PATH%`` |br|
   Linux: ``export PATH=/path/to/3DCityDB-Importer-Exporter/bin:$PATH``

.. note::
   Instead of using the predefined start script, you can also directly invoke the JAR file
   ``impexp-client-<version>.jar`` in the ``lib`` folder of the installation
   directory, which implements the command-line interface. This way, you have full
   control over the Java runtime and options for executing the CLI. Use the
   following command as starting point.

   ``java [JVM_OPTIONS] -jar lib/impexp-client-cli-<version>.jar [CLI_OPTIONS]``


**Synopsis**

.. code-block:: bash

   impexp [-hV] [--ade-extensions=<folder>] [-c=<file>] [--log-file=<file>]
          [--log-level=<level>] [--pid-file=<file>] [--plugins=<folder>]
          [--use-plugin=<plugin[=true|false]>[,<plugin[=true|false]
          >...]]... [@<filename>...] COMMAND

**Description**

The ``impexp`` interface offers a number of commands to interact with
the 3D City Database and to execute import, export and delete operations.
The commands have their own options and parameters and are discussed
in separate sections of this chapter.

.. list-table::  Commands offered by the command-line interface
   :name: impexp_cli_subcomannds_table
   :widths: 30 70

   * - | **Subcommand**
     - | **Description**
   * - | :ref:`impexp_cli_help_command`
     - | Displays help information about the specified command.
   * - | :ref:`impexp_cli_import_command`
     - | Imports data in CityGML or CityJSON format.
   * - | :ref:`impexp_cli_export_command`
     - | Exports data in CityGML or CityJSON format.
   * - | :ref:`impexp_cli_export_vis_command`
     - | Exports data in KML/COLLADA/glTF format for visualization.
   * - | :ref:`impexp_cli_delete_command`
     - | Deletes top-level city objects from the database.
   * - | :ref:`impexp_cli_validate_command`
     - | Validates input files against their schemas.
   * - | :ref:`impexp_cli_gui_command`
     - | Starts the graphical user interface.

.. note::
   Plugins can add their own commands to the command-line interface.
   Thus, the list of available commands may increase depending on
   the number of plugins you have registered with the Importer/Exporter.
   Please refer to the user manual of your plugin for a documentation
   of the plugin-specific commands.

You are required to pass the ``COMMAND`` to be executed as mandatory input
to the ``impexp`` interface. Otherwise, the tool will terminate and print
a general help message to the terminal.

The command-line tool uses exit codes to signify success or failure of an operation.
In general, the value ``0`` means success, ``1`` means the operation aborted abnormally due
to errors, and ``2`` indicates invalid input for an option or parameter. The separate
commands may add their own operation-specific exit codes to this list.

Just like with the graphical user-interface, you can use the environment
variable ``JAVA_OPTS`` in the launch process of the CLI to provide
options to the Java Virtual Maching (JVM) that runs the tool. For example,
you can adapt the amount of main memory that shall be available for the Importer/Exporter.
Please refer to :numref:`impexp_launching_chapter` for how to use the ``JAVA_OPTS`` variable.
Export users may also directly adapt the ``impexp`` start script.

More general functionalities and usage information for the command-line tool are
discussed in :numref:`impexp_cli_general`.

**Global options**

The ``impexp`` main command provides global options that can be used with all commands.
On the command line, you are free to put them in front of or after the ``COMMAND`` that
you want to execute.

.. option:: COMMAND

   The name of the ``COMMAND`` to execute.

.. option:: -c, --config=<file>

   Use the settings from the specified XML config file for the command to be executed. In general,
   every command provides a set of command-line options to adapt its behaviour to your needs.
   These options are meant to cover the most common use cases but do not reflect
   all settings available from the graphical user interface. If you miss specific settings,
   you can provide a config file in addition or even instead of the command-line
   options offered by the command. Note that settings specified through **command-line options always
   take precedence** over settings from a config file.

   The easiest way to create a config file
   is to run the Importer/Exporter in GUI mode, make all required settings,
   and then save the settings to a file using "*File -> Save Settings As...*" from
   the menu bar. You can directly use the resulting file or adapt it
   to your needs before feeding it to the CLI. Of course, you can also create the config file
   fully automatically and only with the content you need. Either way,
   **make sure the config file is valid with respect to the XML Schema definition**
   of the Importer/Exporter. You can easily get a copy of the XSD file via
   the "*File -> Save Settings XSD As...*" entry from the menu bar.

   .. note::
      You can also create a config file programmatically in Java. The
      JAR file *impexp-config-{version}.jar* in the ``lib`` folder of the
      installation directory contains all the classes required for reading and
      writing a config file. Once you have the JAR file on your classpath, use
      the class ``org.citydb.config.ConfigUtil`` as starting point.

.. option:: --log-level=<level>

   Log level to be used for printing log messages to the terminal and
   a log file. Valid values are ``error``, ``warn``, ``info``, and ``debug`` (default: ``info``).
   See also :numref:`impexp_general_preferences_logging`.

.. option:: --log-file=<file>

   If you want the log messages to be printed to a log file in addition to the terminal,
   provide the full path to the log file with this option. The file will be truncated
   if it already exists.

.. option:: --pid-file=<file>

   Create a file containing the process ID (PID) of the ``impexp`` tool at the
   specified path. The PID can be used, for instance, to check whether the
   process is still running or to send signals to the process. The PID file
   is automatically deleted when the ``impexp`` tool terminates.

.. option:: --plugins=<folder>

   Provide an alternative path where to look for Importer/Exporter plugins. By default,
   plugins are searched for in the ``plugins`` folder within the installation directory
   of the Importer/Exporter.

.. option:: --use-plugin=<plugin[=true|false]>[,<plugin[=true|false]>...]

   Comma-separated list of plugins that shall be enabled (default: ``true``) or disabled
   (``false``) on startup. Use the fully qualified class name of the plugin to uniquely identify it.
   Disabling unnecessary plugins can increase performance.

.. option:: --ade-extensions=<folder>

   Provide an alternative path where to look for ADE extensions. By default,
   ADE extensions are searched for in the ``ade-extensions`` folder within the installation directory
   of the Importer/Exporter.

.. option:: @<filename>...

   When creating a command line with lots of options or with long arguments for options,
   you might run into system limitations on the length of the command line. Argument files
   (@-files) are a way to overcome this problem. Argument files are files that themselves
   contain arguments to the command. You can provide one or more argument files on the
   command line by simply prefixing their filenames with the character ``@``. The content
   of each @-file is automatically expanded into the argument list. See :numref:`impexp_cli_argument_files`
   for more information.

.. option:: -h, --help

   Display a help message for the specified command and exit.

.. option:: -V, --version

   Print version information and exit.

.. |br| raw:: html

   <br />