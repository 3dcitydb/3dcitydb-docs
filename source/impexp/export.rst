.. _impexp_citygml_export_chapter:

Export
------

.. toctree::
   :hidden:

   export-filters/feature-version
   export-filters/attribute
   export-filters/sql
   export-filters/lod
   export-filters/feature-counter
   export-filters/bbox
   export-filters/feature-type
   xml-query/index
   export-preferences/index

3D city model content stored in a 3D City Database can be exported
as CityGML and CityJSON datasets on the *Export* tab
of the operations window.

.. figure:: /media/impexp_CityGML_export_dialog_fig.png
   :name: impexp_CityGML_export_dialog_fig
   :align: center

   The export dialog.

**Output file selection**

At the top of the export dialog, the folder and filename of the target
dataset must be specified [1]. You can either enter the output file manually
or open a file selection dialog via the *Browse* button. The export operation
supports the output file formats listed below. Simply make sure the
output file ends with the file extension of the format you want to export.

.. list-table::  *Supported output file formats and extensions*
   :align: center
   :name: export_supported_file_formats

   * - | **Format**
     - | **File extensions**
   * - | **CityGML (version 2.0 or 1.0)**
     - | \*.gml, \*.xml
   * - | **CityJSON (version 1.0)**
     - | \*.json, \*.cityjson
   * - | **GZIP compressed file**
     - | \*.gz, \*.gzip
   * - | **ZIP archive**
     - | \*.zip

For compressed formats, you can specify whether the data should
be encoded in CityGML or CityJSON using the *General* preference settings
(see :numref:`impexp_preferences_export_general`). When choosing ZIP
as target format, additional export files such as texture images are
also written to the ZIP archive by default.

**Coordinate transformation**

In general, coordinate values of geometry
objects are associated with the coordinate reference system defined for
the 3D City Database instance during setup and are exported “as is” from
the database. The export operation allows a user to apply a coordinate
transformation to another reference system during export. The target
coordinate reference system is chosen from the corresponding drop-down
list [2]. This list can be augmented with user-defined reference systems
(cf. :numref:`impexp_crs_management_chapter` for more details). When picking the entry *"Same as
in database"*, no coordinate transformation is applied (default behavior).

**Export filters**

Similar to the import process, the export operation offers thematic and spatial filters to
restrict an export to a subset of the 3D city model content stored in the database [3].
The following filters are available and discussed in separate sections of this chapter:

- :numref:`%s <impexp_export_feature_version_filter>` :ref:`impexp_export_feature_version_filter`
- :numref:`%s <impexp_export_attribute_filter>` :ref:`impexp_export_attribute_filter`
- :numref:`%s <impexp_export_sql_filter>` :ref:`impexp_export_sql_filter`
- :numref:`%s <impexp_export_lod_filter>` :ref:`impexp_export_lod_filter`
- :numref:`%s <impexp_export_feature_counter_filter>` :ref:`impexp_export_feature_counter_filter`
- :numref:`%s <impexp_export_bbox_filter>` :ref:`impexp_export_bbox_filter`
- :numref:`%s <impexp_export_feature_type_filter>` :ref:`impexp_export_feature_type_filter`

To enable a filter, simply select its checkbox. This will automatically make
the filter dialog visible. Make sure to provide the mandatory input for the filter
to work correctly. If more than one filter is enabled, the filters are combined in a
logical AND operation, i.e. all filter criteria must be fulfilled for a city object to
be exported. If no checkbox is enabled, no filters are applied and, thus,
all features contained in the database will be exported.

.. note::
   All export filters are only applied to *top-level features* but *not to nested
   sub-features*.

**Advanced XML-based queries**

Besides the simple export filters discussed above, the export can also be controlled through
a more advanced query expression that offers logical operators (AND, OR,
NOT) to combine thematic and spatial filters to complex conditions.
Moreover, it allows for defining projections on the properties of the
exported city objects and provides a filter for different appearance
themes. All simple filters from above are, of course, also
available for query expressions.

Query expressions are encoded in XML using a ``<query>`` root element. The
query language has been developed for the purpose of the 3D City Database
but is strongly inspired by and very similar to the *OGC Filter Encoding
2.0* and the query expressions used by the *OGC Web Feature
Service 2.0*.

To provide an XML-based query expression, click on *XML query* [5]
at the bottom right of the export dialog
(cf. :numref:`impexp_CityGML_export_dialog_fig`). The
simple filter dialogs will be replaced with an XML input field
like shown below.

.. figure:: /media/impexp_XML_query_dialog_fig.png
   :name: impexp_XML_query_dialog_fig
   :align: center

   Input field to enter an XML-based query expression.

The XML query is entered in [1]. This requires knowledge about the
structure and the allowed elements of the query language. A
documentation of the query language is provided in :numref:`impexp_xml_queries_chapter`.

The *new query* button |new_query_icon| on the right side of the input field
[2] can be used to create an empty query expression that contains all
allowed elements of the query language. The *copy query* button |copy_query_icon| translates all
settings made for the simple filters to an XML query representation.
The results of both actions can therefore be used as starting
point for defining your own query expression. The *validate query*
button |validate_query_icon| [2] performs a validation of the query entered in [1]
and prints the validation report to the console window. Only valid query
expressions are accepted by the export operation. The *Simpe filter*
button [3] takes you back to the simple filter dialog.

.. note::
  You can also use an external XML editor to write query expressions.
  External editors might be more comfortable to use and often offer
  additional tools like auto completion. The XML Schema definition of the
  query language (required for validation and auto completion) can be
  exported via “File -> Save Settings XSD As…” on the main menu of the
  Importer/Exporter (cf. :numref:`impexp_gui_chapter`). Make sure to use a ``<query>`` element
  as root element of the query expression in your external XML editor.

**Tiled exports**

When exporting 3D city model content to a single output file, the file
size may quickly grow. For CityGML files, the Importer/Exporter supports writing
files of arbitrary size (only limited by the file system of the
operating system). However, such files might become too large to be processed by
other applications. *Tiled exports* are useful in this case because the
export is split into a regular grid of tiles each of which is written
to its own output file. Every tile only contains a subset of all
city objects to be exported and, thus, the file sizes will be smaller compared
to a single output file. To enable tiling, you must use a bounding filter
(see :numref:`impexp_export_bbox_filter`) or a corresponding XML query
expression.

You still have to specify just a single output file in [1] for tiled exports.
Each tile  is automatically stored in its own subfolder named ``tile_x_y``
with x and y being the row and column of the tile in the grid.
The names of both the subfolders and the tile files can be augmented
with tile-specific suffixes (see the tiling options
in chapter :numref:`impexp_preferences_export_tiling_chapter`).

**Export preferences**

More fine-grained preference settings affecting the export operation are
available on the *Preferences* tab of the operations window [6]. Make sure to
check these settings *before* starting the export process. A full
documentation of the export preferences is provided in :numref:`impexp_citygml_export_preferences_chapter`.
The following table provides a summary overview.

.. list-table:: Summery overview of the export preferences
   :name: citygml_export_preferences_summary_table
   :widths: 30 70

   * - | **Preference name**
     - | **Description**
   * - | :ref:`impexp_preferences_export_general`
     - | General options like the CityGML version to be used for exports.
   * - | :ref:`impexp_preferences_export_tiling_chapter`
     - | Settings for tiled exports. Requires that tiling is enabled on the bounding box filter.
   * - | :ref:`impexp_preferences_export_cityobjectgroup`
     - | Defines whether group members are exported by value or by reference.
   * - | :ref:`impexp_export_preferences_address_chapter`
     - | Controls the way in which xAL address fragments are exported from the database.
   * - | :ref:`impexp_export_preferences_appearance_chapter`
     - | Defines whether appearance information should be exported.
   * - | :ref:`impexp_export_preferences_citygml_general_chapter`
       | (CityGML only)
     - | General options affecting the CityGML export.
   * - | :ref:`impexp_export_preferences_xlinks_chapter`
       | (CityGML only)
     - | Controls whether referenced features or geometry objects are exported using XLinks or as deep copies.
   * - | :ref:`impexp_export_preferences_xsl_transformation`
       | (CityGML only)
     - | Defines one or more XSLT stylesheets that shall be applied to the exported city objects in the given order before writing them to file.
   * - | :ref:`impexp_export_preferences_cityjson_chapter`
       | (CityJSON only)
     - | General options affecting the CityJSON export.
   * - | :ref:`impexp_export_preferences_resources_chapter`
     - | Allocation of computer resources used in the export operation.

**Starting the export process**

Once all export settings are correct, the *Export* button [4] starts the
export process (cf. :numref:`impexp_CityGML_export_dialog_fig`). If a database connection has not been
established manually beforehand, the currently selected entry on the
*Database* tab is used to connect to the 3D City Database.
The separate steps of the export process as well as all errors and warnings
that might occur during the export are reported to the console window. The
overall progress is shown in a separate status window. This status
window also offers a *Cancel* button to abort the export
process at any time.

.. caution::
  While CityGML files can be written in a streaming fashion (i.e., one top-level feature
  after the other), which allows for keeping the memory footprint low,
  large parts of a CityJSON file must be kept in main memory before
  the entire file can be written to the file system.
  In order to avoid memory issues when exporting to CityJSON, it is
  therefore **strongly recommended** to reduce the number of city objects
  to be exported to the CityJSON output file. This can be achieved, for
  instance, by applying export filters or by using a tiled export (recommended).

.. |new_query_icon| image:: /media/new_query.svg

.. |copy_query_icon| image:: /media/copy_query.svg

.. |validate_query_icon| image:: /media/validate_query.svg