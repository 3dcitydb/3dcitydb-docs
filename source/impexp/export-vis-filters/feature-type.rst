.. _impexp_export_vis_feature_type_filter:

Feature type filter
-------------------

With the feature types filter, you can restrict the export to one or more
features types by enabling the corresponding checkboxes. Only features of the
selected type(s) will be exported.

.. figure:: /media/impexp_export_vis_feature_type_filter.png
   :name: impexp_export_vis_dialog_fig
   :align: center

   Feature type filter for visualization export operations.

The feature type filter only shows top-level feature types. It will automatically
contain feature types from CityGML ADEs if a corresponding ADE extension has been correctly
registered with the Importer/Exporter (see :numref:`impexp_plugin_ade_manager_chapter`).

If a predefined CityGML feature type cannot be represented in the LoD that
has been chosen by the user for the visualization export on the *VIS Export* tab
(see :numref:`impexp_kml_export_chapter`), this feature type will be greyed out
in the filter and thus cannot be selected.

.. note::
   When exporting *city object groups*, the following additional
   rules apply:

   1. If only the feature type *CityObjectGroup* is checked, then all city
      object groups **together with all their group members** (independent of their
      feature types) are exported.
   2. If further feature types are selected in addition to
      *CityObjectGroup*, then only group members matching those feature
      types are exported. Of course, all features that match the type
      selection but are not group members are also exported.

.. caution::
   Support for terrain data in visualization exports is
   limited to the *TINRelief* feature type. Other relief types such
   as *MassPointRelief*, *BreaklineRelief*, and *RasterRelief* are not
   supported. Due to the typically large extent of a
   relief feature it is **recommended** to export it without tiling.
   Alternatively, the relief feature can be split into smaller
   components matching the desired tile size for visualization exports.
   However, this must be applied to the original terrain data
   **prior to** importing it into the 3DCityDB.
   The Importer/Exporter **does not automatically clip terrain data**
   at export time.