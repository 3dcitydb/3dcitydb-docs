.. _impexp-db-citygml-ade:

Displaying supported CityGML ADEs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This tab provides a list of all CityGML Application Domain Extensions
(ADEs) that are registered in the 3DCityDB instance and/or are
supported by the Importer/Exporter. The following screenshot shows the
corresponding dialog.

.. figure:: /media/impexp_gui_ADE_list_fig.png
   :name: impexp_gui_ADE_list_fig
   :align: center

   Table of all supported CityGML ADEs.

The ADE table contains one entry per CityGML ADE. Each entry lists
the *name* and the *version* of the ADE and indicates whether it is
supported by the *database* and/or the *Importer/Exporter* (using check
or cross signs). Database support requires that the ADE has been
successfully registered in the 3DCityDB instance using the ADE Manager
Plugin (see :numref:`impexp_plugin_ade_manager_chapter`).
Additional support by the Importer/Exporter requires that a
corresponding ADE extension has been copied into the *ade-extensions*
folder within the installation directory of the Importer/Exporter. Only
if both conditions are met both fields will contain a check sign. If no
ADE has been detected upon database connection, the table remains empty.

In the example of :numref:`impexp_gui_ADE_list_fig`, there is only an Importer/Exporter
extension for an ADE called *KitEnergyADE* but the connected 3DCityDB
instance lacks support for it. EnergyADE data would therefore not be
handled by the Importer/Exporter and thus not stored into the database
in this scenario.

If you select an entry in the ADE table and click the *Info* button (or
simply double-click on the entry), metadata about the ADE will be
displayed in a separate window as shown below. The *Status* field shows
whether the ADE is fully supported or some user action is required.
The *Encoding* checkboxes illustrate for which output formats the ADE
is available. Thus, ADE content will only be exported when choosing a
supported output format for the export operation (see :numref:`impexp_citygml_export_chapter`).

.. figure:: /media/impexp_ADE_metadata_dialog_fig.png
   :name: impexp_ADE_metadata_dialog_fig
   :align: center

   ADE metadata dialog.
