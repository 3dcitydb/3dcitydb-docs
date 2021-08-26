.. _wfs_getfeature_operation_chapter:

GetFeature
~~~~~~~~~~

The GetFeature operation returns a selection of CityGML features from the 3DCityDB that
satisfy the query expression provided by the client. The query expression can restrict
the feature instances that shall be presented in the response document through logical,
comparison and spatial filter predicates. Moreover, the feature properties to be included
in the response document can be explicitly enumerated and thus restricted.

A simple GetFeature using the predefined *GetFeatureById* stored query is shown below.
The gml:id of the city object that shall be returned by the WFS is passed as id parameter.

.. code-block:: xml

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

If a GetFeature request results in more than one city objects or
consists of more than one (stored) query expression, the response will be wrapped by
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
     - | The service attribute indicates the service type. The value “WFS” is fixed.
   * - | version
     - | M
     - | 2.0.x
     - | The version of the WFS Interface Standard to be used in the communication.
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to associate a mnemonic name with the request that will be used in exception reports.
   * - | outputFormat
     - | O
     - | application/gml+xml;
       | version=3.1
     - | Controls the encoding of the response. Per default, the WFS uses CityGML / GML 3.1.1. The outputFormat attribute may also take the value “application/json”, in which case the response is encoded in CityJSON.
   * - | count
     - | O
     - | unlimited
     - | The maximum number of features to be returned by the WFS service.
   * - | startIndex
     - | O
     - | 0
     - | The startIndex parameter indicates the index within the result set from which the server shall begin presenting results in the response document.
       | The first index is 0.
   * - | resultType
     - | O
     - | results
     - | If the value of the resultType parameter is set to "results" the server generates a response document containing features that satisfy the operation.
       | If set to “hits” the server generates an empty response document indicating the count of the total number of features that the operation would return.

The following query illustrates how to fetch all city objects of a specific feature type
from the 3D City Database.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0"
     xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:fes="http://www.opengis.net/fes/2.0"
     xmlns:bldg="http://www.opengis.net/citygml/building/2.0">
     <wfs:Query typeNames="bldg:Building"/>
   </wfs:GetFeature>

The requested feature type is given as value of the *typeNames* attribute on the
``<wfs:Query>`` element. Make sure to declare and use the correct CityGML namespaces
(see :numref:`wfs_feature_types_chapter`). The above query would return all *Building* instances
from the database encoded in CityGML 2.0. Of course, the WFS has to advertise the feature type and
CityGML version (see :numref:`wfs_feature_type_settings_chapter`). To get the number of all buildings
in the database (without actually fetching the buildings themselves), the *resultType* attribute
has to be set to *hits*.

The *count* attribute lets you restrict the number of returned instances. And *startIndex*
can be used to indicate that the response document shall begin with the *n*-th object from
the result set (the first object is at index 0). Thus, both parameters allow accessing a
specific subset of the objects that fulfill the request. If result paging is enabled for the WFS
(see :numref:`wfs_constraints_settings_chapter`) and at least the *count* parameter is set,
then the client can also use the *previous* and *next* URLs to scroll through the result set.

If you want to query both *Building* and *CityFurniture* instances with a single GetFeature request,
you simply have to use two ``<wfs:Query>`` child elements. The WFS also supports to fetch all instances
of a given feature type and all its (transitive) subtypes using the ``schema-element()`` function.
For instance, ``typeNames="schema-element(core:_CityObject)"`` will return all city object instances.

The ``<wfs:Query>`` element allows the following XML attributes.

.. list-table:: Supported XML attributes of the *wfs:Query* element. (O = optional, M = mandatory)
   :name: wfs_supported_getFeature_attributes_table
   :widths: 20 15 20 50

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | typeNames
     - | M
     - |
     - | The typeNames attribute defines the feature type of the instances to be returned
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to associate a mnemonic name with the request that will be used in exception reports.
   * - | srsName
     - | O
     - | same as in database
     - | If the srsName attribute is provided, a coordinate transformation into the provided SRS is applied to the feature instances in the result set.

In general, the WFS returns all instances of the requested feature type including all mandatory
and optional thematic and spatial properties. In order to apply a projection to the properties
of the feature type, simply enumerate the properties to be fetched using one or more
``<wfs:PropertyName>`` element. If you want to restrict the instances to be returned according
to thematic or spatial criteria, you can provide a ``<fes:Filter>`` expression that denotes how
city objects should be filtered to produce the result set.

In addition to XML-encoded GetFeature requests, this WFS also supports KVP-encoded requests.
Using KVP encoding, one or more ad-hoc queries or precisely one stored query can be expressed.
Also complex filter expressions can be encoded. The following example shows a simple GetFeature
request to query all Building objects within a given bounding box. Note that the values of the
parameters ``NAMESPACES`` and ``SRSNAME`` would have to be URL encoded in this example for the
query to work. This has been omitted for the sake of clarity though.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.2&
   REQUEST=GetFeature&
   NAMESPACES=xmlns(bldg,http://www.opengis.net/citygml/building/2.0)&
   TYPENAMES=bldg:Building&
   BBOX=18.54,-72.3544,18.62,-72.2564&
   SRSNAME=http://www.opengis.net/def/crs/epsg/0/4326

There are many parameters available for the KVP encoding of the GetFeature operation. Some of them
are mutually exclusive or depend on each other. Please refer to the WFS specification (OGC Doc. No. 09-025r2)
for an overview of all parameter dependencies.

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
     - | Used to specify namespaces and their prefixes. The format shall be xmlns(prefix,escaped_url).
   * - | COUNT
     - | O
     - | unlimited
     - | see above
   * - | STARTINDEX
     - | O
     - | 0
     - | see above
   * - | OUTPUTFORMAT
     - | O
     - | application/gml+xml;
       | version=3.1
     - | see above
   * - | RESULTTYPE
     - | O
     - | results
     - | see above

.. list-table:: Additional KVP parameters for ad-hoc queries only. (O = optional, M = mandatory)
   :name: wfs_supported_getFeature_kvp_adhoc_table
   :widths: 20 15 20 50

   * - | **KVP parameter**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | TYPENAMES
     - | M
     - |
     - | see above
   * - | SRSNAME
     - | O
     - | same as in database
     - | see above
   * - | PROPERTYNAME
     - | O
     - |
     - | List of properties (encoded as as QName) that shall be included in the response (projection).
   * - | FILTER
     - | O
     - |
     - | Filter expression encoded using the language specified by FILTER_LANGUAGE.
   * - | FILTER_LANGUAGE
     - | O
     - | urn:ogc:def:
       | queryLanguage:
       | OGC-FES:Filter
     - | Filter language (default: XML encoding according to OGC FES specification).

.. list-table:: Additional KVP parameters for stored queries only. (O = optional, M = mandatory)
   :name: wfs_supported_getFeature_kvp_stored_table
   :widths: 20 15 20 50

   * - | **KVP parameter**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | STOREDQUERY_ID
     - | M
     - |
     - | The identifier of the stored query to invoke.
   * - | *storedquery_parameter*
       | *=value*
     - | O
     - |
     - | Each parameter of the stored query shall be encoded in KVP as key-value pair.

Ensure to use proper XML namespaces for feature type names, property names and XML-encoded filter expressions.
XML namespaces and their prefixes can be specified using the ``NAMESPACES`` attribute. However, the WFS can correctly
deal with the default CityGML prefixes. An additional definition via the ``NAMESPACES`` attribute is therefore obsolete
when using the default prefixes.

**Example 1**

This example fetches a subset of properties of the *Building* feature type. The specific building instances that
are retrieved by the request are identified through a *ResourceId* filter. In addition, the response shall be sorted
by the *bldg:measuredHeight* property of the matching buildings. The output format is set to CityJSON.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0" outputFormat="application/json"
     xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:fes="http://www.opengis.net/fes/2.0"
     xmlns:core="http://www.opengis.net/citygml/2.0" xmlns:bldg="http://www.opengis.net/citygml/building/2.0">
     <wfs:Query typeNames="bldg:Building">
       <wfs:PropertyName>core:externalReference</wfs:PropertyName>
       <wfs:PropertyName>bldg:class</wfs:PropertyName>
       <wfs:PropertyName>bldg:address</wfs:PropertyName>
       <wfs:PropertyName>bldg:lod2Solid</wfs:PropertyName>
       <fes:Filter>
         <fes:ResourceId rid="ID_0815"/>
         <fes:ResourceId rid="ID_0816"/>
         <fes:ResourceId rid="ID_0817"/>
       </fes:Filter>
       <fes:SortBy>
         <fes:SortProperty>
           <fes:ValueReference>bldg:measuredHeight</fes:ValueReference>
         </fes:SortProperty>
       </fes:SortBy>
     </wfs:Query>
   </wfs:GetFeature>

The equivalent KVP-encoding is shown below.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.0&
   REQUEST=GetFeature&
   TYPENAMES=bldg:Building&
   PROPERTYNAME=core:externalReference,bldg:class,bldg:address,bldg:lod2Solid&
   RESOURCEID=ID_0815,ID_0816,ID_0817&
   SORTBY=bldg:measuredHeight&
   OUTPUTFORMAT=application%2Fjson

**Example 2**

In this example, all road objects carrying a generic integer attribute of name *lanes* are fetched. An XPath
expression is required to reference the *gen:name* attribute of the complex property *gen:intAttribute*.
Note that the *matchCase* attribute of *PropertyIsEqualTo* is set to false in order to operate case insensitive.
The *count* attribute ensures that at most 10 roads are contained in the response document. If response paging
is enabled for your WFS (see :numref:`wfs_constraints_settings_chapter`), the response will contain *next* and
*previous* links that allow a client to browse through the entire result set.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0" count="10" xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:gen="http://www.opengis.net/citygml/generics/1.0"
     xmlns:tran="http://www.opengis.net/citygml/transportation/1.0">
     <wfs:Query typeNames="tran:Road">
       <fes:Filter>
         <fes:PropertyIsEqualTo matchCase="false">
           <fes:ValueReference>gen:intAttribute/@gen:name</fes:ValueReference>
           <fes:Literal>lanes</fes:Literal>
         </fes:PropertyIsEqualTo>
          </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

**Example 3**

This example extends the previous one by fetching all road objects whose generic lanes attributes is greater than two.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0" count="10" xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:gen="http://www.opengis.net/citygml/generics/1.0"
     xmlns:tran="http://www.opengis.net/citygml/transportation/1.0">
     <wfs:Query typeNames="tran:Road">
       <fes:Filter>
         <fes:PropertyIsGreaterThan>
           <fes:ValueReference>gen:intAttribute[@gen:name='lanes']/gen:value</fes:ValueReference>
           <fes:Literal>2</fes:Literal>
         </fes:PropertyIsGreaterThan>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

The equivalent KVP-encoding is shown below. Again, note that the filter expression has to be URL encoded.
Otherwise the XML notation will not be correctly transported to the server. This has been omitted for the
sake of clarity.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.0&
   REQUEST=GetFeature&
   TYPENAMES=tran:Road&
   COUNT=10&
   FILTER=<fes:Filter>
          <fes:PropertyIsGreaterThan>
          <fes:ValueReference>gen:intAttribute[@gen:name='lanes']/gen:value</fes:ValueReference>
          <fes:Literal>2</fes:Literal>
          </fes:PropertyIsGreaterThan>
          </fes:Filter>

The URL encoding of the above filter expression is given in the following.

.. code-block:: bash

   FILTER=%3Cfes%3AFilter%3E%3Cfes%3APropertyIsGreaterThan%3E%3Cfes%3AValueReference%3Egen%3AintAttribute%5B%40gen%3A
   name%3D%27lanes%27%5D%2Fgen%3Avalue%3C%2Ffes%3AValueReference%3E%3Cfes%3ALiteral%3E2%3C%2Ffes%3ALiteral%3E%3C%2Ffes%3A
   PropertyIsGreaterThan%3E%3C%2Ffes%3AFilter%3E

**Example 4**

In this example, buildings are fetched if they contain a nested *RoofSurface* feature with a photovoltaic suitability
class between 1 and 3. The *pv_class* attribute is modeled as generic integer attribute of the roof surface features.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0" xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:gen="http://www.opengis.net/citygml/generics/1.0"
     xmlns:bldg="http://www.opengis.net/citygml/building/1.0">
     <wfs:Query typeNames="bldg:Building">
       <fes:Filter>
         <fes:PropertyIsBetween>
           <fes:ValueReference>bldg:boundedBy/bldg:RoofSurface/gen:intAttribute[@gen:name='pv_class']/gen:value</fes:ValueReference>
           <fes:LowerBoundary>
             <fes:Literal>1</fes:Literal>
           </fes:LowerBoundary>
           <fes:UpperBoundary>
             <fes:Literal>3</fes:Literal>
           </fes:UpperBoundary>
         </fes:PropertyIsBetween>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

**Example 5**

This example returns the number (``resultType="hits"``) of buildings in *Berlin* along the road *Unter den Linden*.
Note that the address is not queried using elements of the xAL address language since xAL is way too flexible.
Instead, an ADE has been defined to make address queries as simple as possible (read more
in :numref:`impexp_xml_query_address_metadata`).

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0" resultType="hits"
     xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:fes="http://www.opengis.net/fes/2.0"
     xmlns:core="http://www.opengis.net/citygml/1.0"
     xmlns:bldg="http://www.opengis.net/citygml/building/1.0"
     xmlns:citydb="http://www.3dcitydb.org/citygml-ade/3.0/citygml/1.0">
     <wfs:Query typeNames="bldg:Building">
       <fes:Filter>
         <fes:And>
           <fes:PropertyIsEqualTo>
             <fes:ValueReference>bldg:address/core:Address/citydb:city</fes:ValueReference>
             <fes:Literal>Berlin</fes:Literal>
           </fes:PropertyIsEqualTo>
           <fes:PropertyIsLike wildCard="*" singleChar="." escapeChar="/">
             <fes:ValueReference>bldg:address/core:Address/citydb:street</fes:ValueReference>
             <fes:Literal>Unter den Linden*</fes:Literal>
           </fes:PropertyIsLike>
         </fes:And>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

**Example 6**

This query fetches all city objects whose *gml:Envelope* geometry stored in the *gml:boundedBy*
property *Intersects* a given polygon. Note the usage of the ``schema-element()`` function. It allows you to
request instances of the feature type provided as argument and of all its (transitive) subtypes. The
reference system of the query geometry is denoted by the *srsName* attribute on the ``<gml:Polygon>`` element.
If you omit the *srsName* attribute, the reference system of the 3D City Database is assumed.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature xmlns:wfs="http://www.opengis.net/wfs/2.0" service="WFS" version="2.0.0"
     outputFormat="application/gml+xml; version=3.1" xmlns:gml="http://www.opengis.net/gml"
     xmlns:core="http://www.opengis.net/citygml/2.0" xmlns:fes="http://www.opengis.net/fes/2.0">
     <wfs:Query typeNames="schema-element(core:_CityObject)">
       <fes:Filter>
         <fes:Intersects>
           <fes:ValueReference>gml:boundedBy</fes:ValueReference>
           <gml:Polygon srsName="http://www.opengis.net/def/crs/epsg/0/4326">
             <gml:exterior>
               <gml:LinearRing>
                 <gml:posList>13.3077157 52.5101551 13.3095932 52.5101551 13.3095932 52.511115
                   13.3077157 52.511115 13.3077157 52.5101551</gml:posList>
               </gml:LinearRing>
             </gml:exterior>
           </gml:Polygon>
         </fes:Intersects>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

**Example 7**

This example illustrates two queries that fetch *Building* and *SolitaryVegetationObject* instances that are
within a 500m distance from a given point location.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetFeature service="WFS" version="2.0.0" xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:gml="http://www.opengis.net/gml" xmlns:fes="http://www.opengis.net/fes/2.0"
     xmlns:bldg="http://www.opengis.net/citygml/building/2.0"
     xmlns:veg="http://www.opengis.net/citygml/vegetation/2.0">
     <wfs:Query typeNames="bldg:Building" handle="q01">
       <fes:Filter>
         <fes:DWithin>
           <fes:ValueReference>gml:boundedBy</fes:ValueReference>
           <gml:Point srsName="http://www.opengis.net/def/crs/epsg/0/4326">
             <gml:pos>13.3068144 52.5096392</gml:pos>
           </gml:Point>
           <fes:Distance uom="m">500</fes:Distance>
         </fes:DWithin>
       </fes:Filter>
     </wfs:Query>
     <wfs:Query typeNames="veg:SolitaryVegetationObject" handle="q02">
       <fes:Filter>
         <fes:DWithin>
           <fes:ValueReference>gml:boundedBy</fes:ValueReference>
           <gml:Point srsName="http://www.opengis.net/def/crs/epsg/0/4326">
             <gml:pos>13.3068144 52.5096392</gml:pos>
           </gml:Point>
           <fes:Distance uom="km">0.5</fes:Distance>
         </fes:DWithin>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

The two ad-hoc queries can also be expressed as KVP-encoded request. The parameter values for each query
have to be delimited using parentheses.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.0&
   REQUEST=GetFeature&
   TYPENAMES=(bldg:Building)(veg:SolitaryVegetationObject)&
   FILTER=(<fes:Filter>
           <fes:DWithin>
           <fes:ValueReference>gml:boundedBy</fes:ValueReference>
           <gml:Point srsName="http://www.opengis.net/def/crs/epsg/0/4326">
           <gml:pos>13.3068144 52.5096392</gml:pos>
           </gml:Point>
           <fes:Distance uom="m">500</fes:Distance>
           </fes:DWithin>
           </fes:Filter>)
          (<fes:Filter>
           <fes:DWithin>
           <fes:ValueReference>gml:boundedBy</fes:ValueReference>
           <gml:Point srsName="http://www.opengis.net/def/crs/epsg/0/4326">
           <gml:pos>13.3068144 52.5096392</gml:pos>
           </gml:Point>
           <fes:Distance uom="km">0.5</fes:Distance>
           </fes:DWithin>
           </fes:Filter>)

**Example 8**

The listing below exemplifies the use of result paging. It shows an excerpt of the response document to a
*GetFeature* operation requesting buildings from the 3DCityDB. Because the request uses the parameter
``count=10``, only the first 10 of the 312 matching buildings are returned. The client has to invoke the *next*
URL to retrieve the next 10 buildings.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:FeatureCollection xmlns:xAL="urn:oasis:names:tc:ciq:xsdschema:xAL:2.0" xmlns:gml="http://www.opengis.net/gml"
     xmlns:bldg="http://www.opengis.net/citygml/building/2.0" xmlns:wfs="http://www.opengis.net/wfs/2.0"
     timeStamp="2020-03-11T10:12:42" numberMatched="312" numberReturned="10"
     next="http://some.url.com/citydb-wfs/wfs?pageId=58a5bffa8c416be7-48d3fc698001d622-107643930c40bdc4">
     <wfs:member>
       <bldg:Building gml:id="BLDG_0003000a000afdae">
         ...
       </bldg:Building>
     </wfs:member>
   </wfs:GetFeature>