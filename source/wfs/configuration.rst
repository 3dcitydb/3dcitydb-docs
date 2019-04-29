Configuring the Web Feature Service
-----------------------------------

After deploying but before using the WFS service, you need to edit the
config.xml file to make the service run properly. The config.xml file is
in the WEB-INF directory of the WFS web application. The WEB-INF is a
subfolder of the *application* folder, which is generally named after
the WAR file and itself is a subfolder of the webapps folder in the
Tomcat installation directory. This may be different if you use another
servlet container.

For example, assume that the WFS web application was deployed under the
context name citydb-wfs. Then the location of the WEB-INF folder and the
config.xml file in a default Apache Tomcat installation is shown below.

|image182|

Figure 151: Location of the WEB-INF folder and the config.xml file.

Open the config.xml file with a text or XML editor of your choice and
manually edit the settings. An XML Schema for validating the contents of
the config.xml file is provided as file config.xsd in the subfolder
schemas. **After every edit** to the config.xml file, **make sure** that
the config.xml file **validates against this schema** before reloading
the WFS web application. Otherwise, the application might refuse to
load, or unexpected behavior may occur.

In the config.xml file, the WFS settings are organized into the main XML
elements <capabilities>, <featureTypes>, <operations>, <postProcessing>,
<database>, <server>, <uidCache>, <constraints>, and <logging>. The
discussion of the settings follows this organization in the subsequent
clauses.


.. _database:

Database settings
~~~~~~~~~~~~~~~~~

The *database* settings define the *connection parameters* for
connecting to the 3D City Database instance the WFS service should give
access to. The contents of the <database> element are shown below.

| <database>
| <connection
| initialSize="10"
| maxActive="100"
| maxIdle="50"
| minIdle="0"
| suspectTimeout="60"
| timeBetweenEvictionRunsMillis="30000"
| minEvictableIdleTimeMillis="60000">
| <description/>
| <type>PostGIS</type>
| <server/>
| <port>5432</port>
| <sid/>
| <schema/>
| <user/>
| <password/>
| </connection>
| </database>

Listing 1: Database settings in the WFS config.xml file.

Provide the *type* of the database (Oracle or PostGIS), the *server*
name (network name or IP address) and *port* number (default: 1521 for
Oracle; 5432 for PostgreSQL) of the database server, the *sid* (when
using Oracle, enter the database SID or service name; for PostgreSQL
enter the database name), and the *user* and *password* of the database
user. You can copy&paste these settings from the config file of the
Importer/Exporter. Use the optional *schema* element if you want to
connect to a schema other than the default schema. The *description* is
optional and can be left empty.

In addition to these minimum settings, the <connection> element takes
*optional attributes* that let you configure the use of physical
connections to the database server. This is especially important for
production servers and if more than one WFS service connects to the same
database server (in this case, you should also carefully configure the
database itself). The attributes together with their meaning are
described in the following table.

============================= ==============================================================================================================================================================================================================================================================================================================================================================================================================================
**Attribute**                 **Description**
initialSize                   **(int) the initial number of physical connections that are created when the database connection is established (default: 10).**
maxActive                     **(int) The maximum number of active connections to the database that can be allocated at the same time (default: 100). NOTE – make sure your database is configured to handle this number of parallel active connections.**
maxIdle                       **(int) The maximum number of connections that should be kept active at all times (default: 50). Idle connections are checked periodically (if enabled) and connections that have been idle for longer than** minEvictableIdleTimeMillis **will be released. (also see** testWhileIdle\ **)**
minIdle                       **(int) The minimum number of established connections that should be kept active at all times (default: 0). The connection pool can shrink below this number if validation queries fail. (also see** testWhileIdle\ **)**
maxWait                       **(int) The maximum number of milliseconds that the service will wait (when there are no available connections) for a connection before throwing an exception (default: 30000, i.e. 30 seconds).**
testOnBorrow                  **(boolean) The indication of whether connections will be validated before being used by the service. If the connections fails to validate, it will be dropped, and the service will attempt to borrow another (default: false). NOTE - for a true value to have any effect, the** validationQuery **parameter must be set to a non-null string. In order to have a more efficient validation, see** validationInterval\ **.**
testOnReturn                  **(boolean) The indication of whether connections will be validated before being returned to the internal connection pool (default: false). NOTE - for a true value to have any effect, the** validationQuery **parameter must be set to a non-null string.**
testWhileIdle                 **(boolean) The indication of whether connections will be validated by the idle connections evictor (if any). If a connections fails to validate, it will be dropped (default: false). NOTE - for a true value to have any effect, the** validationQuery **parameter must be set to a non-null string.**
validationQuery               **(String) The SQL query that will be used to validate connections. If specified, this query does not have to return any data (default: null). Example values are “select 1 from dual” (Oracle) or “select 1” (PostgreSQL).**
validationClassName           (String) The name of a class which implements the org.apache.tomcat.jdbc.pool.Validator interface and provides a no-arg constructor (may be implicit). If specified, the class will be used to instead of any validation query to validate connections (default: null). NOTE – for a non-null value to have any effect, the class has to be implemented by you as part of the source code of the WFS service. Use with care.
timeBetweenEvictionRunsMillis (int) The number of milliseconds to sleep between runs of the idle connection validation/cleaner. This value should not be set under 1 second. It dictates how often we check for idle, abandoned connections, and how often we validate idle connections (**default: 30000, i.e. 30 seconds).**
minEvictableIdleTimeMillis    (int) The minimum amount of time a connection may be idle before it is eligible for eviction (default: 60000, i.e. 60 seconds).
removeAbandoned               (boolean) Flag to remove abandoned connections if they exceed the removeAbandonedTimout. If set to true a connection is considered abandoned and eligible for removal if it has been in use longer than the removeAbandonedTimeout See also logAbandoned (default: false).
removeAbandonedTimeout        (int) Timeout in seconds before an abandoned (in use) connection can be removed (default: 60, i.e. 60 seconds). The value should be set to the longest running query.
logAbandoned                  (boolean) Flag to log stack traces for application code which abandoned a connection. NOTE - this adds overhead for every connection borrow (default: false).
connectionProperties          (String) The connection properties that will be sent to the JDBC driver when establishing new connections. Format of the string must be [propertyName=property;]\* NOTE - The "user" and "password" properties will be passed explicitly, so they do not need to be included here (default: null).
initSQL                       (String) A custom query to be run when a connection is first created (default: null).
validationInterval            (long) To avoid excess validation, only run validation at most at this frequency - time in milliseconds. If a connection is due for validation, but has been validated previously within this interval, it will not be validated again (default: 30000, i.e. 30 seconds).
jmxEnabled                    (boolean) Register the internal connection pool with JMX or not (default: true).
fairQueue                     (boolean) Set to true if connection requests should be treated fairly in a true FIFO fashion (default: true)
abandonWhenPercentageFull     (int) Connections that have been abandoned (timed out) will not get closed and reported up unless the number of connections in use are above the percentage defined by abandonWhenPercentageFull. The value should be between 0-100 (default: 0, which implies that connections are eligible for closure as soon as removeAbandonedTimeout has been reached).
maxAge                        (long) Time in milliseconds to keep connections alive. When a connection is returned to the internal pool, it will be checked whether now - time-when-connected > maxAge has been reached, and if so, the connection is closed (default: 0, which implies that connections will be left open and no age check will be done).
suspectTimeout                (int) Timeout value in seconds (default: 0).
============================= ==============================================================================================================================================================================================================================================================================================================================================================================================================================

Table 39: Optional database connection settings.


.. _capabilities:

Capabilities settings
~~~~~~~~~~~~~~~~~~~~~

The *capabilities* settings define the contents of the *capabilities*
document that is returned by the WFS service upon a GetCapabilities
request. The *capabilities* document is generated dynamically from the
contents of the config.xml file at request time.

Only optional *service metadata* must be explicitly specified in the
config.xml file using the <owsMetadata> child element of <capabilities>
(see the example listing below). All other sections of the
*capabilities* document are populated automatically from the config.xml
file. For example, the set of feature types advertised in the
<wfs:FeatureTypeList> section is derived from the content of the
<featureTypes> element (cf. chapter 7.3.3).

Note that the metadata is copied to the *capabilities* document “as is”.
Thus, the WFS implementation neither performs a consistency check nor
validates the provided metadata.

| <capabilities>
| <owsMetadata>
| <ows:ServiceIdentification>
| <ows:Title>3D City Database Web Feature Service</ows:Title>
| <ows:ServiceType>WFS</ows:ServiceType>
| <ows:ServiceTypeVersion>2.0.0</ows:ServiceTypeVersion>
| </ows:ServiceIdentification>
| <ows:ServiceProvider>
| <ows:ProviderName/>
| <ows:ServiceContact/>
| </ows:ServiceProvider>
| </owsMetadata>
| </capabilities>

Listing 2: Service metadata settings in the WFS config.xml file.

Service metadata comprises, for example, information about the *service
itself* that might be useful in machine-to-machine communication or for
display to a human. Such information is announced through the
<ows:ServiceIdentifikation> child element. In contrast, the child
element <ows:ServiceProvider> contains information about the *service
provider* such as contact information. Please refer to the OGC *Web
Services Common Specification* (OGC 06-121r3:2009) to get an overview of
the supported metadata fields that may be included in the *capabilities*
document and therefore can be specified in <owsMetadata>.

.. note::
   Service metadata is *optional* and therefore does not have to be
   included in the config.xml file. Simply provide no content for the
   <capabilities> element or omit it completely. In both cases, the
   *capabilities* document will nevertheless be generated dynamically.

.. note::
   The 3DCityDB WFS implementation supports both versions 2.0.0 and
   2.0.2 of the WFS specification. A list of <ows:ServiceTypeVersion>
   elements is used to denote which versions are offered to clients. The
   default config.xml only uses version 2.0.0 because many WFS clients
   still have issues with correctly handling version 2.0.2.


.. _feature-type:

Feature type settings
~~~~~~~~~~~~~~~~~~~~~

With the *feature type* settings, you can control which feature types
can be queried from the 3D City Database and are served through the WFS
interface. Every feature type that shall be advertised to a client must
be explicitly listed in the config.xml file.

An example of the corresponding <featureTypes> XML element is shown
below. In this example, CityGML *Building* and *Road* objects are
available from the WFS service. In addition, a third feature type
*IndustrialBuilding* coming from a CityGML ADE is advertised.

| <featureTypes>
| <featureType>
| <name>Building</name>
| <ows:WGS84BoundingBox>
| <ows:LowerCorner>-180 -90</ows:LowerCorner>
| <ows:UpperCorner>180 90</ows:UpperCorner>
| </ows:WGS84BoundingBox>
| </featureType>
| <featureType>
| <name>Road</name>
| <ows:WGS84BoundingBox>
| <ows:LowerCorner>-180 -90</ows:LowerCorner>
| <ows:UpperCorner>180 90</ows:UpperCorner>
| </ows:WGS84BoundingBox>
| </featureType>
| <adeFeatureType>
| <name
  namespaceURI="http://www.citygml.org/ade/TestADE/1.0">IndustrialBuilding</name>
| <ows:WGS84BoundingBox>
| <ows:LowerCorner>-180 -90</ows:LowerCorner>
| <ows:UpperCorner>180 90</ows:UpperCorner>
| </ows:WGS84BoundingBox>
| </adeFeatureType>
| <version isDefault="true">2.0</version>
| <version>1.0</version>
| </featureTypes>

Listing 3: Advertised feature types in the WFS config.xml file.

The <featureTypes> element contains one <featureType> node per feature
type to be advertised. The feature type is specified through the
mandatory *name* property, which can only take values from a fixed list
that enumerates the names of the CityGML top-level features (cf.
config.xsd schema file). In addition, the geographic region covered by
all instances of this feature type in the 3D City Database can
optionally be announced as *bounding box* (lower left and upper right
corner). The coordinate values must be given in WGS 84.

.. note::
   The bounding box is not automatically checked against or
   computed from the database, but rather copied to the WFS *capabilities*
   document “as is”.

Feature types coming from a CityGML ADE are advertised using the
<adeFeatureType> element. In contrast to CityGML feature types, the
*name* property must additionally contain the globally unique XML
*namespace URI* of the CityGML ADE, and the type name is not restricted
to a fixed enumeration. Note that a corresponding *ADE extension* must
be installed for the WFS service, and that the ADE extension must add
support for the advertised ADE feature type. Otherwise, the ADE feature
type is ignored. If you do not have ADE extensions, then simply skip the
<adeFeatureType> element.

Besides the list of advertised feature types, also the CityGML *version*
to be used for encoding features in a response to a client’s request has
to be specified. Use the <version> element for this purpose, which takes
either 2.0 (for CityGML 2.0) or 1.0 (for CityGML 1.0) as value. If both
versions shall be supported, simply use two <version> elements. However,
in this case, you should define the *default version* to be used by the
WFS by setting the isDefault attribute to true on one of the elements
(otherwise, CityGML 2.0 will be the default).


.. _operations:

Operations settings
~~~~~~~~~~~~~~~~~~~

The *operations* settings are used to define the operation-specific
behavior of the WFS.

| <operations>
| <requestEncoding>
| <method>KVP+XML</method>
| <useXMLValidation>true</useXMLValidation>
| </requestEncoding>
| <exportCityDBMetadata>false</exportCityDBMetadata>
| <GetFeature>
| <outputFormats>
| <outputFormat name="application/gml+xml; version=3.1"/>
| <outputFormat name="application/json"/>
| </outputFormats>
| </GetFeature>
| </operations>

Listing 4: Operations settings in the WFS config.xml file.

The <requestEncoding> element determines whether the WFS shall support
XML-encoded and/or KVP-encoded requests. The desired method is chosen
using the <method> child element that accepts the values “KVP”, “XML”
and “KVP+XML” (default: KVP+XML). When setting the <useXMLValidation>
child element to true, all XML encoded operation requests sent to the
WFS are first validated against the WFS and CityGML XML schemas.
Requests that violate the schemas are not processed but instead a
corresponding error message is sent back to the client. Although XML
validation might take some milliseconds, it is **highly recommended** to
always set this option to true to avoid unexpected failures due to XML
issues.

With this version of the WFS interface, the only operation that can be
further configured is the <GetFeature> operation. You can choose the
available *output formats* that can be used in encoding the response to
the client. The value “application/gml+xml; version=3.1” is the default
and basically means that the response to a *GetFeature* operation will
be purely XML-encoded (using CityGML as encoding format with the version
specified in the *feature type* settings, cf. chapter 7.3.3). In
addition, the WFS can advertise the output format “application/json”. In
this case, the response is delivered in CityJSON format. [9]_ CityJSON
is a JSON-based encoding of a subset of the CityGML data model. The
3DCityDB WFS supports version 0.6 of CityJSON. Note that the format is
still under development.

.. note::
   The WFS can only advertise the different output formats in the
   *capabilities* document. It is up to the client though to choose one of
   these output formats when requesting feature data from the WFS.


.. _postprocessing:

Postprocessing settings
~~~~~~~~~~~~~~~~~~~~~~~

The *postprocessing* settings allow for specifying XSLT transformations
that are applied on the CityGML data of a WFS response before sending
the response to the client.

| <postProcessing>
| <xslTransformation isEnabled="true">
| <stylesheet>AdV-coordinates-formatter.xsl</stylesheet>
| </xslTransformation>
| </postProcessing>

Listing 5: Postprocessing settings in the WFS config.xml file.

To enable transformations, set the *isEnabled* attribute on the
<xslTransformation> child element to *true*. In addition, provide one or
more <stylesheet> elements enumerating the XSLT stylesheets that shall
be applied in the transformation. The stylesheets are supposed to be
stored in the xslt-stylesheets subfolder of the WEB-INF folder of your
WFS application. Thus, any relative path provided as <stylesheet> will
be resolved against WEB-INF/xslt-stylesheets/. You may alternatively
provide an absolute path pointing to another location in your local file
system. However, note that the WFS web application must have appropriate
access rights to this location.

If you provide more than one XSLT stylesheet, then the stylesheets are
executed in the given sequence of the <stylesheet> elements, with the
output of a stylesheet being the input for its direct successor.

.. note::
   To be able to handle arbitrarily large exports, the WFS process
   reads single top-level features from the database, which are then
   written to the response stream. Each XSLT stylesheet will hence just
   work on individual top-level features but not on the entire response.

.. note::
   The output of each XSLT stylesheet must again be a valid CityGML
   structure.

.. note::
   Only stylesheets written in the XSLT language version 1.0 are
   supported.


.. _server:

Server settings
~~~~~~~~~~~~~~~

*Server-specific* settings are available through the <server> element in
the config.xml file.

| <server>
| <externalServiceURL>http://yourserver.org/citydb-wfs</externalServiceURL>
| <maxParallelRequests>30</maxParallelRequests>
| <waitTimeout>60</waitTimeout>
| <enableCORS>true</enableCORS>
| </server>

Listing 6: Server settings in the WFS config.xml file.

The external service URL of the WFS can be denoted using the
<externalServiceURL> element. The URL should include the *protocol*
(typically http or https), the *server name* and the full *context path*
where the service is available for clients. Also announce the *port* on
which the service listens if it is not equal to the default port
associated with the given protocol.

.. note::
   The service URL is **not configured** through <externalServiceURL>.
   It rather follows from your servlet container settings and network
   access settings (e.g., if your servlet container is behind a reverse
   proxy). The <externalServiceURL> value is *only used in the
   capabilities* document and thus announced to a client. Most clients
   rely on the service URL in the *capabilities* document and will send
   requests to this URL. So, make sure that the WFS is available at the
   <externalServiceURL> provided in the config.xml.

The <maxParallelRequests> value defines how many requests will be
handled by the WFS service at the same time (default: 30). If the number
of parallel requests exceeds the given limit, then new requests are
blocked until active requests have been fully processed and the total
number of active requests has fallen below the limit.

.. note::
   Every WFS can only open a maximum number of physical connections
   to the database system running the 3D City Database instance. This upper
   limit is set through the maxActive attribute on the <connection> element
   (cf. chapter 7.3.1). Since every request may use more than one
   connection, make sure that the total number of parallel requests is
   below the maximum number of physical connections.

In case an incoming request is blocked because the maximum number of
parallel requests has been reached, the <waitTimeout> option lets you
specify the maximum time in seconds the WFS service waits for a free
request slot before sending an error message to the client (default: 60
seconds).

The flag <enableCORS> (default: *true*) allows for enabling
*Cross-Origin Resource Sharing* (CORS). Usually, the
*Same-Origin-Policy* (SOP) forbids a client to send Cross-Origin
requests. If CORS is enabled, the WFS server sends the HTTP header
Access-Control-Allow-Origin with the value \* in the response.


.. _cache:

Cache settings
~~~~~~~~~~~~~~

When exporting data, the WFS must keep track of various temporary
information. For instance, when resolving XLinks, the gml:id values as
well as additional information about the related features and geometries
must be available. This information is kept in main memory for
performance. However, when memory limits are reached, the cache is
written to *temporary tables* in the database.

Per default, temporary tables are created in the *3D City Database
instance* itself. The tables are populated during the export operation
and are automatically dropped after the operation has finished.
Alternatively, the *cache* settings available through the <uidCache>
element let a user choose to store the temporary information in the
*local file system* instead.

| <uidCache>
| <mode>local</mode>
| </uidCache>

Listing 7: Cache settings in the WFS config.xml file.

The <mode> property allows for switching between *database* cache
(default) and *local* cache. Some reasons for using a local, file-based
storage are:

-  The 3D City Database instance is kept clean from any additional
   (temporary) table.

-  If the Importer/Exporter runs on a different machine than the 3D City
   Database instance, sending temporary information over the network
   might be slow. In such cases, using a local storage might help to
   increase performance.


.. _constraints:

Constraints settings
~~~~~~~~~~~~~~~~~~~~

The <constraints> element of the config.xml allows for defining
constraints on dedicated WFS operations.

| <constraints>
| <countDefault>10</countDefault>
| <stripGeometry>false</stripGeometry>
| <lodFilter mode="and" searchMode="depth" searchDepth="2">
| <lod>2</lod>
| <lod>3</lod>
| </lodFilter>
| </constraints>

Listing 8: Security settings in the WFS config.xml file.

The <countDefault> constraint restricts the number of city objects to be
returned by the WFS to the user-defined value, even if the request is
satisfied by more city objects in the 3D City Database. The default
behavior is to return *all* city objects matching a request. If a
maximum count limit is defined, then this limit is automatically
advertised in the server’s capabilities document using the CountDefault
constraint.

When setting <stripGeometry> to *true* (default: *false*), the WFS will
remove all spatial properties from a city object before returning the
city object to the client. Thus, the client will not receive any
geometry values.

The <lodFilter> constraint defines a server-side filter on the LoD
representations of the city objects. When using this constraint, city
objects in a response document will only contain those LoD levels that
are enumerated using one or more <lod> child elements of <lodFilter>.
Further LoD representations of a city object, if any, are automatically
removed. If a city object satisfies a query but does not have a geometry
representation in at least one of the specified LoD levels, it will be
skipped from the response document and thus not returned to the client.

The default behavior of the LoD filter can be adapted using attributes
on the <lodFilter> element. The *mode* attribute defines whether a city
object must have a spatial representation in all (“*and*\ ”) or just one
(“*or*\ ”) of the provided LoD levels. If setting *searchMode* to
“\ *depth*\ ”, then you can use the additional *searchDepth* attribute
to specify how many levels of nested city objects shall be considered
when searching for matching LoD representations. If *searchMode* is set
to “\ *all*\ ”, then all nested city objects will be considered.

.. note::
   The constraint settings in config.xml do not replace a real
   security layer on user, database or network level. So, it is your
   responsibility to take any reasonable physical, technical and
   administrative measures to secure the WFS service and the access to
   the 3D City Database.


.. _logging:

Logging settings
~~~~~~~~~~~~~~~~

The WFS service logs messages and errors that occur during operations to
a dedicated log file. Entries in the log file are associated with a
timestamp, the severity of the event and the IP address of the client
(if available). Per default, the log is stored in the file
WEB-INF/wfs.log within the *application folder* of the WFS web
application.

The <logging> element in the config.xml file is used to adapt these
default settings. The attribute *logLevel* on the <file> child element
lets you change the severity level for log messages to *debug*, *info*,
*warn*, or *error* (default: info). Additionally, you can provide an
alternative absolute path and filename where to store the log messages.

.. note::
   A web application typically has limited access to the file
   system for security reasons. Thus, make sure that the log file is
   accessible for the WFS web application. Check the documentation of your
   servlet container for details.

If you want log messages to be additionally printed to the console, then
simply include the <console> child element as well. The <console>
element also provides a *logLevel* attribute to define the severity
level.

| <logging>
| <console logLevel="info"/>
| <file logLevel="info">
| <fileName>path/to/your/wfs.log</fileName>
| </file>
| </logging>

Listing 9: Logging settings in the WFS config.xml file.

.. note::
   Log messages are continuously written to the same log file. The
   WFS application does not include any mechanism to truncate or rotate the
   log file in case the file size grows over a certain limit. So make sure
   you configure log rotation on your server.

.. |image182| image:: ../media/image189.png
   :width: 3.95312in
   :height: 2.75699in
