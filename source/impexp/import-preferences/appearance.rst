.. _impexp_import_preferences_appearance_chapter:

Appearance
^^^^^^^^^^

The *Appearance* preference settings define how appearance information
of city objects shall be processed at import time.

.. figure:: /media/impexp_import_preferences_appearance_fig.png
   :name: impexp_import_preferences_appearance_fig
   :align: center

   Import preferences – Appearance.

To *import appearances*, simply enable the corresponding checkbox. This
is also the default value. The Importer/Exporter will also import
all texture image files referenced from the appearances. Both relative
references (i.e., relative to the input file) and global
references (e.g., web URLs) to texture files are supported and resolved.
The latter might require internet access though. Alternatively, a user may
choose to only import the appearance information but to skip the
texture files by disabling the *import texture file* option.

Prior to version 1.0 of the CityGML standard, material and texture
information of surface objects was stored using *TexturedSurface*
elements. This concept was, however, replaced by the Appearance module in
CityGML 1.0 and thus has been deprecated. Although the CityGML
specification discourages the use of TexturedSurface elements, it is
still allowed even in CityGML 2.0 datasets. The Importer/Exporter can
parse and interpret TexturedSurface information but will automatically
convert this information losslessly to Appearance elements. Since
TextureSurface information is not organized into themes but a theme
should be used for Appearance elements, the user can
define a *theme* that shall be used in the conversion process. The
default value is *rgbTexture.*