.. _wfs_database_settings_chapter:

Database settings
~~~~~~~~~~~~~~~~~

The *database settings* define the *connection parameters* for
connecting to the 3D City Database instance the WFS service should give
access to. Moreover, spatial reference systems (SRS) that shall be supported
by the WFS server in addition to the reference system of the 3DCityDB
have to defined here.The contents of the ``<database>`` element are shown below.

.. code-block:: xml
   :name: wfs_config_listing

   <database>
     <referenceSystems>
       <referenceSystem id="WGS84">
         <srid>4326</srid>
         <gmlSrsName>http://www.opengis.net/def/crs/epsg/0/4326</gmlSrsName>
         <description>WGS 84</description>
       </referenceSystem>
     </referenceSystems>
      <connection
      initialSize="10"
      maxActive="100"
      maxIdle="50"
      minIdle="0"
      suspectTimeout="60"
      timeBetweenEvictionRunsMillis="30000"
      minEvictableIdleTimeMillis="60000">
       <description/>
       <type>PostGIS</type>
       <server/>
       <port>5432</port>
       <sid/>
       <schema/>
       <workspace/>
       <user/>
       <password/>
     </connection>
   </database>

**Connection parameters**

For the ``<connection>`` element, provide the *type* of the database (PostGIS or Oracle), the *server*
name (network name or IP address) and *port* number (default: 5432 for
PostgreSQL; 1521 for Oracle) of the database server, the *sid* (when
using PostgreSQL, enter the database name; for Oracle, enter the database
SID or service name), and the *user* and *password* of the database
user. You can copy&paste these settings from the config file of the
Importer/Exporter. Use the optional *schema* element if you want to
connect to a schema other than the default schema. The *description* is
optional and can be left empty.

For Oracle databases, you can additionally specify the *workspace*
to connect to in case your database is version-enabled. All operations
will then be executed against this workspace. You must provide the ``<name>``
of the workspace and an optional ``<timestamp>`` as child elements.
If no workspace is specified, the default *LIVE* workspace is chosen by default.

.. hint::
   The WFS service can only give access to one database instance. If you want
   to change this database, you need to adapt the ``config.xml`` file and restart
   the service afterwards. If you want to realize WFS interfaces for several database
   instances, you need to deploy the WFS service multiple times.

In addition to these minimum settings, the ``<connection>`` element takes
*optional attributes* that let you configure the use of physical
connections to the database server. This is especially important for
production servers and if more than one WFS service connects to the same
database server (in this case, you should also carefully configure the
database itself). The attributes together with their meaning are
described in the following table.

.. list-table::  Optional database connection settings.
   :name: wfs_database_connection_settings_table
   :widths: 30 70

   * - | **Attribute**
     - | **Description**
   * - | initialSize
     - | (int) the initial number of physical connections that are created
       | when the database connection is established (default: 10).
   * - | maxActive
     - | (int) The maximum number of active connections to the
       | database that can be allocated at the same time (default: 100).
       | NOTE – make sure your database is configured to handle this
       | number of parallel active connections.
   * - | maxIdle
     - | (int) The maximum number of connections that should be kept
       | active at all times (default: 50). Idle connections are checked
       | periodically (if enabled) and connections that have been idle
       | for longer than minEvictableIdleTimeMillis will be
       | released. (also see testWhileIdle)
   * - | minIdle
     - | (int) The minimum number of established connections that
       | should be kept active at all times (default: 0). The connection
       | pool can shrink below this number if validation queries fail.
   * - | maxWait
     - | (int) The maximum number of milliseconds that the service will
       | wait (when there are no available connections) for a connection
       | before throwing an exception (default: 30000, i.e. 30 seconds).
   * - | testOnBorrow
     - | (boolean) The indication of whether connections will be
       | validated before being used by the service. If the connections
       | fails to validate, it will be dropped, and the service will attempt
       | to borrow another (default: false). NOTE - for a true value to
       | have any effect, the validationQuery parameter must be set
       | to a non-null string. In order to have a more efficient
       | validation, see validationInterval.
   * - | testOnReturn
     - | (boolean) The indication of whether connections will be
       | validated before being returned to the internal connection pool
       | (default: false). NOTE - for a true value to have any effect, the
       | validationQuery parameter must be set to a non-null string.
   * - | testWhileIdle
     - | (boolean) The indication of whether connections will be
       | validated by the idle connections evictor (if any). If a
       | connections fails to validate, it will be dropped (default: false).
       | NOTE - for a true value to have any effect, the
       | validationQuery parameter must be set to a non-null string.
   * - | validationQuery
     - | (String) The SQL query that will be used to validate
       | connections. If specified, this query does not have to return
       | any data (default: null). Example values are “select 1 from
       | dual” (Oracle) or “select 1” (PostgreSQL).
   * - | validationClassName
     - | (String) The name of a class which implements the
       | org.apache.tomcat.jdbc.pool.Validator interface and
       | provides a no-arg constructor (may be implicit). If specified,
       | the class will be used to instead of any validation query to
       | validate connections (default: null). NOTE – for a non-null
       | value to have any effect, the class has to be implemented by
       | you as part of the source code of the WFS service. Use with
       | care.
   * - | timeBetweenEvictionRunsMillis
     - | (int) The number of milliseconds to sleep between runs of the
       | idle connection validation/cleaner. This value should not be
       | set under 1 second. It dictates how often we check for idle,
       | abandoned connections, and how often we validate idle
       | connections (default: 30000, i.e. 30 seconds).
   * - | minEvictableIdleTimeMillis
     - | (int) The minimum amount of time a connection may be idle
       | before it is eligible for eviction (default: 60000, i.e. 60
       | seconds).
   * - | removeAbandoned
     - | (boolean) Flag to remove abandoned connections if they
       | exceed the removeAbandonedTimout. If set to true a
       | connection is considered abandoned and eligible for removal
       | if it has been in use longer than the
       | removeAbandonedTimeout See also logAbandoned (default:
       | false).
   * - | removeAbandonedTimeout
     - | (int) Timeout in seconds before an abandoned (in use)
       | connection can be removed (default: 60, i.e. 60 seconds). The
       | value should be set to the longest running query.
   * - | logAbandoned
     - | (boolean) Flag to log stack traces for application code which
       | abandoned a connection. NOTE - this adds overhead for
       | every connection borrow (default: false).
   * - | connectionProperties
     - | (String) The connection properties that will be sent to the
       | JDBC driver when establishing new connections. Format of
       | the string must be [propertyName=property;]* NOTE - The
       | "user" and "password" properties will be passed explicitly, so
       | they do not need to be included here (default: null).
   * - | initSQL
     - | (String) A custom query to be run when a connection is first
       | created (default: null).
   * - | validationInterval
     - | (long) To avoid excess validation, only run validation at most
       | at this frequency - time in milliseconds. If a connection is due
       | for validation, but has been validated previously within this
       | interval, it will not be validated again (default: 30000, i.e. 30
       | seconds).
   * - | jmxEnabled
     - | (boolean) Register the internal connection pool with JMX or
       | not (default: true).
   * - | fairQueue
     - | (boolean) Set to true if connection requests should be treated
       | fairly in a true FIFO fashion (default: true)
   * - | abandonWhenPercentageFull
     - | (int) Connections that have been abandoned (timed out) will
       | not get closed and reported up unless the number of
       | connections in use are above the percentage defined by
       | abandonWhenPercentageFull. The value should be between
       | 0-100 (default: 0, which implies that connections are eligible
       | for closure as soon as removeAbandonedTimeout has been
       | reached).
   * - | maxAge
     - | (long) Time in milliseconds to keep connections alive. When a
       | connection is returned to the internal pool, it will be checked
       | whether now - time-when-connected > maxAge has been
       | reached, and if so, the connection is closed (default: 0, which
       | implies that connections will be left open and no age check
       | will be done).
   * - | suspectTimeout
     - | (int) Timeout value in seconds (default: 0).

**Schema name**

The optional ``<schema>`` element can be used to define the database schema the WFS
service shall access for answering requests and executing transactions. For PostgreSQL,
``<schema>`` has to reference a schema (default: citydb) within the database given by
the ``<sid>`` parameter of the connection details. Under Oracle, ``<schema>`` refers to
another user account. The ``<user>`` provided in the connection details therefore requires
sufficient privileges to access the database tables and objects of the user ``<schema>``.
The 3DCityDB is shipped with with database scripts to create new schemas under PostgreSQL,
or to grant read-only access to a different user account under Oracle.

**Spatial reference systems**

Additional spatial reference systems the WFS service should support have to be listed within
the ``<referenceSystems>`` element. Provide the *srid* (spatial reference ID) of the SRS.
This value depends on the database system (PostgreSQL/PostGIS or Oracle) running the 3DCityDB.
Be careful to pick the correct value. In most cases it will match the EPSG code of the SRS.
The *gmlSrsName* element defines the string identifier of the SRS that has to be used by clients
in requests. You are not free to pick an arbitrary identifier but should follow the OGC best
practice for encoding SRS names (see WFS 2.0 specification document for details). The description
is optional and can be left empty.

.. note::
   The WFS always supports the SRS of the 3DCityDB per default. Thus, this SRS has not be explicitly
   defined in the ``config.xml``.