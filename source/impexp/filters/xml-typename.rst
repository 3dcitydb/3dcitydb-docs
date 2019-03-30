<typeNames> parameter
^^^^^^^^^^^^^^^^^^^^^

The <typeNames> parameter lists the name of one or more feature types to
query from the 3DCityDB. Each name is given as *xsd:QName* and must use
an official XML namespace from CityGML 2.0 or 1.0. Only top-level
feature types are supported. The CityGML version of the associated XML
namespace determines the CityGML version used for the export dataset.
Namespaces from different CityGML versions shall not be mixed.

The following example shows how to query CityGML 2.0 bridges and
buildings:

| <query xmlns="http://www.3dcitydb.org/importer-exporter/config">
| <typeNames>
| <typeName
  xmlns:brid="http://www.opengis.net/citygml/bridge/2.0">brid:Bridge</typeName>
| <typeName
  xmlns:bldg="http://www.opengis.net/citygml/building/2.0">bldg:Building</typeName>
| </typeNames>
| </query>

If you want to query all feature types, then simply use the name
*core:_CityObject* of the abstract base type in CityGML, or just skip
the <typeNames> paramenter.

The following table shows all supported top-level feature types together
with their official CityGML XML namespace(s) and their recommended XML
prefix.

======================== ============== ====================================================
**Feature type**         **XML prefix** **XML namespace**
\_CityObject             **core**       **http://www.opengis.net/citygml/2.0
                                        http://www.opengis.net/citygml/1.0**
Building                 **bldg**       **http://www.opengis.net/citygml/building/2.0
                                        http://www.opengis.net/citygml/building/1.0**
Bridge                   **brid**       **http://www.opengis.net/citygml/bridge/2.0**
Tunnel                   **tun**        **http://www.opengis.net/citygml/tunnel/2.0**
TransportationComplex    **tran**       **http://www.opengis.net/citygml/transportation/2.0
                                        http://www.opengis.net/citygml/transportation/1.0**
Road                     **tran**       **http://www.opengis.net/citygml/transportation/2.0
                                        http://www.opengis.net/citygml/transportation/1.0**
Track                    **tran**       **http://www.opengis.net/citygml/transportation/2.0
                                        http://www.opengis.net/citygml/transportation/1.0**
Square                   **tran**       **http://www.opengis.net/citygml/transportation/2.0
                                        http://www.opengis.net/citygml/transportation/1.0**
Railway                  **tran**       **http://www.opengis.net/citygml/transportation/2.0
                                        http://www.opengis.net/citygml/transportation/1.0**
CityFurniture            **frn**        **http://www.opengis.net/citygml/cityfurniture/2.0
                                        http://www.opengis.net/citygml/cityfurniture/1.0**
LandUse                  **luse**       **http://www.opengis.net/citygml/landuse/2.0
                                        http://www.opengis.net/citygml/landuse/1.0**
WaterBody                **wtr**        **http://www.opengis.net/citygml/waterbody/2.0
                                        http://www.opengis.net/citygml/waterbody/1.0**
PlantCover               **veg**        **http://www.opengis.net/citygml/vegetation/2.0
                                        http://www.opengis.net/citygml/vegetation/1.0**
SolitaryVegetationObject **veg**        **http://www.opengis.net/citygml/vegetation/2.0
                                        http://www.opengis.net/citygml/vegetation/1.0**
ReliefFeature            **dem**        **http://www.opengis.net/citygml/relief/2.0
                                        http://www.opengis.net/citygml/relief/1.0**
GenericCityObject        **gen**        **http://www.opengis.net/citygml/generics/2.0
                                        http://www.opengis.net/citygml/generics/1.0**
CityObjectGroup          **grp**        **http://www.opengis.net/citygml/cityobjectgroup/2.0
                                        http://www.opengis.net/citygml/cityobjectgroup/1.0**
======================== ============== ====================================================

Table 33: Supported CityGML top-level feature types together with their
XML namespace.

In order to simplify typing the <typeNames> parameter, you can skip the
namespace declaration from the type names. The Importer/Exporter will
then assume the corresponding CityGML 2.0 namespace, but only if you use
the recommended XML prefix from the table above. The listing below
exemplifies how to use this simplification to query all city furniture
objects from the 3DCityDB.

| <query>
| <typeNames>
| <typeName>frn:CityFurniture</typeName>
| </typeNames>
| </query>
