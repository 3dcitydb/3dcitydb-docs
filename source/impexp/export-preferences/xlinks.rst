.. _impexp_export_preferences_xlinks_chapter:

XLinks
^^^^^^

Both the 3D City Database and the Importer/Exporter are capable of
handling XLinks. If the CityGML input document that is imported into the
3D City Database contains XLink references to features and/or
geometries, then this information is kept in the database in order to be
able to reconstruct the XLinks upon database export. This is also the
default behavior.

Depending on the target application that consumes the exported CityGML
dataset, this default behavior may be disadvantageous, especially if the
target application cannot follow and resolve XLink references. In such
cases, the XLinks preference settings let a user change the default
behavior so that the referenced objects are exported *by value* rather
than *by reference*. Put differently, instead of an XLink reference, a
copy of the original feature or geometry is placed into the CityGML
dataset. This necessarily requires that the gml:id of the copy is
different from the gml:id of the original object because identical
gml:id values are not allowed in the same dataset. The Importer/Exporter
takes care of this issue and creates new gml:id values for the copies
based on UUID values.

.. figure:: /media/impexp_export_preferences_xlinks_fig.png
   :name: impexp_export_preferences_xlinks_fig
   :align: center

   Export preferences – XLinks.

The user can define the behavior for exporting XLinks differently for
features [1] and geometries [2]. The settings allow providing a
*prefix* string that will be used when creating new gml:id values
(default: “\ *UUID\_*\ ”) and whether the original gml:id should be
appended to the newly created one. Only for features, the user can
additionally choose to store the original gml:id as ``<ExternalReference>``
property in the copied feature.