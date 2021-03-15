.. _impexp_export_preferences_resources_chapter:

Resources
^^^^^^^^^

Just like with CityGML imports, the export process is implemented based
on multithreaded data processing in order to increase the overall
application performance. Likewise, in order to reconstruct XLinks during
exports (cf. :numref:`impexp_export_preferences_xlinks_chapter`),
the export process also needs to keep
track of each and every gml:id of exported features and geometry
objects. For fast access, the gml:id values are kept in main memory and
are only paged to temporary database tables in case the predefined cache
size limit is reached.

.. figure:: /media/impexp_export_preferences_resources_fig.png
   :name: impexp_export_preferences_resources_fig
   :align: center

   Export preferences – Resources.

The Resource preferences allow for setting the number of *concurrent
threads* to be used in the export process and for defining the *sizes*
and *page factors* of the gml:id caches for features and geometries. The
meaning of the values is identical to the Resource preferences for
CityGML imports. So, please refer to
:numref:`impexp_import_preferences_resources_chapter` for more details.