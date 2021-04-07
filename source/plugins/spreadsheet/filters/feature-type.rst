.. _impexp_plugin_spshg_feature_type_filter:

Feature type filter
-------------------

With the feature types filter, you can restrict the export to one or more
features types by enabling the corresponding checkboxes. Only features of the
selected type(s) will be exported.

.. figure:: /media/impexp_plugin_spshg_feature_type_filter.png
   :name: impexp_CityGML_export_dialog_fig
   :align: center

   Feature type filter for table export operations.

The feature type filter only shows top-level feature types.

.. caution::
   The *Spreadsheet Generator* plugin does not support CityGML
   ADE extensions. Thus, even if you have registered an
   ADE extension with the 3D City Database and the Importer/Exporter,
   the feature type filter will not automatically
   contain feature types from the corresponding CityGML ADE.