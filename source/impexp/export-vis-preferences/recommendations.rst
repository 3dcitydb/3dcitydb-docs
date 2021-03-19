.. _impexp_export_vis_recommendations_chapter:

Recommendations
===============

General setting recommendations
-------------------------------

Depending on the quality and complexity of the 3DCityDB data, export
results may vary greatly in aesthetic and loading performance.
Experimenting will be required in most cases for a fine-tuning of the
export parameters. However, some rules apply for almost all cases:

-     kmz format use is recommended when the files will be accessed over a
      network and the selected display form is *Footprint*, *Extruded*,
      or *Geometry.* In case of glTF-export, only kml format is allowed.

-     Visibility values for the different display forms should be increased
      in steps of around one third of the tile side length.

-     Visibility from 0 pixels (always visible) should be avoided,
      especially for large or complex exports, because otherwise the
      Earth browser will immediately load all data at once since it all
      must be visible.

-     Tile side length (whether tiling is *automatic* or *manual*) should
      be chosen so that the resulting tile files are smaller than 10MB.
      When single files are bigger than that Google Earth gets
      unresponsive. For densely urbanized areas, where many placemarks
      are crimped together a tile side length value between 50 and 100m
      should be used.

-     When not exporting in the *COLLADA/glTF* display form, files will
      seldom reach this 10MB size, but Earth browser will also become
      unresponsive if the file loaded contains a lot of polygons, so do
      not use too large tiles for *footprint*, *extruded* or *geometry*
      exports even if the resulting files are comparatively small.

-     Do not choose too small tile sizes, many of them may become visible
      at the same time and render the tiling advantage useless.

-     Using texture atlas generation when producing *COLLADA/glTF* display
      form exports always results in faster model loading times.

-     From all texture atlas generating algorithms, *BASIC* is the fastest
      (shortest generation time), *TPIM* the most efficient (highest
      used area/total atlas size ratio).

-     Texture images can often be scaled down to 0.2 - 0.5 without
      noticeable quality loss. This depends, of course, on the quality
      of the original textures.

-     Highlighting puts the same polygons twice in the resulting export
      files, one for the buildings themselves, one for their
      highlighting. This has a negative impact on the viewing
      performance. The more complex the buildings are the worse the
      impact. When highlighting is enabled for exports based on a
      CityGML LoD3 or higher Google Earth may become quite slow.

-     If you want to use the 3DCityDB-Web-Map-Client to visualize the
      exported datasets, options for creating highlighting geometries
      should not be chosen, since the highlighting functionality is
      already well-supported by the 3DCityDB-Web-Map-Client which
      requires no extra highlighting geometries.

-     The 3DCityDB-Web-Map-Client allows for on-the-fly activating and
      deactivating shadow visualization of 3D objects exported in the glTF
      format. However, this functionality is currently not available when
      viewing KML models exported in the *Footprint*, *Extruded*, and
      *Geometry* display forms.

-     Balloon generation is slightly more efficient when a single template
      file is applied for all exported objects.

-     When exporting in the *Footprint* or *Extruded* display forms, the
      *altitude/terrain* settings will be silently ignored by the
      KML/COLLADA/glTF-Exporter which will instead internally applies the
      appropriate altitude models to the exported objects to ensure that
      they will be properly placed on the ground in Earth browsers.
      However, when exporting in the *Geometry* or *COLLADA/glTF* display
      forms, the *altitude/terrain* settings must be properly adapted
      regarding the Earth browsers to be used.

-     In most cases, the combination of the *relative* altitude mode with
      the *Move each object to bottom height* *0* altitude offset allows
      for a proper grounding and displaying of the objects in Earth
      browsers. However, when using the Cesium-based
      3DCityDB-Web-Map-Client, its default WGS84 ellipsoid terrain model
      must be activated.

-     When using the *absolute* z-coordinates and displaying the exported
      datasets together with terrain layer in Google Earth, you need to
      choose the following combination of settings, should you have a
      valid Goole Elevation API key: *absolute* altitude mode, *generic
      attribute “GE_LoDn_zOffset”,* and *call Google's elevation API
      when no data is available*.

Loading exported models in Google Earth and Cesium Virtual Globe
----------------------------------------------------------------

In order to make full use of the features and functionalities provided
by Google Earth, it is highly recommended to use the enhanced version of
Google Earth – **Google Earth Pro** which is available free of charge
starting from January 2015. Some of the features described in this
documentation, like highlighting, can also flawlessly work in the normal
Google Earth with version 6.0.1 or higher.

Displaying a file in Google Earth can be achieved by opening it through
the menu ("*File*", "*Open*") or double-clicking on any kml or kmz file
if these extensions are associated with the program (default option at
Google Earth's installation time).

Loaded files can be refreshed when generated again after loading (if for
example the balloon template file was changed) by choosing the
"*Revert*" option in the context menu on the sidebar. There is no need
to delete and load them again or shutdown or restart the Earth browser.

For best performance, cache options ("*Tools*", "*Options*", "*Cache*")
should be set to their maximum values, 1024 MB for memory cache size,
2000 MB for disk cache. Actual maximums may be lower depending on the
computer's hardware.

Google Earth enables showing the terrain layer by default for realistic
display of 3D models. Disabling of terrain layer is only possible in
Google Earth Pro. You may need to disable the terrain layer in case that
the exported models cannot be seen although shown as loaded in Google
Earth's sidebar, since they are probably buried into the ground (see
:numref:`impexp_kml_export_terrain_preferences_chapter`).

When exporting balloons into individual files (one for each object)
written together into a *balloon* directory access to local files and
personal data must be allowed ("*Tools*", "*Options*", "*General*").
Google Earth will issue a security warning that must be accepted,
otherwise the contents of the balloons (when in individual files and not
as a part of the doc.kml file) will not be displayed.

It is also possible to upload the generated KML/COLLADA/glTF files to a
web server and access them from there via internet browser with Cesium
Virtual Globe (starting from December 2015, the Google Earth Plugin is
no longer supported by most modern web browsers due to security
considerations). In this case, the Cross Origin Resource Sharing (CORS)
shall be enabled on the web server to allow cross-domain AJAX requests
sent from the based-web frontend.

.. note::
   Starting with version 7 (and at least up to version 7.1.1.1888)
   Google Earth has changed the way transparent or semi-transparent
   surfaces are rendered. This is especially relevant for visualizations
   containing highlighting surfaces (explained in
   :numref:`impexp_kml_export_rendering_preferences_chapter`). When
   viewing KML/COLLADA models in Google Earth it is strongly recommended to
   use Google Earth (Pro) version 7 or higher and switch to the OpenGL
   graphic mode for an optimal viewing experience. Changing the Graphic
   Mode can be achieved by clicking on *Tools*, *Options* entry, *3D View*
   Tab.

.. figure:: /media/impexp_kml_export_googeearth_settings_fig.png
   :name: pic_kml_collada_gltf_export_google_earth_settings
   :align: center

   Setting the Graphics Mode in Google Earth

.. figure:: /media/impexp_kml_export_googleearth_directx_fig.png
   :name: pic_kml_collada_gltf_export_directx
   :align: center

   KML/COLLADA models rendered with DirectX, highlighting surface borders are noticeable everywhere

.. figure:: /media/impexp_kml_export_googleearth_opengl_fig.png
   :name: pic_kml_collada_gltf_export_opengl
   :align: center

   The same scene rendered in OpenGL mode