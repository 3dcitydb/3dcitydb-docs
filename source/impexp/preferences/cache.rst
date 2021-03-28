.. _impexp_general_preferences_cache:

Cache
^^^^^

Both during CityGML imports at exports, the Importer/Exporter has to
keep track of various temporary information. For instance, when
resolving XLinks, the identifiers as well as additional information
about the features and geometries must be available. Since the
Importer/Exporter is designed to be able to process arbitrarily large
CityGML input files, keeping this information in main memory only is not
a promising strategy. For this reason, the information is written to
*temporary tables* in the database as soon as user-defined memory limits
are reached.

.. figure:: /media/impexp_preferences_general_cache_fig.png
   :name: impexp_preferences_general_cache_fig
   :align: center

   General preferences – Cache.

By default, temporary tables are created in the *3D City Database
instance* itself. The tables are populated during the import and export
operation and are automatically dropped after the operation has
finished. Alternatively, the user can choose to store the temporary
information in the *local file system* instead. An absolute path where
to create the file-based storage has to be provided. Either type the
location manually into the input field or use the *Browse* button to
open a file selection dialog. The file-based storage is also automatically
removed after the operation has finished.

Some reasons for using a file-based storage are:

-  The 3D City Database instance is kept clean from additional
   tables holding temporary process information.
-  If the Importer/Exporter runs on a different machine than the 3D City
   Database instance, sending temporary information over the network
   might be slow. In such cases, using a local storage might help to
   increase performance, especially if fast disk drives are used.