.. _impexp_dynamic_balloon_content_chapter:

Dynamic balloon content
~~~~~~~~~~~~~~~~~~~~~~~

The visualization export operation supports adding KML balloons to the
exported city objects (see :numref:`impexp_export_vis_preferences_balloon_chapter`).
The content of these balloons can contain ``<3DCityDB>`` elements to reference data stored
in the 3DCityDB for each individual city object. These references are dynamically resolved
at export time and replaced with the actual value from the database.
The ``<3DCityDB>`` element offer a rich set of expressions and keywords
that can be used to precisely describe the data you want to fetch from the database.

Rules for simple expressions
""""""""""""""""""""""""""""

-  Expressions must begin with ``<3DCityDB>`` and end with ``</3DCityDB>``. Expressions are case-insensitive.

-  Expressions are coded in the form ``"TABLE/[AGGREGATION FUNCTION]
   COLUMN [CONDITION]"``. The table and column tokens are mandatory and must
   exist in the 3DCityDB schema (see :numref:`citydb_conceptual_database_structure_chapter`).
   Both the aggregation function and the condition are optional.
   When present they must be written in square brackets.
   The expressions are mapped to SQL statements
   of the form: ``SELECT (AGGREGATION FUNCTION) COLUMN FROM TABLE
   (WHERE CONDITION)``.

-  Each expression will only return those entries relevant to the city
   object being currently exported. For this purpose, a filter condition
   of the form ``"TABLE.CITYOBJECT_ID = CITYOBJECT.ID"`` is always added automatically
   to each expression and, thus, is not required to be added by the user manually.

-  Results will be returned as comma-separated list. A list can be empty or contain one ore more
   items satisfying the expression. When only interested in the first
   entry in a list, the aggregation function ``FIRST`` can be used. Other
   possible aggregation functions are ``LAST``, ``MAX``, ``MIN``, ``AVG``, ``SUM`` and
   ``COUNT``.

-  A condition can simply be an index to access a specific entry from the
   result list. Alternatively, it can be a comparison expression involving
   a column name, a comparison operator and a value. For instance: ``[2]`` or ``[NAME = 'abc']``.

-  Invalid results will be silently discarded. Valid results will be
   delivered exactly as stored in the 3DCityDB tables. You can use
   JavaScript functions (e.g., *substring()*) to post-process and adapt
   the results to your needs.

-  All items in the result list are always of the same data type which
   corresponds to the data type of the corresponding column in the database. If
   different result types must be placed next to each other in a balloon, then
   different ``<3DCityDB>`` expressions must be used.

Special keywords
""""""""""""""""

In addition to simple expressions, you can also use ``SPECIAL_KEYWORDS`` that are
predefined placeholders for object-specific content. These keywords refer to data
that is not retrieved “as is” in a single step from a table in the
3DCityDB but has to undergo some preprocessing steps (not achievable by
simple JavaScript means) in order to calculate the final value before
being exported to the balloon. A typical preprocessing step is the
transformation of coordinates into a CRS different from the
one defined for the 3DCityDB instance.

The following table lists the available special keywords.

.. list-table::  Available special keywords
   :name: 3dcitydb_special_keywords_table
   :widths: 30 70

   * - | **Keyword**
     - | **Description**
   * - | ``CENTROID_WGS84``
     - | coordinates of the object’s centroid in WGS84 in the following order:
       | longitude, latitude, altitude
   * - | ``CENTROID_WGS84_LAT``
     - | latitude of the object’s centroid in WGS84
   * - | ``CENTROID_WGS84_LON``
     - | longitude of the object’s centroid in WGS84
   * - | ``BBOX_WGS84_LAT_MIN``
     - | minimum latitude value of the object’s envelope in WGS84
   * - | ``BBOX_WGS84_LAT_MAX``
     - | maximum latitude value of the object’s envelope in WGS84
   * - | ``BBOX_WGS84_LON_MIN``
     - | minimum longitude value of the object’s envelope in WGS84
   * - | ``BBOX_WGS84_LON_MAX``
     - | maximum longitude value of the object’s envelope in WGS84
   * - | ``BBOX_WGS84_HEIGHT_MIN``
     - | maximum longitude value of the object’s envelope in WGS84
   * - | ``BBOX_WGS84_HEIGHT_MAX``
     - | maximum height value of the object’s envelope in WGS84
   * - | ``BBOX_WGS84_LAT_LON``
     - | all four latitude and longitude values of the object’s envelope in WGS84
   * - | ``BBOX_WGS84_LON_LAT``
     - | all four longitude and latitude values of the object’s envelope in WGS84

In order to use these keywords in your balloon template, you must apply the following rules:

-  Expressions for special keywords are case-insensitive. Their syntax
   is similar to simple expressions: start and end are marked
   by ``<3DCityDB>`` and ``</3DCityDB>`` tags, the table name must be
   ``SPECIAL_KEYWORDS`` and the column name must be one of the predefined keywords
   listed in the :numref:`3dcitydb_special_keywords_table` above.

-  No aggregation functions or conditions are allowed for
   ``SPECIAL_KEYWORDS``. If present they will be interpreted as part of the
   keyword and therefore not recognized.

Examples for simple expressions
"""""""""""""""""""""""""""""""

The following examples illustrate the use of simple expressions in your balloon template.

* ``<3DCityDB>ADDRESS/STREET</3DCityDB>`` returns the content of the STREET column of the ADDRESS table for this city object.

* ``<3DCityDB>BUILDING/NAME</3DCityDB>`` returns the content of the NAME column of the BUILDING table for this city object.

* ``<3DCityDB>CITYOBJECT_GENERICATTRIB/ATTRNAME</3DCityDB>`` returns the names of all generic attributes for this city object.
  The names will be provided as comma-separated list.

* ``<3DCityDB>CITYOBJECT_GENERICATTRIB/REALVAL[ATTRNAME = 'H_Trauf_Min']</3DCityDB>`` returns the value stored in the
  column REALVAL for the generic attribute having the name ``H_Trauf_Min`` for this city object.

* ``<3DCityDB>APPEARANCE/[COUNT]THEME</3DCityDB>`` returns the number of appearance themes for this city object.

* ``<3DCityDB>APPEARANCE/THEME[0]</3DCityDB>`` returns the first appearance theme for this city object.

* ``<3DCityDB>SPECIAL_KEYWORDS/CENTROID_WGS84</3DCityDB>`` returns the centroid of the city object in WGS84.

``<3DCityDB>`` expressions cannot only be used to generate plain text in a balloon,
but also to create any valid HTML content such as hyperlinks or embedded images:

* ``<a href="<3DCityDB>EXTERNAL_REFERENCE/URI</3DCityDB>">Click here for more information</a>`` creates a hyperlink
  to the object's external reference.

* ``<img src="<3DCityDB>CITYOBJECT_GENERICATTRIB/URIVAL[ATTRNAME=\'Illustration\']</3DCityDB>" width=400>`` adds
  an image to the balloon whose URL is taken from the generic attribute "Illustration".

For the last example, assume that the "Illustration" attribute contains an URL to an image of the
Pergamon Museum in Berlin on Wikipedia. In the balloon of the corresponding city object,
this would be resolved to a valid ``<img>`` tag and displayed in the viewer as shown below.

.. code-block:: xml

   <img src="https://upload.wikimedia.org/wikipedia/commons/d/d1/FrisoaltarPergamo.jpg" width=400>

.. figure:: /media/impexp_kml_export_balloon_embedded_image_fig.png
   :name: pic_kml_collada_gltf_preferences_balloon_generated
   :align: center

   Dynamically generated balloon containing an embedded image (image taken from Wikipedia).

Rules for iterative expressions
"""""""""""""""""""""""""""""""

Simple expressions are sufficient for most use cases, when only a single
value or a list of values from a single column is needed. However,
sometimes the user will need to access more than one column at the same
time with an unknown amount of results. For such scenarios, iterative
expressions are available. An example use case is to iterate over all
all generic attributes available for the city object to print both their
names and values.

The general syntax of iterative expressions is shown in the snippet below.

.. code-block:: xml

   <3DCityDB>FOREACH TABLE/COLUMN[,COLUMN...][CONDITION]</3DCityDB>
     ...access the returned values here using the tokens %1, %2, etc. ...
   <3DCityDB>END FOREACH</3DCityDB>

You can provide one or more columns for the ``FOREACH`` statement. The values of the separate columns
are accessed using the tokens ``%1``, ``%2``, etc., where the number refers to the index
of the column in the list. The first column is assigned the index 1.
The token ``%0`` can be used to retrieve the current row number.

The following additional rules apply to iterative expressions:

-  No aggregation functions are allowed for iterative expressions. The
   number of columns is not limited, but all of them must belong to the same table.
   A condition is optional. Similar to simple expression, an implicit condition
   is always added to make sure that the data belongs to the currently exported city object.

-  The ``FOREACH`` loop iterates over all returned values. If values shall be skipped
   when displaying the balloon, this must be achieved by JavaScript means.

-  The generated balloon content will have as many repetitions of the HTML/JavaScript code
   between the ``FOREACH`` and ``END FOREACH`` tags as values returned from the query.

-  Additional simple expressions or ``SPECIAL_KEYWORDS`` are not allowed
   inside the ``FOREACH`` and ``END FOREACH`` tags.

-  ``FOREACH`` expressions must not be nested.

The following example shows how to use a ``FOREACH`` expression to list all
generic attributes of a city object in the balloon. A JavaScript function is
used to display the value of a generic attribute as tool tip when hovering
with the mouse over the attribute name. This example is also provided
as template file in the ``templates/balloons`` folder within the installation directory
of the Importer/Exporter.

..

      <script type="text/javascript">
            function ga_value_as_tooltip(attrname, datatype, strval, intval, realval) {
                  document.write("<span title=\"");
                  switch (datatype) {
                        case "1": document.write(strval);
                              break;
                        case "2": document.write(intval);
                              break;
                        case "3": document.write(realval);
                              break;
                        default: document.write("unknown");
                  };
                  document.write("\">" + attrname + "</span>");
            }
            <3DCityDB>FOREACH
                  CITYOBJECT_GENERICATTRIB/ATTRNAME,DATATYPE,STRVAL,INTVAL,REALVAL</3DCityDB>
                  ga_value_as_tooltip("%1", "%2", "%3", "%4", "%5");
            <3DCityDB>END FOREACH</3DCityDB>
      </script>

.. figure:: /media/impexp_kml_export_balloon_dynamic_contents_fig.png
   :name: pic_kml_collada_gltf_preferences_balloon_dynamic
   :align: center

   Dynamic balloon content showing the list of generic attributes and their values as tool tip.