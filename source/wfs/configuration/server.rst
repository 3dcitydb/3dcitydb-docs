.. _wfs_server_settings_chapter:

Server settings
~~~~~~~~~~~~~~~

*Server-specific* settings are available through the ``<server>`` element in
the config.xml file.

.. code-block:: xml
   :name: wfs_server_settings_config_listing

   <server>
     <externalServiceURL>http://yourserver.org/citydb-wfs</externalServiceURL>
     <maxParallelRequests>30</maxParallelRequests>
     <waitTimeout>60</waitTimeout>
     <enableCORS>true</enableCORS>
     <tempCache>
       <mode>database</mode>
     </tempCache>
   </server>

**externalServiceURL**

The external service URL of the WFS can be denoted using the
``<externalServiceURL>`` element. The URL should include the *protocol*
(typically http or https), the *server name* and the full *context path*
where the service is available for clients. Also announce the *port* on
which the service listens if it is not equal to the default port
associated with the given protocol.

.. note::
   The service URL is **not configured** through ``<externalServiceURL>``.
   It rather follows from your servlet container settings and network
   access settings (e.g., if your servlet container is behind a reverse
   proxy). The ``<externalServiceURL>`` value is *only used in the
   capabilities* document and thus announced to a client. Most clients
   rely on the service URL in the *capabilities* document and will send
   requests to this URL. So, make sure that the WFS is available at the
   ``<externalServiceURL>`` provided in the config.xml.

**maxParallelRequests**

The ``<maxParallelRequests>`` value defines how many requests will be
handled by the WFS service at the same time (default: 30). If the number
of parallel requests exceeds the given limit, then new requests are
blocked until active requests have been fully processed and the total
number of active requests has fallen below the limit.

.. note::
   Every WFS can only open a maximum number of physical connections
   to the database system running the 3D City Database instance. This upper
   limit is set through the *maxActive* attribute on the ``<connection>`` element
   (cf. :numref:`wfs_database_settings_chapter`).
   Since every request may use more than one
   connection, make sure that the total number of parallel requests is
   below the maximum number of physical connections.

**waitTimeout**

In case an incoming request is blocked because the maximum number of
parallel requests has been reached, the ``<waitTimeout>`` option lets you
specify the maximum time in seconds the WFS service waits for a free
request slot before sending an error message to the client (default: 60
seconds).

**enableCORS**

The flag ``<enableCORS>`` (default: *true*) allows for enabling
*Cross-Origin Resource Sharing* (CORS). Usually, the
*Same-Origin-Policy* (SOP) forbids a client to send Cross-Origin
requests. If CORS is enabled, the WFS server sends the HTTP header
``Access-Control-Allow-Origin`` with the value ``*`` in the response.

.. note::
  When enabling CORS support through the WFS service, global settings for the
  HTTP header ``Access-Control-Allow-Origin`` on the level of the servlet container
  are overridden. If such global CORS settings are configured for your servlet
  container, it therefore might be better to deactivate the WFS-based CORS
  support (set ``<enableCORS>`` to *false*).

  Please refer to the documentation of your servlet container for information
  about how to enable CORS support on the level of the servlet container. For
  instance, check this `URL <https://tomcat.apache.org/tomcat-9.0-doc/config/filter.html#CORS_Filter>`_
  for the Apache Tomcat 9.0 documentation.

**tempCache**

When exporting data, the WFS must keep track of various temporary
information. For instance, when resolving XLinks, the gml:id values as
well as additional information about the related features and geometries
must be available. This information is kept in main memory for
performance. However, when memory limits are reached, the cache is
written to *temporary tables* in the database.

By default, temporary tables are created in the *3D City Database
instance* itself. The tables are populated during the export operation
and are automatically dropped after the operation has finished.
Alternatively, the ``<tempCache>`` settings let a user choose
to store the temporary information in the *local file system* instead.
For this purpose, the ``<mode>`` property has to be switched from its
default value *database* to *local*. The optional ``<localPath>``
parameter can be used to define the location where the temporary information
should be stored. Without setting ``<localPath>``, the temporary directory of
the web application is used as default location.

Some reasons for using a local, file-based storage are:

-  The 3D City Database instance is kept clean from any additional
   (temporary) table holding temporary process information.
   Please choose a fast local storage device with sufficient storage place.
-  If the WFS runs on a different machine than the 3D City
   Database instance, sending temporary information over the network
   might be slow. In such cases, using a local storage might help to
   increase performance.