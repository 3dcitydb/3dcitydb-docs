.. _impexp_import_preferences_import_log:

Import log
^^^^^^^^^^

An import process not necessarily works on all top-level features
contained in the provided input file(s). An obvious reason is that
spatial or thematic filters naturally narrow down the set of
imported features. Also, in case the import procedure aborts early
(either requested by the user or caused by severe errors), not
all input features might have been processed. To understand which
top-level features were actually loaded into the database during an
import session, the user can choose to let the Importer/Exporter create
an *import log*.

.. figure:: /media/impexp_import_preferences_log_fig.png
   :name: impexp_import_preferences_log_fig
   :align: center

   Import preferences – Import log.

Simply enable the checkbox on this settings dialog to activate import
logs (disabled per default). You additionally must provide a folder
where the import log files will be created in. Either type the folder
name manually or use the *Browse* button to open a file selection
dialog. The timestamp of the import session is used to
create unique filenames for the log files inside this folder
according to following pattern:

``imported-features-yyyy_MM_dd-HH_mm_ss_SSS.log``

The import log is a simple CSV file with one record (line) per imported
top-level feature. The following figure shows an example.

.. figure:: /media/impexp_import_log_example_fig.png
   :name: impexp_import_log_example_fig
   :align: center

   Example import log.

The first three lines of the import log contain metadata about the
*version of the Import/Exporter* that was used for the import,
the database *connection string*, and the *timestamp of the import*.
Each metadata line starts with the ``#`` character as comment marker.

The first line below the metadata block provides a header for the fields
of each record. The field names are FEATURE_TYPE, CITYOBJECT_ID, GMLID_IN_FILE,
and INPUT_FILE. A single comma separates the fields. The records follow
the header line. The meaning of the fields is as follows.

.. list-table::  Fields of the CSV import log file
   :name: impexp_import_log_csv_table
   :widths: 30 70

   * - | **Field name**
     - | **Description**
   * - | FEATURE_TYPE
     - | An string representing the typename of the imported CityGML feature.
   * - | CITYOBJECT_ID
     - | The value of the ID column (primary key) of the CITYOBJECT table where the feature was inserted.
   * - | GMLID_IN_FILE
     - | The original object identifier of the feature in the input file (might differ in database due to import settings).
   * - | INPUT_FILE
     - | The path of the input file from which the feature was imported.

The last line of each import log is a footer that contains metadata
about whether the import was *successfully finished* or *aborted*.

.. note::
  If an import process was aborted by the user or due to
  errors, the import log file can also be used to automatically
  delete the features that were imported until the process terminated.
  Simply use the ``delete`` command of the Importer/Exporter command-line
  interface for this purpose. You can directly feed CSV files like the import log
  to this command to delete all features listed in the CSV file (see
  :numref:`impexp_cli_delete_command` for more information). This way, you can
  ensure a consistent database state even if an import process fails.
