.. _impexp_citygml_import_chapter:

Import
======

.. toctree::
   :hidden:

   import-filters/attribute
   import-filters/import-list
   import-filters/feature-counter
   import-filters/bbox
   import-filters/feature-type
   import-preferences/index

To load 3D city model content into a 3D City Database instance, the
Importer/Exporter supports the import of CityGML and CityJSON files
on the *Import* tab of the operations window.

.. figure:: /media/impexp_CityGML_import_dialog_fig.png
   :name: impexp_CityGML_import_dialog_fig
   :align: center

   The import dialog.

**Input files and formats**

The list of files to be imported must be provided at the top of the import dialog [1]. Files can be
selected through clicking on the *Browse* button. Alternatively, you can drag&drop files from your
preferred file explorer onto the Import tab. If the file list already
contains entries, the drag&drop operation will replace them by default. If
you want to keep the previous entries and only append additional files,
keep the ``CTRL`` key pressed while dropping (on Windows). The *Remove*
button or ``DEL`` key lets you remove selected entries from the input files.
Note that adding folders to the list is also supported. Each folder will
be recursively scanned for input files to be imported.

The import operation supports the following file formats and extensions:

.. list-table::  *Supported input file formats and extensions*
   :align: center
   :name: import_supported_file_formats

   * - | **Format**
     - | **File extensions**
   * - | **CityGML versions 2.0, 1.0, and 0.4**
     - | \*.gml, \*.xml
   * - | **CityJSON version 1.0.x**
     - | \*.json, \*.cityjson
   * - | **GZIP compressed files**
     - | \*.gz, \*.gzip
   * - | **ZIP archives**
     - | \*.zip

The file formats are mainly detected based on the file extension, so please
make sure to use one of the supported file extensions from :numref:`import_supported_file_formats`.
ZIP archives are recursively scanned for contained CityGML and CityJSON
files. Additional files referenced from the CityGML/CityJSON files such as texture images
will also be imported into the database if the references can be correctly
resolved during import. This also holds true if the additional files are located inside
the same ZIP archive as the CityGML/CityJSON files.

.. caution::
  While even large CityGML files can be read in a streaming fashion (i.e., one
  top-level feature after the other), large parts of a CityJSON file must
  be kept in main memory while reading the entire file. To avoid memory
  issues, make sure the file size of the CityJSON input file is small enough,
  otherwise the import process will terminate with an exception.
  You can also increase the available memory for the Importer/Exporter
  application (see :numref:`impexp_launching_chapter`).

**Import modes**

After selecting the input files, you can set the *Import mode* [2] for the import operation. The import mode
specifies how to handle conflicting *top-level city objects* and to avoid duplicate objects in the database. For each
top-level city object to be imported, its identifier (i.e., `gml:id` in CityGML) is queried in the database to
check whether a city object with identical identifier already exists in the database. If the search does not yield a
result, the city object is directly imported. If, however, a duplicate/conflicting city object is found in the
database, you can choose between the following strategies to resolve the conflict:

1. **Import all** (default): All top-level city objects are imported into the database, regardless of whether this
   results in duplicate objects. The identifiers from the input files are not looked up in the database in this mode.
2. **Skip existing**: The top-level city object from the input file is skipped and not imported. Thus, the
   object in the database takes precedence.
3. **Delete existing**: The top-level city object from the input file takes precedence and overwrites the duplicate
   object in the database. For this purpose, the duplicate object in the database is first *physically deleted* before
   importing the object from the input file.
4. **Terminate existing**: Same as 3) except that the duplicate object is only *terminated* and, thus, remains in the
   database.

.. hint::
  The *skip existing* mode also allows you to easily **resume** a previous import process
  that was canceled manually or due to errors.

.. note::
  The duplicate check is only based on the object identifier (`gml:id`). No further checks are performed.
  Only the top-level city objects from the input files are tested but not their nested child features.

**Import filters**

The import dialog allows for setting thematic and
spatial filters to narrow down the set of top-level
city objects that are to be imported from the input files [3]. The
following filters are offered and discussed in separate sections of this chapter:

- :numref:`%s <impexp_import_attribute_filter>` :ref:`impexp_import_attribute_filter`
- :numref:`%s <impexp_import_list_filter>` :ref:`impexp_import_list_filter`
- :numref:`%s <impexp_import_feature_counter_filter>` :ref:`impexp_import_feature_counter_filter`
- :numref:`%s <impexp_import_bbox_filter>` :ref:`impexp_import_bbox_filter`
- :numref:`%s <impexp_import_feature_types_filter>` :ref:`impexp_import_feature_types_filter`

To enable a filter, simply select its checkbox. This will automatically make
the filter dialog visible. Make sure to provide the mandatory input for the filter
to work correctly. If more than one filter is enabled, the filters are combined in a
logical AND operation, i.e. all filter criteria must be fulfilled for a city object to
be imported. If no checkbox is enabled, no filters are applied and, thus,
all features contained in the input files will be imported.

.. note::
   All import filters are only applied to *top-level city objects* but *not to nested
   child features*.

**Schema validation**

Before importing, the input files can be
validated against the official CityGML XML and CityJSON schemas. Simply click the
*Just Validate* button [5] in order to run the validation process.
Filter settings are **not considered** in this process. Note that this
operation does not require internet access since the schemas are
packaged with the application. The features from the input files are **not imported**
into the database during validation. The validation results are printed
to the console window.

.. note::
   It is **strongly recommended** that only input files having
   successfully passed the validation are imported into the database.
   Otherwise, errors in the data may lead to unexpected behavior, error
   messages or even abnormal termination of the import process.

.. note::
   CityGML ADE schemas are automatically considered in the validation process
   if the ADE has been correctly registered with the Importer/Exporter (see
   :numref:`impexp_plugin_ade_manager_chapter` for more details). This way, also
   ADE data can be checked before importing. CityJSON Extension schemas are, however,
   not supported by the validation process. Please use an external tool like
   `cjio <https://github.com/cityjson/cjio/>`_ to validate such datasets.

**Import preferences**

More fine-grained preference settings affecting
the import operation are available on the *Preferences* tab of the
operations window [6]. Make sure to check these settings *before* starting
the import process. A full documentation of the import preferences is
provided in :numref:`impexp_citygml_import_preferences_chapter`.
The following table provides a summary overview.

.. list-table::  Summary overview of the import preferences
   :name: citygml_import_preferences_summary_table
   :widths: 30 70

   * - | **Preference name**
     - | **Description**
   * - | :ref:`impexp_preferences_import_general`
     - | General options like behaviour in error situations to be used for imports.
   * - | :ref:`impexp_import_preference_continuation`
     - | Metadata that is stored for every object in the database such as the data lineage, the updating person or the creationDate property.
   * - | :ref:`impexp_import_preferences_identifier`
     - | Generates UUIDs where object identifiers are missing on input features or replaces all identifiers with UUIDs.
   * - | :ref:`impexp_import_preferences_appearance_chapter`
     - | Defines whether appearance information should be imported.
   * - | :ref:`impexp_import_preferences_geometry`
     - | Allows for applying an affine transformation to the input geometry.
   * - | :ref:`impexp_import_preferences_address_chapter`
       | (CityGML only)
     - | Controls the way in which xAL address fragments are imported into the database.
   * - | :ref:`impexp_import_preferences_xml_validation`
       | (CityGML only)
     - | Performs XML validation automatically and excludes invalid features from being imported.
   * - | :ref:`impexp_import_preferences_xsl_transformation`
       | (CityGML only)
     - | Defines one or more XSLT stylesheets that shall be applied to the city objects in the given order before import.
   * - | :ref:`impexp_cityjson_import_preferences`
     - | Defines import options for CityJSON input files.
   * - | :ref:`impexp_import_preferences_indexes`
     - | Settings for automatically enabling/disabling spatial and normal
       | indexes during imports.
   * - | :ref:`impexp_import_preferences_import_logs`
     - | Additional log files for recording successfully imported top-level features and duplicate objects.
   * - | :ref:`impexp_import_preferences_resources_chapter`
     - | Allocation of computer resources used in the import operation.


**Starting the import process**

Once all import settings are correct, the *Import*
button [4] starts the import process. If a database connection has not
been established manually beforehand, the currently selected entry on
the *Database* tab is used to connect to the 3D City Database. The
separate steps of the import process as well as all errors and warnings that might
occur during the import are reported to the console window, whereas the
overall progress is shown in a separate status window. The import
process can be aborted at any time by pressing the *Cancel* button in
the status window. The Importer/Exporter will make sure that all pending
city objects are completely imported before it terminates the import
process.

After having completed the import, a summary of the imported CityGML
top-level features is printed to the console window.

.. caution::
   The Importer/Exporter **does not check by any means** whether a
   **top-level feature** from an input file **already exists** in the database.
   Thus, if an import is executed twice on the same dataset, all CityGML
   features contained in the dataset will be imported twice.

   One way to avoid duplicate features might be, for instance, to manually
   set a UNIQUE constraint on the GMLID column of the CITYOBJECT table.

.. hint::
   To improve the speed of the import operation for *a very large number of features* (bulk imports),
   the spatial indexes can be disabled before and re-activated after the import.
   For a smaller number of features though, disabling and enabling the spatial indexes might take
   longer than the actual import itself. Normal indexes should never be disabled before
   an import.

   Importing into version-enabled tables under Oracle typically takes
   *considerably more time* than importing into non-version-enabled tables.

.. note::
   The import operation does **not automatically** **apply** a
   **coordinate transformation** to the internal reference system of the 3D
   City Database instance. Thus, if the coordinate reference system of the
   CityGML input data does not match the coordinate reference system
   defined for the 3D City Database instance, the user must transform the
   coordinate values **before importing** the data (or use an affine
   transformation during import if this is enough). A possible workaround
   procedure can be realized as follows:

   1. Set up a second (temporary) instance of the 3D City Database with an
      internal CRS matching the CRS of the CityGML instance document.
   2. Import the dataset into this second 3D City Database instance.
   3. Export the data from this second instance into the target CRS by
      applying a coordinate transformation (see CityGML export
      documentation in :numref:`impexp_citygml_export_chapter`).
   4. The exported CityGML document now matches the CRS of the target 3D
      City Database instance and can be imported into that database. The
      temporary database instance can be dropped.

   Alternatively, you can change the reference system in the database to
   the one used by the imported geometries (see the corresponding
   database operation in :numref:`impexp-db-change-crs`).