.. _impexp_crs_management_chapter:

Management of user-defined coordinate reference systems
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When setting up an instance of the 3D City Database, a coordinate
reference system (CRS) must be chosen for the entire database (cf.
:numref:`3dcitydb_setup_schema_chapter`).
This CRS is used as default reference system for all
spatial objects that are created and stored in the database instance
(expect implicit geometries) as well as for building spatial indexes and
performing spatial functions.

At many places, the Importer/Exporter allows for providing coordinate
values associated with a different CRS though, e.g. when defining
*spatial bounding box filters* for CityGML imports and exports and
KML/COLLADA/glTF exports, or when defining a *target CRS* into which
coordinate values shall be converted during CityGML exports. To add and manage
additional reference systems, the Importer/Exporter provides the
*Reference systems* settings below the *Database* preferences node.

.. figure:: /media/impexp_database_preferences_crs_fig.png
   :name: impexp_database_preferences_crs_fig

   Database preferences – Reference systems.

On top of the dialog [1], a drop-down list allows for choosing
a CRS for display and editing from the list of user-defined CRSs. This
list contains at minimum one predefined entry called *Same as in
database* that represents the internal CRS of the 3D City Database
instance. This entry will always show the SRID and gml:srsName of
the currently connected database instance.

.. note::
  The internal CRS cannot be edited in this dialog. If you want
  to change the internal CRS, then follow the steps in :numref:`change-crs`.

A new user-defined CRS can be added to this list after clicking the
*New* button. Please provide the database-specific SRID in the
corresponding *SRID* input field of the user dialog and enter the URN
encoding of the CRS into the *gml:srsName* input field (optional). This
field also provides a drop-down list of commonly used encoding schemes
which can be used as template (such as the OGC encoding scheme). A
short, meaningful textual description of the CRS must be provided in the
*Description* field. This description is used as value for the drop-down
on top of the dialog, but also for similar CRS drop-down lists on
further tabs of the Importer/Exporter. The new CRS is added to the list
of user-defined CRSs upon clicking the *Apply* button. The following
screenshot provides an example.

.. figure:: /media/impexp_database_preferences_add_crs_fig.png
   :name: impexp_database_preferences_add_crs_fig

   Adding a new CRS to the list of user-defined CRSs.

The *Copy* button allows for adding further CRSs by copying and editing
the information of an already existing one. The currently
selected CRS is deleted from the list by clicking the *Delete* button.
The *Check* button next to the *SRID* input field facilitates to verify
whether the provided SRID is supported by the currently connected 3D
City Database instance. After a successful check, the non-editable
fields *Database name* and *SRS type* will be filled with the
corresponding information queried from the connected 3D City
Database instance. If the Importer/Exporter is not connected to a
database instance, the *Check* button is disabled.

The result of the SRID verification may vary between different 3D City
Database instances since 1) the list of predefined spatial reference
systems differs between different database systems and versions, and 2)
both Oracle and PostgreSQL/PostGIS support the definition of
user-defined spatial reference systems on the database side (please
check the respective database documentation for guidance).

.. note::
   In order to add a user-defined CRS to the Importer/Exporter that
   is not supported by the underlying Oracle or PostgreSQL/PostGIS
   database, you need to first register this CRS in your database. As soon
   as the CRS is available from the database, it can be added to the list
   of user-defined CRSs in the Importer/Exporter.

The list of user-defined CRSs is automatically stored in the config file
of the Importer/Exporter and loaded upon application start. It can
additionally be exported into an extra file (see [2] in Figure 116).
This allows for easily sharing user-defined CRSs between different
installations of the Importer/Exporter. Please provide a valid filename
in the corresponding input field *Filename* (use the *Browse* button to
open a file selection dialog) and click on *Save*.

There are two ways for importing an external list of CRSs:

1. *Add*: the CRSs listed in the external file are
   added to the current list of CRSs.
2. *Replace*: the current list of CRSs is replaced
   with the entries from the external file.

The Importer/Exporter is shipped with a number of predefined CRSs
organized in subfolders below templates/CoordinateReferenceSystems in
the installation folder. Each CRS definition is stored in its own file
and, thus, can be easily imported and added to the list of user-defined
CRSs. Note that the gml:srsName of the predefined CRSs generally lacks
a height reference system. It should therefore be added
before using this CRS as target reference system for CityGML exports.