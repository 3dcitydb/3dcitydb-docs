.. _wfs_dropstoredquery_operation_chapter:

DropStoredQuery
~~~~~~~~~~~~~~~

The DropStoredQuery operation allows previously created stored queries to be dropped from the WFS server.
The request simply accepts the *identifier* of the stored query to drop. The stored query identifier shall
be encoded in XML using the id attribute on the ``<wfs:DropStoredQuery>`` element.

To drop the stored query ``urn:StoredQueries:BridgesInPolygon`` created in
:numref:`wfs_createstoredquery_operation_chapter`, simply use the DropStoredQuery operation as shown in the
following example.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:DropStoredQuery xmlns:wfs="http://www.opengis.net/wfs/2.0" service="WFS" version="2.0.0"
      id="urn:StoredQueries:BridgesInPolygon"/>

The following XML attributes are available for the DropStoredQuery operation.

.. list-table:: Supported XML attributes of a DropStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_dropStoredQuery_attributes_table
   :widths: 20 15 20 50

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the service type. The value “WFS” is fixed.
   * - | version
     - | M
     - | 2.0.x
     - | The version of the WFS Interface Standard to be used in the communication.
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to associate a mnemonic name with the request that will be used in exception reports.
   * - | id
     - | M
     - |
     - | The identifier of the stored query to be dropped.

The corresponding KVP-encoded request is shown below.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.0&
   REQUEST=DropStoredQuery&
   STOREDQUERY_ID=urn:StoredQueries:BridgesInPolygon

The following KVP parameters can be used when invoking the DropStoredQuery operation.

.. list-table:: Supported KVP parameters of a DropStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_dropStoredQuery_kvp_table
   :widths: 20 15 20 50

   * - | **KVP parameter**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | SERVICE
     - | M
     - | WFS (fixed)
     - | see above
   * - | VERSION
     - | M
     - | 2.0.x
     - | see above
   * - | STOREDQUERY_ID
     - | M
     - |
     - | see above