WaterBody Model
^^^^^^^^^^^^^^^

**WATERBODY, WATERBOD_TO_WATERBND_SRF**

The modelling of the WATERBODY database schema corresponds largely to
the respective UML model. For LoD0 and LoD1 additional attributes are
added, e.g. for modelling river geometry (LODx_MULTI_CURVE).

The geometries of LOD0 and LOD1 areal water bodies are stored within the
table SURFACE_GEOMETRY. The foreign keys LODx_MULTI_SURFACE_ID (with 0 ≤
x ≤ 1) refer to the corresponding rows. Geometry for water filled
volumes is handled in a similar way using foreign keys LODx_SOLID_ID
(with 1 ≤ x ≤ 4).

For mapping the *boundedBy* aggregation which identifies the water
body’s exterior shell managed by the WATERBOUNDARY_SURFACE table, the
additional table WATERBOD_TO_WATERBND_SRF is needed to realise the m:n
relationship.

**WATERBOUNDARY_SURFACE**

The exterior shell of a *WaterBody* can be differentiated semantically
using features of the type \_\ *WaterBoundarySurface*. These features
are stored in the WATERBOUNDARY_SURFACE table and can be distinguished
by the OBJECTCLASS_ID attribute:

-  11 (*WaterSurface*)

-  12 (*WaterGroundSurface*)

-  13 (*WaterClosureSurface*)

If a CityGML ADE is used that extends any of the named classes above,
further values for OBJECTCLASS_ID may be added by the ADE manager. Their
concrete numbers depend on the ADE registration (cf. section 6.3.3.1).

Since every \_\ *WaterBoundarySurface* object must have at least one
associated surface geometry, the foreign keys LODx_SURFACE_ID (with 2 ≤x
≤ 4, no *MultiSurface* here) are used to realise these relations.

|image55|

Figure 49: WaterBody database schema

.. |image55| image:: ../../media/image66.png
   :width: 6.42634in
   :height: 5.95858in
