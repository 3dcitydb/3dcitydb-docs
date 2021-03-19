.. _impexp_export_vis_preferences_balloon_chapter:

Balloon
^^^^^^^

KML offers the possibility of enriching its placemark elements with
information bubbles, so-called balloons, which pop up when the placemark
is clicked on. This is supported by the Importer/Exporter regardless of
the display form in which the objects is exported.

.. note::
   When exporting in the COLLADA display form it is recommended to
   enable the "*highlighting on mouseOver*" option, since model placemarks
   not coming from Google Earth servers are not directly clickable, but
   only through the sidebar. Highlighting geometries are, on the contrary,
   directly clickable wherever they are loaded from.

.. note::
   If you want to use the 3DCityDB-Web-Map-Client (see :numref:`webmap_chapter`
   for more details) to visualize the exported datasets (KML/glTF models),
   the options (the both checkboxes shown in :numref:`pic_kml_collada_gltf_preferences_balloon_building`) for creating
   information balloons shall be deactivated, since the
   3DCityDB-Web-Map-Client does not provide support for showing information
   balloons. In stead, it utilizes the online spreadsheet (Google Fusion
   Table) to query and display attribute information of the respective
   objects.

Balloon preferences can be set independently for each CityGML top-level
feature type. That means every object can have its own individual
template file (so that for instance, *WaterBody* balloons display a
different background image as *Vegetation* balloons), and it is
perfectly possible to have information bubbles for some object types
while some others have none. For GenericCityObject, the point and line
geometry object can also has its own individual balloon settings. The
following example is set around *Building* balloons but it applies
exactly the same for all feature classes.

.. figure:: /media/impexp_kml_export_preferences_balloon_building_fig.png
   :name: pic_kml_collada_gltf_preferences_balloon_building
   :align: center

   *Building* Balloon settings

The contents of the balloon can be taken from a generic attribute called
*Balloon_Content* associated individually to each city object in the
3DCityDB. They can also be uniform for all objects in an export by using
an external HTML file as a template, or a combination of both:
individually and uniformly set, the *Balloon_Content* attribute
(individually) having priority over the external HTML template file
(uniform). A few Balloon HTML template files can be found after software
installation in the subfolder ``templates/balloons`` of the installation
directory.

The balloons can be included in the doc.kml file generated at export, or
they can be put into individual files (one for each object) written
together into a "balloon" directory. This makes later adaption work
easier if some post-processing (manual or not) is required. When balloon
contents are put into a separate file for each exported object, access
to local files and personal data must be granted in Google Earth (Tools ->
Options -> General) for the balloons to show.

The balloon contents do not need to be static. They can contain
references to the data belonging to the city object they relate to.
These references will be dynamically resolved (i.e.: the actual value
for the current object will be put in their place) at export time in a
way similar to how `Active Server Pages (ASP) <http://msdn.microsoft.com/en-us/library/ms526064.aspx>`_ work.
Placeholders embedded in the HTML template, beginning with ``<3DCityDB>``
and ending with ``</3DCityDB>`` tags, will be replaced in the resulting
balloon with the dynamically determined value(s). The HTML balloon
templates can also include JavaScript code.

For all concerns, including dynamic content generation, it makes no
difference whether the template is taken from the *Balloon_Content*
generic attribute or from an external file.

**Balloon template format.** As previously stated, a balloon template
consists of ordinary HTML, which may or may not contain JavaScript code
and ``<3DCityDB>`` placeholders for object-specific content. These
placeholders follow several elementary rules.

Rules for simple expressions
""""""""""""""""""""""""""""

-  Expressions begin with ``<3DCityDB>`` and end with ``</3DCityDB>``.
   Expressions are not case-sensitive.

-  Expressions are coded in the form ``"TABLE/[AGGREGATION FUNCTION]
   COLUMN [CONDITION]"``. Aggregation function and condition are optional.
   When present they must be written in square brackets (they belong to
   the syntax). These expressions represent an alternative coding of a
   SQL select statement: ``SELECT [AGGREGATION FUNCTION] COLUMN FROM TABLE
   [WHERE condition]``. Tables refer to the underlying 3DCityDB table
   structure (see :numref:`citydb_conceptual_database_structure_chapter` for details).

-  Each expression will only return those entries relevant to the city
   object being currently exported. That means an implicit condition
   clause somewhat like ``"TABLE.CITYOBJECT_ID = CITYOBJECT.ID"`` is always
   considered and does not need to be explicitly written.

-  Results will be interpreted and printed in HTML as lists separated by
   commas. Lists with only one element are the most likely, but not
   exclusively possible, outcome. When only interested in the first
   result of a list the aggregation function FIRST should be used. Other
   possible aggregation functions are ``LAST``, ``MAX``, ``MIN``, ``AVG``, ``SUM`` and
   ``COUNT``.

-  Conditions can be defined by a simple number (meaning which element
   from the result list must be taken) or a column name (that must exist
   in underlying 3DCityDB table structure) a comparison operator and a
   value. For instance: ``[2]`` or ``[NAME = 'abc']``.

-  Invalid results will be silently discarded. Valid results will be
   delivered exactly as stored in the 3DCityDB tables. Later changes on
   the returned results - like *substring()* functions - can be achieved
   by using JavaScript.

-  All elements in the result list are always of the same type (the type
   of the corresponding table column in the underlying 3DCityDB). If
   different result types must be placed next to each other, then
   different ``<3DCityDB>`` expressions must be placed next to each other.

Special keywords in simple expressions
""""""""""""""""""""""""""""""""""""""

-  The balloon template files have several additional placeholders for
   object-specific content, called ``SPECIAL_KEYWORDS``. They refer to data
   that is not retrieved “as is” in a single step from a table in the
   3DCityDB but has to undergo some processing steps (not achievable by
   simple JavaScript means) in order to calculate the final value before
   being exported to the balloon. A typical processing step is the
   transformation of some coordinate list into a CRS different from the
   one the 3DCityDB is originally set in. The coordinates in the new CRS
   cannot be included in the balloon with their original values as read
   from the database (which was the case with all other expression
   values so far), but must be transformed prior to their addition to
   the balloon contents.

-  Expressions for special keywords are not case-sensitive. Their syntax
   is similar to ordinary simple expressions, start and end are marked
   by ``<3DCityDB>`` and ``</3DCityDB>`` tags, the table name must be
   ``SPECIAL_KEYWORDS`` (a non-existing table in the 3DCityDB), and the
   column name must be one of the following:

.. list-table::  3DCityDB SPECIAL_KEYWORDS
   :name: 3dcitydb_special_keywords_table

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

-  No aggregation functions or conditions are allowed for
   ``SPECIAL_KEYWORDS``. If present they will be interpreted as part of the
   keyword and therefore not recognized.

-  The ``SPECIAL_KEYWORDS`` list is also visible and available in its
   current state in the updated version of the *Spreadsheet Generator
   Plugin* (see the following section). The list can be extended in
   further Importer/Exporter releases.

Examples for simple expressions
"""""""""""""""""""""""""""""""

* ``<3DCityDB>ADDRESS/STREET</3DCityDB>`` returns the content of the STREET column on the ADDRESS table for this city object.

* ``<3DCityDB>BUILDING/NAME</3DCityDB>`` returns the content of the NAME column on the BUILDING table for this city object.

* ``<3DCityDB>CITYOBJECT_GENERICATTRIB/ATTRNAME</3DCityDB>`` returns the names of all existing generic attributes for this city object.
  The names will be separated by commas.

* ``<3DCityDB>CITYOBJECT_GENERICATTRIB/REALVAL [ATTRNAME = 'H_Trauf_Min']</3DCityDB>`` returns the value (of the REALVAL column)
  of the generic attribute with attribute name ``H_Trauf_Min`` for this city object.

* ``<3DCityDB>APPEARANCE/[COUNT]THEME</3DCityDB>`` returns the number of appearance themes for this city object.

* ``<3DCityDB>APPEARANCE/THEME[0]</3DCityDB>`` returns the first appearance for this city object.

* ``<3DCityDB>SPECIAL_KEYWORDS/CENTROID_WGS84_LON</3DCityDB>`` returns the *longitude value of this city object’s centroid longitude in WGS84*.

* ``<3DCityDB>`` simple expressions can be used not only for generating text
  in the balloons, but any valid HTML content, like clickable hyperlinks:

* ``<a href="<3DCityDB>EXTERNAL_REFERENCE/URI</3DCityDB>"> click here for more information</a>`` returns a hyperlink to the object's external reference

or embedded images:

.. code-block:: xml

   <img src= "<3DCityDB>CITYOBJECT_GENERICATTRIB/URIVAL[ATTRNAME='Illustration']</3DCityDB>" width=400>

This last example produces, for instance, in the case of the Pergamon
Museum in Berlin:

.. code-block:: xml

   <img  src="http://upload.wikimedia.org/wikipedia/commons/d/d1/FrisoaltarPergamo.jpg" width=400>

.. figure:: /media/impexp_kml_export_balloon_embedded_image_fig.png
   :name: pic_kml_collada_gltf_preferences_balloon_generated
   :align: center

   Dynamically generated balloon containing an embedded image (image taken from Wikimedia)

Simple expressions are sufficient for most use cases, when only a single
value or a list of values from a single column is needed. However,
sometimes the user will need to access more than one column at the same
time with an unknown amount of results. For these situations (listing of
all generic attributes along with their values is one of them) iterative
expressions were conceived.

Rules for iterative expressions
"""""""""""""""""""""""""""""""

-  Iterative expressions will adopt the form:

   .. code-block:: xml

      <3DCityDB>FOREACH
            TABLE/COLUMN[,COLUMN][,COLUMN][...][,COLUMN][CONDITION]
      </3DCityDB>
      [...]


   HTML and JavaScript code (column content will be referred to as %1, %2, etc. and follow the columns order in the FOREACH line. %0 is reserved for displaying the current row number)

   .. code-block:: xml

      [...]
      <3DCityDB>END FOREACH</3DCityDB>

-  No aggregation functions are allowed for iterative expressions. The
   amount of columns is free, but they must belong to the same table.
   Condition is optional. Implicit condition (data must be related to
   the current city object) applies as for simple expressions.

-  ``FOREACH`` means truly "for each". No skipping is possible. If skipping
   at display time is needed it must be achieved by JavaScript means.

-  The generated HTML will have as many repetitions of the HTML code
   between the ``FOREACH`` and ``END FOREACH`` tags as lines the query result
   has.

-  No inclusion of simple expressions or ``SPECIAL_KEYWORDS`` between
   ``FOREACH`` and ``END FOREACH`` tags is allowed.

-  No nesting of ``FOREACH`` statements is allowed.

Examples for iterative expressions
""""""""""""""""""""""""""""""""""

Listing of generic attributes and their values:

.. code-block::

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

   Model placemark with dynamic balloon contents showing the list of generic attributes