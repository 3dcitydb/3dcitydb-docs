.. _impexp_export_preferences_citygml_general_chapter:

CityGML general
^^^^^^^^^^^^^^^

.. note::
  These preference settings only apply to CityGML exports.

The general CityGML export options allow you to enable *pretty printing*
for the output file.

.. figure:: /media/impexp_export_preferences_citygml_general_fig.png
   :name: impexp_export_preferences_citygml_general_fig
   :align: center

   Export preferences – General CityGML options.

When enabled (default), indentation is used for the XML document
and every XML element is put on a new line. Without pretty printing,
the entire data is put on a single long line. The file size will
be considerably smaller without pretty printing, but the content is
difficult to visually comprehend. Pretty printing should therefore
be disabled in case the file is mainly used in machine-to-machine
communication.