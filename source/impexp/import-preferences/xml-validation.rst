.. _impexp_import_preferences_xml_validation:

XML validation
^^^^^^^^^^^^^^

.. note::
  These preference settings only apply to CityGML input files.

On the *Import* tab of the operations window, the input files to
be imported into the database can be manually validated against the
official CityGML XML Schemas. This preference dialog lets a user choose
to perform XML validation automatically with every database import.

.. figure:: /media/impexp_import_preferences_xml_validation_fig.png
   :name: impexp_import_preferences_xml_validation_fig
   :align: center

   Import preferences – XML validation.

In general, it is **strongly recommended** to ensure (either manually or
automatically) that the input files are valid with respect to the
CityGML XML schemas. Invalid files might cause the import procedure to
behave unexpectedly or even to abort abnormally.

If XML validation is chosen to be performed automatically during
imports, every invalid top-level feature will be discarded from the
import. Nevertheless, the import procedure will continue to work on the
remaining features in the input file(s). To track which features have
been imported, you can enable the import log (see :numref:`impexp_import_preferences_import_logs`).

Validation errors are printed to the console window. Often, error
messages quickly become lengthy and confusing. To keep the console
output low, the user can choose to only report the first validation
error per top-level feature and to suppress all subsequent error
messages.

.. note::

   The XML validation in general does not require internet access
   since the CityGML XML schemas are packaged with the Importer/Exporter.
   These internal copies of the official XML schemas will be used to
   check CityGML XML content in input files. The user cannot change this
   behavior. External XML schemas will only be considered in case of
   unknown XML content, which might require internet access. Precisely,
   the following rules apply:

    -  If the namespace of an XML element is part of the official CityGML 2.0 or
       1.0 standard, it will be validated against the internal copies of
       the official CityGML 2.0 or 1.0 schemas (no internet access
       required).
    -  If the element’s namespace is unknown, the element will be validated
       against the schema pointed to by the *xsi:schemaLocation* value on
       the root element or the element itself. This is necessary when,
       for instance, the input document contains XML content from a
       CityGML Application Domain Extension (ADE). Note that loading the
       schema might require internet access.
    -  If the element’s namespace is unknown and the *xsi:schemaLocation*
       value (provided either on the root element or the element itself)
       is empty, validation will fail with a hint to the element and the
       missing schema document.