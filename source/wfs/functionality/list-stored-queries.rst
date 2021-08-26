.. _wfs_ListStoredQueries_operation_chapter:

ListStoredQueries
~~~~~~~~~~~~~~~~~

Since version 2.0 of the WFS standard, a WFS server is supposed to
manage predefined and parameterized feature query expressions (so called
*stored queries*) that are stored by the server and that can be
repeatedly invoked by the client using different parameter values.
Stored queries hide the complexity of the underlying query expression
from the client since all the client needs to know is the unique
identifier of the stored query as well as the names and types of the
parameters in order to invoke the operation. For example, the stored query
is referenced by its identifier in a GetFeature operation (see :numref:`wfs_getfeature_operation_chapter`)

The ListStoredQuery operation is meant to provide the list of stored
queries that is offered by the WFS server. The response document
contains the unique identifier for each stored query which can then be
used in a subsequent DescribeStoredQuery operation to receive the
details of a specific stored query form the WFS server. The following
listing presents an example ListStoredQuery operation.

.. code-block:: xml
   :name: wfs_listStoredQuery_example_listing

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:ListStoredQueries service="WFS" version="2.0.0"
    xmlns:wfs="http://www.opengis.net/wfs/2.0"/>

The ListStoredQuery operation may take the following XML attributes as
parameters.

.. list-table:: Supported XML attributes of a ListStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_listStoredQuery_attributes_table
   :widths: 20 15 20 50

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the
       | service type. The value “WFS” is fixed.
   * - | version
     - | M
     - | 2.0.x
     - | The service attribute indicates the
       | The version of the WFS Interface
       | Standard to be used in the
       | communication.
   * - | handle
     - | O
     - |
     - | The handle parameter allows a client to
       | associate a mnemonic name with the
       | request that will be used in exception
       | reports.

The corresponding KVP-encoded request is shown below.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   VERSION=2.0.0&
   REQUEST=ListStoredQueries

The following KVP parameters can be used when invoking the
ListStoredQueries operation.

.. list-table:: Supported KVP parameters of a ListStoredQuery operation. (O = optional, M = mandatory)
   :name: wfs_supported_listStoredQuery_kvp_table
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
