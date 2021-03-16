.. _impexp_export_preferences_address_chapter:

Address
^^^^^^^

.. note::
  These preference settings only apply to CityGML exports.

Similar to the import of xAL address information
(see :numref:`impexp_import_preferences_address_chapter`), a
user can choose how address information should be exported to a target
CityGML dataset. The available options of the address export preferences
are shown in the figure below.

.. figure:: /media/impexp_export_preferences_address_fig.png
   :name: impexp_export_preferences_address_fig
   :align: center

   Export preferences – Address.

When importing address data, the xAL fragment is parsed and the main address
elements are stored in the columns of the ADDRESS table.
As discussed in :numref:`impexp_import_preferences_address_chapter`,
the original ``<xal:AddressDetails>`` element can additionally be
imported "as is" into the XAL_SOURCE column.
Thus, there are two possible ways to reconstruct the address information.

1. The default option is to build the xAL address from the columns of
   the ADDRESS table *without considering* the XAL_SOURCE column. In
   this case, the XML encoding of the xAL address uses the
   following template. The column names of the ADDRESS table are printed
   in capital letters and are replaced with the values from the database
   at export time.

   .. code-block:: xml

       <Address>
         <xalAddress>
           <xAL:AddressDetails>
             <xAL:Country>
               <xAL:CountryName>COUNTRY</xAL:CountryName>
               <xAL:Locality Type="City">
                 <xAL:LocalityName>CITY</xAL:LocalityName>
                 <xal:PostBox>
                   <xal:PostBoxNumber>PO_BOX</xal:PostBoxNumber>
                 </xal:PostBox>
                 <xAL:Thoroughfare Type="Street">
                   <xAL:ThoroughfareNumber>HOUSE_NUMBER</xAL:ThoroughfareNumber>
                   <xAL:ThoroughfareName>STREET</xAL:ThoroughfareName>
                 </xAL:Thoroughfare>
                 <xAL:PostalCode>
                   <xAL:PostalCodeNumber>ZIP_CODE</xAL:PostalCodeNumber>
                 </xAL:PostalCode>
               </xAL:Locality>
             </xAL:Country>
           </xAL:AddressDetails>
         </xalAddress>
       </Address>

2. Optionally, the xAL fragment is taken “as is” from the XAL_SOURCE
   column and inserted literally into the target CityGML document. This
   way there will be no loss of information and the address encoding
   will be identical to the original source datasets. Obviously, this
   option requires that the XAL_SOURCE column has been populated during
   import (cf. :numref:`impexp_import_preferences_address_chapter`).

Both options are mutually exclusive, but if the chosen option does
not provide results, the other option can be used as fallback.