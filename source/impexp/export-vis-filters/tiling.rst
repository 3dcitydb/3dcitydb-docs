.. _impexp_export_vis_tiling_filter:

Tiling filter
-------------

As explained in :numref:`impexp_kml_export_chapter`, tiling is an important
mechanism to split the visualization export into multiple tiles with
smaller file size compared to a single output file. This allows a viewer to
employ streaming techniques, for instance, to only load and display tiles
that are visible from the current camera position, and to unload tiles when moving
the camera. This helps to keep the memory footprint low and the visualization
performance high, especially when using a web client.

The tiling filter lets you define the tiling pattern to be used in the
visualization export.

.. figure:: /media/impexp_export_vis_tiling_filter.png
   :name: impexp_export_vis_tiling_filter_fig
   :align: center

   Tiling filter for visualization export operations.

When tiling is enabled, the bounding box of the entire export area
is divided into a regular grid of tiles, and each tile is exported as
separate file (one file per selected display form). The bounding box
can either be defined by the user through a bounding
box filter (see :numref:`impexp_export_vis_bbox_filter`). If a
bounding box is not provided, the export operation will calculate
the bounding box based on the set of top-level features to be exported.

The division of the bounding box into a regular grid of tiles
can then be defined in two ways:

**1. Automatic tiling**
  When selecting the *Fixed side length* option, you can specify the
  preferred side length for each tile in meters (default: 125m). The
  bounding box of the export area is then divided by this value to derive
  the number of rows and columns of the final grid. Note that rounding is
  applied in this process and thus it cannot be guaranteed that the
  resulting tiles are perfectly square.
  For example, assume the bounding box to be exported is 1000m x 1200m,
  and the preferred side length is set to 300m. Then the final
  grid will contain 4 x 4 tiles because 1000 / 300 = 3.3 is rounded to 4
  and 1200 / 300 = 4. The actual size of each tile will therefore be
  250m x 300m in this example.

**2. Manual tiling**
  The second option is to manually define the number of rows and columns.

The export operation will make sure that every top-level feature is only
exported once and thus located on precisely one tile to avoid duplicates.
For this purpose, the center point of the feature's bounding box is required
to lie either inside or on the left or top border of the tile.
As a consequence, also very large or long objects
(e.g., bridges) are only assigned to one tile and will become invisible
as soon as the tile is unloaded from the viewer, even if parts of the bridge
are still close to the camera.

The tile files are organized into a tree structure to be able to easily
access them via their row and column indexes. The tree structure is discussed
in :numref:`impexp_kml_export_chapter`.

.. note::
  You can visualize the grid and the tile borders in a viewer by choosing
  to export the bounding boxes of the entire export area and/or of each
  tile in the *General* preferences of the export operation
  (see :numref:`impexp_kml_export_preferences_general_chapter`). This can be helpful
  for adjusting the tile sizes.