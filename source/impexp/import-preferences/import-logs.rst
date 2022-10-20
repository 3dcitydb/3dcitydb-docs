.. _impexp_import_preferences_import_logs:

Import logs
^^^^^^^^^^^

This preference settings allow you to create additional log files during an import operation that record
the successfully imported top-level city objects (*import log*) or provide a list of the duplicate objects found in
the database (*duplicate log*).

.. figure:: /media/impexp_import_preferences_logs_fig.png
   :name: impexp_import_preferences_logs_fig
   :align: center

   Import preferences – Import logs.

**Import log**

An import process not necessarily works on all top-level features
contained in the provided input file(s). An obvious reason is that
spatial or thematic filters naturally narrow down the set of
imported features. Also, in case the import procedure aborts early
(either requested by the user or caused by severe errors), not
all input features might have been processed. To understand which
top-level features were actually loaded into the database during an
import session, a user can choose to let the Importer/Exporter create
an *import log*.

Simply enable the *Record imported top-level features in log file* checkbox on this settings dialog to activate import
logs (disabled per default). You additionally must provide the full path
to the log file that shall be used to record the imported features. Either type the
file name manually or use the *Browse* button to open a file selection
dialog. The following modes for creating the log file are supported:

1. To ensure that every import operation uses a **unique file name** for the import
   log, you can choose to let the import process append a timestamp of the form
   ``yyyy-MM-dd_HH-mm-ss-SSS`` as suffix to the provided file name (default option).
   This way, every import operation will automatically be recorded in a separate log file.
   For example, when choosing ``import.log`` as file name, the import log will be
   stored in:

   ``import-yyyy-MM-dd_HH-mm-ss-SSS.log``

2. When disabling the default option, all import operations are **logged into the same file**.
   By default, new log entries are *appended* to the end of the file so that entries
   from previous import operations are not lost. You can alternatively choose to *truncate* the log
   file before every import operation by checking the corresponding option. Use
   this option with care.

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
     - | A string representing the type of the imported top-level feature.
   * - | CITYOBJECT_ID
     - | The value of the ID column (primary key) of the CITYOBJECT table where the feature was inserted.
   * - | GMLID_IN_FILE
     - | The original object identifier of the feature in the input file. Note: the GMLID in the database might differ from the original identifier due to import settings.
   * - | INPUT_FILE
     - | The path of the input file from which the feature was imported.

The last line of each import log is a footer that contains metadata
about whether the import was *successfully finished* or *aborted*.

.. hint::
  If an import process was aborted by the user or failed due to
  errors, the import log file can also be used to automatically
  **resume** or **rollback** the import operation. Thus, it
  helps you to ensure a consistent database state.

  - To **resume** the import, you can use the import log as input for the *import list filter*
    and set the filter to *skip* all city objects from the list (see
    :numref:`impexp_import_list_filter`). When re-running the import with these settings,
    only the city objects that have not been processed in the first run will be imported.
  - A **rollback** can be achieved by feeding the import log as delete list to
    the ``delete`` command of the Importer/Exporter command-line interface (see
    :numref:`impexp_cli_delete_command`).

**Duplicate log**

In addition to the list of successfully imported features, you can let the import operation *record
already existing top-level features* in a separate log file. If enabled, this so-called *duplicate log* will list
all city objects in the database having an identical identifier with a top-level city object from the input
file(s) that are used in the import process.

Simply provide a file or directory where the duplicate log should be stored. In case of a directory, the
Importer/Exporter will choose a unique file name for every import operation based on the following pattern:

``duplicate-features-yyyy-MM-dd_HH-mm-ss-SSS.log``

If you provide the file name for the log file instead, this log file will be used by the Importer/Exporter for all
import operations. In contrast to the import log, the duplicate log **always gets truncated** when a new import
operation is started.

Just like the import log, the duplicate log is a simple CSV file with one record per duplicate object. The layout
of the CSV file is almost identical to the import log and shown in the figure below.

.. figure:: /media/impexp_duplicate_log_example_fig.png
   :name: impexp_duplicate_log_example_fig
   :align: center

   Example duplicate log.

The following table explains the meaning of the fields in the duplicate log.

.. list-table::  Fields of the CSV duplicate log file
   :name: impexp_duplicate_log_csv_table
   :widths: 30 70

   * - | **Field name**
     - | **Description**
   * - | CITYOBJECT_ID
     - | The value of the ID column (primary key) of the CITYOBJECT table where the duplicate object is stored.
   * - | GMLID
     - | The duplicate object identifier that is shared by the object in the database and the top-level city object from the input file.
   * - | FEATURE_TYPE_IN_DB
     - | A string representing the type of the duplicate object in the database.
   * - | FEATURE_TYPE_IN_FILE
     - | A string representing the type of the top-level city object from the input file.
   * - | INPUT_FILE
     - | The path of the input file for which duplicates were found in the database.