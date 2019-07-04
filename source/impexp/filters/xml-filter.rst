<filter> selection clause
^^^^^^^^^^^^^^^^^^^^^^^^^

The <filter> parameter is used to identify a subset of city objects from
the 3DCityDB whose property values satisfy a set of logically connected
predicates. If the property values of a city object satisfy all the
predicates in a filter, then that city object is part of the export.

Predicates can be expressed both on properties of the top-level feature
types listed by the <typeNames> parameter and on properties of their
nested feature types. If the predicates are not satisfied, then the
entire top-level feature is not exported.

If the <typeNames> parameter lists more than one top-level feature type,
then predicates may only be expressed on properties common to all of
them.

The <filter> parameter supports *comparison operators*, *spatial
operators* and *logical operators*. The meaning of the operators is
identical to the operators defined in the *OGC Filter Encoding* (FE) 2.0
standard [3]_, but their encoding slightly differs.

Most expressions are formed using a *valueReference* pointing to a
property value and a *literal* value that is checked against the
property value.


.. _value:

Value references
""""""""""""""""

A value reference is a string that represents a value that is to be
evaluated by a predicate. The string can be the name of a property of
the feature type or an *XML Path Language* (XPath) expression that
represents the property of a nested feature type or a complex property.

Property names are given as *xsd:QName*. Examples for valid property
names are *core:creationDate*, *bldg:measuredHeight*, and
*tun:lod2MultiSurface*.

In cases where a property of a nested feature type or complex property
shall be evaluated, the value reference must be encoded using XPath. The
XPath expression is to be formulated based on the XML encoding of
CityGML. Note that the Importer/Exporter only supports a subset of the
full XPath language:

-  Only the abbreviated form of the child and attribute axis specifier
   is supported.

-  The context node is the top-level feature type to be exported. In
   case two or more top-level feature types are listed by the
   <typeNames> parameter, then the context node is their common parent
   type.

-  Each step in the path may include an XPath predicate of the form
   “\ *.=value*\ ” or “\ *child=value*\ ”. Equality tests can be
   logically combined using the "and" or "or" operators. Indexes are not
   supported as XPath predicate.

-  The *schema-element()* function is supported. It takes the
   *xsd:QName* of a feature type as parameter. The function selects the
   given feature type and all its subtypes.

-  The last step of the XPath must be a simple thematic attribute or a
   spatial property. Property elements that contain a nested feature are
   not allowed as last step.

Assuming that *bldg:Building* is the top-level feature type to be
exported, then the following examples are valid XPath expressions:

-  *gen:stringAttribute/@gen:name* selects the gen:name attribute of the
   generic string attributes of the building

-  *gen:stringAttribute[@gen:name=’area’]/gen:value* selects the
   gen:value of a generic string attribute of name “area”

-  *bldg:boundedBy/bldg:WallSurface/bldg:lod2MultiSurface* selects the
   spatial LoD2 representation of the wall surfaces of the building

-  *bldg:boundedBy/bldg:WallSurface[@gml:id=’ID_01’ or
   gml:name=’wall’]/*

*bldg:opening/bldg:Door/core:creationDate* selects the core:creationDate
of doors that are associated with wall surfaces having a specific gml:id
or gml:name

-  *bldg:boundedBy/schema-element(bldg:_BoundarySurface)/@gml:id*
   selects the gml:id attribute of all boundary surfaces of the building

-  *core:externalReference[core:informationSystem='http://somewhere.de']/
   core:externalObject/core:name* selects the core:name of the external
   object in an external reference to a given information system

-  *gen:genericAttributeSet[@gen:name=’energy’]/gen:measureAttribute/
   gen:value* selects the gen:value of all generic measure attributes
   contained in the generic attribute set named “energy”

.. note::
   CityGML uses the *eXtensible Address Language* (xAL) to encode
   addresses of buildings, bridges and tunnels. xAL is very flexible and
   allows an address to be encoded in different ways, which makes XPath
   expressions complex to write. For this reason, the Importer/Exporter
   uses a simple ADE that can be used in XPath expressions to evaluate
   address elements such as the street or city name. More information is
   provided in chapter 5.4.2.9.


.. _literals:

Literals and geometric values
"""""""""""""""""""""""""""""

Literals are explicitly stated values that are evaluated against a
*valueReference*. The type of the literal value must match the type of
the referenced value.

If the literal value is a geometric value, the value must be encoded
using one of the geometry types offered by the query language. Support
for additional geometry encodings like (E)WKT is planned for a future
version. The following geometry types are available:

-  <envelope>

-  <point>

-  <lineString>

-  <polygon>

-  <multiPoint> (list of <point>s)

-  <multiLineString> (list of <lineString>s)

-  <multiPolygon> (list of <polygon>s)

An <envelope> is defined by its <lowerCorner> and <upperCorner> elements
that carry the coordinate values. The coordinates of a <point> are
provided by a <pos> element, whereas <lineString> uses a <posList>
element. A <polygon> can have one <exterior> and zero or more <interior>
rings. Rings are supposed to be closed meaning that the first and the
last coordinate tuple in the list must be identical. Interior rings must
be defined in opposite direction compared to the exterior ring.

The dimension of the points contained in a <posList> as well as in
<exterior> and <interior> rings can be denoted using the *dimension*
attribute. Valid values are *2* (default) or *3*.

Every geometry type offers an optional *srid* attribute to reference an
SRID defined in the underlying database. If *srid* is present, then the
coordinate tuples are assumed to be given in the reference system
associated with the corresponding SRID, which is also used in coordinate
transformations. If *srid* is not present, then the coordinate tuples
are assumed to be given in the SRID of the 3DCityDB instance.

A 2D bounding box:

| <envelope>
| <lowerCorner>30 10</lowerCorner>
| <upperCorner>60 20</upperCorner>
| </envelope>

A 2D point:

| <point>
| <pos>30 10</pos>
| </point>

A 2D line string given in SRID 4326:

| <lineString srid="4326">
| <posList dimension="2">45.67 88.56 55.56 89.44</posList>
| </lineString>

A 2D polygon with hole:

| <polygon>
| <exterior>35 10 45 45 15 40 10 20 35 10</exterior>
| <interior>20 30 35 35 30 20 20 30</interior>
| </polygon>


.. _operators:

Comparison operators
""""""""""""""""""""

A comparison operator is used to form expressions that evaluate the
mathematical comparison between two arguments. The following binary
comparisons are supported:

-  <propertyIsEqualTo> (=)

-  <propertyIsLessThan> (<)

-  <propertyIsGreaterThan> (>)

-  <propertyIsEqualTo> (=)

-  <propertyIsLessThanOrEqualTo> (<=)

-  <propertyIsGreaterThanOrEqualTo> (>=)

-  <propertyIsNotEqualTo> (<>)

The optional *matchCase* attribute can be used to specify how string
comparisons should be performed. A value of *true* means that string
comparisons shall match case (default), *false* means caselessly.

The following example shows how to export all buildings from the
3DCityDB whose *bldg:measuredHeight* attribute has a values less than
50.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <filter>
| <propertyIsLessThan>
| <valueReference>bldg:measuredHeight</valueReference>
| <literal>50</literal>
| </propertyIsLessThan>
| </filter>
| </query>

Besides these default binary operators, the following additional
comparison operators are supported:

-  <propertyIsLike>

-  <propertyIsNull>

-  <propertyIsBetween>

The <propertyIsLike> operator expresses a string comparison with pattern
matching. A combination of regular characters, the *wildCard* character
(default: \*), the *singleCharacter* (default: .), and the
*escapeCharacter* (default: \\) define the pattern. The *wildCard*
character matches zero or more characters. The *singleCharacter* matches
exactly one character. The *escapeCharacter* is used to escape the
meaning of the *wildCard*, *singleCharacter* and *escapeCharacter*
itself. The *matchCase* attribute is also available for the
<propertyIsLike> operator.

The following example shows how to find all roads whose *gml:name*
contains the string “main”.

| <query>
| <typeNames>
| <typeName>tran:Road</typeName>
| </typeNames>
| <filter>
| <propertyIsLike wildCard="*" singleCharacter="." escapeCharacter="\"
  matchCase="false">
| <valueReference>gml:name</valueReference>
| <literal>*main*</literal>
| </propertyIsLike>
| </filter>
| </query>

The <propertyIsNull> operator tests the specified property to see if it
exists for the feature type being evaluated.

The <propertyIsBetween> operator is a compact way of expressing a range
check. The lower and upper boundary values are inclusive. The operator
is used below to find all buildings having between 10 and 20 storeys.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <filter>
| <propertyIsBetween>
| <valueReference>bldg:storeysAboveGround</valueReference>
| <lowerBoundary>10</lowerBoundary>
| <upperBoundary>20</upperBoundary>
| </propertyIsBetween>
| </filter>
| </query>


.. _spatial:

Spatial operators
"""""""""""""""""

A spatial operator determines whether its geometric arguments satisfy
the stated spatial relationship. The following operators are supported:

-  <bbox>

-  <equals>

-  <disjoint>

-  <touches>

-  <within>

-  <overlaps>

-  <intersects>

-  <contains>

-  <dWithin>

-  <beyond>

The semantics of the spatial operators are defined in OGC Filter
Encoding 2.0, 7.8.3, and in ISO 19125-1:2004, 6.1.14.

The *valueReference* of the spatial operators must point to a geometric
property of the feature type or its nested feature types. If
*valueReference* is omitted, then the *gml:boundedBy* property is used
per default.

The listing below exemplifies how to use the <bbox> operator to find all
city objects whose envelope stored in *gml:boundedBy* is not disjoint
with the given geometry.

| <query>
| <filter>
| <bbox>
| <operand>
| <lowerCorner>30 10</lowerCorner>
| <upperCorner>60 20</upperCorner>
| </operand>
| </bbox>
| </filter>
| </query>

The following example exports all buildings having a nested
*bldg:GroundSurface* feature whose *bldg:lod2MultiSurface* property
intersects the given 2D polygon.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <filter>
| <intersects>
| <valueReference>bldg:boundedBy/bldg:GroundSurface/bldg:lod2MultiSurface</valueReference>
| <polygon>
| <exterior>35 10 45 45 15 40 10 20 35 10</exterior>
| </polygon>
| </intersects>
| </filter>
| </query>

The last example demonstrates how to find all city furniture features
whose envelope geometry is within the distance of 80 meters from a given
point location. The *uom* attribute denotes the unit of measure for the
distance. If *uom* is omitted, then the unit is taken from the
definition of the associated reference system. If the reference system
lacks a unit definition, meter is used as default value.

| <query>
| <typeNames>
| <typeName>frn:CityFurniture</typeName>
| </typeNames>
| <filter>
| <dWithin>
| <valueReference>gml:boundedBy</valueReference>
| <point srid="4326">
| <pos>45.67 88.56</pos>
| </point>
| <distance uom="m">80</distance>
| </dWithin>
| </filter>
| </query>


.. _logical:

Logical operators
"""""""""""""""""

A logical operator can be used to combine one or more conditional
expressions. The logical operator <and> evaluates to true if all the
combined expressions evaluate to true. The operator <or> operator
evaluates to true is any of the combined expressions evaluate to true.
The <not> operator reverses the logical value of an expression. Logical
operators can contain nested logical operators.

The following <and> filter combines a <propertyIsLessThan> comparison
and a spatial <dWithin> operator to find all buildings with a
*bldg:measuredHeight* less than 50 and within a distance of 80 meters
from a given point location.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <filter>
| <and>
| <propertyIsLessThan>
| <valueReference>bldg:measuredHeight</valueReference>
| <literal>50</literal>
| </propertyIsLessThan>
| <dWithin>
| <valueReference>gml:boundedBy</valueReference>
| <point srid="4326">
| <pos>45.67 88.56</pos>
| </point>
| <distance uom="m">80</distance>
| </dWithin>
| </and>
| </filter>
| </query>


.. _gmlid:

gml:id filter operator
""""""""""""""""""""""

The <resourceIds> operator is a compact way of finding city objects
whose *gml:id* value is contained in the provided list of <id> elements.

The example below exports all buildings whose *gml:id* matches one of
the values in the list.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <filter>
| <resourceIds>
| <id>ID_01</id>
| <id>ID_02</id>
| <id>ID_03</id>
| </resourceIds>
| </filter>
| </query>


.. _sql:

SQL operator
""""""""""""

The <sql> operator lets you add arbitrary SQL queries to your filter
expression. It can be combined with all other predicates.

The SQL query is provided in the <select> subelement. It must follow the
same rules as discussed in chapter 5.4.1. Most importantly, the query
shall return a list of id values that reference the ID column of the
table CITYOBJECT.

Note that the query is encoded in XML. Thus, characters having special
meaning in the XML language must be encoded using entity references. For
example, the less-than sign < and greater-than sign > must be encoded as
&lt; and &gt; respectively. Instead of using entity references, you can
put your SQL string into a CDATA section. The string is then parsed as
purely character data.

For example, the following SQL filter expression selects all id values
from city objects having a generic attribute called *energy_level* whose
double value is less than 10. The entity reference &lt; must be used
here.

| <query>
| <filter>
| <sql>
| <select>select cityobject_id from cityobject_genericattrib
| where attrname='energy_level' and realval &lt; 10</select>
| </sql>
| </filter>
| </query>

When putting the same query into a CDATA section, the less-than sign
must not be replaced with an entity reference.

| <query>
| <filter>
| <sql>
| <select>
| <![CDATA[
| select cityobject_id from cityobject_genericattrib
| where attrname='energy_level' and realval < 10
| ]]>
| </select>
| </sql>
| </filter>
| </query>
