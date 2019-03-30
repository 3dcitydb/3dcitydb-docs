XML query expressions
~~~~~~~~~~~~~~~~~~~~~

A query expression is an action that directs the export operation to
search the 3DCityDB for city objects that satisfy some filter expression
encoded within the query. Query expressions are given in XML using a
<query> root element. The XML language used is specific to the
Importer/Exporter and the 3DCityDB but draws many concepts from OGC
standards such as *Filter Encoding* (FE) 2.0 and *Web Feature Service*
(WFS) 2.0.

.. note::
   All XML elements of the query language are defined in the XML
namespace http://www.3dcitydb.org/importer-exporter/config. Simply
define this namespace as default namespace on your <query> root element.

A query expression may contain a *typeNames* parameter, a *projection
clause*, a *selection clause*, a *counter filter*, an *LoD filter*, an
*appearance filter*, *tiling* options and a *targetSrid* attribute for
coordinate transformations.

=============== ===========================================================================================================================================
<typeNames>     Lists the name of one or more feature types to query (*optional*).
=============== ===========================================================================================================================================
<propertyNames> Projection clause that identifies a subset of optional feature properties that shall be kept or removed in the target dataset (*optional*).
<filter>        Selection clause that specifies criteria that conditionally select city objects from the 3DCityDB (*optional*).
<count>         Limits the number of requested city objects that are exported to the target dataset (*optional*).
<lod>           Limits the LoDs of the exported city objects to a given subset (*optional*).
<appearance>    Limits the appearances of the exported city objects to a given subset (*optional*).
<tiling>        Defines a tiling scheme for the export (*optional*).
*targetSrid*    Defines a coordinate transformation *(optional)*.
=============== ===========================================================================================================================================


Using XML queries in batch processes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Importer/Exporter offers a Command-Line Interface (CLI) which allows
for embedding the tool in batch processing workflows and third-party
applications (cf. chapter 5.8). XML queries can also be used in CityGML
exports that are triggered via this CLI interface. For this purpose, the
XML query has to be copied into the *config file* that is used for
running the Importer/Exporter. This can be either the *default config
file* (cf. chapter 5.1) or a local file that is passed to the CLI using
the -config command-line parameter.

Each config file must use a <project> root element associated with the
XML namespace http://www.3dcitydb.org/importer-exporter/config. Export
settings are then provided in the <export> subelement. The <query>
element of an XML query expression can simply be copied as child element
of the <export> element. In addition, the *useSimpleQuery* attribute on
the <export> element has to be set to *false*.

The listing below shows an excerpt of a config file using an XML export
query.

| <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
| <project xmlns="http://www.3dcitydb.org/importer-exporter/config">
| <database>
| … database connection details go here …
| </database>
| <export useSimpleQuery="false">
| … copy your query here …
| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| </query>
| … provide more export settings here …
| </export>
| </project>
