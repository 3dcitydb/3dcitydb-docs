.. _impexp_import_feature_counter_filter:

Feature counter filter
----------------------

The feature counter filter limits the number of top-level features to be imported.

.. figure:: /media/impexp_import_feature_counter_filter.png
   :name: impexp_import_feature_counter_filter_fig
   :align: center

   Feature counter filter for import operations.

Simply enter the number of features into the *count* field. The *start index* parameter indicates
the index within the set of all feature over all input files from which the import shall begin.
The parameters can be used together or individually.

.. note::
  The start index uses zero-based numbering. Thus, the first top-level feature is
  assigned the index 0, rather than the index 1.