.. _impexp_xml_query_sort_by:

<sortBy> sorting clause
^^^^^^^^^^^^^^^^^^^^^^^

The ``<sortBy>`` parameter is used to specify a list of property names whose values
should be used to order the set of city objects that satisfy the query. If no
sorting clause is provided, the city objects are exported in an arbitrary order.

The value of the ``<sortBy>`` parameter is a list of one or more ``<sortProperty>``
elements, each of which must define a ``<valueReference>`` pointing to the property
that shall be used for sorting. Only simple thematic attributes of the requested
top-level feature type or one of its nested feature types are supported. If you specify
multiple ``<sortProperty>`` elements, the result set is sorted by the first property
in the list and that sorted result is sorted by the second property, and so on.

For each ``<sortProperty>``, the sort order can be defined using the ``<sortOrder>``
parameter. The value *asc* indicates an ascending sort (default) and *desc* indicates
a descending sort.

The following example illustrates how to sort all buildings according to their
measured height in descending order.

.. code-block:: xml

    <query>
      <typeNames>
        <typeName>bldg:Building</typeName>
      </typeNames>
      <sortBy>
        <sortProperty>
          <valueReference>bldg:measuredHeight</valueReference>
          <sortOrder>desc</sortOrder>
        </sortProperty>
      </sortBy>
    </query>