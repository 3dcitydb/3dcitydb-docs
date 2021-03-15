.. _impexp_xml_query_limit:

<limit> parameter
^^^^^^^^^^^^^^^^^

The ``<limit>`` parameter limits the number of explicitly requested
top-level city objects in the export dataset. It offers the elements ``<count>``
and ``<startIndex>`` that can be used together or individually.

The ``<count>`` parameter indicates the total number of city objects that shall
be exported from the set of city objects satisfying the query. And ``<startIndex>``
lets you define the index within this result set from which the export shall begin.

.. note::
  The ``<startIndex>`` uses zero-based numbering. Thus, the first city object is
  assigned the index 0, rather than the index 1. The default value of
  ``<startIndex>`` is 0.

The query below shows how to export at maximum 10 buildings from the
database, even if more buildings satisfy the query.

.. code-block:: xml

    <query>
      <typeNames>
        <typeName>bldg:Building</typeName>
      </typeNames>
      <limit>
        <count>10</count>
      </limit>
    </query>

The following query exports the next 10 buildings by starting with the 11\ :sup:`th`
building in the result set. If the result set contains less
buildings, the export dataset will, of course, also contain less buildings.

.. code-block:: xml

    <query>
      <typeNames>
        <typeName>bldg:Building</typeName>
      </typeNames>
      <limit>
        <count>10</count>
        <startIndex>10</startIndex>
      </limit>
    </query>