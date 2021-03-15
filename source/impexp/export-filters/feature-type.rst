.. _impexp_export_feature_type_filter:

Feature type filter
-------------------

With the feature types filter, you can restrict the export to one or more
features types by enabling the corresponding checkboxes. Only features of the
selected type(s) will be exported.

.. figure:: /media/impexp_export_feature_type_filter.png
   :name: impexp_CityGML_export_dialog_fig
   :align: center

   Feature type filter for export operations.

The feature type filter only shows top-level feature types. It will automatically
contain feature types from CityGML ADEs if a corresponding ADE extension has been correctly
registered with the Importer/Exporter (see :numref:`impexp_plugin_ade_manager_chapter`).

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