.. _impexp_export_feature_counter_filter:

Feature counter filter
----------------------

The feature counter filter limits the number of top-level features to be exported.

.. figure:: /media/impexp_export_feature_counter_filter.png
   :name: impexp_export_feature_counter_filter_fig
   :align: center

   Feature counter filter for export operations.

Simply enter the number of features into the *count* field. The *start index* parameter indicates
the index within the result set from which the export shall begin. The
parameters can be used together or individually.

.. note::
  The start index uses zero-based numbering. Thus, the first top-level feature is
  assigned the index 0, rather than the index 1.

.. note::
  When using the feature counter filter with tiled exports, the
  *count* and *start index* settings are applied to each tile
  but not to set of all features from all tiles. For example, if you set
  *count* to 10, every tile will contain up to 10 features. The
  total number of exported features will therefore be greater
  than 10.