.. _impexp_export_preferences_xsl_transformation:

XSL Transformation
^^^^^^^^^^^^^^^^^^

As for CityGML imports, you can apply XSLT transformations
during the export process to change the resulting CityGML output data.
Simply check the *Apply XSLT stylesheets* option and point to an XSLT
stylesheet in your local file system using the *Browse* button. The
stylesheet will be automatically considered by the export process to
transform the CityGML data before it is written to a file.

.. figure:: /media/impexp_export_preferences_xsl_fig.png
   :name: impexp_export_preferences_xsl_fig
   :align: center

   Export preferences – XSL transformation.

By clicking the + and - buttons, more than one XSLT stylesheet can be
provided to the exporter. The stylesheets are applied in the given order,
with the output of a stylesheet being the input for its direct
successor. The Importer/Exporter is shipped with example XSLT
stylesheets in the folder ``templates/XSLTransformations`` of the
installation directory.

.. note::
   - To be able to handle arbitrarily large exports, the export
     process reads single top-level features from the database, which are
     then written to the target file. Each XSLT stylesheet will thus just
     work on individual top-level features but not on the entire file.
   - The output of each XSLT stylesheet must again be a valid CityGML
     structure.
   - Only stylesheets written in the XSLT language version 1.0 are
     supported.