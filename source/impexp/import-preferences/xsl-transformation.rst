.. _impexp_import_preferences_xsl_transformation:

XSL Transformation
^^^^^^^^^^^^^^^^^^

.. note::
  These preference settings only apply to CityGML input files.

The *XSL Transformation* settings are used to apply changes to the CityGML input data
before it is imported into the database using XSL transformations.
Simply check the *Apply XSLT stylesheets* option and point to an XSLT
stylesheet in your local file system using the *Browse* button. The
stylesheet will be automatically considered by the import process to
transform the CityGML data.

.. figure:: /media/impexp_import_preferences_xsl_fig.png
   :name: impexp_import_preferences_xsl_fig
   :align: center

   Import preferences – XSL transformation.

By clicking the ``+`` and ``-`` buttons, more than one XSLT stylesheet can
be provided. The stylesheets are executed in the given order,
with the output of a stylesheet being the input for its direct
successor. The Importer/Exporter is shipped with example XSLT
stylesheets in subfolders below ``templates/XSLTransformations`` in the
installation directory.

.. note::
   - To be able to handle arbitrarily large input files, the importer
     chunks every CityGML input file into top-level features, which are then
     imported into the database. Each XSLT stylesheet will hence just work on
     individual top-level features but not on the entire file. Make sure to
     consider this when developing your XSLT.
   - The output of each XSLT stylesheet must again be a valid CityGML
     structure.
   - Only stylesheets written in the XSLT language version 1.0 are
     supported.