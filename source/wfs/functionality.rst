Functionality
-------------

The Web Feature Service is implemented against version 2.0 of the OGC
Web Feature Service Interface Standard. Previous versions are not
supported any more, and clients must make sure to use this version of
the interface when sending requests to the WFS service.

The following chapters provide a documentation of the functionality
offered by the 3D City Database Web Feature Service. They *do not
provide* a general overview or description of the OGC Web Feature
Service Interface Standard itself. If you need more general information
about WFS, please refer to the WFS specification document instead (OGC
Doc. No. 09-025r2).

.. _basic:

Basic functionality
~~~~~~~~~~~~~~~~~~~

WFS operations
^^^^^^^^^^^^^^

The OGC WFS 2.0 interface defines eleven operations that can be invoked
by a client. A WFS server is not required to offer all operations to
conform to the standard but may support a subset only. For this purpose,
the WFS standard defines *conformance classes* named *Simple WFS*,
*Basic WFS*, *Transactional WFS* and *Locking WFS* that grow in the
number of mandatory operations. The current version of the 3D City
Database Web Feature Service implements the *Simple WFS* conformance
class. Thus, it is *fully OGC conformant* but lacks operations from
other conformance classes. It is planned to incrementally increase the
functionality of the WFS in future releases.

The following table lists all WFS 2.0 operations and marks those
supported by the 3D City Database WFS.

.. list-table::  Overview of supported WFS 2.0 operations.
   :name: wfs_supported_operation_overview_table

   * - | **Operation**
     - | **Description**
     - | **Supported**
   * - | **GetCapabilities**
     - | The GetCapabilities operation generates a service
       | metadata document describing the WFS service
       | provided by a server.
     - | X
   * - | **DescribeFeatureType**
     - | The DescribeFeatureType operation returns a
       | schema description of the CityGML feature types
       | offered by the WFS instance.
     - | X
   * - | **ListStoredQueries**
     - | The ListStoredQueries operation lists the stored
       | queries available at the server.
     - | X
   * - | **DescribeStoredQuery**
     - | The DescribeStoredQueries operation provides
       | detailed metadata about each stored query expression
       | that the server offers.
     - | X
   * - | **GetFeature**
     - | The GetFeature operation returns a selection of
       | CityGML features from the 3D City Database using a
       | query expression.
     - | X
   * - | GetPropertyValue
     - | The GetPropertyValue operation allows the value of a
       | feature property or part of the value of a complex
       | feature property to be retrieved from the 3D City
       | Database for a set of features identified using a query
       | expression.
     - | --
   * - | LockFeature
     - | The LockFeature operation is used to expose a long-
       | term feature locking mechanism to ensure consistency
       | in data manipulation operations (e.g., update or delete).
     - | --
   * - | GetFeatureWithLock
     - | The GetFeatureWithLock operation is functionally
       | similar to the GetFeature operation except that in
       | response to a GetFeatureWithLock operation, the
       | WFS shall also lock the features in the result set.
     - | --
   * - | CreateStoredQuery
     - | A stored query may be created using the
       | CreateStoredQuery operation.
     - | --
   * - | DropStoredQuery
     - | The DropStoredQuery operation allows previously
       | created stored queries to be dropped from the system.
     - | --
   * - | Transaction
     - | The Transaction operation is used to describe data
       | transformation operations (i.e., insert, update, replace,
       | delete) to be applied to CityGML feature instances
       | under the control of the web feature service.
     - | --

.. _wfs_service_url_chapter:

Service URL
^^^^^^^^^^^

The *service URL* or service endpoint is the location where the 3D City
Database WFS can be accessed by a client application over a local
network or the internet. This URL is typically composed as follows:

.. code-block::

   http[s]://[host][:port]/[context_path]/wfs

The actual URL depends on the servlet container and your network
configuration. Please ask your network administrator for the *protocol*
(typically http or https), the *host* name and the *port* of the server.
The *context path* is typically added to the URL by the servlet
container. Please refer to the documentation of your servlet container
for more information. The last component wfs of the URL identifies the
service and makes sure that requests are routed to the WFS service
implementation.

.. note::
   For Apache Tomcat, the name of the WFS WAR file will be used as
   *context path* in the service URL. For example, if the WAR file is
   named ``citydb-wfs.war``, then the service URL will be
   ``http[s]://[host][:port]/citydb-wfs/wfs``. To pick a different context
   path, simply rename the WAR file or change Tomcat’s default behavior.

Service bindings
^^^^^^^^^^^^^^^^

A *service binding* refers to the communication protocol that shall be
used for exchanging request and response messages between a WFS server
and a client. The WFS 2.0 interface standard defines *HTTP GET*, *HTTP
POST* and *SOAP over HTTP POST* as possible service bindings for WFS 2.0
implementations.

The 3D City Database WFS implements both the HTTP POST and the HTTP
GET conformance class. Therefore, a client can choose to send a request
either XML-encoded using the HTTP method POST (using ``text/xml`` as content
type) or KVP-encoded (key-value-pair) using the HTTP method GET. Note
that the XML content of POST messages sent to the server must be
well-formed and valid with respect to the
`WFS 2.0 XML Schema <http://schemas.opengis.net/wfs/2.0/wfs.xsd>`_

The following table summarizes the operations and the supported service
binding as offered by the 3D City Database WFS.

.. list-table::  Service bindings for the supported WFS 2.0 operations.
   :name: wfs_service_bindings_operations_table

   * - | **Operation**
     - | **Service Binding**
   * - | GetCapabilities
     - | XML over HTTP POST and KVP over HTTP GET
   * - | DescribeFeatureType
     - | XML over HTTP POST and KVP over HTTP GET
   * - | ListStoredQueries
     - | XML over HTTP POST and KVP over HTTP GET
   * - | DescribeStoredQuery
     - | XML over HTTP POST and KVP over HTTP GET
   * - | GetFeature
     - | XML over HTTP POST and KVP over HTTP GET

.. _wfs_feature_types_chapter:

CityGML feature types
^^^^^^^^^^^^^^^^^^^^^

The 3D City Database WFS supports all CityGML *top-level feature types*,
and corresponding feature instances will be sent to the client upon
request. If you just want to advertise a subset of the CityGML feature
types, you can restrict the feature types in the ``config.xml`` settings
(cf. :numref:`wfs_feature_type_settings_chapter`). In addition to the predefined CityGML feature
types, the WFS can also support feature types defined in a CityGML ADE.
This requires a corresponding ADE extension to be installed for the WFS
and to be registered with the 3DCityDB instance (cf. :numref:`wfs_installation_chapter`).

.. note::
   Appearance properties of CityGML features such as textures or
   color information are *currently not supported* by the WFS
   implementation and thus will not be included in a response document.

The supported CityGML feature types together with their official XML
namespaces (CityGML version 2.0 and 1.0) and recommended prefixes
are listed in the table below.

.. list-table::  Supported CityGML top-level feature types with XML namespaces and prefixes.
   :name: wfs_supported_toplevel_feature_types_table

   * - | **Feature type**
     - | **XML namespace**
     - | **XML prefix**
   * - | Building
     - | http://www.opengis.net/citygml/building/2.0
       | http://www.opengis.net/citygml/building/1.0
     - | bldg
   * - | Bridge
     - | http://www.opengis.net/citygml/bridge/2.0
     - | brid
   * - | Tunnel
     - | http://www.opengis.net/citygml/tunnel/2.0
     - | tun
   * - | TransportationComplex
     - | http://www.opengis.net/citygml/transportation/2.0
       | http://www.opengis.net/citygml/transportation/1.0
     - | tran
   * - | Road
     - | http://www.opengis.net/citygml/transportation/2.0
       | http://www.opengis.net/citygml/transportation/1.0
     - | tran
   * - | Track
     - | http://www.opengis.net/citygml/transportation/2.0
       | http://www.opengis.net/citygml/transportation/1.0
     - | tran
   * - | Road
     - | http://www.opengis.net/citygml/transportation/2.0
       | http://www.opengis.net/citygml/transportation/1.0
     - | tran
   * - | Square
     - | http://www.opengis.net/citygml/transportation/2.0
       | http://www.opengis.net/citygml/transportation/1.0
     - | tren
   * - | Railway
     - | http://www.opengis.net/citygml/transportation/2.0
       | http://www.opengis.net/citygml/transportation/1.0
     - | tran
   * - | CityFurniture
     - | http://www.opengis.net/citygml/cityfurniture/2.0
       | http://www.opengis.net/citygml/cityfurniture/1.0
     - | frn
   * - | LandUse
     - | http://www.opengis.net/citygml/landuse/2.0
       | http://www.opengis.net/citygml/landuse/1.0
     - | luse
   * - | WaterBody
     - | http://www.opengis.net/citygml/waterbody/2.0
       | http://www.opengis.net/citygml/waterbody/1.0
     - | wtr
   * - | PlantCover
     - | http://www.opengis.net/citygml/vegetation/2.0
       | http://www.opengis.net/citygml/vegetation/1.0
     - | veg
   * - | SolitaryVegetationObject
     - | http://www.opengis.net/citygml/vegetation/2.0
       | http://www.opengis.net/citygml/vegetation/1.0
     - | veg
   * - | ReliefFeature
     - | http://www.opengis.net/citygml/relief/2.0
       | http://www.opengis.net/citygml/relief/1.0
     - | dem
   * - | GenericCityObject
     - | http://www.opengis.net/citygml/generics/2.0
       | http://www.opengis.net/citygml/generics/1.0
     - | gen
   * - | CityObjectGroup
     - | http://www.opengis.net/citygml/cityobjectgroup/2.0
       | http://www.opengis.net/citygml/cityobjectgroup/1.0
     - | grp

Simply declare the above namespaces in XML-encoded requests using the
notation ``xmlns:prefix=namspace_uri``. For KVP-encoded requests,
the ``NAMESPACES`` parameter shall be used to declare any namespaces
and their prefixes used in the request based on the format
``xmlns(prefix, escaped_uri)``.

.. note::
  The 3DCityDB WFS can correctly deal with the default CityGML prefixes
  in KVP-encoded requests. Thus, if you use one of the default prefixes
  from above, you can skip the ``NAMESPACES`` parameter. The CityGML
  version that will be associated with the prefix by the WFS depends
  on the default CityGML version in your ``config.xml``
  (cf. :numref:`wfs_feature_type_settings_chapter`).

Exception reports
^^^^^^^^^^^^^^^^^

If the WFS encounters an error while parsing or processing a request, an
XML document indicating that error is generated and sent to the client
as exception response. Please refer to the WFS 2.0 specification for the
structure and syntax of the exception response.

.. _getcapabilities:

GetCapabilities operation
~~~~~~~~~~~~~~~~~~~~~~~~~

The GetCapabilities operation generates an XML-encoded service metadata
document describing the WFS service provided by a server. The
*capabilities* document contains relevant technical and non-technical
information about the service and its provider. Its content mainly
depends on the configuration of the WFS in the ``config.xml`` settings file.

The following XML snippet shows an XML encoding of a GetCapabilities
operation.

.. code-block:: xml
   :name: wfs_getCapabilities_example_listing

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetCapabilities service="WFS"
    xmlns:wfs="http://www.opengis.net/wfs/2.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.opengis.net/wfs/2.0
    http://schemas.opengis.net/wfs/2.0/wfs.xsd"/>

The declaration of the WFS XML namespace http://www.opengis.net/wfs/2.0
is mandatory to be able to validate the request against the official WFS
XML Schema definition. The reference to the schema location using the
``xsi:schemaLocation`` attribute is however optional. It is *recommended*
though if the XML encoding of the request is created manually by the
user (and not automatically by a client software) to ensure schema
validity. By default, the WFS service will reject invalid requests (see
:numref:`wfs_operations_settings_chapter`).

The following table shows the XML attributes that can be used in the
GetCapabilities request and are supported by the WFS implementation.

.. list-table::  Supported XML attributes of a GetCapabilities operation. (O = optional, M = mandatory)
   :name: wfs_supported_getCapabilities_attributes_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the
       | service type. The value “WFS” is fixed.
   * - | AcceptVersions
     - | O
     - |
     - | Used for version number negotiation
       | with the WFS server
       | (cf. OGC Document No. 06-121r3:2009).

As alternative to XML encoding, the GetCapabilities operation may also
be invoked through a KVP-encoded HTTP GET request.

.. code-block::

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   REQUEST=GetCapabilities&
   ACCEPTVERSIONS=2.0.0,2.0.2

The available KVP parameters are listed below.

.. list-table::  Supported KVP parameters of a GetCapabilities operation. (O = optional, M = mandatory)
   :name: wfs_supported_getCapabilities_parameters_table

   * - | **KVP parameter**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | SERVICE
     - | M
     - | WFS (fixed)
     - | see above
   * - | ACCEPTVERSIONS
     - | O
     - |
     - | see above

.. _describefeaturetype:

DescribeFeatureType operation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The DescribeFeatureType operation returns a schema
description of the CityGML feature types advertised by the 3D City
Database WFS instance. Which feature types are offered by the WFS is
controlled through the ``config.xml`` settings file (cf. :numref:`wfs_feature_types_chapter`).
The schema defines the structure and content of the features
(thematic and spatial attributes, nested features, etc.) as well as the
way how features are encoded in responses to GetFeature requests.

The following example shows a valid DescribeFeatureType operation
requesting the XML Schema definition of the CityGML 1.0 *Building*
feature type.

.. code-block:: xml
   :name: wfs_describeFeatureType_example_listing

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:DescribeFeatureType service="WFS" version="2.0.0"
    xmlns:wfs="http://www.opengis.net/wfs/2.0"
    xmlns:bldg="http://www.opengis.net/citygml/building/1.0">
     <wfs:TypeName>bldg:Building</wfs:TypeName>
   </wfs:DescribeFeatureType>

The DescribeFeatureType operations takes the following XML attributes.

.. list-table:: Supported XML attributes of a DescribeFeatureType operation. (O = optional, M = mandatory)
   :name: wfs_supported_describeFeatureType_attributes_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the
       | service type. The value “WFS” is fixed.
   * - | version
     - | M
     - | 2.0.x
     - | The version of the WFS Interface
       | Standard to be used in the
       | communication.
   * - | outputFormat
     - | O
     - | application/gml+xml;
       | version=3.1
     - | Controls the format of the schema
       | description. By default, the request
       | results in a CityGML / GML 3.1.1
       | application schema. The outputFormat
       | attribute may also take the value
       | “application/json”, in which case the
       | response is a CityJSON schema document.
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to
       | associate a mnemonic name with the
       | request that will be used in exception
       | reports.

The ``<wfs:TypeName>`` child element of the DescribeFeatureType operation
identifies the feature type for which the XML Schema description is
requested. Be careful to use the correct spelling of the feature type
name (as specified by the CityGML standard) and to associate the name
with the correct CityGML XML namespace. The ``<wfs:TypeName>`` element may
occur multiple times to request schema definitions of several feature
types in a single DescribeFeatureType operation. If the ``<wfs:TypeName>``
element is omitted, then the complete base schema is returned by the WFS.

The DescribeFeatureType operation can alternatively be invoked through
HTTP GET with key-value pairs.

.. code-block::

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.2&
   REQUEST=DescribeFeatureType&
   TYPENAME=bldg:Building,tran:Road

The following KVP parameters are supported.

.. list-table:: Supported KVP parameters of a DescribeFeatureType operation. (O = optional, M = mandatory)
   :name: wfs_supported_describeFeatureType_kvp_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | SERVICE
     - | M
     - | WFS (fixed)
     - | see above
   * - | VERSION
     - | M
     - | 2.0.x
     - | see above
   * - | NAMESPACES
     - | O
     - |
     - | Used to specify namespaces and their
       | prefixes. The format shall be
       | xmlns(prefix,escaped_url).
   * - | TYPENAMES
     - | M
     - |
     - | A comma-separated list of feature types
       | to describe.
   * - | OUTPUTFORMAT
     - | O
     - | application/gml+xml;
       | version=3.1
     - | see above

The ``TYPENAME`` attribute lists the feature types to describe. Similar to an
XML-encoded request, both the feature type names and the XML namespaces
must be correct. XML namespaces and their prefixes can be specified
using the ``NAMESPACES`` attribute. If you use default CityGML prefixes
though, the ``NAMESPACES`` attribute can be skipped (see :numref:`wfs_feature_types_chapter`).

.. _wfs_ListStoredQueries_operation_chapter:

ListStoredQueries operation
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Since version 2.0 of the WFS standard, a WFS server is supposed to
manage predefined and parameterized feature query expressions (so called
*stored queries*) that are stored by the server and that can be
repeatedly invoked by the client using different parameter values.
Stored queries hide the complexity of the underlying query expression
from the client since all the client needs to know is the unique
identifier of the stored query as well as the names and types of the
parameters in order to invoke the operation.

The ListStoredQuery operation is meant to provide the list of stored
queries that is offered by the WFS server. The response document
contains the unique identifier for each stored query which can then be
used in a subsequent DescribeStoredQuery operation to receive the
details of a specific stored query form the WFS server. The following
listing presents an example ListStoredQuery operation.

.. code-block:: xml
   :name: wfs_listStoredQuery_example_listing

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:ListStoredQueries service="WFS" version="2.0.0"
    xmlns:wfs="http://www.opengis.net/wfs/2.0"/>

The ListStoredQuery operation may take the following XML attributes as
parameters.

.. list-table:: Supported XML attributes of a ListStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_listStoredQuery_attributes_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the
       | service type. The value “WFS” is fixed.
   * - | version
     - | M
     - | 2.0.x
     - | The service attribute indicates the
       | The version of the WFS Interface
       | Standard to be used in the
       | communication.
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to
       | associate a mnemonic name with the
       | request that will be used in exception
       | reports.

The corresponding KVP-encoded request is shown below.

.. code-block::
   
   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.0&
   REQUEST=ListStoredQueries

The following KVP parameters can be used when invoking the
ListStoredQueries operation.

.. list-table:: Supported KVP parameters of a ListStoredQuery operation. (O = optional, M = mandatory)
   :widths: 600 300 300 600
   :name: wfs_supported_listStoredQuery_kvp_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | SERVICE
     - | M
     - | WFS (fixed)
     - | see above
   * - | VERSION
     - | M
     - | 2.0.x
     - | see above

.. _describestoredquery:

DescribeStoredQuery operation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The DescribeStoredQuery operation is used to provide the details of one
or more stored queries offered by the server. The following listing
exemplifies a DescribeStoredQuery request.

.. code-block:: xml
   :name: wfs_describeStoredQuery_example_listing

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:DescribeStoredQueries service="WFS" version="2.0.0"
    xmlns:wfs="http://www.opengis.net/wfs/2.0">
     <wfs:StoredQueryId>http://www.opengis.net/def/query/OGC-WFS/0/GetFeatureById</wfs:StoredQueryId>
   </wfs:DescribeStoredQueries>

The ``<wfs:StoredQueryId>`` child element provides the unique identifier of
the stored query (see ListStoredQuery operation
in :numref:`wfs_ListStoredQueries_operation_chapter`). By
providing more than on unique identifier through multiple
``<wfs:StoredQueryId>`` elements, the descriptions of separate stored
queries can be requested in a single DescribeStoredQuery operation. If
the ``<wfs:StoredQueryId>`` element is omitted, a description of all stored
queries available at the WFS server is returned to the client. The above
request will produce a response similar to the following listing.

.. code-block:: xml
   :name: wfs_describeStoredQuery_example_response_listing

   <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
   <wfs:DescribeStoredQueriesResponse
    xmlns:fes="http://www.opengis.net/fes/2.0"
    xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:wfs="http://www.opengis.net/wfs/2.0">
     <wfs:StoredQueryDescription id="http://www.opengis.net/def/query/OGC-WFS/0/GetFeatureById">
       <wfs:Title xml:lang="en">Get feature by identifier</wfs:Title>
       <wfs:Abstract xml:lang="en">Retrieves a feature by its gml:id.</wfs:Abstract>
       <wfs:Parameter name="id" type="xs:string">
         <wfs:Title xml:lang="en">Identifier</wfs:Title>
         <wfs:Abstract xml:lang="en">The gml:id of the feature to be retrieved.</wfs:Abstract>
       </wfs:Parameter>
       <wfs:QueryExpressionText returnFeatureTypes=""
        language="urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression"
        isPrivate="false">
         <wfs:Query typeNames="schema-element(core:_CityObject)">
           <fes:Filter>
             <fes:ResourceId rid="${id}"/>
           </fes:Filter>
         </wfs:Query>
       </wfs:QueryExpressionText>
     </wfs:StoredQueryDescription>
   </wfs:DescribeStoredQueriesResponse>

Every WFS implementation must at minimum offer the GetFeatureById stored
query having the unique identifier
*http://www.opengis.net/def/query/OGC-WFS/0/GetFeatureById* as shown
above. This stored query takes a single parameter *id* of type xs:string
and returns zero or exactly one feature whose resource identifier
matches the id value. For the 3D City Database WFS, the id value is
evaluated against the gml:id of each feature in the database to find a
match.

The *returnFeatureTypes* attribute lists the feature types that may be
returned by a stored query. Note that this string is empty for the the
GetFeatureById query. Consequently, the query will return a feature
instance of all advertised feature types if its gml:id matches. The set
of advertised feature types can be influenced in the ``config.xml`` settings
file. The DescribeStoredQuery operation allows the following XML
attributes.

.. list-table:: Supported XML attributes of a DescribeStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_describeStoredQuery_attributes_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the
       | service type. The value “WFS” is fixed.
   * - | version
     - | M
     - | 2.0.x
     - | The version of the WFS Interface
       | Standard to be used in the
       | communication.
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to
       | associate a mnemonic name with the
       | request that will be used in exception
       | reports.

A KVP-encoded DescribeStoredQueries request is shown below.

.. code-block::
   
   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.2&
   REQUEST=DescribeStoredQueries&
   STOREDQUERY_ID=http%3A%2F%2Fwww.opengis.net%2Fdef%2Fquery%2FOGC-WFS%2F0%2FGetFeatureById

The supported KVP parameters are listed in the following table.

.. list-table:: Supported KVP parameters of a DescribeStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_describeStoredQuery_kvp_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | SERVICE
     - | M
     - | WFS (fixed)
     - | see above
   * - | VERSION
     - | M
     - | 2.0.x
     - | see above
   * - | STOREDQUERY_ID
     - | O
     - |
     - | A comma-separated list of stored query
       | identifiers to describe.

.. _wfs_getfeature_operation_chapter:

GetFeature operation
~~~~~~~~~~~~~~~~~~~~

The GetFeature operation lets a client query CityGML features from the
3D City Database. The *Simple WFS* conformance class only mandates WFS
server implementations to support GetFeature queries that use the
predefined stored query GetFeatureById as query expression and filter
criteria. For this reason, the current version of the 3D City Database
WFS handles GetFeatureById queries but no ad-hoc queries. The GetFeature
support will be extended in future releases.

A valid GetFeature operation is shown below. The gml:id of the city
object that shall be returned by the WFS is passed as parameter to the
GetFeatureById stored query.

.. code-block:: xml
   :name: wfs_getFeature_example_listing

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0"
    xmlns:wfs="http://www.opengis.net/wfs/2.0">
     <wfs:StoredQuery id="http://www.opengis.net/def/query/OGC-WFS/0/GetFeatureById">
       <wfs:Parameter name="id">ID_0815</wfs:Parameter>
     </wfs:StoredQuery>
   </wfs:GetFeature>

The WFS will answer the above request with either the CityGML city
object(s) whose gml:id value matches ``ID_0815`` or with an exception report
in case no matching city object was found in the 3D City Database.

A single GetFeature operation can also be used to request more than one
feature.

.. code-block:: xml
   :name: wfs_getFeature_example_request_two_objects_listing

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0"
    xmlns:wfs="http://www.opengis.net/wfs/2.0">
     <wfs:StoredQuery id="http://www.opengis.net/def/query/OGC-WFS/0/GetFeatureById">
       <wfs:Parameter name="id">first gml:id</wfs:Parameter>
     </wfs:StoredQuery>
     <wfs:StoredQuery id="http://www.opengis.net/def/query/OGC-WFS/0/GetFeatureById">
       <wfs:Parameter name="id">second gml:id</wfs:Parameter>
     </wfs:StoredQuery>
   </wfs:GetFeature>

If a GetFeature request results in more than one city objects or
consists of more than one stored query, the response will be wrapped by
one or more ``<wfs:FeatureCollection>`` elements. Please refer to the WFS
2.0 specification for details on the encoding of the response document.

The GetFeature operation can be influenced by the following XML
attributes.

.. list-table:: Supported XML attributes of a GetFeature operation. (O = optional, M = mandatory)
   :name: wfs_supported_getFeature_attributes_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the
       | service type. The value “WFS” is fixed.
   * - | version
     - | M
     - | 2.0.x
     - | The version of the WFS Interface
       | Standard to be used in the
       | communication.
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to
       | associate a mnemonic name with the
       | request that will be used in exception
       | reports.
   * - | outputFormat
     - | O
     - | application/gml+xml;
       | version=3.1
     - | Controls the encoding of the response.
       | Per default, the WFS uses CityGML /
       | GML 3.1.1. The outputFormat attribute
       | may also take the value
       | “application/json”, in which case the
       | response is encoded in CityJSON.
   * - | count
     - | O
     - | unlimited
     - | The maximum number of features to be
       | returned by the WFS service.
   * - | resultType
     - | O
     - | results
     - | If the value of the resultType parameter
       | is set to "results" the server generates a
       | response document containing features
       | that satisfy the operation. If set to “hits”
       | the server generates an empty
       | response document indicating the count
       | of the total number of features that the
       | operation would return.

A KVP-encoded GetFeature request is shown below.

.. code-block::
 
   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.2&
   REQUEST=GetFeature&
   STOREDQUERY_ID=http%3A%2F%2Fwww.opengis.net%2Fdef%2Fquery%2FOGC-WFS%2F0%2FGetFeatureById&
   ID=ID_0815

Note that the last parameter ID in the above request is not a WFS
parameter but instead is required by the invoked stored query.

The supported KVP parameters are listed in the following table.

.. list-table:: Supported KVP parameters of a GetFeature operation. (O = optional, M = mandatory)
   :name: wfs_supported_getFeature_kvp_table

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | SERVICE
     - | M
     - | WFS (fixed)
     - | see above
   * - | VERSION
     - | M
     - | 2.0.x
     - | see above
   * - | NAMESPACES
     - | O
     - |
     - | Used to specify namespaces and their
       | prefixes. The format shall be
       | xmlns(prefix,escaped_url).
   * - | OUTPUTFORMAT
     - | O
     - | application/gml+xml;
       | version=3.1
     - | see above
   * - | COUNT
     - | O
     - | unlimited
     - | see above
   * - | RESULTTYPE
     - | O
     - | results
     - | see above
   * - | STOREDQUERY_ID
     - | M
     - |
     - | The identifier of the stored query to
       | invoke.
   * - | *storedquery_parameter*
       | *=value*
     - | O
     - |
     - | Each parameter of the stored query
       | shall be encoded in KVP as key-value
       | pair.
