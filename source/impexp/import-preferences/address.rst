.. _impexp_import_preferences_address_chapter:

Address
^^^^^^^

.. note::
  These preference settings only apply to CityGML input files.

CityGML uses the *OASIS Extensible Address Language* (xAL)
standard for the representation and exchange of address information. xAL
provides a flexible and generic framework for encoding address data
according to arbitrary address schemes. The columns of the ADDRESS table
of the 3D City Database, however, only map the most common fields in
address records (cf. :numref:`citydb_schema_chapter`). Moreover, the Importer/Exporter
currently does not support arbitrary xAL fragments but is tailored to
the parsing of the following two xAL templates that are taken from the
CityGML specification.

.. code-block:: xml

       <Address>
         <xalAddress>
           <!-- Bussardweg 7, 76356 Weingarten, Germany -->
           <xAL:AddressDetails>
             <xAL:Country>
               <xAL:CountryName>Germany</xAL:CountryName>
               <xAL:Locality Type="City">
                 <xAL:LocalityName>Weingarten</xAL:LocalityName>
                 <xAL:Thoroughfare Type="Street">
                   <xAL:ThoroughfareNumber>7</xAL:ThoroughfareNumber>
                   <xAL:ThoroughfareName>Bussardweg</xAL:ThoroughfareName>
                 </xAL:Thoroughfare>
                 <xAL:PostalCode>
                   <xAL:PostalCodeNumber>76356</xAL:PostalCodeNumber>
                 </xAL:PostalCode>
               </xAL:Locality>
             </xAL:Country>
           </xAL:AddressDetails>
         </xalAddress>
       </Address>

.. code-block:: xml

       <Address>
         <xalAddress>
           <!-- 46 Brynmaer Road Battersea LONDON, SW11 4EW United Kingdom -->
           <xAL:AddressDetails>
             <xAL:Country>
               <xAL:CountryName>United Kingdom</xAL:CountryName>
               <xAL:Locality Type="City">
                 <xAL:LocalityName>LONDON</xAL:LocalityName>
                 <xAL:DependentLocality Type="District">
                   <xAL:DependentLocalityName>Battersea</xAL:DependentLocalityName>
                   <xAL:Thoroughfare>
                     <xAL:ThoroughfareNumber>46</xAL:ThoroughfareNumber>
                     <xAL:ThoroughfareName>Brynmaer Road</xAL:ThoroughfareName>
                   </xAL:Thoroughfare>
                 </xAL:DependentLocality>
                 <xAL:PostalCode>
                   <xAL:PostalCodeNumber>SW11 4EW</xAL:PostalCodeNumber>
                 </xAL:PostalCode>
               </xAL:Locality>
             </xAL:Country>
           </xAL:AddressDetails>
         </xalAddress>
       </Address>

If xAL address information in a CityGML instance document does not
comply with one of these templates (e.g., because of additional or
completely different entries), the address information will only
partially be stored in the database (if at all). To not lose
any original address information, the entire ``<xal:AddressDetail>`` XML
fragment can be imported “as is” from the input CityGML file and stored
in the XAL_SOURCE column of the ADDRESS table in the 3D City Database.
For this purpose, simply check the *Import original <xal:AddressDetail>
XML fragment* option (this is the default value).

.. figure:: /media/impexp_import_preferences_address_fig.png
   :name: impexp_import_preferences_address_fig
   :align: center

   Import preferences – Address.

See :numref:`impexp_export_preferences_address_chapter` for how to export the
xAL fragment from XAL_SOURCE.

.. note::

  The Importer/Exporter always tries and populates the columns of the
  ADDRESS table (STREET, HOUSE_NUMBER, etc.) from the xAL address information
  independent of whether the ``<xal:AddressDetail>`` element shall be imported.
  Thus, the original XML representation is always imported in addition.