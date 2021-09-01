.. _impexp_cli_environment_variables:

Environment variables
---------------------

In addition to the command line options for defining the database connection details,
the CLI also supports the following environment variables for this purpose.

.. option:: CITYDB_TYPE=<postgresql|oracle>

  The type of the 3DCityDB to connect to (default: *postgresql*).

.. option:: CITYDB_HOST=<hostname or ip>

  Name of the host or IP address on which the 3DCityDB is running.

.. option:: CITYDB_PORT=<port>

  Port of the 3DCityDB to connect to (default: *5432* for PostgreSQL, *1521* for Oracle).

.. option:: CITYDB_NAME=<name>

  Name of the 3DCityDB database to connect to.

.. option:: CITYDB_SCHEMA=<schema>

  Schema to use when connecting to the 3DCityDB (default: *citydb* for PostgreSQL, username for Oracle).

.. option:: CITYDB_USERNAME=<username>

  Username to use when connecting to the 3DCityDB.

.. option:: CITYDB_PASSWORD=<password>

  Password to use when connecting to the 3DCityDB.

The environment variables can be used *instead of* or *together with* the command line options.
For example, instead of providing the password for connecting to the database as clear
text on the command line using the :option:`--db-password` option, you could store it in the
``CITYDB_PASSWORD`` variable. The following snippet illustrates this example for a UNIX/Linux
system, where the `export` command is used for setting environment variables. Use the `set`
command under Windows instead.

.. code-block:: bash

   $ export CITYDB_PASSWORD=my_password
   $ impexp export -H localhost -d citydb_v4 -u citydb_user -o my_city.gml

.. note::
   The command line options **always take precedence**. For example, if you additionally provide the
   password using the :option:`--db-password` in the above example, the value of the
   ``CITYDB_PASSWORD`` variable will be ignored. The environment variables are *always ignored*
   when running the Importer/Exporter in GUI mode.

.. hint::
   The environment variables can also be used to configure the database connection
   when running the Importer/Exporter as Docker container or when using the Web Feature Service
   (dockerized or not).
