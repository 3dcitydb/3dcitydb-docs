.. _impexp_export_preferences_citygml_general_chapter:

CityGML general
^^^^^^^^^^^^^^^

.. note::
  These preference settings only apply to CityGML exports.

.. figure:: /media/impexp_export_preferences_citygml_general_fig.png
   :name: impexp_export_preferences_citygml_general_fig
   :align: center

   Export preferences – General CityGML options.

The general CityGML export options allow you to enable *pretty printing*
for the output file. When enabled (default), indentation is used for the XML document
and every XML element is put on a new line. Without pretty printing,
the entire data is put on a single long line. The file size will
be considerably smaller without pretty printing, but the content is
difficult to visually comprehend. Pretty printing should therefore
be disabled in case the file is mainly used in machine-to-machine
communication.

If your database contains *global appearances*, you can choose to convert
them into *local appearances* during export. Global appearances are stored as
``<appearanceMember>`` properties of the ``<CityModel>`` root element in the output file.
They typically affect more than one city object and, thus, are not stored
as property of a single city object. When enabling this option, the export operation
dissolves the global appearances and stores the appearance information as
property of each city object to which it belongs (so-called *local appearance*).
The CityGML specification recommends using local appearances whenever possible.
This option does not change the contents in the database but only affects the
structure of the output file(s).

.. note::
  Global appearances are the only way to assign textures or materials to
  implicit geometries that are used by multiple city objects. Thus, global
  appearances of implicit geometries are not dissolved when enabling this option.