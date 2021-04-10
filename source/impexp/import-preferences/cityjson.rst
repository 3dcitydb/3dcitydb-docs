.. _impexp_cityjson_import_preferences:

CityJSON options
^^^^^^^^^^^^^^^^

.. note::
  These preference settings only apply to CityJSON input files.

.. figure:: /media/impexp_import_preferences_cityjson_fig.png
   :name: impexp_import_preferences_cityjson_fig
   :align: center

   Import preferences – CityJSON options.

CityJSON offers an extension mechanism similar to CityGML
Application Domain Extensions (ADEs). A *CityJSON Extension*
allows to extend the core data model of CityJSON by adding new attributes
to predefined city objects, defining new city object types and adding additional
properties to the root of a document.

The 3D City Database has support for storing and managing
CityGML ADEs in extra tables that seamlessly integrate with
the 3DCityDB core schema (see :numref:`citydb_managing_ades_chapter`).
The mechanisms can also be used for CityJSON Extensions but then
also requires a Java extension package for the Importer/Exporter
(see :numref:`ade_manager_plugin_impexp_extension_chapter`).

In contrast to CityGML datasets, the Importer/Exporter can parse
CityJSON Extensions in a more generic way and map new city object
types to GenericCityObject instances and additional attributes of
city objects to generic attributes. This way,
the extension data is not lost and can be stored in the core 3DCityDB
schema without the need for extra tables. Please note that the data
will, of course, also be exported as generic objects and attributes
again so that the original CityJSON Extension structure cannot be
restored. If you want to handle CityJSON Extensions in this generic
way, simply enable the corresponding option in the above
preference settings dialog.