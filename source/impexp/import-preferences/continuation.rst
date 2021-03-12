.. _impexp_import_preference_continuation:

Continuation
^^^^^^^^^^^^

The *Continuation* preferences allow for specifying metadata that is
assigned to every city object during import. The metadata is stored in
columns of the table CITYOBJECT and is therefore accessible in SQL
queries.

.. figure:: /media/impexp_import_preferences_continuation_fig.png
   :name: impexp_import_preferences_continuation_fig
   :align: center

   Import preferences – Continuation.

The following metadata can be set:

.. list-table:: Metadata stored with every city object in the table CITYOBJECT.
   :name: impexp_cityobject_metadata_table
   :widths: 30 70

   * - | **ADE Metadata**
     - | **Description**
   * - | Data lineage [1]
     - | A string value denoting the origin of the data.
       | (column: LINEAGE; default value: NULL)
   * - | Reason for update [1]
     - | A string value providing the reason for a data update.
       | (column: REASON_FOR_UPDATE; default value: NULL)
   * - | Updating person [2]
     - | A string value identifying the person being responsible for importing or updating the city object.
       | (column: UPDATING_PERSON; default value: name of the database user)
   * - | creationDate [3]
     - | A timestamp value denoting the date of creation of the city object. If
       | this date is not available from the city object during import, it
       | may either be set to the import date or be inherited from the parent
       | object (if available). Alternatively, the user can choose to replace all
       | creation dates from the input files with the import date.
       | (column: CREATION_DATE; default value: import date)
   * - | terminationDate [4]
     - | A timestamp value denoting the date of termination of the city object. If
       | this date is not available from the city object during import, it
       | may either be set to NULL or be inherited from the parent object (if
       | available). Alternatively, the user can choose to replace all termination
       | dates in the input files with NULL.
       | (column: TERMINATION_DATE; default value: NULL)

.. note::
   Both *creationDate* and *terminationDate* are properties available for city objects
   in both CityGML and CityJSON. When exporting data from the database, the
   properties are therefore written to a CityGML/CityJSON dataset.
   The remaining metadata information, however, does not map to predefined properties
   and does not get exported with this version of the Importer/Exporter.