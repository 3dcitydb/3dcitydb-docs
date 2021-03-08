.. _impexp-db-change-crs:

Managing the spatial reference system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When setting up a 3DCityDB instance, you have to choose a spatial
reference system (SRS) by picking a spatial reference ID (SRID)
supported by the database and a corresponding SRS name identifier
(*gml:srsName*) that is used in CityGML exports (see
and :numref:`3dcitydb_setup_schema_chapter`).
These settings can be easily changed at any later time using the
reference system operation.

.. figure:: /media/impexp_gui_change_srs_fig.png
   :name: impexp_gui_change_srs_fig
   :align: center

   Changing the SRS information of the 3DCityDB instance.

After connecting to a 3DCityDB, the *SRID* and *gml:srsName* input
fields shown in the above dialog [1] are assigned the current values
from the database. Simply edit the fields to pick a new SRID or SRS name
identifier. Since changing the SRID potentially affects all geometries
in your database and thus may take a long time to complete, the *SRID*
field is disabled per default. Click on *Edit* [2] to enable changes to
this field. Use the *Check* button [2] to make sure that your new SRID
value is supported by the database. The *gml:srsName* field provides a
drop-down list of common SRS identifier encoding schemes (such as OGC
URN encoding, see :numref:`citydb_crs_definition_chapter`). You may pick one of these proposals
(be careful to replace the HEIGHT_SRID token with the correct value if
required) or enter any other value.

When changing the SRID, you can choose whether the *coordinates* of
geometry objects already stored in the database should be *transformed*
to the new SRID or whether only the *metadata* should be *updated* [3].
The latter option might be enough, for example, if you accidentally
picked a wrong SRID that does not match the imported geometries when
setting up the database, and you simply want to correct this mistake.

Click on *Apply* to update the reference system information in the
database according to your settings. The *Restore* button lets you
discard any changes made to the *SRID* and *gml:srsName* fields.

.. note::
   If you just want to use different *gml:srsName* values for
   different CityGML exports, then instead of changing the identifier in
   the database before every export it is simpler to create multiple
   user-defined reference systems for the same SRID (cf. :numref:`impexp_crs_management_chapter`) and
   pick one for each CityGML export (cf. :numref:`impexp_citygml_export_chapter`).