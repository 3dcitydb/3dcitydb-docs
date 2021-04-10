.. _ade_manager_plugin_tagged_values:

Adding tagged values to the XML Schema
--------------------------------------

One typical issue in the transformation of the XML Schema of a CityGML ADE
is that relevant information about the ADE data model is often missing in the
XML Schema and, thus, cannot be considered for the derivation of the
database schema.

For example, if you use UML as data modelling tool for your ADE,
the information whether an association is modelled as composition or aggregation
in UML cannot be expressed in XML Schema and is therefore typically lost when
deriving the XML Schema from the UML using standard UML-to-GML tools
like `ShapeChange <https://shapechange.net/>`_. The type of association is, however,
important for creating the database schema (e.g., for deciding where to put
foreign keys, whether to use *n:m* relationship tables, etc.) and for
deriving delete scripts that not only delete a single ADE feature but also
all of its nested subfeatures stored in different tables.
Some information like whether or not an ADE feature is a top-level feature
cannot be represented in UML or XML Schema at all.

To address these issues, you can annotate the XML Schema of the CityGML ADE
with the missing information. For this purpose, the *ADE Manager* plugin
defines and supports a set of so-called *tagged values* that can be added to
the elements of the XML Schema as application-specific metadata using
``<xs:annotation>`` tags. A tagged value is a name-value pair that is encoded
as ``<taggedValue tag="name">value</taggedValue>`` in XML. If you use
UML for modelling your ADE, you can even add the tagged values directly to
the UML constructs and they will be automatically carried to ``<xs:annotation>``
elements in the XML Schema when using tools like `ShapeChange <https://shapechange.net/>`_.

The following tagged values are supported by the ADE transformation operation.

.. list-table:: Tagging top-level ADE features
   :widths: 20 80

   * - | Tagged value
     - | :code:`topLevel` (true \| false)
   * - | Description
     - | This tagged value allows for defining whether an ADE feature is top-level
   * - | Example use in XML-Schema
     - .. code-block:: XML

        <element name="IndustrialBuilding"
          substitutionGroup="bldg:_AbstractBuilding"
          type="TestADE:IndustrialBuildingType">
          <annotation>
            <appinfo>
              <taggedValue tag="topLevel">true</taggedValue>
            </appinfo>
          </annotation>
        </element>


.. list-table:: Tagging the multiplicity of ADE hook properties
   :widths: 20 80

   * - | Tagged value
     - | :code:`minOccurs` and :code:`maxOccurs` (integer value \| „unbounded")
   * - | Description
     - | The combination of the two tagged values allows for defining the
       | multiplicity information of each ADE hook property. In UML model, this
       | multiplicity information can be explicitly specified but it is lost in
       | the XML Schema, because every ADE hook property is hard-encoded with a
       | multiplicity of [0..*] in the XML Schema. Since the ShapeChange tool
       | up to version 2.5.1 is not able to read the multiplicity of the hook properties
       | from the UML model directly, the two tagged values are required although
       | they just replicate the information from the UML.
   * - | Example use in XML-Schema
     - .. code-block:: XML

        <element name="ownerName"
          substitutionGroup="bldg:_GenericApplicationPropertyOfAbstractBuilding"
          type="string">
          <annotation>
            <appinfo>
              <taggedValue tag="maxOccurs">1</taggedValue>
            </appinfo>
          </annotation>
        </element>


.. list-table:: Tagging the relationship type between classes
   :widths: 20 80

   * - | Tagged value
     - | :code:`relationType` (association \| aggregation \| composition)
   * - | Description
     - | An enumeration attribute allowing to distinguish the three relationships
       | between two associated classes. This meta-information is also lost
       | in the mapping UML -> XML Schema, because XML Schema does not
       | distinguish between the three relation types. This tagged value is also
       | redundant from the view of UML, but required when using ShapeChange.
   * - | Example use in XML-Schema
     - .. code-block:: XML

        <element maxOccurs="unbounded" minOccurs="0" name="boundedBy"
          type="bldg:BoundarySurfacePropertyType">
          <annotation>
            <appinfo>
              <taggedValue tag="relationType">composition</taggedValue>
            </appinfo>
          </annotation>
        </element>

.. list-table:: Tagging the LoD of geometry properties
   :widths: 20 80

   * - | Tagged value
     - | :code:`lod` (integer value between 0 and 4)
   * - | Description
     - | An integer value to denote the LoD representation of the respective
       | geometry property. If this tagged value is not provided, the ADE manager
       | will check if the property name is prefixed with 'lod' (not case-sensitive)
       | and the forth character is an integer between 0 and 4. If this is true,
       | then this integer value will used as LoD information.
   * - | Example use in XML-Schema
     - .. code-block:: XML

        <complexType abstract="true" name="_AbstractBuildingUnitType">
          <complexContent>
            <extension base="core:AbstractCityObjectType">
              <sequence>
                <element name="footprint" type="gml:MultiSurfacePropertyType">
                  <annotation>
                    <appinfo>
                      <taggedValue tag="lod">0</taggedValue>
                    </appinfo>
                  </annotation>
                </element>
              </sequence>
            </extension>
          </complexContent>
        </complexType>

.. list-table:: Tagging property elements to be ignored
   :widths: 20 80

   * - | Tagged value
     - | :code:`ignore` (true \| false)
   * - | Description
     - | This tagged value allows for labeling selected properties that shall not be taken into account when deriving the ADE database schema and schema mapping file.
   * - | Example use in XML-Schema
     - .. code-block:: XML

        <complexType abstract="true" name="_AbstractBuildingUnitType">
          <complexContent>
            <extension base="core:AbstractCityObjectType">
              <sequence>
                <element name="legacyAttr" type="string">
                  <annotation>
                    <appinfo>
                      <taggedValue tag="ignore">true</taggedValue>
                    </appinfo>
                  </annotation>
                </element>
              </sequence>
            </extension>
          </complexContent>
        </complexType>