.. _impexp_import_preferences_identifier:

Object identifier
^^^^^^^^^^^^^^^^^

Globally unique object identifiers are crucial for ensuring data
consistency and for enabling data management workflows. Especially when
it comes to (subsequently) updating the city model content in the
database, unique identifiers will help to quickly identify and replace
objects in the database with candidates from external datasets.
Unfortunately, both in CityGML and CityJSON, identifiers do not meet
the requirement of global uniqueness since they are, per definition,
only unique within the scope of a single dataset.

CityGML uses the XML attribute ``gml:id`` to store identifiers for both
features and geometries as shown below:

.. code-block:: xml

   <bldg:Building gml:id="fid_1234">
     ...
     <bldg:lod2Solid>
       <gml:Solid gml:id="gid_5678">
       ...
       </gml:Solid>
     </bldg:lod2Solid>
     ...
   </bldg:Building>

In CityJSON, city objects are stored in the ``"CityObjects"`` property.
The value of this property is a collection of key-value pairs, where the key
is the identifier of the city object, and the value is the city object itself.
Note that CityJSON does not have identifiers for geometry objects.

.. code-block:: json

   {
     "type": "CityJSON",
     "CityObjects": {
       "fid_1234": {
         "type": "Building",
         "geometry": [{ }]
       }
     }
   }


By default, the Importer/Exporter assumes that the identifiers
associated with the city objects to be imported are globally unique and
therefore imports them “as is” into the database. Only in case a city
object (or geometry object) lacks an identifier, a UUID value will be
generated at import time and stored with the object.

.. figure:: /media/impexp_import_preferences_gmlid_handling_fig.png
   :name: impexp_import_preferences_gmlid_handling_fig
   :align: center

   Import preferences – Object identifier.

This default behavior can be changed with the above preferences dialog in
order to let the Importer/Exporter replace all identifiers in the
input file(s) with generated UUID values. The user may choose a prefix
for the identifier. The original identifier value may optionally be
stored as external reference to not lose this information.

In addition to the identifier, the 3DCityDB allows for storing a second
GMLID_CODESPACE metadata value. The idea is that the compound value of
identifier and GMLID_CODESPACE is globally unique. The user can choose to
use the file name of the input file, its complete path or a
user-defined string as GMLID_CODESPACE. By default, the
Importer/Exporter does not import a GMLID_CODESPACE value though.

.. note::
   The Importer/Exporter internally **only relies** on the identifier value
   to identify objects, for example, when resolving XLink references. The
   GMLID_CODESPACE value is meant to support user-defined data management
   processes in the first place.