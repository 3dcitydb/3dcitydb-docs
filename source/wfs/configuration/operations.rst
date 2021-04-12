.. _wfs_operations_settings_chapter:

Operations settings
~~~~~~~~~~~~~~~~~~~

The *operations* settings are used to define the operation-specific
behavior of the WFS.

.. code-block:: xml
   :name: wfs_operation_settings_config_listing

   <operations>
     <requestEncoding>
       <method>KVP+XML</method>
       <useXMLValidation>true</useXMLValidation>
     </requestEncoding>
     <GetFeature>
       <outputFormats>
         <outputFormat name="application/gml+xml; version=3.1"/>
         <outputFormat name="application/json"/>
       </outputFormats>
     </GetFeature>
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

With this version of the WFS interface, the
only operation that can be
further configured is the ``<GetFeature>`` operation. You can choose the
available *output formats* that can be used in encoding the response to
the client. The value “application/gml+xml; version=3.1” is the default
and basically means that the response to a *GetFeature* operation will
be purely XML-encoded (using CityGML as encoding format with the version
specified in the *feature type* settings, cf. :numref:`wfs_feature_type_settings_chapter`). In
addition, the WFS can advertise the output format “application/json”. In
this case, the response is delivered in `CityJSON format <http://www.cityjson.org>`_. CityJSON
is a JSON-based encoding of a subset of the CityGML data model.

.. note::
   The WFS can only advertise the different output formats in the
   *capabilities* document. It is up to the client though to choose one of
   these output formats when requesting feature data from the WFS.

For CityGML, the following additional options are available.

.. list-table::  Output format options for CityGML.
   :name: wfs_database_citygml_format_options_table
   :widths: 30 70

   * - | **Option**
     - | **Description**
   * - | ``prettyPrint``
     - | Formats the XML response document using additional line breaks and indentations (boolean true / false, default: false).

The CityJSON output format options are presented below.

.. list-table::  Output format options for CityJSON.
   :name: wfs_database_cityjson_format_options_table
   :widths: 30 70

   * - | **Option**
     - | **Description**
   * - | ``prettyPrint``
     - | Formats the JSON response document using additional line breaks and indentations (boolean true / false, default: false).
   * - | ``significantDigits``
     - | Maximum number of digits for vertices (integer, default: 3). Identical vertices are snapped.
   * - | ``transformVertices``
     - | Apply the CityJSON-specific compression (boolean true / false, default: false).
   * - | ``generateCityGMLMetadata``
     - | Adds an attribute called CityGMLMetadata that contains CityGML-specific metadata like the data types of generic attributes (boolean true / false, default: true).

The options are simply added beneath the corresponding ``<outputFormat>``
element and are applied to all response documents of the WFS in
that format. The following snippet illustrates the use of the CityJSON
format options.

.. code-block:: xml
   :name: wfs_format_options_listing

   <outputFormat name="application/json">
     <options>
       <option name="prettyPrint">true</option>
       <option name="significantDigits">5</option>
       <option name="transformVertices">true</option>
       <option name="generateCityGMLMetadata">true</option>
     </options>
   </outputFormat>