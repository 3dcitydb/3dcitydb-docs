.. _impexp_plugin_spshg_feature_version_filter:

Feature version filter
----------------------

In both CityGML and CityJSON, the temporal *creationDate* and *terminationDate*
attributes can be used to represent different versions of the same feature
that are valid at different points in time. The 3D City Database allows for
storing multiple versions of the same feature to enable object histories. The
timestamps are stored in the CREATION_DATE and TERMINATION_DATE columns of
the CITYOBJECT table.

Using the feature version filter, a user can choose which version of the
top-level features should be selected in a table export operation.

.. figure:: /media/impexp_plugin_spshg_feature_version_filter.png
   :name: impexp_export_feature_version_filter_fig
   :align: center

   Feature version filter for table export operations.

The different feature version options available from the drop-down list are described below.

.. list-table:: Overview of the different feature version options
   :name: impexp_plugin_spshg_feature_versions_table
   :widths: 20 80

   * - | **Feature Version**
     - | **Description**
   * - | ``Latest version``
     - | Selects top-level features that are **not** marked as terminated in the database and, thus, whose TERMINATION_DATE attribute is ``null``.
   * - | ``Valid version``
     - | Selects top-level features that were valid *at a given timestamp* or *for a given time range*. The filter is evaluated against the CREATION_DATE and TERMINATION_DATE attributes.
   * - | ``Terminated version``
     - | Selects only *terminated* top-level features. You can choose to either select *all* terminated features or only those that were terminated *at a given timestamp*. The filter is evaluated against the TERMINATION_DATE attribute.

For example, you can use *Valid version* to export attributes from a past status of your 3D city model
(e.g., at *March 1st, 2018*) and compare them to the current version.

.. note::
  For the feature version filter to work correctly, you must make sure that
  the validity times of subsequent feature versions **do not overlap**.
  The Importer/Exporter does not provide specific tools for managing
  feature versions in the database.

.. hint::
  If your 3D City Database does not contain multiple feature versions, you
  should always disable the feature version filter to avoid unnecessarily complex
  SQL queries.