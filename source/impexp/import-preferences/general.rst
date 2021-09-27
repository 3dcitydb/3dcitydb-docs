.. _impexp_preferences_import_general:

General
^^^^^^^

This preferences dialog lets you define general settings affecting
the import operation.

.. figure:: /media/impexp_import_preferences_general_fig.png
   :name: impexp_import_preferences_general_fig
   :align: center

   Import preferences – General.

When importing a CityGML/CityJSON dataset, the import operation might run into errors, for instance, because of
invalid data violating the CityGML or CityJSON schemas, missing external resources such as texture images
or invalid geometries. The default behaviour of the import operation is to *fail fast* on errors and to immediately
cancel the import process. This way, invalid top-level city objects will not be imported into your
database. If your import aborts due to errors, you can use the import log to resume or rollback the import operation
(see :numref:`impexp_import_preferences_import_log` for more details).

You can disable the fail fast behaviour by unchecking the *Cancel import immediately in case of errors* option
offered by this preferences dialog. When doing so, errors encountered during the import process are still recorded
in the log but the operation tries to continue and complete the import.