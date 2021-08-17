.. _impexp_cli_help_command:

Help command
------------

**Synopsis**

.. code-block:: bash

   impexp help [-h] [--ade-extensions=<folder>] [-c=<file>]
               [--log-file=<file>] [--log-level=<level>]
               [--pid-file=<file>] [--plugins=<folder>]
               [--use-plugin=<plugin[=true|false]>[,<plugin[=true|false]
               >...]]... [COMMAND...]

**Description**

The ``help`` command is used to display the usage help for the
specified command. When no command is given, the usage help for
the main command is shown instead. Alternatively, you can invoke
the command with the :option:`--help` flag to get its usage help.

**Options**

.. option:: COMMAND...

   The name of the ``COMMAND`` for which the usage help shall be displayed.

**Examples**

.. code-block:: bash

   $ impexp help import

Print the help information about the :ref:`import <impexp_cli_import_command>` command.