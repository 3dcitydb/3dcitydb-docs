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

As described in :numref:`impexp_citygml_export_chapter`, the export operation
supports using compressed output formats like GZIP and ZIP, which helps to keep file
sizes small. You can choose whether *CityGML* (default) or *CityJSON* shall
be used for data encoding [1] in this dialog.

**Bounding box options**

For every city object in the database, a bounding box can be stored in the
ENVELOPE column of the CITYOBJECT table. However, when exporting data, only the
bounding boxes of *top-level objects* [2] are written to the dataset by default.
You can change this default behaviour and choose to export bounding boxes *for all
objects* or even to *not export object bounding boxes* at all instead.

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