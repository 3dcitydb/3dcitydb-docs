.. _wfs_basic_functionality_chapter:

Basic functionality
~~~~~~~~~~~~~~~~~~~

WFS operations
^^^^^^^^^^^^^^

The OGC WFS 2.0 interface defines eleven operations that can be invoked
by a client. A WFS server is not required to offer all operations to
conform to the standard but may support a subset only. The following table
lists all WFS 2.0 operations and marks those supported by the 3D City Database WFS.

.. list-table::  Overview of supported WFS 2.0 operations
   :name: wfs_supported_operation_overview_table
   :widths: 20 70 10

   * - | **Operation**
     - | **Description**
     - | **Supported**
   * - | **GetCapabilities**
     - | The GetCapabilities operation generates a service metadata document describing the WFS service provided by a server.
     - | X
   * - | **DescribeFeatureType**
     - | The DescribeFeatureType operation returns a schema description of the CityGML feature types offered by the WFS instance.
     - | X
   * - | **GetFeature**
     - | The GetFeature operation returns a selection of CityGML features from the 3D City Database using a query expression.
     - | X
   * - | **GetPropertyValue**
     - | The GetPropertyValue operation allows the value of a feature property or part of the value of a complex feature property to be retrieved from the 3D City Database for a set of features identified using a query expression.
     - | X
   * - | **ListStoredQueries**
     - | The ListStoredQueries operation lists the stored queries available at the server.
     - | X
   * - | **DescribeStoredQuery**
     - | The DescribeStoredQueries operation provides detailed metadata about each stored query expression that the server offers.
     - | X
   * - | **CreateStoredQuery**
     - | A stored query may be created using the CreateStoredQuery operation.
     - | X
   * - | **DropStoredQuery**
     - | The DropStoredQuery operation allows previously created stored queries to be dropped from the system.
     - | X
   * - | Transaction
     - | The Transaction operation is used to describe data transformation operations (i.e., insert, update, replace, delete) to be applied to CityGML feature instances under the control of the web feature service.
     - | --
   * - | LockFeature
     - | The LockFeature operation is used to expose a long-term feature locking mechanism to ensure consistency in data manipulation operations (e.g., update or delete).
     - | --
   * - | GetFeatureWithLock
     - | The GetFeatureWithLock operation is functionally similar to the GetFeature operation except that in response to a GetFeatureWithLock operation, the WFS shall also lock the features in the result set.
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
for more information. The last component ``wfs`` of the URL identifies the
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
type) or KVP-encoded (key-value-pair) using the HTTP method GET. Use the
``config.xml`` to determine which method the WFS server should support
(see :numref:`wfs_operations_settings_chapter`).

.. note::
   The WFS specification does not define a KVP encoding for all operations.
   These operations must therefore be XML-encoded and sent to the server through *HTTP POST*.
   Also note that the XML content of POST messages sent to the server must be well-formed and
   valid with respect to the `WFS 2.0 XML Schema <http://schemas.opengis.net/wfs/2.0/wfs.xsd>`_

The following table summarizes the operations and the supported service
binding as offered by the 3D City Database WFS.

.. list-table::  Service bindings for the supported WFS 2.0 operations.
   :name: wfs_service_bindings_operations_table
   :widths: 20 80

   * - | **Operation**
     - | **Service Binding**
   * - | GetCapabilities
     - | XML over HTTP POST and KVP over HTTP GET
   * - | DescribeFeatureType
     - | XML over HTTP POST and KVP over HTTP GET
   * - | GetFeature
     - | XML over HTTP POST and KVP over HTTP GET
   * - | GetPropertyValue
     - | XML over HTTP POST and KVP over HTTP GET
   * - | ListStoredQueries
     - | XML over HTTP POST and KVP over HTTP GET
   * - | DescribeStoredQuery
     - | XML over HTTP POST and KVP over HTTP GET
   * - | CreateStoredQuery
     - | XML over HTTP POST
   * - | DropStoredQuery
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