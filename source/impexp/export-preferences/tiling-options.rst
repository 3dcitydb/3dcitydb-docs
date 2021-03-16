.. _impexp_preferences_export_tiling_chapter:

Tiling options
^^^^^^^^^^^^^^

The Importer/Exporter allows for applying a spatial *bounding box
filter* to exports on the *Export* tab of the operations window.
To trigger a tiled export, a user can additionally provide the number of rows of columns
into which the bounding box shall be evenly split (cf. :numref:`impexp_export_bbox_filter`).

When tiling is enabled in this way, the export operation iterates over all tiles
within the bounding box and exports the city objects on each tile. Every
tile is exported to its own file within a separate subfolder. With the *Tiling options*
settings, the names of the subfolders and tile files can be adapted as shown below.

.. figure:: /media/impexp_export_preferences_tiling_options_fig.png
   :name: impexp_export_preferences_tiling_options_fig
   :align: center

   Export preferences – Tiling options.

The user must always specify just a single output file on the *Export* tab
even if tiling shall be used (:numref:`impexp_citygml_export_chapter`).
For example, assume a user has entered the following output file:

.. code-block::

   /home/user/my_3d_model/my_city.gml

For a tiled export, this information is split into two components:

- Export directory: ``/home/user/my_3d_model/``
- Filename: ``my_city.gml``

The Importer/Exporter will create one subfolder for each tile inside the export
directory. By default, the folder name is ``tile_x_y``, with x and y being the
row and column of the current tile. Both the *tile subdirectory* (default: tile) and the
*subdirectory suffix* (default: row / column) can be adapted using the corresponding
input fields. When a combination of the tile's lower and upper corner
is chosen instead of the row and column numbers, the coordinates will be given in the
reference system specified for the export (cf. :numref:`impexp_citygml_export_chapter`;
default value is the internal SRS of the 3D City Database instance),
even if the coordinates of the bounding box filter are given in another
user-defined SRS. This makes it easy to relate city objects to tiles since
the coordinates of the city objects contained in the tile are exported in the
same reference system.

The filename of each tile file is chosen to be identical with the filename of
the output file on the *Export* tab by default (``my_city.gml`` in the above
example). However, you may also decide to append a tile-specific *filename suffix*.

**Example:** Based on the example output file from above and assuming default settings,
the export would create the following folder structure:

.. code-block::

   /home/user/my_3d_model/
   ├── tile_0_0/
   |   └── my_city.gml
   ├── tile_1_0/
   |   └── my_city.gml
   ├── tile_2_0/
   |   └── my_city.gml
   ...

For further traceability, it is possible to attach a generic string
attribute of name *TILE* to each exported top-level feature, indicating
which tile it belongs to. The options for the value of the generic
attribute are the same as for the suffix of the tile subfolder.