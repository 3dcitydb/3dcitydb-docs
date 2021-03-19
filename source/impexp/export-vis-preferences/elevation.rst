.. _impexp_kml_export_terrain_preferences_chapter:

Elevation
^^^^^^^^^

In order to ensure a perfect display of the exported datasets in the
Earth browser, some adjustments on the z coordinate for the exported 3D
objects may be necessary.

.. figure:: /media/impexp_kml_export_preferences_terrain_fig.png
   :name: pic_kml_collada_gltf_preferences_terrain
   :align: center

   Altitude/Terrain settings

Use original z-Coordinates without transformation
"""""""""""""""""""""""""""""""""""""""""""""""""

Depending on the spatial database used, the transformation of the
original coordinates to WGS84 will include transformation of the
z-coordinates (PostGIS starting from version 2.0 or Oracle starting from version 11g) or not (Oracle 10g). To
make sure only the planimetric (x,y) and not the z-coordinates are
transformed this checkbox must be selected. This is useful when the used
terrain model is different from Google Earth’s and the z-coordinates are
known to fit perfectly in that terrain model.

Another positive side-effect of this option is that *GE_LoDn_zOffset*
attribute values (explained in the following section) calculated for
Oracle 10g keep being valid when imported into PostGIS starting from version 2.0 or Oracle
starting from version 11g. Otherwise, when switching database versions and not making use
of this option, *GE_LoDn_zOffset* values must be recalculated again.

*GE_LoDn_zOffset* attribute values calculated for Oracle 10g are
consistent for all KML/COLLADA/glTF exports from Oracle 10g. The same
applies to PostGIS starting from version 2.0 or Oracle starting from version 11g. Only cross-usage
(calculation in one version, export from the other) creates
inconsistencies that can be solved by turning z-coordinate
transformation off.

This setting affects the resulting *GE_LoDn_zOffset* if used when a
cityobject has none such value yet and is exported in KML/COLLADA for
the first time, so it is recommended to remember its status
(z-coordinate transformation on or off) for all future exports.

Altitude mode
"""""""""""""

Allows the user to choose between *relative* (to the ground),
interpreting the altitude as a value in meters above the terrain, or
*absolute*, interpreting the altitude as an absolute height value in
meters according to the vertical reference system used by the Earth
browser (e.g., Google Earth uses the EGM96 geoid, whereas Cesium uses
the WGS84 ellipsoid), or *clamp to ground*, which allows the exported
objects to be always clamped to ground.

This means, when *relative* altitude mode is chosen, the z-coordinates
of the exports represent the vertical distance from the digital terrain
model (DTM) of the Earth browser, which should be 0 for those points on
the ground (the building's footprint) and higher for the rest (roof
surfaces, for instance). However, z-coordinate values of the city
objects stored in a 3DCityDB usually have values bigger than 0, so
choosing this altitude mode will often result in exports hovering over
the ground.

.. figure:: /media/impexp_kml_export_example_relative_atitude_mode_fig.jpeg
   :name: pic_kml_collada_gltf_preferences_terrain_example
   :align: center

   Possible export result with relative altitude mode

When *absolute* altitude mode is chosen, the z-coordinates of the
exports represent the vertical distance from the vertical datum - the
ellipsoid or geoid which most closely approximates the Earth curvature,
regardless of the DTM at that point. This implies, choosing this
altitude mode may result in buildings sinking into the ground wherever
the DTM indicates there is a hill or hovering over the ground wherever
the DTM indicates a dent.

When the *clamp to ground* altitude mode is chosen, the z-coordinate
values of the exported objects will be ignored and every surface
geometry of the KML models will be forced to lie on the surface of the
ground.

For a proper grounding, the **Altitude offset** setting can additionally
be used so that a positive or negative offset value can be applied to
all z-coordinates of the exports, moving the city objects up and down
along the z-axis until they match the ground.

.. note::
   Both **Altitude mode** and **Altitude offset** settings will
   only take effect when the city objects are exported in the *Geometry* or
   *COLLADA/glTF* display forms. When, for example, the *Footprint* display
   form is selected, The KML/COLLADA/glTF-Exporter will internally use the
   *clamp to ground* altitude mode to ensure that the exported geometries
   will be always clamped to ground regardless of the altitude mode chosen
   by the user. Likewise, when exporting in the *Extruded* display form,
   the *relative* altitude model will be internally applied and the height
   value of the respective city object will be used to represent the
   relative height above the ground.

Altitude offset
"""""""""""""""

A value, positive or negative, can be added to the z coordinates of all
geometries in one export in order to place them higher or lower over the
earth surface. This offset can be 0 for all exported objects (*no
offset*), it can be constant for all (*constant*), or it can have an
individual value for each object to ensure that the bottom of the object
is placed on the earth surface.

The first option *no offset* implies that the z-coordinates of all
geometries are kept unchanged at export time if the option *Use original
z-Coordinates without transformation* is selected. The second option
*constant* is particularly appropriate for exports of a single city
ob­ject, allowing some fine-tuning of its position along the z-axis.

When exporting regions - via bounding box settings -, the other two
options, *Move each object to bottom height 0* and *Use generic
attribute "GE_LoDn_zOffset"*, are recom­mended.

Once the option *Move each object to bottom height 0* is selected, the
elevation value of the lowest point for every object will be calculated
and its inversed value should exactly equal to the zOffset value of the
respective object. This zOffset value will be used for adjusting the z-
coordinates of the object to ensure that its lowest point has a height
of 0 meter. This setting is particularly advisable, since combined with
the *relative* altitude mode the exported objects can always be properly
placed on the ground in Google Earth regardless of whether its terrain
layer is activated or not. However, if the *absolute* altitude is
chosen, a proper grounding of the objects requires that the terrain
layer in Google Earth must be deactivated.

.. note::
   Regardless of the chosen altitude mode, the Cesium-based
   3DCityDB-Web-Map-Client always interprets the altitude as an absolute
   height value in meters according to the WGS84 ellipsoid reference
   system. Thus, the option *Move each object to bottom height 0* can only
   ensure a proper grounding of the objects on the Cesium Virtual Globe
   when its WGS84 ellipsoid terrain model (default) is activated.

When choosing the *absolute* altitude model and displaying city objects
on Google Earth with enabled terrain layer, the option *Use generic
attribute "GE_LoDn_zOffset"* shall be selected. Here the
*GE_LoDn_zOffset* generic attribute value can be automatically
calculated by the Importer/Exporter if not available. This calculation
uses data returned by
`Google's Elevation API <https://developers.google.com/maps/documentation/elevation/>`_.
After completing the calculation, the results will be stored in
the ``CITYOBJECT_GENERICATTRIB`` table of the 3DCityDB for future use.

.. note::
   Starting from July 2018, an Elevation API key is required in
   order to enable access to the Google Elevation Service. Thus, the option
   *Call the Google Elevation API when no data is available* should only be
   enabled when a valid Elevation API key is available. Users can provide
   their own Elevation API key in the general preferences as described in
   :numref:`impexp_preferences_general_apiKeys_chapter`.
   For more details on the Google Maps Platform Terms of
   Service, please refer to https://cloud.google.com/maps-platform/terms/.

Since city objects may have different geometries for different LoDs, the
anchoring points and their elevation values may also differ for each
LoD. This explains the need for having *GE_LoD1_zOffset*,
*GE_LoD2_zOffset,* etc. generic attributes for one single object.

The algorithm used to calculate the individual zOffset for an object
iterates over the points with the lowest z-coordinate in the object,
calling Google's elevation API in order to get their elevation. The
point with the lowest elevation value will be chosen for anchoring the
object to the ground. The zOffset value results from subtracting the
point's z-coordinate from the point's elevation value.

When calling Google's elevation API for calculating the zOffset of an
object a message is shown: "Getting zOffset from Google's elevation
service for BLDG_0003000e008c4dc4".

Saving the building's height offset in the form of a generic attribute
ensures this information will be present in every export in CityGML
format (and therefore at every re-import) and can thus be transported
across databases. Please note, that not the DTM height value of Google
Earth will be stored but the difference of the individual building’s
minimum z value and the value reported by the Google Elevation Service.
Following this approach further usage restrictions of the Google
Elevation Service are avoided.

In some unusual cases, even after automatic calculation of the
*GE_LoDn_zOffset* value the object may still not be perfectly grounded
to the Earth surface for a number of reasons; e.g. wrong height data of
the model, or low resolution of the DTM at that area. In those cases a
manual adjustment of the value in the 3DCityDB is needed. After the
content of *GE_LoDn_zOffset* has been fine-tuned to a proper value it
should be persistently stored in the database.

.. figure:: /media/impexp_kml_export_altitude_points_zOffset_fig.jpeg
   :name: pic_kml_collada_gltf_preferences_terrain_example_relative
   :align: center

   Points sent to Google's Elevation API for calculation of the zOffset

.. figure:: /media/impexp_kml_export_example_absolute_noOffset_fig.png
   :name: pic_kml_collada_gltf_preferences_terrain_example_absolute_without_grounding
   :align: center

   Export with *absolute* altitude mode and *no offset*

.. figure:: /media/impexp_kml_export_example_absolute_grounding_fig.jpeg
   :name: pic_kml_collada_gltf_preferences_terrain_example_absolute_with_grounding
   :align: center

   Export with *absolute* altitude mode and use of *GE_LoDn_zOffset*