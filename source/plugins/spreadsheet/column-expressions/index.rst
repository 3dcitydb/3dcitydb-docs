.. _impexp_plugin_spshg_column_expressions:

Rules for column expressions
----------------------------

The *Spreadsheet Generator* plugin uses simple expressions to reference
the columns of the 3DCityDB database schema that shall be exported to the
output CSV/XLSX file. The syntax of these expressions is based
on the balloon content language used by the visualization export operation
(see :numref:`impexp_dynamic_balloon_content_chapter`).

If you want to create your column expressions manually rather than
using the graphical user interface of the plugin, you must comply with the
following rules.

-  Expressions are coded in the form ``"TABLE/[AGGREGATION FUNCTION]
   COLUMN [CONDITION]"``. The table and column tokens are mandatory and must
   exist in the 3DCityDB schema (see :numref:`citydb_conceptual_database_structure_chapter`).
   Both the aggregation function and the condition are optional.
   When present they must be written in square brackets.
   The expressions are mapped to SQL statements
   of the form: ``SELECT (AGGREGATION FUNCTION) COLUMN FROM TABLE
   (WHERE CONDITION)``.

-  Expressions are case-insensitive.

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

-  Invalid results will be silently discarded.

-  Multi-line values are supported for the columns in the output file.
   Use ``"[EOL]"`` to add a break line to the expression.

-  Expressions can be surrounded by static strings that are exported as-is.

**Examples**

.. code-block::

   ADDRESS/STREET

The above expression returns the value of the STREET column of the ADDRESS table for
each city object, for example:

    *Unter den Linden*

If the city object is assigned multiple addresses, the ADDRESS table
will contain more than one row for this city objects.
In such cases, all values of the STREET column are returned as
comma-separated list.

    *Unter den Linden, Friedrichstraße*

If you want to avoid comma-separated lists as return value, you
can additionally use an aggregation function for the column values
as shown below.

.. code-block::

   ADDRESS/[FIRST]STREET

This expression will only return the first STREET value for the
city object independent of how many addresses the city object has.

.. code-block::

   ADDRESS/[FIRST]STREET ADDRESS/[FIRST]HOUSE_NUMBER, [EOL] ADDRESS/[FIRST]ZIP_CODE ADDRESS/[FIRST]CITY

The above snippet combines multiple expressions that are mapped to
a single value in the output CSV/XSLX file. The ``[EOL]`` token
adds a line break to the value. Note the use of the comma ``,`` that
is added as static content to every value. The return value for a
given city object might look as follow:

   *Unter den Linden 135,* |br|
   *10623 Berlin*

.. code-block::

   CITYOBJECT_GENERICATTRIB/ATTRNAME

Return the names of all generic attributes assigned to a city object.
The names will be returned as comma-separated list.

.. code-block::

   CITYOBJECT_GENERICATTRIB/REALVAL[ATTRNAME = 'SOLAR_SUM_INVEST'] EUR

Return the value of the REALVAL column of the generic
attribute whose ATTRNAME is equal to ``SOLAR_SUM_INVEST``.
The string ``EUR`` is added to the number as static content.
This expression might produce the following result.

   *23,000.00 EUR*

.. |br| raw:: html

   <br />