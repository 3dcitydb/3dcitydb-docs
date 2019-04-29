Vegetation Model
^^^^^^^^^^^^^^^^

The vegetation model specified in paragraph 2.2.4.10 is realized by the
tables shown in Figure 48 which correspond largely to the UML model.

|image54|

Figure 48: Vegetation database schema

**SOLITARY_VEGETAT_OBJECT**

The attributes *class*, *function, usage, species*, *height*,
*trunkDiameter*, and *crownDiameter* describe single vegetation objects.
The attribute *species* is of type *gml:CodeList* in CityGML that can be
referenced to a certain codespace. Therefore, another \_CODESPACE column
is provided in the SOLITARY_VEGETAT_OBJECT table. Similar to the
building table attribute with measure information can optionally be
coupled with a reference to the used measuring scale by an additional
\_UNIT column.

The geometry of the vegetation can either be described explicitly using
the attribute LOD4_OTHER_GEOM or LOD4_BREP_ID or implicitly using a
foreign key relation the IMPLICIT_GEOMETRY table including a reference
point and optionally a transformation matrix (LODx_IMPLICIT_REP_ID,
LODx_IMPLICIT_REF_POINT LODx_IMPLICIT_TRANSFORMATION, with 1 ≤ x ≤ 4).

**PLANT_COVER**

Information on vegetation areas are contained in attributes *usage*,
*class*, *function*, and *averageHeight*. There is also a \_UNIT column
to specify the scale the *averageHeight* values are based on. The
geometry is restricted to a *MultiSurface* or (and this is unique for
*PlantCover* features) a *MultiSolid*, represented respectively by the
foreign keys LODx_MULTI_SURFACE_ID (with 1 ≤x ≤ 4) and
LODx_MULTI_SOLID_ID which refer to the SURFACE_GEOMETRY table.

.. |image54| image:: ../../media/image65.png
   :width: 6.13958in
   :height: 7.0463in
