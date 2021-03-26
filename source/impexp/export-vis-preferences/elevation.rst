.. _impexp_kml_export_terrain_preferences_chapter:

Elevation
^^^^^^^^^

The *Elevation* preferences dialog offers settings to adapt the
height values of exported city objects in the resulting visualization models.
This also has an impact when rendering the models in a viewer.

.. figure:: /media/impexp_kml_export_preferences_terrain_fig.png
   :name: pic_kml_collada_gltf_preferences_terrain
   :align: center

   Visualization export preferences – Elevation settings.

**Altitude mode**

The altitude mode parameter [1] specifies how the height values (altitude)
of the exported city objects are to be interpreted by a
viewer or application. It can be set to either *absolute* (default), *relative* or
*clamp to ground*.

.. list-table::  *Supported altitude modes for city objects*
   :align: center
   :name: export_vis_elevation_altitude_mode
   :widths: 20 70

   * - | **Altitude mode**
     - | **Description**
   * - | **absolute**
     - | The altitude is interpreted as an absolute height value in meters according to the vertical reference system (EGM96 geoid in Google Earth, WGS84 ellipsoid in Cesium).
   * - | **relative**
     - | The altitude is interpreted as a value in meters above the terrain. The absolute height value can be determined by adding the terrain elevation.
   * - | **clamp to ground**
     - | The altitude will be ignored and the city object will always be clamped to the ground.

To pick the right mode, it is important to understand how the height values
have been derived for the city objects stored in the database.
Nowadays, absolute height values are more commonly used in the
creation of 3D city models than relative height values. Either way, the
chosen method should be consistent for all features in the database.

If you choose an altitude mode that does not fit your height values,
this might obviously result in city objects flying over or sinking
into the terrain in a viewer. This effect can, however, also occur if your viewer
uses another terrain model than the one used for deriving
the height values for your city objects. In the best case, the viewer
lets you use your own terrain model for visualization that matches your city objects.

.. caution::
   The altitude mode setting **is ignored** by the Cesium-based 3D web map client
   shipped with the 3D City Database (see :numref:`webmap_chapter`).
   The height values are **always interpreted as absolute values** by
   this viewer.

.. note::
   The altitude mode settings will **only be considered**
   when the city objects are exported in the *Geometry* or
   *COLLADA/glTF* display forms. When choosing the *Footprint* display
   form instead, *clamp to ground* is enforced automatically to ensure
   that the exported footprints are always draped onto the terrain.
   Likewise, when exporting in the *Extruded* display form,
   the *relative* mode will be used independent of the
   choice made in this dialog.

In addition to the altitude mode, you can also specify to *use the
original height values without transformation* in the
export process. When creating the visualization models, the coordinates of the
city objects are internally transformed to WGS84, for instance, to be
used in KML geometries. Depending on the spatial database (PostgreSQL/PostGIS, Oracle)
and its version used for running the 3DCityDB, this transformation
will not only affect the x and y coordinates but also the height values.
If, for instance, the city objects have absolute height values and you use
a matching terrain in the viewer, then changes to the height values due
to transformations might again result in flying or sinking city objects.
With this option enabled (default), the export operation will always
keep the original height values as stored in the database in the entire process.

**Height offset**

In addition to the altitude mode, a *height offset* [2] can be specified
that is added to every z coordinate of all geometries that will be exported.
If you do not want to apply a height offset, simply choose the
*No offset* option which is also the default value.

The *Constant offset* option lets you define a constant value,
positive or negative, and every height value is incremented by this
value. This option is particularly useful when exporting only
a single city object or in case all height values share a global
error that can be easily corrected with a constant offset.

When choosing the *Move every object on the globe (zero elevation)* option,
the lowest point for each city object will be determined and the height
value of this point will be set to 0. The same offset will then be
applied to the remaining points of the city object. In contrast to a
constant offset, this method uses an individual offset for each
city object. It makes sense to combine this option with the *relative*
altitude mode.

.. note::
   When using Google Earth as viewer, moving every object to
   zero elevation and combining this with a *relative* altitude mode
   results in city objects that are perfectly sitting on the ground
   independent of whether the terrain layer is activated or not.

   For the Cesium-based 3D web map client, this option can be used
   to render the city objects on the WGS84 ellipsoid. However, as
   soon as you load a terrain, remember that the height values are
   interpreted as being absolute (see above) by this viewer.
   Thus, the city objects will most likely be located under the terrain.

A last option available for choosing a height offset is to
use the value stored in the generic attribute **GE_LoDn_zOffset** of
the city object. Since the value can be different for different
city objects, also this options can be used to adapt the height
value for each city object individually.

This option is mainly intended to be used with Google Earth as viewer.
For the terrain model used in Google Earth, an `Elevation API <https://developers.google.com/maps/documentation/elevation/>`_
is offered by Google that lets you query the height value of the terrain for every
location on earth. The idea is to use this service to calculate an
individual offset for each city object so that the city object
perfectly sits on the Google Earth terrain. For this purpose,
the terrain height is queried for all points of the city object that
have the lowest z coordinate value. The offset is then chosen so
that the city object is moved to the lowest terrain height returned
for these points. The Elevation API is however only queried, if you
enable the *Query the Google Elevation API when no data is available*
option (default: false).

Using the Elevation API service requires internet access and might be time
consuming if you export a large number of city objects or even
if you export a smaller scene multiple times. For this reason,
the export operation automatically stores the determined offset
as generic attribute *GE_LoDn_zOffset* for every city object in the
database. Before querying the Elevation API, it will first be checked
whether this attribute exists. If so, the value of the attribute
will be directly used as height offset without querying the
Elevation API again for this city object.

Since city objects may have different geometries with different height values
in different LoDs, the height offset must be stored for each LoD
separately. For this purpose, the ``n`` in *GE_LoDn_zOffset* is meant
as placeholder for the LoD the offset should be applied to. Thus,
the attribute names stored in the database are actually *GE_LoD1_zOffset*,
*GE_LoD2_zOffset*, etc.

Storing the height offset as generic attribute with the city object
also ensures that this information is available when exporting the
city object in CityGML/CityJSON format and therefore can be consumed by
other applications or even be transported across 3DCityDB instances.
Moreover, you can manually adjust the offsets at any time in the database
or even delete the attribute so that it will be calculated anew
with the next visualization export.

.. note::
   Although the *GE_LoDn_zOffset* attribute is mainly intended to be used
   with Google Earth as viewer and to be automatically populated using the Google Elevation API,
   it is not restricted to this use case. In contrast, you can also use
   different data and algorithms to calculate a height offset that is specific
   to each city object and store your results in *GE_LoDn_zOffset*. The
   export operation will consider and apply these height offsets just the
   same if you enable the *Use generic attribute GE_LoDn_zOffset* option.
   Thus, the way how the *GE_LoDn_zOffset* attribute is populated is irrelevant.

.. caution::
   Starting from July 2018, the Elevation API cannot be used for free
   anymore but requires an API key to be able to query the service. Thus, the option
   *Query the Google Elevation API when no data is available* should only be
   enabled when a valid Elevation API key is available. Users can provide
   their own Elevation API key in the general preferences as described in
   :numref:`impexp_preferences_general_apiKeys_chapter`.
   Please refer to https://cloud.google.com/maps-platform/terms/ for more details
   on the Google Maps Platform Terms of Service.

.. caution::
   Be careful if you have already calculated the *GE_LoDn_zOffset* attribute and
   want to export and re-import your city objects into another 3DCityDB instance.
   If this target 3DCityDB is running on a different spatial database system or a
   different version of the same system, applying the height offset from *GE_LoDn_zOffset* might
   give you different results. The reason is that z coordinates might or might not
   be changed in coordinate transformation by the different spatial database systems
   (see discussion above). Results will be consistent in case the target
   database system is identical our if you always make sure to keep
   the original height values in the export process.