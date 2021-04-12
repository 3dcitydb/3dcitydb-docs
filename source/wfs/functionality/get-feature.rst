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
   :widths: 20 15 20 50

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

.. code-block:: bash

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
   :widths: 20 15 20 50

   * - | **KVP parameter**
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