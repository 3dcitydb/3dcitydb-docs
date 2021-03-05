Mapping rules and metadata
~~~~~~~~~~~~~~~~~~~~~~~~~~

Mapping of classes onto tables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In general, every class of the UML diagram is mapped onto a separate
table; the name of the table is identical to the class name (leading
underscores indicating abstract classes are omitted). If multiple classes
are contained in an orange box in the UML diagram though, these classes
are mapped onto a single table in the relational schema.

Scalar attributes of the classes become columns of the corresponding table
with identical name. The types of attributes are customized to corresponding
data types of the target database systems PostgreSQL/PostGIS and Oracle as
shown in the following :numref:`data_type_mapping_table`.

.. list-table::  *Data type mapping (excerpt)*
   :name: data_type_mapping_table

   * - | **UML**
     - | **PostgreSQL / PostGIS**
     - | **Oracle**
   * - | String, anyURI
     - | VARCHAR, TEXT
     - | VARCHAR2, CLOB
   * - | Integer
     - | NUMERIC
     - | NUMBER
   * - | Double, gml:LengthType
     - | DOUBLE PRECISION
     - | BINARY_DOUBLE
   * - | Boolean
     - | NUMERIC
     - | NUMBER(1,0)
   * - | Date
     - | DATE or
       | TIMESTAMP WITH TIME ZONE
     - | DATE or
       | TIMESTAMP WITH TIME ZONE
   * - | Complex Types
       | (Color, TransformationMatrix,
       | CodeType etc.)
     - | VARCHAR
     - | VARCHAR2
   * - | Enumeration
     - | VARCHAR
     - | VARCHAR2
   * - | GML Geometry,
       | textureCoordinates
     - | GEOMETRY
     - | SDO_GEOMETRY
   * - | GML RectifiedGridCoverage
     - | RASTER
     - | SDO_GEORASTER
       | & SDO_RASTER
   * - | Texture (only reference
       | of type anyURI in CityGML)
     - | BYTEA
     - | BLOB

.. _citydb_class_affiliation_declaration_chapter:

Explicit metadata about feature classes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The central metadata table OBJECTCLASS contains all feature classes supported by the 3D City
Database. Every CityGML feature class is
assigned a unique and stable ID in this table. For example, *Building*
is assigned the ID value 26 and *Bridge* has the value 64. In addition,
the name of the feature class is stored in the attribute CLASSNAME. The
name of the table onto which the feature class has been mapped is provided
by the TABLENAME column.

The SUPERCLASS_ID attribute references the direct superclass of the feature class
and, thus, maps the class hierarchy. The additional BASECLASS_ID attribute
points to the root class of the hierarchy and can be used to quickly
understand whether an entry in OBJECTCLASS represents a GML feature type,
object type or data type without having to traverse the entire class hierarchy.
If a feature class represents a CityGML top-level feature, the IS_TOPLEVEL
flag is set to 1 and 0 otherwise.

All city objects stored in the 3D City Database are registered in the
root table CITYOBJECT. This table has an attribute OBJECTCLASS_ID which
references an entry in OBJECTCLASS. This way, the class of the city object
can be easily and efficiently identified. If required, its class name, its feature
table, whether it is a top-level feature, and even its (transitive) superclasses can also be queried.
In addition to CITYOBJECT, all tables that are used to store CityGML features
provide an OBJECTCLASS_ID attribute.

.. note::
  Registering CityGML ADEs in the 3D City Database leads to additional
  entries in the OBJECTCLASS table for each class defined in the ADE.
  The OBJECTCLASS table has two more attributes IS_ADE_CLASS and ADE_ID
  which are used to manage and identify ADE classes. More information is
  provided in :numref:`citydb_managing_ades_chapter` for more information.

.. list-table::  Excerpt of the *OBJECTCLASS* table
   :name: citydb_objectclass_table

   * - | **ID**
     - | **CLASSNAME**
     - | **SUPERCLASS_ID**
   * - | 0
     - | Undefined
     - |
   * - | 1
     - | \_GML
     - |
   * - | 2
     - | \_Feature
     - | 1
   * - | 3
     - | \_CityObject
     - | 2
   * - | 4
     - | LandUse
     - | 3
   * - | 5
     - | GenericCityObject
     - | 3
   * - | 6
     - | \_VegetationObject
     - | 3
   * - | 7
     - | SolitaryVegetationObject
     - | 6
   * - | 8
     - | PlantCover
     - | 6
   * - | 9
     - | WaterBody
     - | 105
   * - | 10
     - | \_WaterBoundarySurface
     - | 3
   * - | 11
     - | WaterSurface
     - | 10
   * - | 12
     - | WaterGroundSurface
     - | 10
   * - | 13
     - | WaterClosureSurface
     - | 10
   * - | 14
     - | ReliefFeature
     - | 3
   * - | 15
     - | \_ReliefComponent
     - | 3
   * - | 16
     - | TINRelief
     - | 15
   * - | 17
     - | MassPointRelief
     - | 15
   * - | 18
     - | BreaklineRelief
     - | 15
   * - | 19
     - | RasterRelief
     - | 15
   * - | 20
     - | \_Site
     - | 3
   * - | 21
     - | CityFurniture
     - | 3
   * - | 22
     - | \_TransportationObject
     - | 3
   * - | 23
     - | CityObjectGroup
     - | 3
   * - | 24
     - | \_AbstractBuilding
     - | 20
   * - | 25
     - | BuildingPart
     - | 24
   * - | 26
     - | Building
     - | 24
   * - | 27
     - | BuildingInstallation
     - | 3
   * - | 28
     - | IntBuildingInstallation
     - | 3
   * - | 29
     - | \_BuildingBoundarySurface
     - | 3
   * - | 30
     - | BuildingCeilingSurface
     - | 29
   * - | 31
     - | InteriorBuildingWallSurface
     - | 29
   * - | 32
     - | BuildingFloorSurface
     - | 29
   * - | 33
     - | BuildingRoofSurface
     - | 29
   * - | 34
     - | BuildingWallSurface
     - | 29
   * - | 35
     - | BuildingGroundSurface
     - | 29
   * - | 36
     - | BuildingClosureSurface
     - | 29
   * - | 37
     - | \_BuildingOpening
     - | 3
   * - | 38
     - | BuildingWindow
     - | 37
   * - | 39
     - | BuildingDoor
     - | 37
   * - | 40
     - | BuildingFurniture
     - | 3
   * - | 41
     - | BuildingRoom
     - | 3
   * - | 42
     - | TransportationComplex
     - | 22
   * - | 43
     - | Track
     - | 42
   * - | 44
     - | Railway
     - | 42
   * - | 45
     - | Road
     - | 42
   * - | 46
     - | Square
     - | 42
   * - | 47
     - | TrafficArea
     - | 22
   * - | 48
     - | AuxiliaryTrafficArea
     - | 22
   * - | 49
     - | FeatureCollection
     - | 2
   * - | 50
     - | Appearance
     - | 2
   * - | 51
     - | \_SurfaceData
     - | 2
   * - | 52
     - | \_Texture
     - | 51
   * - | 53
     - | X3DMaterial
     - | 51
   * - | 54
     - | ParameterizedTexture
     - | 52
   * - | 55
     - | GeoreferencedTexture
     - | 52
   * - | 56
     - | \_TextureParametrization
     - | 1
   * - | 57
     - | CityModel
     - | 49
   * - | 58
     - | Address
     - | 2
   * - | 59
     - | ImplicitGeometry
     - | 1
   * - | 60
     - | OuterBuildingCeilingSurface
     - | 29
   * - | 61
     - | OuterBuildingFloorSurface
     - | 29
   * - | 62
     - | \_AbstractBridge
     - | 20
   * - | 63
     - | BridgePart
     - | 62
   * - | 64
     - | Bridge
     - | 62
   * - | 65
     - | BridgeInstallation
     - | 3
   * - | 66
     - | IntBridgeInstallation
     - | 3
   * - | 67
     - | \_BridgeBoundarySurface
     - | 3
   * - | 68
     - | BridgeCeilingSurface
     - | 67
   * - | 69
     - | InteriorBridgeWallSurface
     - | 67
   * - | 70
     - | BridgeFloorSurface
     - | 67
   * - | 71
     - | BridgeRoofSurface
     - | 67
   * - | 72
     - | BridgeWallSurface
     - | 67
   * - | 73
     - | BridgeGroundSurface
     - | 67
   * - | 74
     - | BridgeClosureSurface
     - | 67
   * - | 75
     - | OuterBridgeCeilingSurface
     - | 67
   * - | 76
     - | OuterBridgeFloorSurface
     - | 67
   * - | 77
     - | \_BridgeOpening
     - | 3
   * - | 78
     - | BridgeWindow
     - | 77
   * - | 79
     - | BridgeDoor
     - | 77
   * - | 80
     - | BridgeFurniture
     - | 3
   * - | 81
     - | BridgeRoom
     - | 3
   * - | 82
     - | BridgeConstructionElement
     - | 3
   * - | 83
     - | \_AbstractTunnel
     - | 20
   * - | 84
     - | TunnelPart
     - | 83
   * - | 85
     - | Tunnel
     - | 83
   * - | 86
     - | TunnelInstallation
     - | 3
   * - | 87
     - | IntTunnelInstallation
     - | 3
   * - | 88
     - | \_TunnelBoundarySurface
     - | 3
   * - | 89
     - | TunnelCeilingSurface
     - | 88
   * - | 90
     - | InteriorTunnelWallSurface
     - | 88
   * - | 91
     - | TunnelFloorSurface
     - | 88
   * - | 92
     - | TunnelRoofSurface
     - | 88
   * - | 93
     - | TunnelWallSurface
     - | 88
   * - | 94
     - | TunnelGroundSurface
     - | 88
   * - | 95
     - | TunnelClosureSurface
     - | 88
   * - | 96
     - | OuterTunnelCeilingSurface
     - | 88
   * - | 97
     - | OuterTunnelFloorSurface
     - | 88
   * - | 98
     - | \_TunnelOpening
     - | 3
   * - | 99
     - | TunnelWindow
     - | 98
   * - | 100
     - | TunnelDoor
     - | 98
   * - | 101
     - | TunnelFurniture
     - | 3
   * - | 102
     - | HollowSpace
     - | 3
   * - | 103
     - | TexCoordList
     - | 56
   * - | 104
     - | TexCoordGen
     - | 56
   * - | 105
     - | \_WaterObject
     - | 3
   * - | 106
     - | \_BrepGeometry
     - | 0
   * - | 107
     - | Polygon
     - | 106
   * - | 108
     - | BrepAggregate
     - | 106
   * - | 109
     - | TexImage
     - | 0
   * - | 110
     - | ExternalReference
     - | 0
   * - | 111
     - | GridCoverage
     - | 0
   * - | 112
     - | \_genericAttribute
     - | 0
   * - | 113
     - | genericAttributeSet
     - | 112
