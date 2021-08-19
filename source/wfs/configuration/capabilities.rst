.. _wfs_capabilities_settings_chapter:

Capabilities settings
~~~~~~~~~~~~~~~~~~~~~

The *capabilities* settings define the contents of the *capabilities*
document that is returned by the WFS service upon a GetCapabilities
request.

The *capabilities* document is generated dynamically from the
contents of the ``config.xml`` file at request time. Only mandatory and optional
*service metadata* has to be explicitly specified with the ``<capabilities>``
element by the user in addition. All other sections of the
*capabilities* document are populated automatically from the ``config.xml``
file. For example, the set of feature types advertised in the
``<wfs:FeatureTypeList>`` section is derived from the content of the
``<featureTypes>`` element (cf. :numref:`wfs_feature_type_settings_chapter`).

The service metadata is provided using the <owsMetadata> child element (see the example listing below).
It is copied to the capabilities document “as is” and thus should be consistent and valid.

.. code-block:: xml
   :name: wfs_metadata_settings_listing

   <capabilities>
     <owsMetadata>
       <ows:ServiceIdentification>
         <ows:Title>3D City Database Web Feature Service</ows:Title>
         <ows:ServiceType>WFS</ows:ServiceType>
         <ows:ServiceTypeVersion>2.0.0</ows:ServiceTypeVersion>
       </ows:ServiceIdentification>
       <ows:ServiceProvider>
         <ows:ProviderName/>
         <ows:ServiceContact/>
       </ows:ServiceProvider>
     </owsMetadata>
   </capabilities>

Service metadata comprises information about the *service
itself* that might be useful in machine-to-machine communication or for
display to a human. This information is announced through the
``<ows:ServiceIdentifikation>`` child element. Mandatory components are
the service title (``<ows:Title>``), the service type (``<ows:ServiceType>``,
which may only take the fixed value WFS), and the supported WFS protocol
versions (``<ows:ServiceTypeVersion>``). The 3DCityDB WFS currently supports
the protocol versions 2.0.2 and 2.0.0.

.. note::
  If, for example, the service should
  only offer the protocol version 2.0.0 to clients, then only provide one
  ``<ows:ServiceTypeVersion>`` element for this version. This is recommended
  if the software accessing the WFS does only support version 2.0.0
  (e.g., FME 2018/2019). Invalid values of the ``<ows:ServiceIdentifikation>``
  element will be overridden with reasonable default values at startup of the
  WFS service.

The child element ``<ows:ServiceProvider>`` contains information about the
service provider such as contact information. Please refer to the OGC Web
Services Common Specification (OGC 06-121r3:2009) to get an overview of
the supported metadata fields that may be included in the capabilities
document and therefore can be specified in ``<owsMetadata>``.