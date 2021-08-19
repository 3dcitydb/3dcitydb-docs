.. _wfs_filter_capabilities_settings_chapter:

Filter capabilities settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The WFS service supports logical, comparison and spatial filter predicates in query operations
to specify how city objects in the 3D City Database should be filtered to produce a result set.
The *filter capabilities* settings specify which filter operations shall be available for clients
and thus can be used in requests to the server.

The ``<filterCapabilities>`` element is used to enumerate the filter operators that are advertised
to clients. The following listing shows an example ``<filterCapabilities>`` element that contains
all filter operations that are supported by the WFS implementation.

.. code-block:: xml
   :name: wfs_filter_capabilities_settings_config_listing

   <filterCapabilities>
     <scalarCapabilities>
       <logicalOperators>true</logicalOperators>
       <comparisonOperators>
         <operator>PropertyIsEqualTo</operator>
         <operator>PropertyIsNotEqualTo</operator>
         <operator>PropertyIsLessThan</operator>
         <operator>PropertyIsGreaterThan</operator>
         <operator>PropertyIsLessThanOrEqualTo</operator>
         <operator>PropertyIsGreaterThanOrEqualTo</operator>
         <operator>PropertyIsLike</operator>
         <operator>PropertyIsNull</operator>
         <operator>PropertyIsNil</operator>
         <operator>PropertyIsBetween</operator>
       </comparisonOperators>
     </scalarCapabilities>
     <spatialCapabilities>
       <operator>BBOX</operator>
       <operator>Equals</operator>
       <operator>Disjoint</operator>
       <operator>Touches</operator>
       <operator>Within</operator>
       <operator>Overlaps</operator>
       <operator>Intersects</operator>
       <operator>Contains</operator>
       <operator>DWithin</operator>
       <operator>Beyond</operator>
     </spatialCapabilities>
   </filterCapabilities>

Simply remove single or multiple items from this list in order to reduce the filter
capabilities of your WFS service. For instance, if you do not want clients to issue
spatial queries against your WFS service, simply remove the ``<spatialCapabilities>`` element.
At minimum, every WFS supports querying objects by their ``gml:id`` identifier through
the *ResourceId* operation to satisfy the *WFS Simple* conformance class. For this reason,
the *ResourceId* operation is mandatory and thus not part of the ``<filterCapabilities>``
enumeration.

Please refer to the OGC Filter Encoding 2.0 Encoding Standard version 2.0 (OGC 09-026r2)
for a comprehensive documentation of the filter semantics.