.. _impexp_xml_query_address_metadata:

Address information and metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The 3DCityDB comes with a CityGML extension called *3DCityDB ADE* that simplifies using address
properties and metadata columns in XML queries. The following table
shows the recommended XML prefix, namespaces and the XSD schema locations
of the 3DCityDB ADE for both CityGML 2.0 and 1.0.

.. list-table:: XML prefix, namespace and schema location of the 3DCityDB ADE.
   :name: impexp_citydb_ade_table
   :widths: 25 75

   * - | **XML prefix**
     - | citydb
   * - | **XML namespace**
     - | http://www.3dcitydb.org/citygml-ade/3.0/citygml/2.0
       | http://www.3dcitydb.org/citygml-ade/3.0/citygml/1.0
   * - | **XSD schema location**
     - | http://www.3dcitydb.org/citygml-ade/3.0/3dcitydb-ade-citygml-2.0.xsd
       | http://www.3dcitydb.org/citygml-ade/3.0/3dcitydb-ade-citygml-1.0.xsd

**Address information**

CityGML uses the OASIS xAL 2.0 standard for the
representation of address information. xAL is very flexible in that it
supports various address styles that can be XML-encoded in many ways. As
a drawback, this flexibility makes it difficult to define a filter on
address elements (e.g., the street or the city) using an XPath
expression based on xAL. When importing address information into the
3DCityDB, the xAL address fragment is parsed and mapped onto the columns
STREET, HOUSE_NUMBER, PO_BOX, ZIP_CODE, CITY, STATE and COUNTRY of the
ADDRESS table. Thus, it is much simpler to express filter
criteria on these columns.

For this reason, the 3DCityDB ADE injects additional properties into the
*core:Address* feature of CityGML that correspond to the columns of the
ADDRESS table. By this means, these properties can be used in filter
expressions. The mapping between ADE properties and columns of the
ADDRESS table is shown below. Note that the *citydb* prefix must be
associated with the ADE XML namespace (see above). If omitted, the
CityGML 2.0 namespace of the 3DCityDB ADE is assumed given that the prefix *citydb* is used.

.. list-table:: 3DCityDB ADE properties for accessing address information.
   :name: impexp_ade_address_properties_table
   :widths: 30 20 50

   * - | **ADE property**
       | (injected into core:Address)
     - | **Data type**
     - | **Column of the ADDRESS table**
   * - | citydb:street
     - | xs:string
     - | STREET
   * - | citydb:houseNumber
     - | xs:string
     - | HOUSE_NUMBER
   * - | citydb:poBox
     - | xs:string
     - | PO_BOX
   * - | citydb:zipCode
     - | xs:string
     - | ZIP_CODE
   * - | citydb:city
     - | xs:string
     - | CITY
   * - | citydb:state
     - | xs:string
     - | STATE
   * - | citydb:country
     - | xs:string
     - | COUNTRY

The following example illustrates how to query all buildings along the
street *Unter den Linden*. It uses the *citydb:street* ADE property as
value reference in the filter expression.

.. code-block:: xml

    <query>
      <typeNames>
        <typeName>bldg:Building</typeName>
      </typeNames>
      <filter>
        <propertyIsLike wildCard="*" singleCharacter="." escapeCharacter="\" matchCase="true">
          <valueReference>bldg:address/core:Address/citydb:street</valueReference>
          <literal>Unter den Linden*</literal>
        </propertyIsLike>
      </filter>
    </query>

.. note::
   In the output file, the address information is **always encoded using xAL** and not using the 3DCityDB ADE
   properties to ensure that regular CityGML applications without ADE support can still parse the address.

**3DCityDB metadata of city objects**

The 3DCityDB stores database-specific metadata with every city object using the columns
LAST_MODIFICATION_DATE, UPDATING_PERSON, REASON_FOR_UPDATE and LINEAGE
of the CITYOBJECT table. These metadata properties are only defined for the 3DCityDB **but not** in CityGML.
To make them available in filter expressions, the 3DCityDB ADE therefore injects them into the
CityGML *core:_CityObject* feature. This way, you can filter city objects based values stored in the
metadata columns of the 3DCityDB.

.. list-table:: 3DCityDB ADE properties for accessing database-specific metadata information.
   :name: impexp_ade_metadata_properties_table
   :widths: 40 20 40

   * - | **ADE property**
       | (injected into core:_CityObject)
     - | **Data type**
     - | **Column of the CITYOBJECT table**
   * - | citydb:lastModificationDate
     - | xs:string
     - | LAST_MODIFICATION_DATE
   * - | citydb:updatingPerson
     - | xs:string
     - | UPDATING_PERSON
   * - | citydb:reasonForUpdate
     - | xs:string
     - | REASON_FOR_UPDATE
   * - | citydb:lineage
     - | xs:string
     - | LINEAGE

The following example shows how to use the ADE metadata properties in filter expressions. The
query fetches all bridges that have been modified in the database after *2018-01-01*.

.. code-block:: xml

    <query>
      <typeNames>
        <typeName>brid:Bridge</typeName>
      </typeNames>
      <filter>
        <propertyIsGreaterThan>
          <valueReference>citydb:lastModificationDate</valueReference>
          <literal>2018-01-01</literal>
        </propertyIsGreaterThan>
      </filter>
    </query>