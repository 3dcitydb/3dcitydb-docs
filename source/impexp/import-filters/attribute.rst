.. _impexp_import_attribute_filter:

Attribute filter
----------------

The attribute filter takes an object *identifier* and/or a *gml:name* as
parameter and only imports top-level features having a matching value for
the respective attribute.

.. figure:: /media/impexp_import_attribute_filter.png
   :name: impexp_import_attribute_filter_fig
   :align: center

   Attribute filter for import operations.

More than one identifier can be provided in a
comma-separated list. Multiple gml:name values are not supported though.

The gml:name search string supports two wildcard characters: "*" representing zero
or more characters and "." representing a single character. You can use the
escape character "\\" to escape the wildcards. For example, if you provide ``*abc``
for the gml:name filter, then features
with a gml:name of "xyzabc" and "abc" will both be imported. If you enter ``\*abc``
instead, the gml:name must exactly match "*abc" for the feature to be imported.