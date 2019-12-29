Appearance Model
^^^^^^^^^^^^^^^^

**APPEARANCE, APPEARANCE_SEQ**

The table APPEARANCE contains information about the surface data of
objects (attribute DESCRIPTION), its category is stored in attribute
THEME. Since each city model or city object may store its own appearance
data, the table APPEARANCE is related to the tables for the base classes
*CityObject* and *CityModel* by two foreign keys which may be used
alternatively. The classes *Appearance* and \_\ *SurfaceData* represent
features, which can be referenced by GML identifiers. For this reason,
the attributes GMLID and GMLID_CODESPACE were added to the corresponding
tables.

|image36|

Figure 33: Appearance database schema

**SURFACE_DATA, TEX_IMAGE, APPEAR_TO_SURFACE_DATA**

An appearance is composed of data for each surface geometry object.
Information on the data types and its appearance are stored in table
SURFACE_DATA.

IS_FRONT determines the side a surface data object applies to
(IS_FRONT=1: front face IS_FRONT=0: back face of a surface data object).
The OBJECTCLASS_ID column denotes if materials or textures are used for
the specific object (values: *X3DMaterial*, *Texture* or
*GeoreferencedTexture*). Materials are specified by the attributes
X3D_xxx which define its graphic representation. Details on using
georeferenced textures, such as orientation and reference point, are
contained in attributes GT_xxx. See chapter 2.2.3 for more information
on SURFACE_DATA attributes or the CityGML specification [Gröger et al.
2012, p. 33-45] which explains the texture mapping process in detail.

Raster-based 2D textures are stored in table TEX_IMAGE. The name of the
corresponding images for example is specified by the attribute
TEX_IMAGE_URI. The texture image can be stored within this table in the
attribute TEX_IMAGE_DATA using the BLOB data type under Oracle and the
BYTEA data type under PostgreSQL.

Table APPEAR_TO_SURFACE_DATA represents the interrelationship between
appearances and surfaces for different themes.

**TEXTUREPARAM**

Attributes for mapping textures to objects (point list or transformation
matrix) which are defined by the CityGML classes
*\_TextureParameterization*, *TexCoordList*, and *TexCoordGen* are
stored in the table TEXTUREPARAM.

|BuildingAppearance|

Figure 34: Simple example explaining texture mapping using texture
coordinates

================ ============ ============= ============= ===========
**TEXTUREPARAM**                                         
**SURFACE\_**    **IS_TEXTURE **WORLD_TO**  **TEXTURE\_   **SURFACE
                 \_PARAME                   COORDINATES** \_DATA_ID**
**GEOMETRY**     TRIZATION**  **\_TEXTURE**              
                                                         
**\_ID**                                                 
**7**            **1**        **NULL**      **GEOMETRY**  **20**
**…**            **…**        **…**         **…**         **…**
================ ============ ============= ============= ===========

Table 7: Example for table TEXTUREPARAM

Texture coordinates are applicable to polygonal surfaces, whose
boundaries are described by a closed linear ring (last coordinate is
equal to first). Coordinates are stored with a geometry data type. The
WORLD_TO_TEXTURE attribute defines a transformation matrix from a
location in world space to texture space. For more details see the
CityGML Implementation Specification [Gröger et al. 2012].

|LoD1-2|

Figure 35: Visualisation of a simple building in LoD1 and LoD2 using the
appearance model. Two themes are defined for the building and the
surrounding terrain: (a) building in summertime and (b) building in
wintertime

Six surface representations are listed in table SURFACE_DATA (cf. Table
10). First of all, a homogeneous material is defined (ID=1), represented
by a 3-component (RGB) colour value which will be used for both
appearances (summer and winter). This also applies to a general side
façade texture (ID=3, Figure 36 right) which is repeated (wrapped) to
fill the entire surface. For each of the front side, the back side and
the ground two images are available: parameterized ones for the sides
(Figure 36 left and middle) and georeferenced ones for the ground and
the roof surfaces (Figure 38). The information of textures is stored in
a separate table TEX_IMAGE. The coordinates for mapping the textures to
the object are stored in table TEXTUREPARAM. For the general side
texture (SURFACE_DATA_ID=3) five coordinate pairs are needed to define a
closed ring (here: rectangle). Table SURFACE_GEOMETRY contains the
information of all geometry parts that form the building and its
appropriate 3D coordinates (cf. tables on the next page).

See the following page for an example of the storage of appearances in
the city database. Figure 36 and Figure 38 show the images used for
texturing a building in LoD2. In LoD1, a material definition is used to
define the wall colors of the building.

Table 8 to Table 11 show a combination of tables representing the
building’s textures. There are different images available for summer and
winter resulting in two themes: Summer and Winter. The tuples within the
tables are color-coded according to their relation to the respective
theme:

-  Green: only summer related data

-  Light-grey: only winter related data

-  Orange: both summer and winter related data

Figure 37 shows the LoD2 representation of summer appearances (theme
Summer).

-  

|image39|

============== ========= ========== ================ =================
**APPEARANCE**                                      
**ID**         **GMLID** **THEME**  **CITYMODEL_ID** **CITYOBJECT_ID**
**...**                  **...**    **...**          **...**
**1**          **App1**  **Summer**                  **1000**
**2**          **App2**  **Winter**                  **1000**
**...**                  **...**    **...**          **...**
============== ========= ========== ================ =================

======================== =================== ======================
**                                           
APPEAR_TO_SURFACE_DATA**                     
**APPEARANCE_ID**        **SURFACE_DATA_ID**  **COMMENTS**
**1**                    **7**                **LoD1 S**
**2**                    **7**                **LoD1 W**
**1**                    **8**                **LoD2 ground/roof S**
**1**                    **3**                **LoD2 façade S**
**1**                    **4**                **LoD2 front/back S**
**2**                    **5**                **LoD2 ground/roof W**
**2**                    **3**                **LoD2 façade W**
**2**                    **6**                **LoD2 front/back W**
======================== =================== ======================

Table 8: Excerpt of table APEARANCE

The relation to the building feature is given by the foreign key
CITYOBJECT_ID

Table 9: APPEAR_TO_SURFACE table

|image40|\ |image41|\ |image42|

| front_back\_
| summer.png

| front_back\_
| winter.png

| facade.png
| summer & winter

SURFACE_DATA_ID = 4 SURFACE_DATA_ID = 6 SURFACE_DATA_ID = 3

Figure 36: Images for parameterized textures

Figure 37: Surface geometries for the building in LoD2 (the IDs for LoD1
are the same as in Figure 31)

================ ============ ============================= ===================== ================ ================= ========================= ======================
**SURFACE_DATA**                                                                                                                              
**ID**           **IS_FRONT** **OBJECTCLASS_ID**            **X3D_DIFFUSE_COLOR** **TEX_IMAGE_ID** **TEX_WRAP_MODE** **GT_ORIENTATION**        **GT_REFERENCE_POINT**
**7**            **1**        **53 (X3DMaterial)**          **1.0 0.6 0.0**                                                                   
**3**            **1**        **54 (ParameterizedTexture)**                       **31**           **wrap**                                   
**4**            **1**        **54**                                              **32**           **none**                                   
**6**            **1**        **54**                                              **33**           **none**                                   
**8**            **1**        **55 (GeoreferencedTexture)**                       **34**           **none**          **0.05 0.0 0.0 0.066667** **GEOMETRY**
**5**            **1**        **55**                                              **35**           **none**          **0.05 0.0 0.0 0.066667** **GEOMETRY**
================ ============ ============================= ===================== ================ ================= ========================= ======================

============= ================== =========================
**TEX_IMAGE**                   
**ID**        **TEX_IMAGE_DATA** **TEX_IMAGE_URI**
**31**        **BLOB(…)**        **facade.png**
**32**        **BLOB(…)**        **front_back_summer.png**
**33**        **BLOB(…)**        **front_back_winter.png**
**34**        **BLOB(…)**        **ground_summer.png**
**35**        **BLOB(…)**        **ground_winter.png**
============= ================== =========================

Table 10: Excerpt of table SURFACE_DATA and table TEX_IMAGE

   |image43|

================ ==================== ====================== ======================= ============= ==========================
**TEXTUREPARAM**                                                                                   
**SURFACE\_**    **IS_TEXTURE\_**     **WORLD_TO\_**         **TEXTURE_COORDINATES** **SURFACE\_**  C
                                                                                                    *OMMENTS*
**GEOMETRY_ID**  **PARA-METRIZATION** **TEXTURE**                                    **DATA_ID**   
**30**           **0**                **NULL**               **NULL**                **8**          **LoD 2 ground S**
**16**           **0**                **NULL**               **NULL**                **8**          **LoD 2 roof left S**
**17**           **0**                **NULL**               **NULL**                **8**          **LoD 2 roof right S**
**13**           **1**                **NULL**               **GEOMETRY**            **4**          **LoD 2 front S**
**15**           **1**                **NULL**               **GEOMETRY**            **4**          **LoD 2 back S**
**12**           **1**                **NULL**               **GEOMETRY**            **3**          **LoD 2 façade left S/W**
**11**           **1**                **NULL**               **GEOMETRY**            **3**         
**14**           **1**                **-0.4 0.0 0.0 1.0**   **NULL**                **3**          **LoD 2 façade right S/W**
                                                                                                   
                                      **0.0 0.0 0.3333 0.0**                                       
                                                                                                   
                                      **0.0 0.0 0.0 1.0**                                          
**30**           **0**                **NULL**               **NULL**                **5**          **LoD2 ground W**
**16**           **0**                **NULL**               **NULL**                **5**          **LoD 2 roof left W**
**17**           **0**                **NULL**               **NULL**                **5**          **LoD 2 roof right W**
**13**           **1**                **NULL**               **GEOMETRY**            **6**          **LoD 2 front W**
**15**           **1**                **NULL**               **GEOMETRY**            **6**          **LoD 2 back W**
**2**            **0**                **NULL**               **NULL**                **7**          **LoD1 walls S/W**
**10**           **0**                **NULL**               **NULL**                **8**          **LoD1 roof S/W**
================ ==================== ====================== ======================= ============= ==========================

Ground\_

winter.png

SURFACE_DATA_ID = 5

|image44|

Table 11: Table TEXTUREPARAM

Ground\_

summer.png

SURFACE_DATA_ID = 8

Figure 38: Images for georeferenced textures (The image
*round_winter.png* is assigned to the terrain and the roof surfaces of
the building both in LoD1 and LoD2 within the winter theme (a),
*ground_summer.png* within the summer theme (b))

.. |image36| image:: ../../media/image46.png
   :width: 6.29921in
   :height: 5.33656in

.. |BuildingAppearance| image:: ../../media/image47.png
   :width: 6.21736in
   :height: 1.73056in

.. |LoD1-2| image:: ../../media/image48.png
   :width: 5.03472in
   :height: 1.99097in

.. |image39| image:: ../../media/image49.png
   :width: 3.75472in
   :height: 3.26415in

.. |image40| image:: ../../media/image51.png
   :width: 0.60417in
   :height: 0.72917in

.. |image41| image:: ../../media/image52.png
   :width: 1.575in
   :height: 0.78056in

.. |image42| image:: ../../media/image53.png
   :width: 1.58333in
   :height: 0.79167in

.. |image43| image:: ../../media/image54.png
   :width: 2.3125in
   :height: 1.72917in

.. |image44| image:: ../../media/image55.png
   :width: 2.3125in
   :height: 1.72917in
