.. _impexp_preferences_export_general:

General
^^^^^^^

This preferences dialog lets you define general settings affecting
the export operation.

.. figure:: /media/impexp_export_preferences_general_fig.png
   :name: impexp_export_preferences_general_fig
   :align: center

   Export preferences – General options.

**General options**

In the first part of the dialog [1], you can choose the CityGML version that
shall be used for exports. The default value is CityGML 2.0, which
is the current version of the OGC CityGML Encoding Standard. In addition, also the
previous version 1.0 is still supported.

When exporting data in CityGML format, this option obviously determines whether
the data is encoded using the CityGML XML schemas of version 2.0 or 1.0.
But since CityGML is not just an encoding but also a conceptual data model,
this option has other impacts as well. Most importantly, feature types such
as bridges and tunnels are not available in CityGML 1.0. When choosing
CityGML 1.0 on this preferences dialog, these feature types cannot be selected
in the corresponding feature type filter (see :numref:`impexp_export_feature_type_filter`)
and will also not be exported from the database. Thus, even if you choose
CityJSON as encoding format for the export operation and your database contains
bridges or tunnels, they will not be written to the output file if the
CityGML version is set to 1.0 here.

The *Cancel export immediately in case of errors* option [1] lets you define how the export
operation should deal with error situations during database exports. By default, the process
*fails fast* on errors and thus aborts immediately. Disable this option in case you rather want the
export operation to continue on errors and complete the export process if possible. The errors
encountered during the export process are always recorded in the log in both cases.

By default, the export operation first *computes the number top-level objects* [1] matching
the provided filters on the *Export* tab before starting the export process. This number is
printed to the log and also used to render a progress bar for the export operation. However,
computing this number can take a long time on large databases. Thus, this option can be disabled
so that the export process starts immediately instead.

For every city object in the database, a bounding box can be stored in the
ENVELOPE column of the CITYOBJECT table. However, when exporting data, only the
bounding boxes of *top-level objects* [1] are written to the dataset by default.
You can change this default behaviour and choose to export bounding boxes *for all
objects* or even to *not export object bounding boxes* at all instead.

As described in :numref:`impexp_citygml_export_chapter`, the export operation
supports using compressed output formats like GZIP and ZIP, which helps to keep file
sizes small. You can choose whether *CityGML* (default) or *CityJSON* shall
be used as data encoding for compressed formats using the drop-down list offered by this
preferences dialog [1].

**Dataset metadata**

The metadata attributes ``name`` and ``description`` of the output datasets can be specified as free text or using
tokens, which are described in the section (:numref:`impexp_preferences_export_tiling_chapter`) in details.
The following rules of using the tokens are applied:

* ``row/column``: If tiling is enabled, row and column are taken from the active tile. Otherwise both are set to 0.
* ``minx/miny/maxx/maxy``: If the option is enabled to calculate the bounding box for the entire dataset, the values
  are taken from this bounding box. Otherwise, they are set to 0.0.

In addition, a bounding box embracing all top-level features in the output file can be
written to the dataset. Simply enable the corresponding option shown in
:numref:`impexp_export_preferences_general_fig`. The bounding box will be
calculated anew for every export process. This might take some time depending
on the number of features to be exported.

For tiled exports, also the tile extent can be used as bounding box for
the entire dataset instead. The tile extent follows from the specified bounding
box filter and the number of rows and columns (see :numref:`impexp_export_bbox_filter`) and thus does not
need to be calculated. However, note that the tile extent might be larger
or even smaller than the actual extent calculated from the features.