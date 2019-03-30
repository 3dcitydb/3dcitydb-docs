Relational database schema
--------------------------

Mapping rules, schema conventions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mapping of classes onto tables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Generally, one or more classes of the UML diagram are mapped onto one
table; the name of the table is identical to the class name (a leading
underscore indicating an abstract class is left out). Classes are
combined into a single table according to the class relations as shown
in the UML diagrams by using orange coloured boxes. The scalar
attributes of the classes become columns of the corresponding table with
identical name.

The types of the attributes are customized to corresponding database
(Oracle/PostgreSQL) data types (see Table 1). Some attributes of the
data type date were mapped to TIMESTAMP WITH TIME ZONE to allow a more
accurate storage of time values.

=============================================================== ============================== ============================
**Data type mapping (excerpt)**                                                               
**UML**                                                         **Oracle**                     **PostgreSQL / PostGIS**
**String, anyURI**                                              **VARCHAR2, CLOB**             **VARCHAR, TEXT**
**Integer**                                                     **NUMBER**                     **NUMERIC**
**Double, gml:LengthType**                                      **BINARY_DOUBLE**              **DOUBLE PRECISION**
**Boolean**                                                     **NUMBER(1,0)**                **NUMERIC**
**Date**                                                        **DATE,**                      **DATE,**
                                                                                              
                                                                **TIMESTAMP WITH TIME ZONE**   **TIMESTAMP WITH TIME ZONE**
**Primitive Type (Color, TransformationMatrix, CodeType etc.)** **VARCHAR2**                   **VARCHAR**
**Enumeration**                                                 **VARCHAR2**                   **VARCHAR**
**GML Geometry, textureCoordinates**                            **SDO_GEOMETRY**               **GEOMETRY**
**GML RectifiedGridCoverage**                                   **SDO_GEORASTER & SDO_RASTER** **RASTER**
**Texture (only reference of type anyURI in CityGML)**          **BLOB**                       **BYTEA**
=============================================================== ============================== ============================

Table 1: *Data type mapping*

Explicit declaration of class affiliation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In the (meta) table OBJECTCLASS, all class names (attribute CLASSNAME)
of the schema are managed. The relation of the subclass to its parent
class is represented via the attribute SUPERCLASS_ID in the subclass as
a foreign key to the ID of the parent class (see **Fehler! Verweisquelle
konnte nicht gefunden werden.**).

The table OBJECTCLASS is used to efficiently determine the affiliation
to a class in the superclass tables. In addition, the table CITYOBJECT
contains the attribute OBJECTCLASS_ID which refers to the respective
table OBJECTCLASS. This way, while looking at a tuple in CITYOBJECT, the
subclass and – if needed – the name of the class can be determined
directly. This mechanism has also been adopted in other tables that are
used to store different CityGML features, e.g. THEMATIC_SURFACE (for all
different *BoundarySurfaces* of a *Building* feature) or
BUILDING_INSTALLATION (outer or interior) etc. Please consider that
using CityGML ADEs could lead to additional OBJECTCLASS_IDs in this
table (please also refer to 2.3.3.1 Metadata Model).

=============== =========================== =================
**OBJECTCLASS**                            
=============== =========================== =================
**ID**          **CLASSNAME**               **SUPERCLASS_ID**
0               Undefined                  
1               \_GML                      
2               \_Feature                   1
3               \_CityObject                2
4               LandUse                     3
5               GenericCityObject           3
6               \_VegetationObject          3
7               SolitaryVegetationObject    6
8               PlantCover                  6
9               WaterBody                   105
10              \_WaterBoundarySurface      3
11              WaterSurface                10
12              WaterGroundSurface          10
13              WaterClosureSurface         10
14              ReliefFeature               3
15              \_ReliefComponent           3
16              TINRelief                   15
17              MassPointRelief             15
18              BreaklineRelief             15
19              RasterRelief                15
20              \_Site                      3
21              CityFurniture               3
22              \_TransportationObject      3
23              CityObjectGroup             3
24              \_AbstractBuilding          20
25              BuildingPart                24
26              Building                    24
27              BuildingInstallation        3
28              IntBuildingInstallation     3
29              \_BuildingBoundarySurface   3
30              BuildingCeilingSurface      29
31              InteriorBuildingWallSurface 29
32              BuildingFloorSurface        29
33              BuildingRoofSurface         29
34              BuildingWallSurface         29
35              BuildingGroundSurface       29
36              BuildingClosureSurface      29
37              \_BuildingOpening           3
38              BuildingWindow              37
39              BuildingDoor                37
40              BuildingFurniture           3
41              BuildingRoom                3
42              TransportationComplex       22
43              Track                       42
44              Railway                     42
45              Road                        42
46              Square                      42
47              TrafficArea                 22
48              AuxiliaryTrafficArea        22
49              FeatureCollection           2
50              Appearance                  2
51              \_SurfaceData               2
52              \_Texture                   51
53              X3DMaterial                 51
54              ParameterizedTexture        52
55              GeoreferencedTexture        52
56              \_TextureParametrization    1
57              CityModel                   49
58              Address                     2
59              ImplicitGeometry            1
60              OuterBuildingCeilingSurface 29
61              OuterBuildingFloorSurface   29
62              \_AbstractBridge            20
63              BridgePart                  62
64              Bridge                      62
65              BridgeInstallation          3
66              IntBridgeInstallation       3
67              \_BridgeBoundarySurface     3
68              BridgeCeilingSurface        67
69              InteriorBridgeWallSurface   67
70              BridgeFloorSurface          67
71              BridgeRoofSurface           67
72              BridgeWallSurface           67
73              BridgeGroundSurface         67
74              BridgeClosureSurface        67
75              OuterBridgeCeilingSurface   67
76              OuterBridgeFloorSurface     67
77              \_BridgeOpening             3
78              BridgeWindow                77
79              BridgeDoor                  77
80              BridgeFurniture             3
81              BridgeRoom                  3
82              BridgeConstructionElement   3
83              \_AbstractTunnel            20
84              TunnelPart                  83
85              Tunnel                      83
86              TunnelInstallation          3
87              IntTunnelInstallation       3
88              \_TunnelBoundarySurface     3
89              TunnelCeilingSurface        88
90              InteriorTunnelWallSurface   88
91              TunnelFloorSurface          88
92              TunnelRoofSurface           88
93              TunnelWallSurface           88
94              TunnelGroundSurface         88
95              TunnelClosureSurface        88
96              OuterTunnelCeilingSurface   88
97              OuterTunnelFloorSurface     88
98              \_TunnelOpening             3
99              TunnelWindow                98
100             TunnelDoor                  98
101             TunnelFurniture             3
102             HollowSpace                 3
103             TexCoordList                56
104             TexCoordGen                 56
105             \_WaterObject               3
106             \_BrepGeometry              0
107             Polygon                     106
108             BrepAggregate               106
109             TexImage                    0
110             ExternalReference           0
111             GridCoverage                0
112             \_genericAttribute          0
113             genericAttributeSet         112
=============== =========================== =================

Conceptual database structure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Starting from version 4.0.0, the 3DCityDB database schema has been
slightly modified to support the handling of CityGML ADEs (Application
Domain Extensions). With this enhancement, user-defined database schemas
can be dynamically created and attached to a 3DCityDB instance for
storing ADE data contents. In addition, every existing CityGML class
table is now equipped with an OBJECTCLASS_ID column which allows to
distinguisch the stored data contents of different CityGML and ADE
classses having inheritance relationships. Moreover, a set of new
metadata tables are introduced in addition to the existing OBJECTCLASS
table, for holding the relevant meta-information of the registered
CityGML ADEs. In general, all 3DCityDB tables now logically belong to
one of the three modules *Metadata Module*, *Core Data Module*, and
*Dynamic Data Module*, whose relations are shown in the following
figure.

|image30|

Figure 27: New conceptual 3DCityDB database structure for handling
CityGML ADEs

The green tables enclosed in the *Core Data Module* represent those
database tables that are responsible for storing the standard CityGML
models such as *Building*, *Transportation, Tunnel, CityFurniture,
CityObjectGroup, Generic, Appearance* etc. This module comprises
basically the tables of the database schema of previous versions of the
3DCityDB (cf. the next section for more details). For a given CityGML
ADE, an additional group of database tables forming a separate module
belonging to the *Dynamic Data Module* (pink tables in the figure) can
be created and attached to the 3DCityDB database schema. In addition,
the relationships (e.g. generalization/specialization and associations)
among the model classes of CityGML and CityGML ADEs are adequately
reflected using database foreign key constraints which allow to ensure
the data integrity and consistency within the database system. The
*Metadata Module* associated with the *Dynamic Data Module* is utilized
for storing the relevant meta-information (e.g. the XML namespaces,
schema files, and class affiliations etc.) about ADEs as well as the
referencing relations among the ADE and CityGML application schemas.
This way, the dependencies between the registered ADE application
schemas can be directly read from the 3DCityDB database schema to
facilitate the database administration process, i.e. the registration
and deregistration of multiple CityGML ADEs within a 3DCityDB instance.
