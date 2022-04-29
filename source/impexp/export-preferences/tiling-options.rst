.. _impexp_preferences_export_tiling_chapter:

Tiling options
^^^^^^^^^^^^^^

The Importer/Exporter allows for applying a spatial *bounding box
filter* to exports on the *Export* tab of the operations window.
To trigger a tiled export, a user can additionally provide the number of rows and columns
into which the bounding box shall be evenly split (cf. :numref:`impexp_export_bbox_filter`).

When tiling is enabled in this way, the export operation iterates over all tiles
within the bounding box and exports the city objects on each tile. Every
tile can be exported to its own unique file within a separate subdirectory. With
the *Tiling options* settings, the names of the subdirectories and tile files can
be flexibly adapted as shown below.

.. figure:: /media/impexp_export_preferences_tiling_options_fig.png
   :name: impexp_export_preferences_tiling_options_fig
   :align: center

   Export preferences – Tiling options.

.. note::
  You can always just provide a **single output file** on the *Export* tab
  even if tiling shall be used (:numref:`impexp_citygml_export_chapter`).
  The *tiling options* then allow you to define unique a subdirectory and/or filename
  for each tile based on specified output file.

For example, assume a user has entered the following output file:

::

   /home/user/my_3d_model/my_city.gml

For a tiled export, this information is split into the following components:

- Export directory: ``/home/user/my_3d_model/``
- Filename: ``my_city``
- File extension: ``.gml``

If you want every tile to be stored in a separate subdirectory beneath the predefined
export directory ``/home/user/my_3d_model/``, simply enable the *Subdirectory* checkbox
and enter the name of the subdirectory into the input field. To ensure unique names,
you can use predefined *tokens* that will be replaced by tile-specific values during the export.
Either pick a token from the drop-down list next to the input field or enter it manually.
The available tokens are explained below. You can even define nested subdirectories by using
one or more *path separator* in the input field (Windows: ``\``, Linux/UNIX: ``/``). If all
tiles shall rather be stored in the predefined export directory, just disable the
*Subdirectory* checkbox.

In addition to the subdirectory, you can also add a suffix to the filename of
each tile. Enable the *Filename suffix* checkbox and enter your choice into the input field.
You can again use tokens here. The suffix will be automatically appended to the predefined
filename from the *Export* tab. By checking the *Just use suffix as filename* option, you can
even choose to replace the filename with your suffix. In either case, note that the file
extension cannot be changed.

By default, the Importer/Exporter will create a subdirectory per tile with the
name ``tile_%COLUMN%_%ROW%``. The ``%COLUMN%`` and ``%ROW%`` tokens will be replaced
by the column and row indexes of the current tile. A filename suffix is not added by default.

.. warning::
  Make sure that the combination of subdirectory name and filename suffix results in a
  *unique location* for each tile. Otherwise, tile files might get overwritten during
  the export. The Importer/Exporter does not perform any checks on your settings.

**Available tokens and their meaning**

The tokens listed below are available and can be used to define the subdirectory and filename
suffix.

.. list-table::  *Available tokens and their meaning*
   :align: center
   :name: export_tiling_options_tokens
   :widths: 25 75

   * - | **Token**
     - | **Description**
   * - | %ROW%
     - | Row index of the current tile. The first tile has the row index 0.
   * - | %COLUMN%
     - | Column index of the current tile. The first tile has the column index 0.
   * - | %X_MIN%
     - | X value of the lower left corner of the tile extent.
   * - | %Y_MIN%
     - | Y value of the lower left corner of the tile extent.
   * - | %X_MAX%
     - | X value of the upper right corner of the tile extent.
   * - | %Y_MAX%
     - | Y value of the upper right corner of the tile extent.

The coordinates of the tile extent are always given in the reference system
that has been chosen as target system for the export
(cf. :numref:`impexp_citygml_export_chapter`). By default, this is the internal
reference system of the 3D City Database instance.

**Formatting tokens**

When clicking on the settings button |token_settings| next to an input field,
you can define a format string for each token. This format string is applied to
the value before replacing the token. Format strings are helpful, for example, to
format the coordinate values of the tile extent (e.g., to limit the number of
decimal places or to even cut off all decimal places from the coordinate value).

The format string syntax follows and supports the specification of the ``format()``
method of the Java Language. A full documentation of the syntax is available
`here <https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Formatter.html#syntax>`_.
Some example format strings are shown below.

- ``%s``: This is the default format and simply formats the value as string.
- ``%d``: The value is formatted as decimal integer.
- ``%f``: The value is formatted as decimal number.
- ``%.2f``: The value is formatted as decimal number with two decimal places.
- ``%4.4s``: Format the value as string but only print exactly four characters.


**Examples**

Based on the example output file from above and assuming default value ``tile_%COLUMN%_%ROW%``
for subdirectories, the export would create the following folder structure:

::

   /home/user/my_3d_model/
   ├── tile_0_0/
   |   └── my_city.gml
   ├── tile_1_0/
   |   └── my_city.gml
   ├── tile_2_0/
   |   └── my_city.gml
   ...

When choosing ``%COLUMN%/%ROW%`` as value for the subdirectory, even nested folders
will be created:

::

   /home/user/my_3d_model/
   ├── 0/
   |   └── 0/
   |       └── my_city.gml
   ├── 0/
   |   └── 1/
   |       └── my_city.gml
   ...
   ├── 1/
   |   └── 1/
   |       └── my_city.gml
   ...

**Adding a generic tile attribute to top-level features**

For further traceability, it is possible to attach a generic string
attribute to each exported top-level feature to indicate the tile
to which it belongs. Simply enable the corresponding option in the *tiling options*
dialog and define the name and the value of the generic attribute to be created.
You can again use tokens for the attribute value that will be replaced
with the actual values at export time.

.. |token_settings| image:: /media/settings.svg