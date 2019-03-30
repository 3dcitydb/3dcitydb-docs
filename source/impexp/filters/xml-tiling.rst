<tiling> parameter
^^^^^^^^^^^^^^^^^^

The <tiling> parameter allows for exporting the requested top-level
features in tiles. Every tile is exported to its own target file within
a separate subfolder of the export directory.

Like the tiling settings of the simple GUI-based export filter (cf.
chapter 5.4), the <tiling> parameter requires three mandatory inputs:
the <extent> of the geographic region that should be tiled and the
number of <rows> and <columns> into which the region should be evenly
split. The <extent> must be provided as bounding box using a
<lowerCorner> and an <upperCorner> element.

The example below exports all buildings within the provided <extent>
into 2x2 tiles.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <tiling>
| <extent srid="4326">
| <lowerCorner>10.7005978 47.5707931</lowerCorner>
| <upperCorner>10.7093525 47.5767573</upperCorner>
| </extent>
| <rows>2</rows>
| <columns>2</columns>
| </tiling>
| </query>

Besides the mandatory input, the optional <cityGMLTilingOptions> element
can be used to control the names of the subfolders and tile files, and
whether tile information should be stored as generic attribute. The
following subelements are supported:

-  | <tilePath> Name of subfolder that is created for each tile
   | (default: *tile*).

-  <tilePathSuffix> Suffix to append to each <tilePath>. Allowed values
   are *row_column* (default), *xMin_yMin*, *xMax_yMin*, *xMin_yMax*,
   *xMax_yMax* and *xMin_yMin_xMax_yMax*.

-  <tileNameSuffix> Suffix to append to each tile filename. Allowed
   values are *none* (default) and *sameAsPath*.

-  <includeTileAsGenericAttribute> Add a generic attribute named *TILE*
   to each city object.

-  <genericAttributeValue> Value for the generic attribute. Allowed
   values are identical to those for <tilePathSuffix> (default:
   *xMin_yMin_xMax_yMax)*.

If the <cityGMLTilingOptions> element is not present, then the settings
defined for the Tiling options export preference (cf. chapter 5.6.2.2)
are used instead.
