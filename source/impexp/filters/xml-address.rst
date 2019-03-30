Address information
^^^^^^^^^^^^^^^^^^^

The 3DCityDB comes with a CityGML ADE that allows to easily use address
information and metadata columns in XML queries. The following table
shows the XML namespaces to be used with CityGML version 2.0
respectively 1.0 and the recommended XML prefix of the 3DCityDB ADE.

================ ============== =====================================================
**ADE**          **XML prefix** **XML namespace**
**3DCityDB ADE** **citydb**     **http://www.3dcitydb.org/citygml-ade/3.0/citygml/2.0
                                http://www.3dcitydb.org/citygml-ade/3.0/citygml/1.0**
================ ============== =====================================================

Table 34: XML prefix and namespace of the 3DCityDB ADE.

**Address information.** CityGML uses the OASIS xAL standard for the
representation of address information. xAL is very flexible in that it
supports various address styles that can be XML-encoded in many ways. As
a drawback, this flexibility makes it difficult to define a filter on
address elements (e.g., the street or the city) using an XPath
expression based on xAL. When importing address information into the
3DCityDB, the xAL address fragment is parsed and mapped onto the columns
STREET, HOUSE_NUMBER, PO_BOX, ZIP_CODE, CITY, STATE and COUNTRY of the
ADDRESS table. Thus, it is preferable and simpler to express filter
criteria on these columns.

For this reason, the 3DCityDB ADE injects additional properties into the
*core:Address* feature of CityGML that correspond to the columns of the
ADDRESS table. By this means, these properties can be used in filter
expressions. The mapping between ADE properties and columns of the
ADDRESS table is shown below. Note that the citydb prefix must be
associated with the ADE XML namespace (see above). If omitted, the
CityGML 2.0 namespace is assumed given that the prefix *citydb* is used.

============================== ============= ===============================
**ADE property                 **Data type** **Column of the ADDRESS table**
(injected into core:Address)**              
**citydb:street**              **xs:string** **STREET**
**citydb:houseNumber**         **xs:string** **HOUSE_NUMBER**
**citydb:poBox**               **xs:string** **PO_BOX**
**citydb:zipCode**             **xs:string** **ZIP_CODE**
**citydb:city**                **xs:string** **CITY**
**citydb:state**               **xs:string** **STATE**
**citydb:country**             **xs:string** **COUNTRY**
============================== ============= ===============================

Table 35: 3DCityDB ADE properties for accessing address information.

The following example illustrates how to query all buildings along the
street *Unter den Linden*. It uses the *citydb:street* ADE property as
value reference in the filter expression.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <filter>
| <propertyIsLike wildCard="*" singleCharacter="." escapeCharacter="\"
  matchCase="true">
| <valueReference>bldg:address/core:Address/citydb:street</valueReference>
| <literal>Unter den Linden*</literal>
| </propertyIsLike>
| </filter>
| </query>
