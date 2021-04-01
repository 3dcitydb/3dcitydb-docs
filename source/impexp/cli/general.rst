.. _impexp_cli_general:

General
=======

The following sections cover more general functionalities and
usage information for the ``impexp`` command-line tool.

Abbreviated options
-------------------

The ``impexp`` tool can recognize abbreviated option names. This way
you can avoid typing long options such as :option:`--gltf-draco-compression`
from the :ref:`export-vis <impexp_cli_export_vis_command>` command. To abbreviate an option,
simply specify the initial letter(s) of the first component of its name and
optionally of one or more subsequent components. The name "components" are
separated by ``-`` dash characters or by case if the option name uses camelCase.

For example, the option :option:`--gltf-draco-compression` has three components.
Valid abbreviations for this option name are listed below.

.. list-table::  Examples of valid option name abbreviations
   :name: impexp_cli_general_valid_abbreviated_options_table
   :widths: 30 70

   * - | :option:`--gltf-draco-compression`
     - | :option:`--glt`, :option:`--gDC`, :option:`--g-d-c`, :option:`--g-dra`, :option:`--g-com`, :option:`--gCom`, ...

However, not all of the valid abbreviations shown in :numref:`impexp_cli_general_valid_abbreviated_options_table`
are also allowed and would work when provided on the command line. The reason is that
**abbreviations must be unambiguous** and may not match multiple options.

For example, the ``export-vis`` command also offers the option :option:`--gltf-version`.
The abbreviation :option:`--glt` would match both :option:`--gltf-draco-compression` and
:option:`--gltf-version` and, thus, is not allowed. The ``impexp`` tool aborts with an
error message in case of ambiguous abbreviations.

Double dash
-----------

To explicitly separate command-line options from the list of positional parameters,
you can use two double dashes ``--`` without any
characters attached as delimiter. For example, consider the following
command to import the CityGML file ``my_city.gml`` into the 3D City Database.

.. code-block:: bash

   $ impexp import -H localhost -d citydb_v4 -u citydb_user -p my_city.gml

The :option:`-p` option of the :ref:`import <impexp_cli_import_command>` command specifies the password
to use when connecting to the 3DCityDB. The option can either take a value
or be empty. In the latter case, the user is prompted for the password.
In the above example, the intention of the user was to leave the password
option empty. However, the ``impexp`` will use the filename ``my_city.gml``
as value for :option:`-p` instead. The reason is that the filename
is a positional parameter and cannot be distinguished from the option value of
:option:`-p` in this example.

To solve this issue, simply separate the filename parameter from the options using
two dashes.

.. code-block:: bash

   $ impexp import -H localhost -d citydb_v4 -u citydb_user -p -- my_city.gml

Reordering the options is also a valid solution for this example.

.. code-block:: bash

   $ impexp import -H localhost -d citydb_v4 -p -u citydb_user my_city.gml

.. _impexp_cli_argument_files:

Using @-files
-------------

When creating a command line with lots of options or with long arguments for options,
you might run into system limitations on the length of the command line. Argument files
(@-files) are a way to overcome this problem. Argument files are files that themselves
contain arguments to the command. You can provide one or more argument files on the
command line by simply prefixing their filenames with the character ``@``. The content
of each @-file is automatically expanded into the argument list.

The options within an @-file can be space-separated or newline-separated. If the value of an option
contains embedded whitespace, put the whole value in double ``"`` or single ``'`` quotes. Within
quoted values, embedded quotes must be escaped with a backslash ``\`` and backslashes
themselves need to be escaped with another backslash if they are part of the value.
You can also split long option values on multiple lines by escaping each line break
with a backslash. Multi-line options are typically only required to increase
the readability of the @-file. Lines starting with ``#`` are comments and are ignored.

The following example shows a simple @-file containing options to be used with
the :ref:`export <impexp_cli_export_command>` command.

.. code-block:: bash
   :linenos:

   # This line is a comment and is ignored.
   --log-level=debug
   --bbox=13.3508824,52.4799281,13.3578297,52.4862805,4326 -g 3,4
   --type-name
   Building

Line 1 is a comment and is ignored. Line 2 contains a single option, whereas two
options separated by a space are put on line 3. Lines 4 and 5 illustrate that you
can also put an option and its value on separate lines.

To use an argument file on the command line, use the ``@`` character followed
by a relative or absolute path to the file. If the path contains spaces, such as ``C:\Program Files``,
you can put the entire path into double quotes ``"C:\\Program Files"``.
Note that all backslashes in the quoted path must be escaped in this case.
To avoid escaping, you can also use ``C:\Program" "Files`` instead.
If the file does not exist, or cannot be read, then the argument will be treated
literally, and not removed. Of course, you can specify multiple @-files
on the same command line.

For example, assume the above argument file exists
at ``/home/foo/args``. An :ref:`export <impexp_cli_export_command>` command using
this argument file can be invoked like shown below.

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   @/home/foo/args -o my_city.gml

The content of the @-file is automatically expanded into the argument list of
the above command in the background.

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   --log-level=debug \
                   --bbox=13.3508824,52.4799281,13.3578297,52.4862805,4326 -g 3,4 \
                   --type-name Building \
                   -o my_city.gml

.. note::
   The argument file may itself contain additional @-file arguments.
   Any such arguments will be processed recursively.

.. note::
   Argument files are also a nice way to create templates for different
   purposes. For example, you can specify separate files for different
   logging options such as *"full logging"* (using the ``debug`` log level and an
   additional log file) and *"minimum logging"* (only ``error`` log level without
   a log file). Depending on the use case and scenario, you can simply pick
   one or the other, even programmatically.