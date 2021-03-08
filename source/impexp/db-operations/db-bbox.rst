.. _impexp-db-calc-bbox:

Calculating/updating bounding boxes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This dialog lets you calculate the 2D bounding box of the city objects
stored in the database. The bounding box is useful, for instance, as
input to spatial filters in CityGML imports and exports as well as
KML/COLLADA/glTF exports (see documentation of the corresponding
operations).

.. figure:: /media/impexp_gui_calc_boundingbox_fig.png
   :name: impexp_gui_calc_boundingbox_fig
   :align: center

   Calculating the bounding box for selected feature types.

The coordinate values of the lower left (x\ :sub:`min`, y\ :sub:`min`) and upper
right (x\ :sub:`max`, y\ :sub:`max`) corner of the calculated bounding box are
rendered in the corresponding fields of the dialog [3]. The values are
also copied to the clipboard of your operating system and can therefore
easily be pasted into the import and export dialogs. You can also
manually copy the values to the clipboard by clicking the
|bbox_copy| button [4], or by right-clicking on a data field [3] and choosing the
corresponding option from the context menu.

The calculation of the bounding box can be restricted to a specific city
object type such as Building or WaterBody [1]. Similar to the generation of a
database report, the user can request the bounding box for city objects
living in a specific *workspace* at a given *timestamp* if the database
is version-enabled (Oracle only). The coordinate values can optionally
be transformed into a user-defined coordinate *reference system* that is
available from the drop-down list [2]. Per default, the coordinate
values are presented in the same reference system as specified for the
3D City Database instance during setup. See :numref:`citydb_crs_definition_chapter`
for details on how to define and manage user-defined reference systems.

By using the map |map_select| button [4],
the calculated bounding box is rendered in a separate 2D map window
for visual inspection as shown below. The usage of this map window is
described in :numref:`impexp_preferences_map_window_chapter`.

.. figure:: /media/impexp_map_window_fig.png
   :name: impexp_map_window_fig
   :align: center

   Map window for displaying and choosing bounding boxes. Note
   that the coordinate values of the bounding box are shown in the upper
   left component.

The calculation of the bounding box is based on the values stored in the
ENVELOPE column of the CITYOBJECT table. If this column is NULL or
contains an incorrect value (e.g., in case the value could not correctly
filled during import or the geometry representation of a city object has
been changed), then the resulting bounding box will be wrong and
subsequent operations might not provide the expected result. To fix the
ENVELOPE values in the database, simply let the Importer/Exporter
*create missing* values (i.e., replace NULL values) or *recreate all*
values by clicking on the corresponding buttons [5]. This update process
either affects only the city objects of a given feature type or all city
objects based on the selection made in [1].

.. note::
   This process directly updates the ENVELOPE column of the
   affected city objects and might take long to complete since the new
   values are calculated by evaluating all geometries of the city objects
   in all LoDs including implicit geometries.

.. |bbox_copy| image:: /media/bbox_copy.svg

.. |map_select| image:: /media/map_select.svg