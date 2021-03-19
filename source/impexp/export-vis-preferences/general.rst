.. _impexp_kml_export_preferences_general_chapter:

General
^^^^^^^

Some common features of the exported files, especially those related to
tiling options, can be set under the preferences tab, node
*KML/COLLADA/glTF Export*, subnode *General*.

.. figure:: /media/impexp_kml_export_preferences_general_fig.png
   :name: pic_kml_collada_gltf_preferences_general
   :align: center

   General settings for the KML/COLLADA/glTF export

Create glTF model
"""""""""""""""""

In addition to COLLADA models, the Importer/Exporter can also create
glTF models for efficient loading and rendering of 3D contents on
WebGL-enabled web browsers. If the “\ *Create glTF model”* option is
activated, the Importer/Exporter requires an open source tool called
`COLLADA2glTF <https://github.com/KhronosGroup/COLLADA2GLTF/wiki>`_
to convert the exported COLLADA models to glTF models.
The COLLADA2glTF tool is available for Windows, Linux, and Mac OS X and
has been installed together with the Importer/Exporter and located in
the subfolder *contribs/collada2gltf* of the installation directory. Per
default, the relative path (depending on the operating system in use) of
the COLLADA2glTF tool is proposed in the *Path of the COLLADA2glTF tool*
text field whose value will be used by the Importer/Exporter to run the
target executable file. Thus, if you want to use another version of the
COLLADA2glTF tool, its absolute path has to be manually specified using,
for example, the *Browse* button to open a file selection dialog.
Starting with the Importer/Exporter version 4.0.0 however, version 2.1.0
or later of the COLLADA2glTF tool is required in order to enable support
for both glTF version 1.0 and 2.0. The pre-installed COLLADA2glTF
binaries come already in version 2.1.3. It is also possible to just
export glTF models without COLLADA models by activating the *Do not
create COLLADA (.dae) files* checkbox.

When exporting a textured city object in glTF, its texture images can
either be encoded in the Base64 format and embedded into the glTF file,
or saved as separate image files in the same directory as the glTF file
having references to them. This can be controlled by the setting *Embed
textures in glTF (.gltf) files*. In fact, both options have their pros
and cons: the glTF file without embedded texture images allows client
applications to realize an incremental loading effect which may give a
better user experience, since the geometry contents and texture images
can be loaded and rendered consecutively. However, this will result in a
large amount of AJAX requests which might possibly impair the overall
visualization performance especially when a large number of city objects
are loaded simultaneously. This issue can be avoided by choosing the way
of embedding the texture images into the glTF file. However, loading of
the geometries and textures of a city object must be performed within
one AJAX request that may slightly slow down the speed of the
visualization of individual city object.

.. note::
   The exported glTF file can be further converted to the so-called
   *binary glTF* file which is a binary container for glTF models and
   allows for faster loading and processing 3D objects. However, this
   conversion process is currently not yet supported by the
   KML/COLLADA/glTF Exporter and therefore needs to be carried out later
   using third party tools which can be found on the
   https://github.com/KhronosGroup/glTF website.

Export in kmz format
""""""""""""""""""""

Determines in which format single files and tiled exports should be
written: kmz when selected, kml when not. Whatever format is chosen, the
main file (so called master file, pointing to all others) will always be
a kml file, all other files will comply with this setting.

Tests have shown shorter loading times (in Google Earth) for the kml
format (as opposed to kmz) when loading from the local hard disk. The
Earth Browser's stability also seems to improve when using the
uncompressed format. On the other hand, when loading files from a server
kmz reduces the amount of requests considerably, thus increasing
performance. Kmz is also recommended for a better overview since kml
exports may lead to a large number of directories and files.

The *Export in kmz format* and *Create glTF model* options are mutually
exclusive. A warning message will be displayed when the user trys to
choose the both.

Show bounding box borders
"""""""""""""""""""""""""

When exporting a region of interest via the bounding box option in the
KML/COLLADA/glTF Export tab, this checkbox specifies whether the borders
of the whole bounding box will be shown or not. The frame of the
bounding box is four times thicker than the borders of any single tile
in a tiled export.

Show tile borders
"""""""""""""""""

Specifies whether the borders of the single tiles in a tiled export will
be shown or not.

Tile side length for automatic tiling
"""""""""""""""""""""""""""""""""""""

Applies only to automatically tiled exports and sets the approximate
square size of the tiles. Since the Bounding Box settings in the
*KML/COLLADA/glTF Export* tab are the determining factor for the area to
be exported and have priority over this setting, the resulting tiles may
not be perfectly square or have exactly the side length fed into this
field.

Each CityObject in an own region
""""""""""""""""""""""""""""""""

The visibility of the objects exported can be further fine-tuned by this
option. While the visibility settings on the main *KML/COLLADA/glTF
Export* tab apply to the whole area (*no tiling*) or to each tile
(*automatic*, *manual*) being exported, this checkbox allows to
individually define a KML <Region> for every single city object. The
limits of the object’s region are those of the object’s CityGML
Envelope.

.. note::
   This setting only takes effect when if the export KML/KMZ files
   are opened with Google Earth (Pro). The Cesium-based 3D web client will
   silently ignore this setting.

Following the KML Specification [Wils2008]_, each KML ``<Region>`` is
defined inside a KML ``<NetworkLink>`` and has an associated KML ``<Link>``
pointing to a file. This implies when this option is chosen a subfolder
is created for each object exported, identified by the object’s gmlId.
The object’s subfolder will contain any KML/COLLADA/glTF files needed
for the visualization of the object in the Earth browser. This folder
structure (which can contain a large number of subfolders) is required
for the KML ``<Region>`` visibility mechanism to work.

When active, the parameters affecting the visibility of the object’s KML
``<Region>`` can be set through the following related fields.

The field *visible from* determines from which size on screen the
object’s KML ``<Region>`` becomes visible, regardless of the visibility
value of the containing tile, if any. Since this value is the same for
every single object and they have all different envelope sizes a good
average value should be chosen.

The field *view refresh mode* specifies how the KML ``<Link>`` corresponding
to the KML ``<Region>`` is refreshed when the geographic view changes. May
be one of the following:

-  **never** - ignore changes in the geographic view.

-  **onRequest** - refresh the content of the KML ``<Region>`` only when the user explicitly requests it.

-  **onStop -** refresh the content of the KML ``<Region>`` *n* seconds after movement stops, where *n* is specified in the field *view refresh time*.

-  **onRegion** - refresh the content of the KML ``<Region>`` when it becomes active.

As stated above, the field *view refresh time* specifies how many
seconds after movement stops the content of the KML ``<Region>`` must be
refreshed. This field is only active and its value is only applied when
*view refresh mode* is onStop.

Write JSON file
"""""""""""""""

After exporting some cityobjects in KML/COLLADA/glTF you may need to
include them into websites or somehow embed them into HTML. When working
with tiled exports referring to a specific object inside the
KML/COLLADA/glTF files can become a hard task if the contents are loaded
dynamically into the page. It is impossible to tell beforehand which
tile contains which object. This problem can be solved by using a JSON
file that is automatically generated when this checkbox is selected.

In the resulting JSON file each exported object is listed, identified by
its gmlId acting as a key and some additional information is provided:
the envelope coordinates in CRS WGS84 and the tile, identified by row
and column, the object belongs to. For untiled exports the tile’s row
and column values are constantly 0.

This JSON file has the same name as the so-called master file and is
located in the same folder. Its contents can be used for indexed search
of any object in the whole KML/COLLADA/glTF export.

.. code-block:: json

   {
      "BLDG_0003000b0013fe1f": {
         "envelope": [13.411962, 52.51966, 13.41277, 52.520091],
         "tile": [1, 1]
      },
      "BLDG_00030009007f8007": {
         "envelope": [13.406815, 52.51559, 13.40714, 52.51578],
         "tile": [0, 0]
      }
   }

The JSON file can automatically be turned into JSONP (JSON with padding)
by means of adding a function call around the JSON contents. JSONP
provides a method to request data from a server in a different domain,
something typically forbidden by web browsers since it is considered a
cross-site-scripting attack (XSS). Thanks to this minimal addition, the
JSON file contents can be more easily embedded into webpages or
interpreted by web kits without breaking any rules. The function call
name to be added to the original JSON contents is arbitrary and must
only be entered in the callback method name field.

.. note::
   Another solution for overcoming the restriction on making
   cross-domain requests is to make use of the *Cross-Origin Resource
   Sharing* (CORS) mechanism by enabling the web server to include
   additional HTTP headers in the response that allows web browsers to
   access the requested data. When working with the
   3DCityDB-Web-Map-Client, it is required that the web server storing the
   KML/COLLADA/glTF datasets must be CORS-enabled. In this case, there is
   no need anymore to use this JSONP solution and the option *of type
   JSONP* should be deactivated.