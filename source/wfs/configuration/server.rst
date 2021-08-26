.. _wfs_server_settings_chapter:

Server settings
~~~~~~~~~~~~~~~

*Server-specific* settings are available through the ``<server>`` element in
the ``config.xml`` file.

.. code-block:: xml

   <server>
     <externalServiceURL>http://server:port/context-path</externalServiceURL>
     <maxParallelRequests>30</maxParallelRequests>
     <waitTimeout>60</waitTimeout>
     <responseCacheTimeout>300</responseCacheTimeout>
     <enableCORS>true</enableCORS>
     <timeZone>Europe/Berlin</timeZone>
     <textureServiceURL>http://server:port/context-path</textureServiceURL>
     <textureCache isEnabled="true">
       <localCachePath>/some/local/path</localCachePath>
     </textureCache>
     <security isEnabled="true">
       <accessControl>
         <scope operation="GetFeature"/>
         <allow ip="192.168.10.0/24 172.16.0.0/16"/>
         <allow token="25f8feac-2b25-4e03-b7fc-7634bc4be863"/>
         <deny ip="::1"/>
       </accessControl>
     </security>
     <tempCache>
       <mode>database</mode>
     </tempCache>
   </server>

**externalServiceURL**

The external service URL of the WFS can be denoted using the
``<externalServiceURL>`` element. The URL should include the *protocol*
(typically *http* or *https*), the *server name* and the full *context path*
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
   ``<externalServiceURL>`` provided in the ``config.xml``.

**maxParallelRequests**

The ``<maxParallelRequests>`` value defines how many requests will be
handled by the WFS service at the same time (default: 30). If the number
of parallel requests exceeds the given limit, then new requests are
blocked until active requests have been fully processed and the total
number of active requests has fallen below the limit. Note that this parameter
mainly affects requests that require a database connection. To disable the request
limit, simply set ``<maxParallelRequests>`` to 0.

.. note::
   Every WFS can only open a maximum number of physical connections
   to the database system running the 3D City Database instance. This upper
   limit is set through the *maxActive* attribute on the ``<connection>`` element
   (cf. :numref:`wfs_database_settings_chapter`).
   Since every request may use more than one
   connection, make sure that the number of ``<maxParallelRequests>`` is
   below the maximum number of physical connections.

**waitTimeout**

In case an incoming request is blocked because the maximum number of
parallel requests has been reached, the ``<waitTimeout>`` option lets you
specify the maximum time in seconds the WFS service waits for a free
request slot before sending an error message to the client (default: 60
seconds).

**responseCacheTimeout**

If result paging is enabled for the 3DCityDB WFS (see :numref:`wfs_constraints_settings_chapter`),
the ``<responseCacheTimeout>`` parameter can be used to define the length of time (in seconds) that
responses shall be cached for the purpose of paging using the *next* and *previous* URLs in the
response document (default: 300 seconds).

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

**timeZone**

The optional ``<timeZone>`` parameter is used to set the time zone of the WFS service
to a specific value. The time zone is relevant, for instance, to process ``Date`` and ``TimeStamp`` data
from the database. The parameter expects an official time zone ID, either an abbreviation such as "PST",
a full name such as "Europe/Berlin", or a custom ID such as "GMT-08:00". If no ``<timeZone>``
is provided, the time zone of servlet container running the WFS is used as default. In most scenarios,
this default setting should be fine.

.. note::
   Note that if a time zone is provided but cannot be set (e.g. due to an invalid or unsupported ID),
   the start of the WFS service is aborted with an error message. Subsequent requests to the service
   also result in an error message.

**textureServiceURL**

In case the WFS has been configured to export appearances of city objects
(see :numref:`wfs_constraints_settings_chapter`), the appearance information itself is encoded as
CityGML ``<Appearance>`` element in a response document to a *GetFeature* request
(or using similar structures in alternative output formats such as CityJSON). Texture images,
however, are not delivered by the WFS service itself but through a separate REST interface.

This RESTful texture image service is part of the WFS web application and, thus, is automatically
started with the WFS service. Assume that ``http://[host][:port]/citydb-wfs/`` is the context path
of your WFS service (see :numref:`wfs_installation_chapter` for more details). Then the URL of the
REST service will be ``http://[host][:port]/citydb-wfs/texture/``. This URL is used in the response
document to reference texture images in the following way:

::

   http[s]://[host][:port]/citydb-wfs/texture/[bucket]/[filename]

The ``[bucket]`` path element is an integer value under control of the REST service and is used to
organize the texture images into separate subfolders. The ``[filename]`` of the texture image is also
managed by the REST service and may differ from the filename stored in the 3DCityDB to ensure unique names.
The following CityGML snippet illustrates how texture images are referenced based on this scheme in
a WFS response document. A client consuming this document can easily follow the URL to download the
texture image.

.. code-block:: xml

   <bldg:Building gml:id="BLDG_0815">
   …
     <app:appearance>
       <app:Appearance>

         <app:surfaceDataMember>
           <app:ParameterizedTexture>
             <app:imageURI>http://some.host.com/citydb-wfs/texture/3/tex_2.jpg</app:imageURI>
             …
             </app:target>
         </app:surfaceDataMember>
       </app:Appearance>
     </app:appearance>
     …
   </bldg:Building>

The optional ``<textureServiceURL>`` element lets you change the external URL of the REST service
that is used in the response document. By default, the URL is composed from the request of the client,
and this will already be appropriate in most cases. If an ``<externalServiceURL>`` is specified (see above),
then this value will be used for creating the URL to the texture image. The ``<textureServiceURL>``
element allows you to override the default behavior and to use a dedicated value for the REST service.

**textureCache**

By default, every time a client requests a texture image through the REST service, the image is queried
anew from the 3DCityDB. In order to reduce database traffic, the REST service can use a local texture cache
instead. Simply set the *isEnabled* attribute on the ``<textureCache>`` element to true to make use
of this feature. You can provide a ``<localCachePath>`` pointing to your local file system where the
texture cache should be stored. Make sure that this path is both read and write accessible to the
WFS service. If you omit the ``<localCachePath>`` element, the cache will be created in the
``WEB-INF/texture_cache`` folder within your web application.

.. note::
   Texture images can be served faster to the client when using a texture cache. Enabling the texture
   cache is therefore the recommended setting. Note that depending on the number and size of texture images
   stored in your 3DCityDB instance, the texture cache might require substantial space on your hard disk.

**security**

Individual WFS operations can be secured using IP- and token-based access control rules. If an access rule
has been defined for an operation, then this operation may only be invoked by clients having explicit access
permission. Otherwise, the execution of the operation is denied and a corresponding error message is sent back
to the client. The ``<security>`` element can therefore be used to control, for example, that only specific clients
are allowed to request city objects from the database.

To use access rules, the *isEnabled* attribute of the ``<security>`` element must first be set to true.
The rules are then given by one or more ``<accessControl>`` child element. Each ``<accessControl>`` element
can define its *scope* by enumerating the WFS operations to which it shall be applied. The WFS operations
must simply be listed using the *operation* attribute of the ``<scope>`` element. The allowed values are
defined as fixed enumeration in the ``config.xsd`` schema file. If more than one operation shall be on the list,
then a white space must be used as delimiter. If the ``<scope>`` is omitted, then the ``<accessControl>``
element **applies to all WFS operations**.

Access to the operations of an ``<accessControl>`` element is either granted or restricted through
``<allow>`` and ``<deny>`` elements. An ``<accessControl>`` element may have multiple ``<allow>``
and ``<deny>`` child elements in an arbitrary order. The *ip* attribute of both elements is then used to
define the IP addresses of the clients that shall be affected by the rule. The value of the *ip* attribute
can be a simple IP address, but notations based on subnet masks and IP ranges are also supported. Moreover,
both IPv4 and IPv6 addresses can be used. More than one IP address target can be listed on the *ip* attribute
using a single white space as delimiter.

In addition to IP addresses, one or more access token can be defined for ``<allow>`` elements using the
*token* attribute. A token is an arbitrary character string that must be sent by a client on each request
in order to get access. Independent of whether the request is sent using *HTTP Get* or *HTTP Post*,
the token must be provided as separate parameter of the form ``token=<string>``. Tokens can be useful,
for example, if requests are forwarded using internal proxy servers.

The following simple scheme is used to decide whether the request of a client will be processed or rejected:

- If the ``<security>`` settings are inactive because *isEnabled* is set to false, then all requests of all
  clients will be processed (default behavior).
- If the WFS operation invoked by the client is not covered by any ``<accessControl>`` element, then the
  request will be processed.
- If the WFS operation invoked by the client is addressed by one or more ``<accessControl>`` elements,
  then the request will be rejected if the client fulfills one of the ``<deny>`` rules. But even if no
  ``<deny>`` rule matches, the request still will only be processed if at least one ``<allow>`` rule is
  applicable.

.. note::
   Note that a client must always sent a token if one or more tokens are defined for the operation.
   Otherwise, the request will also be rejected immediately.

.. caution::
   Further security mechanisms besides the ``<security>`` settings are not offered by the WFS. So,
   it is your responsibility as service provider to take any reasonable physical, technical and
   administrative measures to secure the WFS service and the access to the 3DCityDB.

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