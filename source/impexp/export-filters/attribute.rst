.. _impexp_export_attribute_filter:

Attribute filter
----------------

The attribute filter lets you define values for the object *identifier*,
*gml:name* and *citydb:lineage*, which must be matched by a top-level feature
to be exported.

.. figure:: /media/impexp_export_attribute_filter.png
   :name: impexp_export_attribute_filter_fig
   :align: center

   Attribute filter for export operations.

More than one identifier can be provided in a
comma-separated list. Multiple gml:name and citydb:lineage values are not supported though.

Both the gml:name and citydb:lineage search strings support two wildcard characters: "*" representing zero
or more characters and "." representing a single character. You can use the
escape character "\\" to escape the wildcards. For example, if you provide ``*abc``
for the gml:name filter, then features with a gml:name of "xyzabc" and "abc" will both be exported.
If you enter ``\*abc`` instead, the gml:name must exactly match "*abc" for the feature to be exported.