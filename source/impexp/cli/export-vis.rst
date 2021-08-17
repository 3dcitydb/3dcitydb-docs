.. _impexp_cli_export_vis_command:

Visualization export command
----------------------------

**Synopsis**

.. code-block:: bash

   impexp export-vis [-hjVz] [--ade-extensions=<folder>] [-c=<file>]
                     [--log-file=<file>] [--log-level=<level>] -o=<file>
                     [--pid-file=<file>] [--plugins=<folder>]
                     [--use-plugin=<plugin[=true|false]>[,<plugin
                     [=true|false]>...]]... [--worker-threads=<threads[,max]
                     >] [-D=<form[=pixels]>[,<form[=pixels]>...] [-D=<form
                     [=pixels]>[,<form[=pixels]>...]]... -l=<0..4 | halod>
                     [-a=<theme>]] [[[-t=<[prefix:]name>[,<[prefix:]
                     name>...]]... [--namespace=<prefix=name>[,
                     <prefix=name>...]]...] [[-r=<version>] [-R=<timestamp[,
                     timestamp]>]] [-i=<id>[,<id>...] [-i=<id>[,
                     <id>...]]...] [-b=<minx,miny,maxx,maxy[,srid]>]
                     [-g=<rows,columns | auto[=length]>] [-s=<select>]]
                     [[-B] [--no-surface-normals] [-C] [-f=<0..1>]
                     [-x=<mode>] [--no-pot-atlases]] [-G
                     [--gltf-version=<version>] [--gltf-converter=<file>]
                     [--gltf-embed-textures] [--gltf-binary]
                     [--gltf-draco-compression] [-m]] [[-A=<mode>]
                     [-O=<number|globe|generic>]
                     [--google-elevation-api=<api-key>]
                     [--transform-height]] [[-T=<database>] -H=<host>
                     [-P=<port>] -d=<name> [-S=<schema>] -u=<name> [-p
                     [=<password>]]] [@<filename>...]

**Description**

The ``export-vis`` command exports top-level city objects from the 3D City
Database in KML/COLLADA/glTF format for visualization. It corresponds to the
visualization export operation offered on the *VIS Export* tab
of the graphical user interface (see :numref:`impexp_citygml_export_chapter`).
The command provides a range of options to adapt the export process.
If you miss settings available for the GUI version of this command,
you can use the :option:`--config` option to pass a config file
containing these settings.

**General options**

.. option:: -o, --output=<file>

   Specify the path where where the main KML output file should be saved.

.. option:: -z, --kmz

   Flag to indicate that KML/COLLADA output should be packaged into a
   KMZ archive per tile and display form (see :numref:`impexp_kml_export_preferences_general_chapter`
   for more details). This option cannot be used with glTF exports.

.. option:: -j, --json-metadata

   Flag to indicate that metadata about the exported top-level
   features should be recorded in a separate JSON file.
   Further details are provided in :numref:`impexp_kml_export_preferences_general_chapter`.

.. option:: --worker-threads=<threads[,max]>

   Number of parallel threads to use for the visualization export process.
   You can also specify a minimum and maximum number of threads
   separated by commas.

**Display options**

.. option:: -D, --display-form=<form[=pixels]>[,<form[=pixels]>...]

   This option controls the display forms to be exported. The display forms
   are given as comma-separated list of one or more ``form[=pixels]`` pairs.
   Allowed values for the display form are ``collada``, ``geometry``, ``extruded``, and
   ``footprint``. For each display form, its visibility in terms of screen pixels
   can be optionally defined. If omitted, the display form will always be visible
   in the viewer. More information about display forms and their visibility
   can be found in :numref:`impexp_kml_export_chapter`.

.. option:: -l, --lod=<0..4 | halod>

   Define the LoD representation of the city objects that shall be used
   for creating the visualization models. The value must either be given as
   integer between ``0`` and ``4`` to export a specific LoD. Alternatively,
   you can set the value to ``halod`` in which case the highest available
   LoD representation will be chosen for each city object.

.. option:: -a, --appearance-theme=<theme>

   Appearance theme to use for texturing and coloring the exported city objects.
   Use ``none`` as value to address appearance features in the database lacking
   a theme attribute. This option is only considered for COLLADA/glTF exports.

**Query and filter options**

The ``export-vis`` command offers additional options to define both thematic and spatial filters
that are used to restrict the visualization export to a subset of the top-level city objects stored in
the 3D City Database.

.. option:: -t, --type-name=<[prefix:]name>[,<[prefix:]name>...]

   Comma-separated list of one or more names of the top-level feature types to be exported. The
   type names are case sensitive and shall match one of the official CityGML feature
   type names or a feature type name from a CityGML ADE. To avoid ambiguities,
   you can use an optional prefix for each name. The prefix must be associated with the
   official XML namespace of the feature type. You can either use the official CityGML namespace
   prefixes listed in :numref:`impexp_toplevel_feature_types_table`. Or you can use
   the :option:`--namespace` option to declare your own prefixes.

.. option:: --namespace=<prefix=name>[,<prefix=name>...]

   Used to specify namespaces and their prefixes as comma-separated list of one or more
   ``prefix=name`` pairs. The prefixes can be used in other options such as :option:`--type-name`.

.. option:: -r, --feature-version=<version>

   Specify the version of the top-level features to use for the visualization export. Allowed values are
   ``latest``, ``at``, ``between``, ``terminated``, ``terminated_at`` and ``all``. When choosing
   ``latest``, only those features that have not been terminated in the database are exported,
   whereas ``all`` will export all features. You can also choose to export only features that were
   valid at a given timestamp using ``at`` or for a given time range using ``between``. Likewise,
   ``terminated`` will return all terminated features whereas ``terminated_at`` will select features that
   were terminated at a given timestamp. In all cases, timestamps must be provided using the
   :option:`--feature-version-timestamp` option. Further details about the feature version filter
   are available in :numref:`impexp_export_vis_feature_version_filter`.

.. option:: -R, --feature-version-timestamp=<timestamp[,timestamp]>

   One or two timestamps to be used with the :option:`--feature-version` option. A
   timestamp can be given as date in the form ``YYYY-MM-DD`` or as date-time specified as
   ``YYYY-MM-DDThh:mm:ss[(+|-)hh:mm``. The date-time format supports an optional
   UTC offset. Use one timestamp with the ``at`` and ``terminated_at`` values and two timestamps
   separated by comma with the ``between`` value of the :option:`--feature-version` option.

.. option:: -i, --resource-id=<id>[,<id>...]

   Comma-separated list of one or more identifiers. Only top-level features having a matching
   value for their identifier attribute will be exported.

.. option:: -b, --bbox=<minx,miny,maxx,maxy[,srid]>

   2D bounding box to use as spatial filter. The bounding box is given by four coordinates
   that define its lower left and upper right corner. By default, the coordinates are assumed
   to be in the same CRS that is used by the 3DCityDB instance. Alternatively, you
   can provide the database ``srid`` of the CRS associated with the coordinates as fifth value
   (e.g. ``4326`` for WGS84). All values must be separated by commas. The bounding box is evaluated
   against the GMLID column of the CITYOBJECT table.

.. option:: -g, --tiling=<rows,columns | auto[=length]>

   Use this option to enable tiling for the visualization export. The bounding box of the
   entire export area is split into a regular grid, and each grid cell is exported as
   separate tile. You can either define the number of ``rows`` and ``columns`` separated by a
   comma for the grid, or use ``auto[=length]`` as value. In the latter case, the bounding
   box will be automatically split into tiles having a size of the given ``length`` value.
   If ``length`` is omitted, a default side length of 125m is used. The bounding box used
   for tiling is either specified through the :option:`--bbox` option or calculated
   automatically in case :option:`--bbox` is not provided. More information about tiled exports
   is provided in :numref:`impexp_kml_export_chapter`.

.. option:: -s, --sql-select=<select>

   Provide an SQL SELECT statement to be used as SQL filter when querying the database.
   In general, any SELECT statement can be used as long as it returns a list
   of database IDs of the selected city objects (see :numref:`impexp_export_vis_sql_filter` for more information).
   You can also use an @-file to provide the SELECT statement (see :numref:`impexp_cli_argument_files`).

**COLLADA/glTF rendering options**

The following options are specific to COLLADA/glTF exports. They are ignored
if the list of display forms given by the :option:`--display-form` option
does not contain the value ``collada``.

.. option:: -B, --double-sided

   Flag to disable backface culling and rather force a viewer to render both sides
   of each polygons. Be aware that this might decrease the visualization performance.

.. option:: --no-surface-normals

   If this flag is set, surface normals of polygons are not stored in the output.
   When exporting textured models, surface normals often do not increase the visual quality
   and can be omitted.

.. option:: -C, --crop-textures

   Use this flag to let the export operation cut each texture image to the minimum size
   required for texturing the corresponding polygon. This option can help to avoid loading
   massive texture data into a viewer.

.. option:: -f, --texture-scale-factor=<0..1>

   Scale down texture images by the given factor. A value between ``0`` and ``1`` must be
   provided. The default value of ``1`` means no scaling.

.. option:: -x, --texture-atlas=<mode>

   Specify if and how texture atlases should be created for each top-level feature
   from the exported texture images. Allowed values are ``none``, ``basic``,
   ``tpim``, and ``tpim_wo_rotation``. By default, texture atlases
   are created for all visualization exports using the ``basic`` mode.
   Use ``none`` to disable the creation of texture atlases. The further values correspond
   to the names of the supported algorithms for creating texture atlases as described in
   :numref:`impexp_kml_export_preferences_general_chapter`.

.. option:: --no-pot-atlases

   By default, texture atlases are created with power-of-two (PoT) side lengths. Use this flag
   to disable the default behaviour.

**glTF export options**

The ``export-vis`` command supports exporting glTF models by converting the COLLADA
output to glTF on the fly. This is achieved by using the open source
`COLLADA2glTF <https://github.com/KhronosGroup/COLLADA2GLTF/>`_ converter tool.
The following options control the glTF export.

.. option:: -G, --gltf

   Set this flag on your command line to enable the glTF export. This requires that
   ``collada`` has been selected as display form with the :option:`--display-form` option.

.. option:: --gltf-version=<version>

   Specify the glTF version to use for the export. Allowed values are ``1.0`` and
   ``2.0`` (default).

.. option:: --gltf-converter=<file>

   Provide the path to the executable of the COLLADA2glTF converter tool.
   By default, the COLLADA2glTF tool shipped with the Importer/Exporter
   is used in the conversion. Be careful when using a different version of the tool as this
   might result in unexpected behaviour. More information is provided
   in :numref:`impexp_kml_export_preferences_general_chapter`.

.. option:: --gltf-embed-textures

   By default, texture images are exported as separate files relative to the location of the glTF output.
   With this flag, texture images are rather embedded in the glTF files by encoding the texture
   data using a base 64 encoding.

.. option:: --gltf-binary

   Flag to indicate that the glTF output should be converted and compressed to
   binary glTF format.

.. option:: --gltf-draco-compression

   Flag to indicate that geometry data should be compressed using the
   `Google Draco compression technology <https://github.com/google/draco>`_.
   Draco compression is only available for glTF version 2.0, so make sure
   the :option:`--gltf-version` option is correctly set.

.. option:: -m, --remove-collada

   Use this flag to remove the COLLADA files after the conversion and only keep
   the glTF output as result.

.. include:: database-options.rst

**Examples**

.. code-block:: bash

   $ impexp export-vis -T oracle -H localhost -d citydb_v4 -u citydb_user -p my_password \
                       -D collada -l 2 -a visual -o my_vis.kml

Export all city objects from the database as COLLADA in LoD2. The appearance theme
``visual`` is used for texturing/coloring the objects. The output is stored in
the main KML file ``my_vis.kml`` that can be loaded with a viewer.
The 3DCityDB to connect to is supposed to be running on an Oracle database on
the same machine. The connection will be established to the ``citydb_v4`` database
with the user ``citydb_user`` and the password ``my_password``.

.. code-block:: bash

   $ impexp export-vis -H localhost -d citydb_v4 -u citydb_user -p my_password \
                       -D footprint=50,extruded=125,collada=200 -l halod \
                       -t Building -b 13.3508824,52.4799281,13.3578297,52.4862805,4326 \
                       -g 3,4 -o my_vis.kml

Export all ``Building`` features inside the given bounding box as ``footprint``,
``extruded`` and ``collada`` models using their highest available LoD representation.
Tiling is enabled for this export, and 3x4 tiles are created per display form.
Each display form is assigned a different visibility value. A ``footprint``
tile will become visible in the viewer as soon as it occupies
50 square pixels of screen space. When zooming in, the viewer will switch to ``extruded`` and
finally to ``collada`` as soon as their visibility value is reached.

.. code-block:: bash

   $ impexp export-vis -H localhost -d citydb_v4 -u citydb_user -p my_password \
                       -D geometry -l 2 \
                       -t CityFurniture,Bridge -g auto \
                       -z -j -o my_vis.kml

Export all ``CityFurniture`` and ``Bridge`` objects as ``geometry`` from LoD2.
Again, tiling is applied to the export. Since a :option:`--bbox` option is not provided,
the bounding box is automatically calculated from all features. This bounding
box is then automatically tiled using a default side length of 125m per tile.
Moreover, each tile file is compressed as ZIP archive and an additional JSON
file containing metadata about all exported features is created.

.. code-block:: bash

   $ impexp export-vis -H localhost -d citydb_v4 -u citydb_user -p my_password \
                       -D collada -l halod \
                       -i ID_0815,ID_0816 --double-sided \
                       -a visual -x tpim_wo_rotation -C \
                       -o my_vis.kml

Export the top-level city objects with the identifiers ``ID_0815`` and ``ID_0816``
as ``collada`` using their highest available LoD representation. Textures shall
be exported from the appearance theme ``visual``. The texture images are first
cropped and then packaged into texture atlases using the ``tpim_wo_rotation`` algorithm.
Moreover, backface culling is disabled for this export.

.. code-block:: bash

   $ impexp export-vis -H localhost -d citydb_v4 -u citydb_user -p my_password \
                       -D collada -l 3 \
                       -s "select cityobject_id from cityobject_genericattrib \
                       where attrname='energy_level' and realval < 12" \
                       -G --gltf-binary --gltf-draco-compression -m
                       -o my_vis.kml

Export all city objects satisfying the given SQL SELECT statement as
``collada`` based on their LoD3 geometries. The models are converted
to binary glTF format and Draco compression is applied to the geometries.
The intermediate COLLADA output is removed for the final result.

