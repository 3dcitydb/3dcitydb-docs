Database connections and operations
-----------------------------------

The Database tab of the operations window shown in the figure below
allows a user to manage and establish database connections [1] and to
execute database operations [2].

|image64|

Figure 56: Database tab.

Managing and establishing database connections
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to connect to an instance of the 3D City Database, valid
connection parameters must be provided on the Database tab.

Mandatory database connection details comprise the *username* and
*password* of the database user, the *type* of the database, the
*server* name (network name or IP address) and *port* number (default:
1521 for Oracle; 5432 for PostgreSQL) of the database server, and the
*database* name (when using Oracle, enter the database SID or service
name here). The optional *schema* parameter lets you define the database
schema you which to connect to. Leave it empty to connect to the default
schema. More information on how to work with multiple 3DCityDB schemas
can be found in chapter 3.4. If you need assistance, ask your database
administrator for connection details and schemas. For convenience, a
user can choose to *save* the *password* in the config file of the
Importer/Exporter. Please be aware that the password will be stored as
plain text.

To manage more than one database connection, connection details are
assigned a short *description* text. The drop-down list at the top of
the Database tab allows a user to switch between connections based on
their description. By using the *Apply*, *New*, *Copy* and *Delete*
buttons, edits to the parameters of the currently selected connection
can be saved, a new connection with empty connections details can be
created, and existing connections can be copied or deleted from the
list.

The *Connect* / *Disconnect* button lets a user connect to / disconnect
from a 3D City Database instance based on the provided connection
details.

.. note::
   With this version of the Importer/Exporter, you will be able to
   **connect to version 4.0 to 3.0 instances** of the 3D City Database
   **but not to any previous version**. See chapter 3.5 for a guide on how
   to migrate a version 2 and 3 instances of the 3D City Database to the
   latest version 4.0.

The console window logs all messages that occur during the connection
attempt. In case a connection could not be established, error messages
are displayed that help to identify the cause of the connection problem.
Otherwise, the console window contains information about the connected
3D City Database instance like those shown in Figure 57. This
information comprises the version of the 3D City Database, the name and
version of the underlying database system, the connection string, the
schema name, the spatial reference system ID (SRID) as well as its name
and GML encoding (as specified during the setup of the 3D City
Database), and whether the database tables are version-enabled.

|image65|

Figure 57: Log messages for a successful database connection.

This information can be requested from a connected 3D City Database at
any time using the *Info* button on the Database tab. Upon successful
connection, the description of the active connection is moreover
displayed in the title bar of the application window.

.. |image64| image:: media/image75.png
   :width: 4.29167in
   :height: 4.39671in

.. |image65| image:: media/image76.png
   :width: 5.07292in
   :height: 2.20248in
