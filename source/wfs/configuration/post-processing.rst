.. _wfs_postprocessing_settings_chapter:

Postprocessing settings
~~~~~~~~~~~~~~~~~~~~~~~

.. note::
  These settings are only applied when a client chooses CityGML as output format.

The *postprocessing* settings allow for specifying XSLT transformations
that are applied on the CityGML data of a WFS response before sending
the response to the client.

.. code-block:: xml

   <postProcessing>
     <xslTransformation isEnabled="true">
       <stylesheet>AdV-coordinates-formatter.xsl</stylesheet>
     </xslTransformation>
   </postProcessing>

To enable transformations, set the *isEnabled* attribute on the
``<xslTransformation>`` child element to *true*. In addition, provide one or
more ``<stylesheet>`` elements enumerating the XSLT stylesheets that shall
be applied in the transformation. The stylesheets are supposed to be
stored in the ``xslt-stylesheets`` subfolder of the ``WEB-INF`` folder of your
WFS application. Thus, any relative path provided as ``<stylesheet>`` will
be resolved against ``WEB-INF/xslt-stylesheets/``. You may alternatively
provide an absolute path pointing to another location in your local file
system. However, note that the WFS web application must have appropriate
access rights to this location.

If you provide more than one XSLT stylesheet, then the stylesheets are
executed in the given sequence of the ``<stylesheet>`` elements, with the
output of a stylesheet being the input for its direct successor.

.. note::
   - To be able to handle arbitrarily large exports, the export
     process reads single top-level features from the database, which are
     then written to the target file. Thus, each XSLT stylesheet will just
     work on individual top-level features but not on the entire file.
   - The output of each XSLT stylesheet must again be a valid CityGML
     structure.
   - Only stylesheets written in the XSLT language version 1.0 are
     supported.