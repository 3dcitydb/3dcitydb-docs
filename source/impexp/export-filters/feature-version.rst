.. _impexp_export_feature_version_filter:

Feature version filter
----------------------

In both CityGML and CityJSON, the temporal *creationDate* and *terminationDate*
attributes can be used to represent different versions of the same feature
that are valid at different points in time. The 3D City Database allows for
storing multiple versions of the same feature to enable object histories. The
timestamps are stored in the CREATION_DATE and TERMINATION_DATE columns of
the CITYOBJECT table.

Using the feature version filter, a user can choose which version of the
top-level features should be selected in an export operation.

.. figure:: /media/impexp_export_feature_version_filter.png
   :name: impexp_export_feature_version_filter_fig
   :align: center

   Feature version filter for export operations.

The default option *Latest version* will only select those top-level features
that have not been marked as terminated in the database and, thus, whose
TERMINATION_DATE is ``null``. When switching to *Valid version*, you can specify that only
features that were valid at a given timestamp or within a given time range should
be considered. This is done by evaluating the CREATION_DATE and TERMINATION_DATE
values in the database against the specified filter values. For example,
you can use *Valid version* to query a past status of your 3D city model
(e.g., at *March 1st, 2018*) and compare it to the current version.

.. note::
  For the feature version filter to work correctly, you must make sure that
  the validity times of subsequent feature versions **do not overlap**.
  The Importer/Exporter does not provide specific tools for managing
  feature versions in the database.

.. hint::
  If your 3D City Database does not contain multiple feature versions, you
  should always disable the feature version filter to avoid unnecessarily complex
  SQL queries.

.. caution::
  Typically, the same object identifier is used for the different feature
  versions to be able to relate them to each other. If your data is structured
  like this and you export all versions to the same output file (e.g., by
  disabling the feature version filter), then this will result in duplicate
  ``gml:id`` values for CityGML and, thus, the output file will be invalid
  according to the CityGML XML schema. You can still process the data though.

  For CityJSON, the object identifier is used as key for the ``"CityObjects"``
  property. Thus, it is not possible to write multiple city objects sharing
  the same identifier to the same file. As a consequence, the output file
  will always contain just one version but not all versions.