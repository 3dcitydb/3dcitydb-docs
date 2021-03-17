.. _impexp_export_preferences_resources_chapter:

Resources
^^^^^^^^^

.. figure:: /media/impexp_export_preferences_resources_fig.png
   :name: impexp_export_preferences_resources_fig
   :align: center

   Export preferences – Resources.

**Multithreaded processing**

The export process is implemented based
on multithreaded data processing in order to increase the overall
export performance. The *Resource* preferences allow for setting the
minimum and maximum number of *concurrent threads* to be used in the
export process [1]. Make sure to enter reasonable values depending on
your hardware configuration. By default, the maximum number is set to
the number of available CPUs/cores times two.

.. caution::
   A higher number of threads *does not necessarily result in a
   better performance*. In contrast, a too high number of active threads
   faces disadvantages such as thread life-cycle overhead and resource
   thrashing. Also note that each thread requires its *own physical
   connection to the database*. Therefore, your database must be ready to
   handle enough parallel physical connections. Ask you database
   administrator for assistance.

**Batch processing**

In order to optimize database response times, multiple features
satisfying the export filters are requested from a feature table
with a single SELECT statement rather than using a separate request
for each feature (*batch processing*). This allows for reducing
the number of overall SELECT statements that are sent to the database
to fetch the data. Especially in case the Importer/Exporter and the database
server are not running in the same local network and, thus, the
network latency is high, batch processing can help to reduce the export
time. Batch processing is also applied in the same way for fetching
geometries and BLOB data such as texture images. The preferences dialog
allows for setting the number of objects to be fetched with one request
for all three cases [2] (default: 30).

.. note::
   All objects requested within one batch from the database are
   processed in main memory. Thus, the Importer/Exporter might run
   out of memory if the batch size is too high
   (see :numref:`impexp_launching_chapter` for how to increase the available
   main memory).

**Object identifier cache**

To be able to reconstruct XLink references
for CityGML exports (cf. :numref:`impexp_export_preferences_xlinks_chapter`),
the export process needs to keep track of the identifiers (*gml:id* values) of
exported features and geometry objects. For fast access, the identifiers
are kept in main memory and are only drained to temporary tables
in case the predefined cache size limit is reached.

You can define the maximum number of identifiers to be held in main memory
as well as the *page factor* and number of parallel *table partitions*
used for the feature and geometry caches [3]. The meaning of the values is identical to
the *Resource* preferences for the import operation. So, please refer to
:numref:`impexp_import_preferences_resources_chapter` for more details.

.. note::
  By default, the temporary tables for draining the caches are created
  in the same 3D City Database instance. You can also choose
  to use a local cache instead (see :numref:`impexp_general_preferences_cache` for more details).
  However, note that some temporary information must be stored in the
  database even if you use a local cache to be able to perform JOINs between
  temporary tables and tables of the 3DCityDB schema.