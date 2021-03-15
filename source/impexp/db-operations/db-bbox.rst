.. _impexp-db-calc-bbox:

Calculating/updating bounding boxes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This dialog lets you calculate the 2D bounding box of the city objects
stored in the database. The bounding box is useful, for instance, as
spatial filter for the different import and export operations of the
Importer/Exporter (see documentation of the corresponding operations)
or for use in external tools.

.. figure:: /media/impexp_gui_calc_boundingbox_fig.png
   :name: impexp_gui_calc_boundingbox_fig
   :align: center

   Calculating the bounding box for selected feature types.

First, the *top-level feature type* needs to be selected for which the bounding box
should be calculated [1]. The default option *core:_CityObject* will operate on all
city objects in the database, but you can also restrict the calculation to a specific
type such as bldg:Building or wtr:WaterBody. The bounding box can optionally be
transformed into a user-defined coordinate *reference system* [2].
By default, the bounding box is presented in the same reference system as specified for the
3D City Database instance during setup. See :numref:`citydb_crs_definition_chapter`
for details on how to define and manage user-defined reference systems.

If your database contains terminated city objects, the optional *Feature Version*
filter [6] lets you define the version of the city objects that should be used for the
calculation. The default option *Latest version* will only operate on non-terminated
objects. With *Valid version*, you can specify that only city objects that were valid
at a given timestamp or within a given time range should be considered. If you
want the bounding box to be calculated for all city objects independent of whether they
are terminated or not, simply disable the filter.

.. note::
  The *Feature Version* filter works on the CREATION_DATE and TERMINATION_DATE columns
  of the table CITYOBJECT. More information can be found in :numref:`impexp_export_feature_version_filter`.

To trigger the calculation, press the *Calculate* button. The coordinates
of the lower left (x\ :sub:`min`, y\ :sub:`min`) and upper
right (x\ :sub:`max`, y\ :sub:`max`) corner of the resulting bounding box are
rendered in the corresponding fields of the dialog [3]. The values are
also copied to the clipboard of your operating system and can therefore
easily be pasted into the import and export dialogs. You can also
manually copy the values to the clipboard by clicking the
|bbox_copy| button [4], or by right-clicking on a text field [3] and choosing the
corresponding option from the context menu.

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
be filled during import or the geometry representation of a city object has
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