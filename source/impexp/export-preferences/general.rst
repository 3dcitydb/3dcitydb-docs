.. _impexp_preferences_export_general:

General
^^^^^^^

The CityGML version preference settings let you choose the target
CityGML version when exporting 3D city model content from the database
to a CityGML dataset.

.. figure:: /media/impexp_export_preferences_citygml_version_fig.png
   :name: impexp_export_preferences_citygml_version_fig
   :align: center

   Export preferences – CityGML version.

The default value is CityGML *version 2.0.0*, which is the current
version of the OGC CityGML Encoding Standard. In addition, also the
preceding *version 1.0.0* is still supported.

.. note::
   CityGML 2.0.0 introduces new feature types such as bridges and
   tunnels that are not available in CityGML 1.0.0. If the 3D City Database
   instance contains features of these types, they *will be neglected* in
   an export to CityGML version 1.0.0 simply because they cannot be encoded
   in this version.