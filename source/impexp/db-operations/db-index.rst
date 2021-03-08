.. _impexp-db-indexes:

Managing indexes
^^^^^^^^^^^^^^^^

The Importer/Exporter allows the user to manually activate or deactivate
indexes on predefined tables of the 3D City Database schema, and to
check their status.

.. figure:: /media/impexp_gui_managing_indexes_fig.png
   :name: impexp_gui_managing_indexes_fig
   :align: center

   Managing spatial and normal indexes.

The operation dialog differentiates between *spatial indexes* on
geometry columns and *normal indexes* on columns with any other datatype
[1]. The buttons *Activate*, *Deactivate*, and *Status* trigger a
corresponding database process on spatial indexes only, normal indexes
only or both index types depending on which checkboxes are selected [1].
Again, the user can define a *workspace* and *timestamp* on which the
operation shall be executed if the database is version-enabled (Oracle
only).

The index operations only affect the following subset of all indexes
defined by the 3D City Database schema:

.. list-table::  Spatial and normal indexes affected by the index operation
   :name: impexp_gui_managing_indexes__table

   * - | **Index type**
     - | **Column(s)**
     - | **Table**
   * - | Spatial
     - | ENVELOPE
     - | CITYOBJECT
   * - | Spatial
     - | GEOMETRY
     - | SURFACE_GEOMETRY
   * - | Spatial
     - | SOLID_GEOMETRY
     - | SURFACE_GEOMETRY
   * - | Normal
     - | GMLID, GMLID_CODESPACE
     - | CITYOBJECT
   * - | Normal
     - | LINEAGE
     - | CITYOBJECT
   * - | Normal
     - | CREATION_DATE
     - | CITYOBJECT
   * - | Normal
     - | TERMINATION_DATE
     - | CITYOBJECT
   * - | Normal
     - | LAST_MODIFICATION_DATE
     - | CITYOBJECT
   * - | Normal
     - | GMLID, GMLID_CODESPACE
     - | SURFACE_GEOMETRY
   * - | Normal
     - | GMLID, GMLID_CODESPACE
     - | APPEARANCE
   * - | Normal
     - | THEME
     - | APPEARANCE
   * - | Normal
     - | GMLID, GMLID_CODESPACE
     - | SURFACE_DATA
   * - | Normal
     - | GMLID, GMLID_CODESPACE
     - | ADDRESS

The result of an index operation is reported in the console window as
shown below. For instance, :numref:`impexp_gui_indexes_status_report_fig` shows the
result of a status query on both spatial and normal indexes. The status *ON* means
that the corresponding index is enabled.

.. figure:: /media/impexp_gui_indexes_status_report_fig.png
   :name: impexp_gui_indexes_status_report_fig
   :align: center

   Result of a status query on spatial and normal indexes.

.. note::
   It is *strongly recommended* to *deactivate the spatial indexes
   before running a CityGML import* on a *big amount of data* and to
   reactive the spatial indexes afterwards. This way the import will
   typically be a lot faster than with spatial indexes enabled. The
   situation may be different when importing only a small dataset.

.. warning::
   Activating and deactivating indexes can take a long time,
   especially if the database fill level is high. Note that the operation
   **cannot be aborted** by the user since this would result in an
   inconsistent database state.