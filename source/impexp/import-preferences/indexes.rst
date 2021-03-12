.. _impexp_import_preferences_indexes:

Indexes
^^^^^^^

In addition to the *Database* tab on the operations window, which lets you
enable and disable spatial and normal indexes in the 3D City Database
manually (cf. :numref:`impexp-db-indexes`), this preference dialog lets
you set a default index strategy for database imports.

.. figure:: /media/impexp_import_preferences_indexes_fig.png
   :name: impexp_import_preferences_indexes_fig
   :align: center

   Import preferences – Indexes.

The dialog differentiates between settings for *spatial indexes* [1] and
*normal indexes* [2] but offers the same options for each index type.

The default setting is to not change the status (i.e., either enabled or
disabled) of the indexes. This default behavior can be changed so that
indexes are always disabled before starting an import process. The user
can choose whether the indexes shall be automatically reactivated after
the import has been finished.

.. note::
   It is *strongly recommended* to *deactivate the spatial indexes
   before running a CityGML/CityJSON import* on a *big amount of data* and to
   reactive the spatial indexes afterwards. This way the import will
   typically be a lot faster than with spatial indexes enabled. The
   situation may be different when importing only a small dataset.

.. caution::
   Activating and deactivating indexes can take a long time,
   especially if the database fill level is high. Note that the operation
   **cannot be aborted** by the user since this would result in an
   inconsistent database state.