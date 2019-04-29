Generic Objects and Attributes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

3D city models will most likely contain attributes, which are not
explicitly modelled in CityGML. Moreover, there may be 3D objects that
are not covered by the thematic classes of CityGML. Generic objects and
attributes help to support the storage of such data.

**GENERIC_CITYOBJECT**

For generic objects the full variety of different geometrical
representations known from other tables is offered. Explicit
(LODx_BREP_ID, LODx_OTHER_GEOM) and implicit geometry
(LODx_IMPLICIT_REP_ID, LODx_IMPLICIT_REF_POINT,
LODx_IMPLICIT_TRANS-FORMATION) as well as terrain intersection curves
(LODx_TERRAIN_INTERSECTION) (all with 0 ≤ x ≤ 4).

|image50|

Figure 44: GenericCityObject and generic attributes database schema

**CITYOBJECT_GENERICATTRIB, CITYOBJECT_GENERICATT_SEQ**

**The table CITYOBJECT_GENERICATTRIB is used to represent the concept of
generic attributes. However, the creation of a table for every type of
attribute was omitted. Instead a single table CITYOBJECT_GENERICATTRIB
represents all types and the types are differentiated via the values of
the attribute DATATYPE.**

**The table provides fields for every data type, but only one of those
fields is relevant in each case. An overview of the meaning of the
entries in the field DATATYPE is given in** Table 17\ **. The relation
between the generic attribute and the corresponding CityObject is
established by the foreign key CITYOBJECT_ID.**

============ =======================================================
**DATATYPE** **attribute type**
**1**        **STRING**
**2**        **INTEGER**
**3**        **REAL**
**4**        **URI**
**5**        **DATE**
**6**        **MEASURE**
**7**        **Group of generic attributes**
**8**        **BLOB**
**9**        **Geometry type**
**10**       **Geometry via surfaces in the table SURFACE_GEOMETRY**
============ =======================================================

Table 17: Attribute type

**Please note that the binary and geometric data types (incl. geometry
via surfaces) are not supported by CityGML and cannot be exported using
the CityGML Import / Export tool! But, if a user wants to add additional
attributes to thematic tables, he should use the schema of the
CITYOBJECT_GENERICATTRIB table rather than adding additional columns to
existing tables, because only in this way the Import / Export tool can
automatically write them to CityGML.**

Moreover, generic attributes can be grouped using the CityGML class
*genericAttributeSet*. Since *genericAttributeSet* itself is a generic
attribute, it may also be contained in a generic attribute set
facilitating a recursive nesting of arbitrary depth. This hierarchy
within a *genericAttributeSet* is realized by the foreign key
PARENT_GENATTRIB_ID which refers to the superordinate
*genericAttributeSet* (aggregate) and contains NULL, if such does not
exist. The foreign key ROOT_GENATTRIB_ID refers directly to the top
level (root) of a *genericAttributeSet* tree. In order to select all
generic attributes forming a *genericAttributeSet* one only has to
select those with the same ROOT_GENATTRIB_ID.

The next available ID for the table CITYOBJECT_GENERICATTRIB is provided
by the sequence CITYOBJECT_GENERICATT_SEQ.

.. |image50| image:: media/image61.png
   :width: 5.30706in
   :height: 6.24138in
