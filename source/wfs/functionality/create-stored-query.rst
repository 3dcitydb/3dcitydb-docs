.. _wfs_createstoredquery_operation_chapter:

CreateStoredQuery
~~~~~~~~~~~~~~~~~

The CreateStoredQuery enables you to define and create your own server-side stored queries. Once the stored
query has been created, it is immediately available for use in requests (e.g. GetFeature oder GetPropertyValue
operations) and can be queried through the ListStoredQueries und DescribeStoredQueries operations.

The following listing shows a simple example to create a stored query for requesting *Bridge* features
using a polygon-based spatial filter.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:CreateStoredQuery service="WFS" version="2.0.0"
     xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:fes="http://www.opengis.net/fes/2.0"
     xmlns:gml="http://www.opengis.net/gml"  xmlns:brid="http://www.opengis.net/citygml/bridge/2.0">
     <wfs:StoredQueryDefinition id="urn:StoredQueries:BridgesInPolygon">
       <wfs:Title>Bridges In Polygon</wfs:Title>
       <wfs:Abstract>Find all the bridges in a polygon.</wfs:Abstract>
       <wfs:Parameter name="AreaOfInterest" type="gml:Polygon"/>
       <wfs:QueryExpressionText
         returnFeatureTypes="brid:Bridge"
         language="urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression"
         isPrivate="false">
         <wfs:Query typeNames="brid:Bridge">
           <fes:Filter>
             <fes:Within>
               <fes:ValueReference>bldg:boundedBy</fes:ValueReference>
               ${AreaOfInterest}
             </fes:Within>
           </fes:Filter>
         </wfs:Query>
       </wfs:QueryExpressionText>
     </wfs:StoredQueryDefinition>
   </wfs:CreateStoredQuery>

The details of the stored query are defined within the ``<wfs:StoredQueryDefinition>`` element.
The mandatory *id* attribute provides the identifier of the stored query, which is later used to invoke the query
(cf. :numref:`wfs_getfeature_operation_chapter`). The WFS server ensures that the identifier is unique and not in
use by any other stored query. The metadata attributes ``<wfs:Title>`` and ``<wfs:Abstract>`` allow
for providing a human-readable description of the stored query. The list of parameters that have to be passed
by a client when invoking the stored query is determined by one or more ``<wfs:Parameter>`` elements.
The *name* attribute on a ``<wfs:Parameter>`` element defines the parameter name, whereas the *type* attribute
defines its data type.

The query definition follows in the ``<wfs:QueryExpressionText>`` element. The *returnFeatureTypes* attribute
must contain the qualified XML name of the CityGML feature type that is returned by the stored query. Note
that this attribute may as well contain a list of feature type names or even be empty in case the response may
comprise all feature types that are advertised by the WFS service (like for the predefined stored query
GetFeatureById). The language used to define the query expression is restricted to
``urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression`` for this version of the WFS. The *isPrivate* attribute
lets you decide whether the actual query expression should be visible to clients in a response to a
DescribeStoredQueries request.

The actual query expression is provided by one or more ``<wfs:Query>`` elements. The parameters defined
within the ``<wfs:StoredQueryDefinition>`` element are referenced in this query expression using the
notation *${parameter_name}*. For more details on the CreateStoredQuery operation, please refer to the
official WFS specification document.

The CreateStoredQuery operation may take the following XML attributes.

.. list-table:: Supported XML attributes of a CreateStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_createStoredQuery_attributes_table
   :widths: 20 15 20 50

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the service type. The value “WFS” is fixed.
   * - | version
     - | M
     - | 2.0.x
     - | The version of the WFS Interface Standard to be used in the communication.
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to associate a mnemonic name with the request that will be used in exception reports.

.. note::
   The WFS specification does not define a KVP encoding for the CreateStoredQuery operation.