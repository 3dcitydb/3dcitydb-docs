.. _impexp_xml_query_tiling:

<tiling> parameter
^^^^^^^^^^^^^^^^^^

The ``<tiling>`` parameter allows for exporting the requested top-level
features in tiles. Every tile is exported to its own target file within
a separate subfolder of the export directory.

Like the bounding box settings of the simple GUI-based export filter (cf.
chapter :numref:`impexp_export_bbox_filter`),
the ``<tiling>`` parameter requires three mandatory inputs:
the ``<extent>`` of the geographic region that should be tiled and the
number of ``<rows>`` and ``<columns>`` into which the region should be evenly
split. The ``<extent>`` must be provided as bounding box using a
``<lowerCorner>`` and an ``<upperCorner>`` element.

The example below exports all buildings within the provided ``<extent>``
into 2x2 tiles.

.. code-block:: xml

    <query>
      <typeNames>
        <typeName>bldg:Building</typeName>
      </typeNames>
      <tiling>
        <extent srid="4326">
          <lowerCorner>10.7005978 47.5707931</lowerCorner>
          <upperCorner>10.7093525 47.5767573</upperCorner>
        </extent>
        <rows>2</rows>
        <columns>2</columns>
      </tiling>
    </query>

Besides the mandatory input, the optional ``<tilingOptions>`` parameter
can be used to control the names of the subfolders and tile files, and
whether tile information should be stored as generic attribute. The
following subelements are supported:

.. list-table::  Tiling options of the <tiling> parameter.
   :name: impexp_query_expression_table
   :widths: 30 70

   * - | ``<tilePath>``
     - | Name of subfolder that is created for each tile (default: *tile*).
   * - | ``<tilePathSuffix>``
     - | Suffix to append to each <tilePath>. Allowed values are *row_column* (default), *xMin_yMin*, *xMax_yMin*, *xMin_yMax*, *xMax_yMax* and *xMin_yMin_xMax_yMax*.
   * - | ``<tileNameSuffix>``
     - | Suffix to append to each tile filename. Allowed values are *none* (default) and *sameAsPath*.
   * - | ``<includeTileAsGenericAttribute>``
     - | Add a generic attribute named *TILE* to each city object.
   * - | ``<genericAttributeValue>``
     - | Value for the generic attribute. Allowed values are identical to those for <tilePathSuffix> (default: *xMin_yMin_xMax_yMax)*.

If the ``<tilingOptions>`` element is not present, then the settings
defined in the export preferences
(cf. :numref:`impexp_preferences_export_tiling_chapter`) are used instead.