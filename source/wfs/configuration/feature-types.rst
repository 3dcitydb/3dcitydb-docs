.. _wfs_feature_type_settings_chapter:

Feature type settings
~~~~~~~~~~~~~~~~~~~~~

With the *feature type* settings, you can control which feature types
can be queried from the 3D City Database and are served through the WFS
interface. Every feature type that shall be advertised to a client must
be explicitly listed in the ``config.xml`` file.

An example of the corresponding ``<featureTypes>`` XML element is shown
below. In this example, CityGML *Building* and *Road* objects are
available from the WFS service. In addition, a third feature type
*IndustrialBuilding* coming from a CityGML ADE is advertised.

.. code-block:: xml
   :name: wfs_feature_types_config_listing

   <featureTypes>
     <featureType>
       <name>Building</name>
       <ows:WGS84BoundingBox>
         <ows:LowerCorner>-180 -90</ows:LowerCorner>
         <ows:UpperCorner>180 90</ows:UpperCorner>
       </ows:WGS84BoundingBox>
     </featureType>
     <featureType>
       <name>Road</name>
       <ows:WGS84BoundingBox>
         <ows:LowerCorner>-180 -90</ows:LowerCorner>
         <ows:UpperCorner>180 90</ows:UpperCorner>
       </ows:WGS84BoundingBox>
     </featureType>
     <adeFeatureType>
       <name namespaceURI="http://www.citygml.org/ade/TestADE/1.0">IndustrialBuilding</name>
       <ows:WGS84BoundingBox>
         <ows:LowerCorner>-180 -90</ows:LowerCorner>
         <ows:UpperCorner>180 90</ows:UpperCorner>
       </ows:WGS84BoundingBox>
     </adeFeatureType>
     <version isDefault="true">2.0</version>
     <version>1.0</version>
   </featureTypes>

The ``<featureTypes>`` element contains one ``<featureType>`` node per feature
type to be advertised. The feature type is specified through the
mandatory *name* property, which can only take values from a fixed list
that enumerates the names of the CityGML top-level features (cf.
``config.xsd`` schema file). In addition, the geographic region covered by
all instances of this feature type in the 3D City Database can
optionally be announced as *bounding box* (lower left and upper right
corner). The coordinate values must be given in WGS 84.

.. note::
   The bounding box is not automatically checked against or
   computed from the database, but rather copied to the WFS *capabilities*
   document “as is”.

Feature types coming from a CityGML ADE are advertised using the
``<adeFeatureType>`` element. In contrast to CityGML feature types, the
*name* property must additionally contain the globally unique XML
*namespace URI* of the CityGML ADE, and the type name is not restricted
to a fixed enumeration. Note that a corresponding *ADE extension* must
be installed for the WFS service, and that the ADE extension must add
support for the advertised ADE feature type. Otherwise, the ADE feature
type is ignored. If you do not have ADE extensions, then simply skip the
``<adeFeatureType>`` element.

Besides the list of advertised feature types, also the CityGML *version*
to be used for encoding features in a response to a client’s request has
to be specified. Use the ``<version>`` element for this purpose, which takes
either 2.0 (for CityGML 2.0) or 1.0 (for CityGML 1.0) as value. If both
versions shall be supported, simply use two ``<version>`` elements. However,
in this case, you should define the *default version* to be used by the
WFS by setting the isDefault attribute to true on one of the elements
(otherwise, CityGML 2.0 will be the default).