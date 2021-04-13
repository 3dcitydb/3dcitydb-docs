.. _impexp_cli_gui_command:

GUI command
-----------

**Synopsis**

.. code-block:: bash

   impexp gui [-hV] [--no-splash] [--ade-extensions=<folder>] [-c=<file>]
              [--log-file=<file>] [--log-level=<level>] [--pid-file=<file>]
              [--plugins=<folder>] [@<filename>...]

**Description**

The only purpose of the ``gui`` command is to launch the graphical user
interface of the Importer/Exporter. In contrast to the start script
``3DCityDB-Importer-Exporter`` provided in the installation
directory, you can use the global and command-specific options
in the launch process, for instance, to load a specific config file.

**Options**

.. option:: --no-splash

   Hide the Importer/Exporter splash screen during startup.

**Examples**

.. code-block:: bash

   $ impexp gui -c /path/to/my_config.xml \
                --no-splash --log-level=debug \
                --plugins=/my/plugins/folder/

Launch the GUI of the Importer/Exporter and do not show the splash
screen during startup. The settings are loaded from the config
file ``my_config.xml`` instead of the default config file. Moreover,
the log level ``debug`` is used and plugins are searched for in the folder
``/my/plugins/folder/`` rather than in the ``plugins`` folder within the
installation directory.