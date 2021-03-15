.. _impexp_xml_queries_chapter:

XML query expressions
~~~~~~~~~~~~~~~~~~~~~

.. toctree::
   :hidden:

   typenames
   property-names
   filter
   sort-by
   limit
   lods
   appearance
   tiling
   target-srid
   address
   metadata

A query expression is an action that directs the export operation to
search the 3DCityDB for city objects that satisfy some filter expression
encoded within the query. Query expressions are given in XML using a
``<query>`` root element. The XML language used is specific to the
Importer/Exporter and the 3DCityDB but draws many concepts from OGC
standards such as *Filter Encoding* (FE) 2.0 and *Web Feature Service*
(WFS) 2.0.

.. note::
   All XML elements of the query language are defined in the XML
   namespace http://www.3dcitydb.org/importer-exporter/config. Simply
   define this namespace as default namespace on your ``<query>`` root element.

A query expression may contain a *typeNames* parameter, a *projection
clause*, a *selection clause*, a *sorting clause*, a *counter filter*, an *LoD filter*,
an *appearance filter*, *tiling* options and a *targetSrid* attribute for
coordinate transformations.

.. list-table::  Elements of an XML query expression.
   :name: impexp_query_expression_table
   :widths: 30 70

   * - | **Element**
     - | **Description**
   * - | :ref:`<typeNames> <impexp_xml_query_typenames>`
     - | Lists the name of one or more feature types to query (*optional*).
   * - | :ref:`<propertyNames> <impexp_xml_query_property_names>`
     - | Projection clause that identifies a subset of optional feature properties that shall be kept or removed in the target dataset (*optional*).
   * - | :ref:`<filter> <impexp_xml_query_filter>`
     - | Selection clause that specifies criteria that conditionally select city objects from the 3DCityDB (*optional*).
   * - | :ref:`<sortBy> <impexp_xml_query_sort_by>`
     - | Sorting clause to specify how city objects shall be ordered in the target dataset (*optional*).
   * - | :ref:`<limit> <impexp_xml_query_limit>`
     - | Limits the number of requested city objects that are exported to the target dataset (*optional*).
   * - | :ref:`<lod> <impexp_xml_query_lods>`
     - | Limits the LoDs of the exported city objects to a given subset (*optional*).
   * - | :ref:`<appearance> <impexp_xml_query_appearance>`
     - | Limits the appearances of the exported city objects to a given subset (*optional*).
   * - | :ref:`<tiling> <impexp_xml_query_tiling>`
     - | Defines a tiling scheme for the export (*optional*).
   * - | :ref:`targetSrid <impexp_xml_query_target_srid>`
     - | Defines a coordinate transformation *(optional)*.

In addition, the following chapters discuss how to use address information and 3DCityDB
metadata in query expressions:

- :numref:`%s <impexp_xml_query_address>` :ref:`impexp_xml_query_address`
- :numref:`%s <impexp_xml_query_metadata>` :ref:`impexp_xml_query_metadata`

.. caution::
  **XML queries are based on the CityGML XML schemas**. For instance,
  feature and property names as well as property values and multiplicities
  follow the CityGML XML schema definitions. Likewise, value references in filter
  conditions must be given as XPath expressions based on the XML schemas.
  The alternative **CityJSON encoding is not supported**.