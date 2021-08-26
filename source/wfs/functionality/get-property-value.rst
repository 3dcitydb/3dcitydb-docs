.. _wfs_getpropertyvalue_operation_chapter:

GetPropertyValue
~~~~~~~~~~~~~~~~

The GetPropertyValue operation enables a client to query the value of a property of one or more feature instances
in the 3D City Database. Similar to the GetFeature operation, the set of feature instances can be restricted using
filter expressions.

The example below shows a simple GetPropertyValue request.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetPropertyValue service="WFS" version="2.0.0" xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:bldg="http://www.opengis.net/citygml/building/2.0"
     valueReference="bldg:measuredHeight">
     <wfs:Query typeNames="bldg:Building">
       <fes:Filter>
         <fes:ResourceId rid="ID_0815"/>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetPropertyValue>

The name of the property whose value shall be presented to the client is passed to the GetPropertyValue operation
via the *valueReference* attribute. The *valueReference* uses an XPath expression to identify the feature property.
Therefore, also a part of the value of a complex feature property or even properties of nested features can be
requested. Like with the GetFeature operation, the requested feature type is given as value of the *typeNames*
attribute on the ``<wfs:Query>`` element. The client has to ensure to declare and use the correct CityGML namespaces
(see :numref:`wfs_feature_types_chapter`).

As response to the above request, the WFS will send a ``<wfs:ValueCollection>`` document containing the value of
the *bldg:measuredHeight* property of the building with gml:id *ID_0815*.

In addition to the *valueReference* attribute, the GetPropertyValue operation may take the same XML attributes
as the GetFeature operation as discussed in :numref:`wfs_getfeature_operation_chapter`. Please also refer to
this chapter for the encoding of ad-hoc queries (``<wfs:Query>``) and stored queries (``<wfs:StoredQuery>``)
within a GetPropertyValue operation.

.. list-table:: Supported XML attributes of a GetPropertyValue operation. (O = optional, M = mandatory)
   :name: wfs_supported_getPropertyValue_attributes_table
   :widths: 20 15 20 50

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | valueReference
     - | M
     - |
     - | XPath expression identifying the feature property to be requested.

The GetPropertyValue operation is also available as KVP-encoded request. The following snippet illustrates
the KVP encoding of the above request.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.0&
   REQUEST=GetPropertyValue&
   TYPENAMES=bldg:Building&
   VALUEREFERENCE=bldg:measuredHeight&
   RESOURCEID=ID_0815

The available KVP parameters are identical to those of a GetFeature request (cf. :numref:`wfs_feature_types_chapter`).
The *VALUEREFERENCE* has to be provided in addition.

.. list-table:: Supported KVP parameters of a GetFeature operation. (O = optional, M = mandatory)
   :name: wfs_supported_getPropertyValue_kvp_table
   :widths: 20 15 20 50

   * - | **KVP parameter**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | VALUEREFERENCE
     - | M
     - |
     - | see above

Ensure to use proper XML namespaces for feature type names, property names and XML-encoded filter expressions.
XML namespaces and their prefixes can be specified using the NAMESPACES attribute. However, the 3DCityDB
WFS can correctly deal with the default CityGML prefixes. An additional definition via the NAMESPACES attribute
is therefore obsolete when using the default prefixes.

**Example 1**

This example shows the usage of the GetPropertyValue operation to query the names of all *gen:stringAttribute*
properties associated with a specific *Building* object. The *Building* is identified by through a
*ResourceId* filter.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetPropertyValue service="WFS" version="2.0.0" xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:bldg="http://www.opengis.net/citygml/building/2.0"
     xmlns:gen="http://www.opengis.net/citygml/generics/2.0"
     valueReference="gen:stringAttribute/@gen:name">
     <wfs:Query typeNames="bldg:Building">
       <fes:Filter>
         <fes:ResourceId rid="ID_0815"/>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetPropertyValue>

**Example 2**

The XPath expression of the *valueReference* may also point to the property of a nested feature.
The following request retrieves the value of the generic *pv_class* property used to store the photovoltaic
suitability class of a *RoofSurface* of the same building. The *RoofSurface* is identified within the XPath
expression via its gml:id *ID_ROOF_01*. The list of possible gml:id values might be the result of a preceding
GetPropertyValue operation.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetPropertyValue service="WFS" version="2.0.0" xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:bldg="http://www.opengis.net/citygml/building/2.0"
     xmlns:gen="http://www.opengis.net/citygml/generics/2.0"
     valueReference="bldg:boundedBy/bldg:RoofSurface[@gml:id='ID_ROOF_01']/gen:intAttribute[@gen:name='pv_class']/gen:value">
     <wfs:Query typeNames="bldg:Building">
       <fes:Filter>
         <fes:ResourceId rid="ID_0815"/>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetPropertyValue>

The equivalent KVP-encoded request is shown below. Note that the URL encoding of the XPath expression has
been omitted for the sake of clarity.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.0&
   REQUEST=GetPropertyValue&
   TYPENAMES=bldg:Building&
   VALUEREFERENCE=bldg:boundedBy/bldg:RoofSurface[@gml:id='ID_ROOF_01']/gen:intAttribute[@gen:name='pv_class']/gen:value&
   RESOURCEID=ID_0815

The URL encoding of the above XPath expression results in the following string.

.. code-block:: bash

   VALUEREFERENCE=bldg%3AboundedBy%2Fbldg%3ARoofSurface%5B%40gml%3Aid%3D%27ID_ROOF_01%27%5D%2Fgen%3A
   intAttribute%5B%40gen%3Aname%3D%27pv_class%27%5D%2Fgen%3Avalue