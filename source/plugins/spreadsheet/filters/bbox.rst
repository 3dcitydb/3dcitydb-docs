.. _impexp_plugin_spshg_bbox_filter:

Bounding box filter
-------------------

The bounding box filter takes a 2D bounding box as parameter that is given by the
coordinate values of its lower left (x\ :sub:`min`, y\ :sub:`min`) and upper right (x\ :sub:`max`, y\
:sub:`max`) corner. It is evaluated against the ENVELOPE column of the CITYOBJECT table.

.. figure:: /media/impexp_plugin_spshg_bbox_filter.png
   :name: impexp_export_bbox_filter_fig
   :align: center

   Bounding box filter for table export operations.

All top-level features whose envelopes overlap with the provided bounding box will be
exported.

Similar to the CityGML/CityJSON export operation, the coordinate values of the bounding box filter
can either be entered manually or chosen interactively in a 2D map window.
To open the map window, click on the map button |map_select|.
A comprehensive guide on how to use the map window is provided in chapter
:numref:`impexp_preferences_map_window_chapter`.

.. note::
   When choosing a spatial *bounding filter*, make sure that
   *spatial indexes are enabled* (use the index operation on the *Database tab* to check the
   status of indexes, cf. :numref:`impexp-db-indexes`).

.. note::
   If the entire 3D city model stored in the 3DCityDB instance
   shall be exported with tiling enabled, then a bounding box spanning the
   overall area of the model must be provided. This bounding box can be
   easily calculated on the *Database* tab (cf. :numref:`impexp-db-calc-bbox`).

.. |map_select| image:: /media/map_select.svg