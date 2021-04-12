.. _wfs_describestoredquery_operation_chapter:

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

A KVP-encoded DescribeStoredQueries request is shown below.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.2&
   REQUEST=DescribeStoredQueries&
   STOREDQUERY_ID=http%3A%2F%2Fwww.opengis.net%2Fdef%2Fquery%2FOGC-WFS%2F0%2FGetFeatureById

The supported KVP parameters are listed in the following table.

.. list-table:: Supported KVP parameters of a DescribeStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_describeStoredQuery_kvp_table
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
   * - | STOREDQUERY_ID
     - | O
     - |
     - | A comma-separated list of stored query
       | identifiers to describe.