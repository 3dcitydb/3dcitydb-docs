.. _wfs_operations_settings_chapter:

Operations settings
~~~~~~~~~~~~~~~~~~~

The *operations* settings are used to define the WFS operations that shall
be available to clients. The *Simple WFS* conformance class mandates that every WFS
must at least support the operations *GetCapabilities*, *DescribeFeatureType*, *ListStoredQueries*,
*DescribeStoredQueries* and the stored query *GetFeatureById*. These operations therefore have
not to be explicitly listed in the ``<operations>`` element to be offered by the WFS.

.. code-block:: xml

   <operations>
     <requestEncoding>
       <method>KVP+XML</method>
       <useXMLValidation>true</useXMLValidation>
     </requestEncoding>
     <GetPropertyValue isEnabled="true"/>
     <GetFeature>
       <outputFormats>
         <outputFormat name="application/gml+xml; version=3.1"/>
         <outputFormat name="application/json"/>
       </outputFormats>
     </GetFeature>
     <ManageStoredQueries isEnabled="true"/>
   </operations>

**Request encoding**

The ``<requestEncoding>`` element determines
whether the WFS shall support
XML-encoded and/or KVP-encoded requests. The desired method is chosen
using the ``<method>`` child element that accepts the values KVP, XML
and KVP+XML (default: KVP+XML). When setting the ``<useXMLValidation>``
child element to true, all XML encoded operation requests sent to the
WFS are first validated against the WFS and CityGML XML schemas.
Requests that violate the schemas are not processed but instead a
corresponding error message is sent back to the client. Although XML
validation might take some milliseconds, it is **highly recommended** to
always set this option to true to avoid unexpected failures due to XML
issues.

**GetFeature operation**

For the ``<GetFeature>`` operation, the ``<outputFormats>`` element lets you choose the
available *output formats* that can be used in encoding the response to
the client. The value “application/gml+xml; version=3.1” is the default
and basically means that the response to a *GetFeature* operation will
be purely XML-encoded (using CityGML as encoding format with the version
specified in the *feature type* settings, cf. :numref:`wfs_feature_type_settings_chapter`). In
addition, the WFS can advertise the output format “application/json”. In
this case, the response is delivered in `CityJSON format <https://www.cityjson.org>`_. CityJSON
is a JSON-based encoding of a subset of the CityGML data model.

.. note::
   The WFS can only advertise the different output formats in the
   *capabilities* document. It is up to the client though to choose one of
   these output formats when requesting feature data from the WFS.

For CityGML, the following additional options are available.

.. list-table::  Output format options for CityGML
   :name: wfs_database_citygml_format_options_table
   :widths: 30 70

   * - | **Option**
     - | **Description**
   * - | ``prettyPrint``
     - | Formats the XML response document using additional line breaks and indentations (boolean true / false, default: false).
   * - | ``convertGlobalAppearances``
     - | Convert global appearances to local ones on-the-fly during export (boolean true / false, default: false).  Global appearances typically affect more than one city object and must therefore be placed separately from the city objects within the <wfs:additionalObjects> element of the response document. When this option is enabled, the export operation dissolves the global appearances and stores the appearance information as property of each city object to which it belongs. The <wfs:additionalObjects> element is therefore no longer required.

The CityJSON output format options are presented below.

.. list-table::  Output format options for CityJSON
   :name: wfs_database_cityjson_format_options_table
   :widths: 30 70

   * - | **Option**
     - | **Description**
   * - | ``prettyPrint``
     - | Formats the JSON response document using additional line breaks and indentations (boolean true / false, default: false).
   * - | ``significantDigits``
     - | Maximum number of digits for vertices (integer, default: 3). Identical vertices are snapped.
   * - | ``significantTextureDigits``
     - | Maximum number of digits for texture coordinates (integer, default: 7). Identical texture coordinates are snapped.
   * - | ``transformVertices``
     - | Apply the CityJSON-specific compression (boolean true / false, default: false).
   * - | ``addSequenceIdWhenSorting``
     - | If the response document shall be sorted (by using a *fes:SortBy* expression), then this option allows for adding a sequenceId attribute to each CityJSON object that maps the sorting order. This is required because CityJSON itself does not support sorting (boolean true / false, default: false).
   * - | ``generateCityGMLMetadata``
     - | Adds an attribute called CityGMLMetadata that contains CityGML-specific metadata like the data types of generic attributes (boolean true / false, default: true).
   * - | ``removeDuplicateChildGeometries``
     - | CityJSON does not support reusing or referencing geometries. If geometries are reused within a city object in the database (e.g. between a Building and its BuildingInstallation child), they will be duplicate in the CityJSON output. Use this option to remove duplicate geometries from child objects. If the child object does not have any remaining geometry after removing the duplicates, it will be removed as well (boolean true / false, default: false).

The options are simply added beneath the corresponding ``<outputFormat>``
element and are applied to all response documents of the WFS in
that format. The following snippet illustrates the use of the CityJSON
format options.

.. code-block:: xml

   <outputFormat name="application/json">
     <options>
       <option name="prettyPrint">true</option>
       <option name="significantDigits">5</option>
       <option name="significantTextureDigits">5</option>
       <option name="transformVertices">true</option>
       <option name="addSequenceIdWhenSorting">true</option>
       <option name="generateCityGMLMetadata">true</option>
       <option name="removeDuplicateChildGeometries">true</option>
     </options>
   </outputFormat>

**GetPropertyValue operation**

Per default, the *GetPropertyValue* operation is not offered by the WFS service.
In order to make this operation available to clients, the *isEnabled* attribute
of the ``<GetPropertyValue>`` element has to be set to true (default: false).

**DescribeFeatureType operation**

The ``<DescribeFeatureType>`` operation lets you define the list of supported ``<outputFormats>``
similar to the *GetFeature* operation. This way you can enable clients to choose between
the CityGML XML schemas or the CityJSON JSON schema for describing feature types.

**Manage Stored Queries**

To advertise the operations *CreateStoredQuery* and *DropStoredQuery* for the server-side
management of stored queries, the element ``<ManageStoredQueries>`` has to be included and its
attribute *isEnabled* has to be set to true (default: false).