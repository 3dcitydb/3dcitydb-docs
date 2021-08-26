.. _wfs_describefeaturetype_operation_chapter:

DescribeFeatureType
~~~~~~~~~~~~~~~~~~~

The DescribeFeatureType operation returns a schema
description of the feature types advertised by the 3D City
Database WFS instance. Which feature types are offered by the WFS is
controlled through the ``config.xml`` settings file (cf. :numref:`wfs_feature_types_chapter`).
The schema defines the structure and content of the features
(thematic and spatial attributes, nested features, etc.) as well as the
way how features are encoded in responses to GetFeature requests.

The following example shows a valid DescribeFeatureType operation
requesting the XML Schema definition of the CityGML 1.0 *Building*
feature type.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:DescribeFeatureType service="WFS" version="2.0.0"
    xmlns:wfs="http://www.opengis.net/wfs/2.0"
    xmlns:bldg="http://www.opengis.net/citygml/building/1.0">
     <wfs:TypeName>bldg:Building</wfs:TypeName>
   </wfs:DescribeFeatureType>

The DescribeFeatureType operations takes the following XML attributes.

.. list-table:: Supported XML attributes of a DescribeFeatureType operation. (O = optional, M = mandatory)
   :name: wfs_supported_describeFeatureType_attributes_table
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
with the correct CityGML XML namespace (see :numref:`wfs_feature_types_chapter`).
The ``<wfs:TypeName>`` element may
occur multiple times to request schema definitions of several feature
types in a single DescribeFeatureType operation. If the ``<wfs:TypeName>``
element is omitted, then the complete base schema is returned by the WFS.

The DescribeFeatureType operation can alternatively be invoked through
HTTP GET with key-value pairs.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.2&
   REQUEST=DescribeFeatureType&
   TYPENAME=bldg:Building,tran:Road

The following KVP parameters are supported.

.. list-table:: Supported KVP parameters of a DescribeFeatureType operation. (O = optional, M = mandatory)
   :name: wfs_supported_describeFeatureType_kvp_table
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
   * - | TYPENAME
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