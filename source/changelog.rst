Changelog
=========

This appendix provides an overview of the most important changes in
version 4.0 of the 3D City Database and version 4.1 of the
Importer/Exporter compared to the previous release version 3.3.0.

.. _3dcitydb:

3D City Database relational schema
----------------------------------

General changes
~~~~~~~~~~~~~~~

-  New metadata tables ADE, SCHEMA, SCHEMA_REFERENCING and
   SCHEMA_TO_OBJECTCLASS for registering CityGML ADEs

-  Added OBJECTCLASS_ID column to all feature tables to distinguish
   objects from CityGML ADEs. Also extended OBJECTCLASS table by more
   feature-specific details and inserted new entries for feature
   properties such as geometry, generic attributes etc.

-  Added NOT NULL constraints on each OBJECTCLASS_ID column

-  New prefilled metadata table AGGREGATION_INFO that supports the
   automatic generation of DELETE and ENVELOPE scripts

-  Changed delete rule of one foreign key in link tables to ON DELETE
   CASCADE to produce better delete scripts


3D City Database scripts
------------------------

-  Moved interactive prompts from SQL to batch/shell scripts for better
   setup automation

-  Provide batch (Windows) and shell scripts (UNIX, macOS) for both
   PostgreSQL and Oracle DBMS

-  Re-added scripts to create a read-only user (UTIL folder), called
   GRANT_ACCESS and REVOKE_ACCESS (removed in v3.x). Also includes a
   read-write option.

-  New MIGRATION scripts to upgrade from a 3DCityDB v2.1.0 or v3.3.2 to
   v4.0.0.

-  Tidier folder and script structure:

   -  Removed folders PL_SQL (Oracle) and PL_pgSQL (PostgreSQL) to make
      CITYDB_PKG a top-level directory under the SQLScripts folder

   -  Moved OBJECTCLASS_INSTANCES script to SCHEMA/OBJECTCLASS folder

   -  PostgreSQL: New SCHEMAS directory in UTIL folder

   -  Oracle: One instead of two CREATE_DB scripts

   -  Oracle: Moved versioning scripts to its own directory in the UTIL
      folder

   -  Oracle: Renamed CREATE_DB folder in UTIL directory to HINTS

-  Oracle: Better treatment if SDO_GEORASTER support is missing

-  Oracle: Defining spatial metadata on all geometry columns with new
   function set_schema_sdo_metadata in CITYDB_CONSTRAINT package instead
   of a hard-coded part in SPATIAL_INDEX.sql script

   
.. _3dcitydb-sproc:

3D City Database stored procedures
----------------------------------

.. _general-changes-sproc:

General changes
~~~~~~~~~~~~~~~

-  New packages: CITYDB_CONSTRAINT and CITYDB_OBJCLASS

-  Removed parts with dynamic SQL where possible. Required renaming of
   some function arguments to avoid conflicts with column names in
   querys

-  PostgreSQL: Added volatility categories for better query planning

UTIL package
~~~~~~~~~~~~

-  Updated version numbers in citydb_version function

-  Moved update_schema_constraints and update_table_constraint
   procedures into new CITYDB_CONSTRAINT package and renamed them to
   set_schema_fkey_delete_rule and set_fkey_delete_rule. Change data
   type for on_delete_param to CHAR as only one letter is needed to set
   a new delete rule: 'a' for ON DELETE NO ACTION , 'n'for ON DELETE SET
   NULL ('n'), 'c' for ON DELETE CASCADE or (PostgreSQL-only) 'r' for ON
   DELETE RESTRICT

-  Moved objectclass_id_to_table_name function to new CITYDB_OBJCLASS
   package.

-  Added schema_name parameter to functions db_metadata and db_info

-  Removed schema_name parameter from get_seq_values function

-  Oracle: Removed schema_name parameter from construct_solid function

IDX package
~~~~~~~~~~~

-  Oracle: Added schema_name parameter to get_index function

-  Oracle: Dropping spatial indexes will not delete spatial metadata
   anymore

SRS package 
~~~~~~~~~~~~

-  Added schema_name parameter to is_db_ref_sys_3d function

-  Oracle: Added schema_name parameter to get_dim function

-  Oracle: Do not delete spatial metadata when spatial index is not
   valid

STAT package
~~~~~~~~~~~~

-  Exclude new metadata tables from database report

DELETE package
~~~~~~~~~~~~~~

-  Aligned API of Oracle version with PostgreSQL (no more \_pre and
   \_post methods)

-  Two delete endpoints are provided for each feature class: Delete by
   single ID value or delete by a set of IDs

-  All 1:n references are deleted right away. **Replaced all explicit
   cleanup scripts** (except for cleanup_appearances) with one generic
   cleanup function

-  New prefix del\_ instead of delete\_

-  The DELETE scripts have been generated automatically by the ADE
   Manager Plugin of the Importer/Exporter. This process shall be
   repeated when introducing ADE extensions to the database schema.

DELETE_BY_LINEAGE package
~~~~~~~~~~~~~~~~~~~~~~~~~

-  The package and included stored procedures have been removed

-  New function del_delete_cityobjects_by_lineage in DELETE package

ENVELOPE package
~~~~~~~~~~~~~~~~

-  New prefix env\_ instead of get_envelope\_ (except for
   get_envelope_cityobjects function)

-  The ENVELOPE scripts have been generated automatically by the ADE
   Manager Plugin of the Importer/Exporter. This process shall be
   repeated when introducing ADE extensions to the database schema.

   
.. _impexp:

3D City Database Importer/Exporter 
-----------------------------------

The new version 4.1 of the Importer/Exporter contains many bug fixes as
well as stability and performance improvements. A full list of fixes and
changes is available from the GitHub repository at
https://github.com/3dcitydb/importer-exporter.

.. _general-changes-impexp:

General changes
~~~~~~~~~~~~~~~

-  Java 8 is required since version 3.3.0.

-  The Importer/Exporter can now connect to both Oracle and PostgreSQL.

-  Temporary information required during data imports and exports (e.g.,
   for resolving of XLink references) can now optionally be stored to a
   local file-based database instead of using temporary tables in the 3D
   City Database instance.

-  3.1: Importer/Exporter now checks the version of the 3DCityDB before
   connecting

-  3.1: Re-Added user dialog to control GMLID_CODESPACE during import

-  3.1: Added user dialog to calculate the ENVELOPE of city objects in
   the database

-  3.3: The location of the main config file (‘project.xml’) has been
   changed to %HOMEDRIVE%%HOMEPATH%\3dcitydb\importer-exporter\config
   (Windows 7 and higher) respectively
   $HOME/3dcitydb/importer-exporter/config (UNIX/Linux, Mac OS
   families). Old config files can still be loaded manually (note: was
   ../importer-exporter-3.0/.. in versions 3.0 to 3.2)

-  4.1: OSM Nominatim is now used as default geocoder for the map
   window. Google Map API services can still be used for the map window
   and for KML/COLLADA exports but require an API key.

-  4.2: Reworked Plugin API to support non-GUI plugins.

CityGML import
~~~~~~~~~~~~~~

-  4.2: Fixed broken feature type filter for CityGML imports.

-  4.2: Added possibility to define a gml:id prefix for the UUIDs that
   are created during CityGML imports.

-  4.1: Added support for importing CityGML data from (G)ZIP files.

-  CityGML import now supports CityGML versions 2.0, 1.0 and 0.4.

-  A new import log optionally tracks all successfully imported
   top-level city objects in a separate CSV file. In case an import
   process aborts abnormally, this file can be used to understand which
   city objects have been processed and stored in the database before
   termination.

-  The import process now follows a fail-on-first-error strategy, i.e.
   the import terminates upon the first error thrown instead of trying
   to continue.

-  Improved import of texture atlases. Each texture atlas is only stored
   once in the database (new table ‘tex_image’) even if it is referenced
   by more than one city object.

-  Local appearance information is now resolved in main memory to reduce
   import times instead of using temporary database tables.

-  Texture metadata is imported even if texture images are chosen to be
   not imported

-  3.1: Changed the way global appearances are imported

-  3.1: Fixed bug in BRIDGE importer preventing import of bridges with
   thematic surfaces

CityGML export
~~~~~~~~~~~~~~

-  4.2: Property projections can now also be defined for abstract
   feature types.

-  4.1: Added support for using SQL and XML queries for CityGML exports
   to be able express more flexible and complex filter conditions.

-  4.1: Added support for exporting CityGML content to (G)ZIP files.

-  Database content can now be exported to CityGML 2.0 or 1.0. When
   exporting to CityGML 1.0, feature types only available in CityGML 2.0
   such as bridges and tunnels are omitted.

-  City object group members can now be exported as-reference (using
   XLink references) instead of as-value to reduce export times.
   However, note that filter criteria are not applied in this case,
   which might result in CityGML files containing non-resolvable XLink
   references.

-  When exporting city objects with textures, the texture image files
   can now be organized into subfolders. This reduces the number of
   files per folder.

KML/COLLADA/glTF export
~~~~~~~~~~~~~~~~~~~~~~~

-  Support for glTF version 2.0 in addition to version 1.0. New
   COLLADA2glTF binaries (version 2.1.3) for Windows, Linux and MacOS.

-  Solved bugs that might prevent exporting LandUse 3D models from
   functioning correctly.


.. _wfs:

Web Feature Service
-------------------

-  Since 3.0: Added a basic Web Feature Service interface for the 3D
   City Database

-  Fixed a SQL Injection vulnerability with version 3.3.0. It is
   **strongly recommended** to update to this version.


.. _webmap:

3D Web Map Client
-----------------

-  Introduced geolocation-based features such as the first-person view on mobile devices.

-  Support for glTF 2.0.

-  Support for Cesium 3D Tiles.
