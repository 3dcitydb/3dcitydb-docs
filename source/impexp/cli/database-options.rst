**Database connection options**

The following options allow you to define the connection details that
shall be used for establishing a connection to the 3D City Database.

.. option:: -T, --db-type=<database>

   Specify the database system used for running the 3DCityDB. Allowed values are
   ``postgresql`` for PostgreSQL/PostGIS databases (default), and ``oracle``
   for Oracle Spatial/Locator databases.

.. option:: -H, --db-host=<host>

   Specify the host name of the machine on which the 3DCityDB database server is running.

.. option:: -P, --db-port=<port>

   Specify the TCP port on which the 3DCityDB database server is listening for connections.
   The default value is ``5432`` for PostgreSQL and ``1521`` for Oracle.

.. option:: -d, --db-name=<name>

   Specify the name of the 3DCityDB database to connect to. When connecting to an Oracle
   database, provide the database SID or service name as value.

.. option:: -S, --db-schema=<schema>

   Name of the database schema to use when connecting to the 3DCityDB. If not provided,
   the ``citydb`` schema is used for PostgreSQL by default, whereas the schema of
   the user specified by the option :option:`--db-username` is used under Oracle.

.. option:: -u, --db-username=<name>

   Connect to the 3DCityDB database server as the user given by ``name``.

.. option:: -p, --db-password[=<password>]

   Specify the password to use when connecting to the 3DCityDB database server. You can either
   provide the password as value for this option or leave the value empty to be prompted
   to enter the password on the console before connecting to the database. If you skip this option completely,
   the ``impexp`` tool will try to connect to the database without a password. If the database
   server requires password authentication and a password is not available by other means,
   the connection attempt will fail in this case.