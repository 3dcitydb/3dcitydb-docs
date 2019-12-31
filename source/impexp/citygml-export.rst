.. _impexp_citygml_export_chapter:

Exporting to CityGML
--------------------

3D city model content stored in a 3D City Database instance can be fully
or partially exported as CityGML datasets. The CityGML export is
available on the Export tab of the operations window as depicted in the
following figure.

|image79|

Figure 68: The CityGML export dialog.

**Output file selection.** At the top of the export dialog, the folder
and filename of the target CityGML dataset must be specified [1]. You
can either manually enter the target file or open a file selection
dialog via the *Browse* button.

The exporter supports the following file formats for writing CityGML
datasets: 1) regular XML files (*.gml, \*.xml), 2) GZIP compressed XML
files (*.gz, \*.gzip), and 3) ZIP archives (*.zip). Simply make sure to
add the file extension of the file format of your choice to the name of
the target file in [1]. When choosing ZIP as target format, then all
additional files such as texture images are also written into the ZIP
container per default.

The export operation supports tiled exports, which typically results in
multiple datasets being written to the file system. Nevertheless, also
for tiled exports, only a single target file must be specified. More
details on tiled exports are provided below and in chapter 5.6.2.2.

**Workspace selection.** If the 3D City Database instance is
version-enabled (Oracle only), the name of the *workspace* and the
*timestamp* from which the data shall be exported can be specified [2].
If no workspace is provided, the default workspace is assumed (Oracle:
*LIVE*).

**Coordinate transformation.** In general, coordinate values of geometry
objects are associated with the coordinate reference system defined for
the 3D City Database instance during setup and are exported “as is” from
the database. The export operation allows a user to apply a coordinate
transformation to another reference system during export. The target
coordinate reference system is chosen from the corresponding drop-down
list [3]. This list can be augmented with user-defined reference systems
(cf. chapter 5.6.4 for more details). When picking the entry “\ *Same as
in database*\ ”, no transformation will be applied (default behavior).

**Simple export filters.** Like the import of CityGML datasets, the
export operation supports thematic and spatial filter criteria to
restrict exports to subsets of the 3D city model content. The checkboxes
on the left side of the export dialog let you choose between an
*attribute filter*, an *SQL filter*, an *LoD filter*, a *feature*
*counter filter*, a spatial *bounding box filter* and a *feature type
filter* [4]. If more than one filter is chosen, then the filter criteria
are combined in a logical AND operation. If no checkbox is enabled, no
filter criteria are applied and thus all CityGML features contained in
the database will be exported.

The export filters work similar to the ones on the Import tab. Please
refer to chapter 5.3 for a description of the filter settings that are
common to both operations.

======================== ===========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================
-  *Attribute filter*    This filter lets you define values for the attributes gml:id, gml:name and citydb:lineage which must be matched by city objects to be exported. More than one gml:id can be provided in a comma-separated list. Multiple gml:name or citydb:lineage values are not supported though.
======================== ===========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================
-  *SQL filter*          The attribute filter only operates on predefined attributes (see above). To overcome this limitation, you can alternatively choose the *SQL Filter* tab and enter an arbitrary SELECT statement into the input field. The query must return a list of database ids of the city objects to be exported (i.e., references to the column ID of the table CITYOBJECT). The SQL filter is very powerful as you can access every column of every table in the 3DCityDB and make use of all functions and operations offered by the underlying database system to define your filter. More information about the SQL filter is provided in chapter 5.4.1.
-  *Bounding box filter* The bounding box filter takes the same parameters as on the Import tab. It is evaluated against the ENVELOPE column of the CITYOBJECT table. The user can choose whether the bounding box of top-level features only needs to *overlap with* or must be strictly *inside* the filter geometry to satisfy the filter. Alternatively, the export can be tiled by splitting the bounding box into a regular grid. The number of rows and columns can be defined by the user. Each tile of this grid is exported into its own file. To make sure that every city object is assigned to one tile only, the center point of its envelope is checked to be either inside or on the left or top border of the tile.
-  *LoD filter*          This filter allows for exporting only specific LoDs of the city objects. The LoD selection can be either AND or OR combined. City objects not having a spatial representation in one (OR) or all (AND) of the selected LoDs will not be exported. The *search depth* parameter specifies how many levels of nested city objects shall be considered when searching for matching LoD representations.
======================== ===========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================

When exporting 3D city model content to a single CityGML file, the file
size may quickly grow. Although the Importer/Exporter supports writing
files of arbitrary size (only limited by the file system of the
operating system), such files might become too big to be processed by
other applications. A bounding box filter with tiling enabled is useful
in this case because the contents of each tile are written to separate
and thus smaller files. The output files are put into subfolders, and
the names of both the subfolders and the output files can be augmented
with tile-specific suffixes (see the tiling options of the export
preferences).

.. note::
   Both the gml:name and the citydb:lineage filter internally use
   an SQL LIKE operator and wildcards for identifying matches. For example,
   if you provide the string “castle” as gml:name, this will be translated
   to “LIKE ‘%castle%’” in the SQL query.

.. note::
   When choosing a spatial *bounding filter*, make sure that
   *spatial indexes are enabled* so that filtering can be performed on the
   database (use the index operation on the Database tab to check the
   status of indexes, cf. chapter 5.2.2).

.. note::
   If the entire 3D city model stored in the 3DCityDB instance
   shall be exported with tiling enabled, then a bounding box spanning the
   overall area of the model must be provided. This bounding box can be
   easily calculated on the Database tab (cf. chapter 5.2.2).

.. note::
   Using the center point of the envelope as criterion for a tiled
   export has a side-effect when tiling is combined with the *counter
   filter*: the number of city objects on the tile can be less than the
   number of city objects returned by the database query because the tile
   check happens after the objects have been queried. Therefore, the
   *counter filter* only sets a possible maximum number in this filter
   combination. This is a correct behavior, so the Importer/Exporter will
   not report any errors.

.. note::
   The *feature type filter* in general behaves like for the
   CityGML import. However, regarding *city object groups* the following
   rules apply:

1) If only the feature type *CityObjectGroup* is checked, then all city
   object groups and all their group members (independent of their
   feature type) are exported.

2) If further feature types are selected in addition to
      *CityObjectGroup*, then only group members matching those feature
      types are exported. Of course, all features that match the type
      selection but are not group members are also exported.

**Advanced XML export query.** The export can also be controlled through
a more advanced query expression. In addition to the filter capabilities
explained above, a query expression offers logical operators (AND, OR,
NOT) that combine thematic and spatial filters to complex conditions.
Moreover, it allows for defining projections on the properties of the
exported city objects and provides a filter for different appearance
themes. Operators like the LoD filter or tiling are, of course, also
available for query expressions.

Query expressions are encoded in XML using a <citydb:query> element. The
query language used has been developed for the purpose of the 3DCityDB
but is strongly inspired by and very similar to the OGC Filter Encoding
2.0 standard and the query expressions used by the OGC Web Feature
Service 2.0 standard.

To enter an XML-based query expression, click on the *Use XML query*
button [6] at the bottom right of the export dialog (cf. Figure 68). The
simple filter settings dialog will be replaced with an XML input field
like shown below.

|image80|

Figure 69: Input field to enter an XML-based query expression for
CityGML exports.

The XML query is entered in [7]. This requires knowledge about the
structure and the allowed elements of the query language. A
documentation of the query language is provided in chapter 5.4.2.

The *new query* button |image81| on the right side of the input field
[8] can be used to create an empty query element that contains all
allowed subelements. The *copy query* button |image82| translates all
settings defined on the simple filter dialog (cf. Figure 68) to an XML
query. The results of both actions can therefore be used as starting
point for defining your own query expression. The *validate query*
button |image83| [8] performs a validation of the query entered in [7]
and prints the validation report to the console window. Only valid query
expressions are accepted by the export operation. The *Use simpe filter*
button [9] takes you back to the simple filter dialog.

You can also use an external XML editor to write XML query expressions.
External editors might be more comfortable to use and often offer
additional tools like auto completion. The XML Schema definition of the
query language (required for validation and auto completion) can be
exported via “Project Save Project XSD As…” on the main menu of the
Importer/Exporter (cf. chapter 5.1). Make sure to use a <query> element
as root element of the query expression in your external XML editor.

**Export preferences.** In addition to the settings on the Export tab,
more fine-grained preference settings affecting the CityGML export are
available on the Preferences tab of the operations window. Make sure to
check these settings before starting the export process. A full
documentation of the export preferences can be found in chapter 5.6.2.
The following table provides a summary overview.

==================== ===========================================================================================================================================
**Preference name**  **Description**
*CityGML version*    **CityGML version to be used for exports.**
*Tiling options*     **More settings for tiled exports. Requires that tiling is enabled on the bounding box filter.**
*CityObjectGroup*    **Defines whether group members are exported by value or by reference.**
*Address*            **Controls the way in which xAL address fragments are exported from the database.**
*Appearance*         **Defines whether appearance information is exported.**
*XLinks*             **Controls whether referenced features or geometry objects are exported using XLinks or as deep copies.**
*XSL transformation* **Defines one or more XSLT stylesheets that shall be applied to the exported city objects in the given order before writing them to file.**
**Resources**        Allocation of computer resources used in the export operation.
==================== ===========================================================================================================================================

Table 32: Summery overview of the export preferences.

**CityGML export.** Having completed all settings, the CityGML data
export is triggered with the *Export* button [5] at the bottom of the
dialog (cf. Figure 68). If a database connection has not been
established manually beforehand, the currently selected entry on the
Database tab is used to connect to the 3D City Database. Progress
information is displayed within a separate status window. This status
window also offers a *Cancel* button that lets a user abort the export
process. The separate steps of the export process as well as possible
error messages are reported to the console window.

.. |image79| image:: ../media/image90.png
   :width: 4.87795in
   :height: 6.45608in

.. |image80| image:: ../media/image91.png
   :width: 5.17391in
   :height: 3.23198in

.. |image81| image:: ../media/image92.png
   :width: 0.1875in
   :height: 0.1875in

.. |image82| image:: ../media/image93.png
   :width: 0.18681in
   :height: 0.18681in

.. |image83| image:: ../media/image94.png
   :width: 0.18681in
   :height: 0.18681in
