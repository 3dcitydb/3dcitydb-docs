.. _wfs_getcapabilities_operation_chapter:

GetCapabilities
~~~~~~~~~~~~~~~

The GetCapabilities operation generates an XML-encoded service metadata
document describing the WFS service provided by a server. The
*capabilities* document contains relevant technical and non-technical
information about the service and its provider. Its content mainly
depends on the configuration of the WFS in the ``config.xml`` settings file.

The following XML snippet shows an XML encoding of a GetCapabilities
operation.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:GetCapabilities service="WFS"
    xmlns:wfs="http://www.opengis.net/wfs/2.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.opengis.net/wfs/2.0
    http://schemas.opengis.net/wfs/2.0/wfs.xsd"/>

The declaration of the WFS XML namespace http://www.opengis.net/wfs/2.0
is mandatory to be able to validate the request against the official WFS
XML Schema definition. The reference to the schema location using the
``xsi:schemaLocation`` attribute is however optional. It is *recommended*
though if the XML encoding of the request is created manually by the
user (and not automatically by a client software) to ensure schema
validity. By default, the WFS service will reject invalid requests (see
:numref:`wfs_operations_settings_chapter`).

The following table shows the XML attributes that can be used in the
GetCapabilities request and are supported by the WFS implementation.

.. list-table::  Supported XML attributes of a GetCapabilities operation. (O = optional, M = mandatory)
   :name: wfs_supported_getCapabilities_attributes_table
   :widths: 20 15 20 50

   * - | **XML attribute**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | service
     - | M
     - | WFS (fixed)
     - | The service attribute indicates the service type. The value “WFS” is fixed.
   * - | AcceptVersions
     - | O
     - |
     - | Used for version number negotiation with the WFS server (cf. OGC Document No. 06-121r3:2009).

As alternative to XML encoding, the GetCapabilities operation may also
be invoked through a KVP-encoded HTTP GET request.

.. code-block:: bash

   http[s]://[host][:port]/[context_path]/wfs?
   SERVICE=WFS&
   REQUEST=GetCapabilities&
   ACCEPTVERSIONS=2.0.0,2.0.2

The available KVP parameters are listed below.

.. list-table::  Supported KVP parameters of a GetCapabilities operation. (O = optional, M = mandatory)
   :name: wfs_supported_getCapabilities_parameters_table
   :widths: 20 15 20 50

   * - | **KVP parameter**
     - | **O / M**
     - | **Default value**
     - | **Description**
   * - | SERVICE
     - | M
     - | WFS (fixed)
     - | see above
   * - | ACCEPTVERSIONS
     - | O
     - |
     - | see above