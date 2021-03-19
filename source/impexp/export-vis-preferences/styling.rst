.. _impexp_kml_export_rendering_preferences_chapter:

Styling
^^^^^^^

Most aspects regarding the look of the KML/COLLADA/glTF exports when
visualized in virtual globes like Google Earth and Cesium can be
customized under the preferences tab, node *KML/COLLADA/glTF Export*,
subnode *Rendering*. Each of the top-level feature categories has its
own *Rendering* settings. For the sake of clarity the most complex
*Rendering* settings for *Buildings* will be explained here as an
example. Settings for all other top-level features are either identical
or simpler. An exceptional case is *GenricCityObject* which can be
exported into point or line geometries, and the corresponding settings
will be explained at the end of this section.

.. figure:: /media/impexp_kml_export_preferences_rendering_building_fig.png
   :name: pic_kml_collada_gltf_preferences_rendering
   :align: center

   Rendering settings for the KML/COLLADA/glTF *Building* export

All settings in this menu are grouped according to the display form they
relate to.

Footprint and extruded display options
""""""""""""""""""""""""""""""""""""""

In this section the fill and line colors can be selected. Additionally,
it can be chosen whether the displayed objects should be highlighted
when being run over with the mouse or not. Highlighting colors can only
be set when the highlighting option is enabled. The alpha value affects
the transparency of all colors equally: 0 results in transparent
(invisible) colors, 255 in completely opaque ones. A click on any color
box opens a color choice dialog.

As defined in the CityGML specification [GKNH2012]_ CityGML
version 2.0.0 allows LoD0 representation (footprint and roofprint
representations) for buildings and building parts. If LoD0 in the Level
of Export setting on the main *KML/COLLADA/glTF Export* tab is selected,
there are three options available for LoD0 geometry export:

-  **footprint**: the footprint geometries of the buildings or building
   parts will be exported

-  **roofprint**: the roofprint geometries of the buildings or building
   parts will be exported

-  **roofprint, if none then footprint**: footprint geometries will be
   exported if none of the roofprint geometries are found.

Geometry display options
""""""""""""""""""""""""

This parameter section distinguishes between roof and wall surfaces and
allows the user to color them independently. The alpha value affects the
transparency of all roof and wall surface colors in the same manner as
in the footprint and extruded cases: 0 results in transparent
(invisible) colors, 255 in completely opaque ones. A click on any color
box opens a color choice dialog.

As previously stated: when not explicitly modeled, thematic surfaces
will be inferred for LoD1 or LoD2 based exports following a trivial
logic (surfaces touching the ground –that is, having a lowest
z-coordinate- will be considered wall surfaces, all other will be
considered roof surfaces), in LoD3 or LoD4 based exports surfaces not
thematically modeled will be colored as wall surfaces.

The highlighting effect when running with the mouse over the exported
objects can also be switched on and off. Since the highlighting
mechanism relies internally on a switch of the alpha values on the
highlighting surfaces, the alpha value set in this section does not
apply to the highlighted style of geometry exports, only to their normal
style. For a detailed explanation of the highlighting mechanism see the
following section.

COLLADA/glTF display options
""""""""""""""""""""""""""""

These parameters control the export of COLLADA and glTF models. The
first option addresses the fact that sometimes objects may contain
wrongly oriented surfaces (points ordered clockwise instead of
counter-clockwise) as a result of errors in some previous data gathering
or conversion process. When rendered, wrongly oriented surfaces will
only be textured on the inside and become transparent when viewed from
the outside. Ignore surface orientation informs the viewer to disable
back-face culling and render all polygons even if some are technically
pointing away from the camera.

.. note::
   This will result in lowered rendering performance. Correcting
   the surface orientation data is the recommended solution. This option
   only provides a quick fix for visualization purposes.

The activation of the option *Generate surface normal* allows
calculating the surface normals for the exported object surfaces that
can be illuminated with a shading effect in 3D scenes and therefore
provides a better visual representation of the 3D object which has a
constant color throughout its surfaces. If this option is not activated,
this 3D object will be rendered as a solid geometry without any visual
distinction of its boundary surfaces (cf. :numref:`pic_kml_collada_gltf_preferences_rendering_comparison`). However, when
exporting textured 3D models, the shading effect is not relevant, since
the texture information can already provide a sophisticated visual
effect.

.. note::
   Starting with version 4.0.0, the Importer/Exporter activates the
   option *Generate surface normal* by default for all (top-level)
   features if such information is available.

.. figure:: /media/impexp_kml_export_surface_normal_comparison_fig.png
   :name: pic_kml_collada_gltf_preferences_rendering_comparison
   :align: center

   Comparison of the different visual effects of the same
   3D model with (the left figure) and without (the right figure)
   surface normals

Surface textures can be stored in an image file, or grouped into large
canvases containing all images clustered together so-called texture
atlases, which can significantly increase the storage efficiency and
loading speed of 3D models. However, in some CityGML datasets, it might
occur that a very large texture atlas image is shared by multiple
surface geometries belonging to many different city objects. In this
case, every exported COLLADA/glTF model representing a city object will
receive a complete copy of the texture atlas image in which only a small
portion of it is actually used. This will result in extreme performance
issues when loading and rendering such COLLADA/glTF models in Earth
browsers. In order to avoid this, the option *Crop texture images* shall
be activated which allows cropping the large texture atlas image into a
number of small texture images, each of which could be very small in
size and should correspond to only one surface geometry of the city
object.

With the option *Generate texture atlases with algorithm*, grouping
images in an atlas or not and the algorithm selected for the texture
atlas construction (differing in generation speed and canvas efficiency)
can be set here. Depending on the algorithm and size of the original
textures, an object can have one or more atlases, but atlases are not
shared between separate objects.

The texture atlas algorithms address the problem of two-dimensional
image packing, also known as 'knapsack problem’ in different ways (see
[CGJT1980]_):

-  **BASIC**\ *:* recursively divides the texture atlas into empty and
   filled regions (see
   http://www.blackpawn.com/texts/lightmaps/default.html). The first
   item is placed in the top left corner. The remaining empty region is
   split into two rectangles along the sides of the item. The next item
   is inserted into one of the free rectangles and the remaining empty
   space is split again. Doing this in a recursive way builds a binary
   tree representing the texture atlas. When adding an item, there is no
   information of the sizes of the items that are going to be packed
   after this one. This keeps the algorithm simple and fast. The items
   may be rotated when being inserted into the texture atlas.

-  **TPIM**\ *:* touching perimeter (see [LoMV1999]_ and [LoMM2002]_).
   Sorts images according to non-increasing area and orients
   them horizontally. One item is packed at a time. The first item
   packed is always placed in the bottom-left corner. Each following
   item is packed with its lower edge touching either the bottom of the
   atlas or the top edge of another item, and with its left edge
   touching either the left edge of the atlas or the right edge of
   another item. The choice of the packing position is done by
   evaluating a score, defined as the percentage of the item perimeter
   which touches the atlas borders and other items already packed. For
   each new item, the score is evaluated twice, for the two item
   orientations, and the highest value is selected.

-  **TPIM w/o image rotation**\ *:* touching perimeter without rotation.
   Same as TPIM, but not allowing for rotation of the original images
   when packing. Score is evaluated only once since only one orientation
   is possible.

From the algorithms, *BASIC* is the fastest (shortest generation time)
and produces good results, whereas *TPIM* is the most efficient (highest
used area/total atlas size ratio).

Scaling texture images is another means of reducing file size and
increasing loading speed. A scale factor of 0.2 to 0.5 often still
offers a fairly good image quality while it has a major positive effect
on these both issues. Default value is 1.0 (no scaling). This setting is
independent from the atlas setting and both can be combined together. It
is possible to generate atlases and then scale them to a smaller size
for yet shorter loading times in Earth browsers.

In the next parameter section, the fill color of the roof and wall
surfaces can be set by clicking on the corresponding color box to open
the color selection dialog. The alpha value that affect the transparency
of all surface colors can also be selected from a range of 0 (completely
transparent) to 255 (completely opaque).

.. note::
   This setting only takes effect if none of the appearance themes
   (as defined in the CityGML specification [GKNH2012]_) is
   selected or available in the currently connected 3DCityDB instance.

Buildings can be put together in groups into a single model/placemark.
This can also speed up loading, however it can lead to conflicts with
the digital terrain model (DTM) of the Earth browser, since buildings
grouped together have coordinates relative to the first building on the
group (taken as the origin), not to the Earth browser's DTM. Only the
first building of the group is guaranteed to be correctly placed and
grounded in the Earth browser. If the objects being grouped are too far
apart this can result in buildings hovering over or sinking into the
ground or cracks appearing between buildings that should go smoothly
together.

Up to Google Earth 7, no highlighting of model placemarks loaded from a
location other than Google Earth's own servers is supported natively
(glowing blue on mouse over). Therefore, a highlighting mechanism of its
own was implemented in the KML/COLLADA/glTF exporter: highlighting is
achieved by displaying a somewhat "exploded" version of the city object
being highlighted around the original object itself. "Exploded" means
all surfaces belonging to the object are moved outwards, displaced by a
certain distance orthogonally to the original surface. This "exploded"
highlighting surface is always present, but not always visible: when the
mouse is not placed on any building (or rather, on the highlighting
surface surrounding it closely) this "exploded" highlighting surface has
a normal style with an alpha value of 1, invisible to the human eye.
When the mouse is place on it, the style changes to highlighted, with an
alpha value of 140 (hard-coded), becoming instantly visible, creating
this model placemark highlighted feel. The displacement distance for the
exploded highlighting surfaces can be set here. Default value is 0.75m.

.. figure:: /media/impexp_kml_export_mouseover_highlighting_fig.png
   :name: pic_kml_collada_gltf_preferences_rendering_collada
   :align: center

   Object exported in the COLLADA display form being
   highlighted on mouseOver

This highlighting mechanism only works in Google Earth and has an
important side effect: the model's polygons will be loaded and displayed
twice (once for the representation itself, once for the highlighting),
having a negative impact in the viewing performance of the Earth
browser. The more complex the models are, the higher the impact is. This
becomes particularly noticeable for models exported from a LoD3 basis
upwards. The highlighting and grouping options are mutually exclusive.

GenericCityObject
"""""""""""""""""

As previously stated: in addition to the standard support for surface
and solid geometry exports, other geometry types like point and line for
the feature class *GenricCityObject* can also be exported in KML format.
The related *rendering* node contains two further independent subnodes
(“*Surface and Solid*\ ” and “\ *Point and Curve*\ ”) that allows for
customizing the export of different geometry types individually. As the
subnode “\ *Surface and Solid*\ ” has similar settings illustrated in
the previous section, only the settings within the subnode “\ *Point and
Curve*\ ” will be explained in the following paragraphs.


.. figure:: /media/impexp_kml_export_point_curve_rendering.png
   :name: pic_kml_collada_gltf_preferences_rendering_point
   :align: center

   Rendering settings for point and curve geometry exports for *GenericCityObject*

The field *Altitude mode* specifies how the Z-coordinates (altitude) of
the exported point geometries are interpreted by the earth browser.
Possible value may be one of the following options:

-  **absolute**: the altitude is interpreted as an absolute height value
   in meters according to the vertical reference system (EGM96 geoid in
   KML).

-  **relative**: the altitude is interpreted as a value in meters above the terrain.
   The absolute height value can be determined by adding the attitude to the elevation of the point.

-  **clamp to ground**: the altitude will be ignored and the point geometry will be
   always clamp to the ground regardless of whether the terrain layer is activated or not.

Three setting options are available which allow user to choose a more
appropriate display form for point geometry on the 3D map:

-  **Cross**: The point geometry can be spatially represented by using a
   cross-line in the form like “X” with the length size of around 2
   meters (hard-encoded). Changing the thickness and color settings will
   affect the width of the cross-line geometry in pixels and the display
   color respectively. The mouseOver highlighting effect is also
   supported and can be switched on and off by the user. When
   highlighting is enabled, further settings can be made for the
   thickness and color properties of the highlighting geometry.

.. figure:: /media/impexp_kml_export_example_cross_fig.png
   :name: pic_kml_collada_gltf_preferences_cross
   :align: center

   An exported point geometry object displayed as a cross-line

-  **Icon**: An alternative way for displaying point geometry in the earth browser
   is to use the KML’s native point placemark that can be represented with an icon
   in a user-defined color. The size of the icon can be determined with the help of the *Scale* option,      where
   the default value is 1.0 (no scaling) which can give a fairly good perception.

.. figure:: /media/impexp_kml_export_example_icon_fig.png
   :name: pic_kml_collada_gltf_preferences_point
   :align: center

   An exported point geometry object displayed as an icon

-  **Cube**\ *:* Another possibility of representing the point geometry
   is to use a small solid particle whose central point should be
   identical to the target point. Similar to the options (*Cross and
   Icon*) described above, settings options for the size, color, and
   highlighting effect can also be adjusted to achieve an optimal visual
   effect.

.. figure:: /media/impexp_kml_export_example_cube_fig.png
   :name: pic_kml_collada_gltf_preferences_cube
   :align: center

   An exported point geometry object displayed as a small cube

The rendering settings for the export of curve geometry objects can be
configured in a similar manner as those of point geometry with the
display form “\ *Cross*\ ”.

.. note::
   When displaying curve geometry objects in Google Earth, the
   altitude modes like *absolute* and *relative* may result in the curves
   intersecting with or hovering over the earth ground. If the user wants
   to keep the curve geometry objects always being draped on the earth
   ground, the altitude mode *clamp to ground* shall be chosen.



Support of GenericCityObject
""""""""""""""""""""""""""""

The earlier versions of KML/COLLADA/glTF Exporter have been designed to
only support exports of surface-based geometries for all CityGML
classes. Starting from version 3.0.0 of the 3DCityDB, the
KML/COLLADA/glTF Exporter has been functionally enhanced with the
support for exporting point and curve geometry types of
*GenricCityObject* objects in KML/KMZ format. *GenricCityObject* is a
feature class defined within the CityGML’s Generics module
(see :numref:`citydb_generic_model_chapter`) that
allows for modeling and exchanging of 3D city objects
which are not covered by any other thematic modules of CityGML. The
geometry of a *GenericCityObject* can be explicitly defined in LOD0-4
using arbitrary 3D GML geometry object (class *gml:_Geometry*). Thus,
any complex structured objects that have point, line, surface, or solid
geometries can be geometrically represented by means of
*GenricCityObject* objects for every LOD. For example, the indoor
routing network model, which are not defined in the current CityGML
specification, could be even though modeled using the CityGML’s Generics
module where each *GenricCityObject* object may represent a node or an
edge of the network model.

.. figure:: /media/impexp_kml_export_tum_vis_example_fig.png
   :name: pic_kml_collada_gltf_export_tum_vis
   :align: center

   Visualization of the network model of the building interior of Technical University Munich (TUM)

Depending on the chosen Level of Detail, the point and curve geometries
of *GenericCityObject* objects are exported, along with their surface and
solid geometries, into the output KML/KMZ file whose filename is
enhanced with a suffix denoting the selected display form (e.g.
*Footprint*, *Extruded*, *Geometry*, or *COLLADA/glTF).*