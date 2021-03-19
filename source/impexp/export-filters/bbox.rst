.. _impexp_export_bbox_filter:

Bounding box filter
-------------------

The bounding box filter takes a 2D bounding box as parameter that is given by the
coordinate values of its lower left (x\ :sub:`min`, y\ :sub:`min`) and upper right (x\ :sub:`max`, y\
:sub:`max`) corner. It is evaluated against the ENVELOPE column of the CITYOBJECT table.

.. figure:: /media/impexp_export_bbox_filter.png
   :name: impexp_export_bbox_filter_fig
   :align: center

   Bounding box filter for export operations.

You can choose whether features whose envelopes *overlap* with the provided bounding box are to be
exported (default), or whether their envelope must be *inside*. Alternatively, the export can be *tiled* by splitting the
bounding box into a regular grid. The number of rows and columns of this grid can be defined by the user. Each
tile is exported to its own output file. To make sure that every city object is assigned to one tile only,
the center point of its envelope is checked to be either inside or on the left or top border of the tile.

Similar to the import operation, the coordinate values of the bounding box filter
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

.. note::
   Using the center point of the envelope as criterion for a tiled
   export has a side-effect when tiling is combined with the *counter
   filter*: the number of city objects on the tile can be less than the
   number of city objects returned by the database query because the tile
   check happens after the objects have been queried. Therefore, the
   *counter filter* only sets a possible maximum number in this filter
   combination. This is a correct behavior, so the Importer/Exporter will
   not report any errors.

.. |map_select| image:: /media/map_select.svg