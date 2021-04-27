.. _impexp_export_lod_filter:

LoD filter
----------

The Level-of-Detail (LoD) filter allows for exporting only
specific LoDs of the city objects.

.. figure:: /media/impexp_export_lod_filter.png
   :name: impexp_export_lod_filter_fig
   :align: center

   LoD filter for export operations.

Simply enable the checkboxes of the LoDs that you want to export
from the database. If you select more than one LoD, the *Filter mode*
drop-down list lets you choose how the selected LoDs should be
evaluated.

.. list-table::  Filter modes for combining LoDs
   :name: citygml_export_lod_filter_mode_table
   :widths: 20 70

   * - | **Filter mode**
     - | **Description**
   * - | **Or**
     - | City objects having a spatial representation in *at least one* of the selected LoDs will be exported. Additional LoD representations of the city object that do not match the user selection are not exported.
   * - | **And**
     - | Only city objects having a spatial representation in *all* of the selected LoDs will be exported. Additional LoD representations of the city object that do not match the user selection are not exported.
   * - | **Minimum LoD**
     - | This is a special version of the *Or* mode that only exports the lowest LoD representation from the matching ones. The exported LoD may therefore differ for each city object.
   * - | **Maximum LoD**
     - | This is a special version of the *Or* mode that only exports the highest LoD representation from the matching ones. The exported LoD may therefore differ for each city object.

Many feature types in both CityGML and CityJSON can have nested sub-features. In such
cases, the top-level feature itself is not required to have a spatial
property, but the geometry can be modelled for its nested sub-features.
For example, a CityGML *bldg:Building* feature does not need to provide an LoD 2
geometry through its own *bldg:lod2Solid* or *bldg:lod2MultiSurface*
properties. Instead, it can have a list of nested boundary surfaces such
as *bldg:WallSurface* and *bldg:RoofSurface* features that have own LoD
2 representations. Nevertheless, in this case the *bldg:Building* is
considered to be represented in LoD 2.

To handle these cases, the LoD filter provides the *search depth*
parameter to specify how many levels of nested features shall
be considered when searching for matching LoD representations.
The default value of "1" means that the top-level feature itself and all its direct
child features (i.e., features on the first nesting level)
are searched for matching LoD representations.
If an LoD representation is found for any (transitive)
sub-feature, then the top-level feature is considered to satisfy the
filter condition. If you pick the wildcard "*" for the search depth, all nested objects
independent of their nesting level are considered.

For example, the following CityGML *bldg:Building* feature has a nested
*bldg:BuildingInstallation* sub-feature and a nested *bldg:WallSurface*
sub-feature. Moreover, the *bldg:BuildingInstallation* itself has a
nested *bldg:RoofSurface* sub-feature.

.. code-block:: xml

    <bldg:Building>
      …
      <bldg:outerBuildingInstallation>
        <bldg:BuildingInstallation>
          <bldg:boundedBy>
            <bldg:RoofSurface> … </bldg:RoofSurface>
          </bldg:boundedBy>
        </bldg:BuildingInstallation>
      </bldg:outerBuildingInstallation>
      …
      <bldg:boundedBy>
        <bldg:WallSurface> … </bldg:WallSurface>
      </bldg:boundedBy>
      …
    </bldg:Building>

When setting *search depth* to "1" in this example, not only the
*bldg:Building* but also its nested *bldg:BuildingInstallation* and
*bldg:WallSurface* are searched for a matching LoD representation, but
**not** the *bldg:RoofSurface* of the *bldg:BuildingInstallation*. This
roof surface is on the nesting depth 2 when counted from the
*bldg:Building*. Thus, *search depth* would have to be set to "2" to also
consider this *bldg:RoofSurface* feature.


.. caution::
  The higher you choose the search depth, the more joins and sub queries
  are required on the database to consider all nested features to the
  specified depth. This might result in a slower query performance.