CityGML 2.0 simplifications
---------------------------

The analysis of previous versions of the 3D City Database and of existing
real-world CityGML models has shown that a less complex database schema
is already sufficient to be able to store and manage CityGML data without
loss of information. Using a simplified schema makes it also easier to
retrieve data or to build software and tools against the schema. Moreover,
it helps to improve performance because, for instance, less joins
are needed in database queries. Therefore, the first task in deriving a
database design from the CityGML data model was to identify and implement
possible simplifications.

General simplifications that have been applied to multiple and different
feature types and elements of CityGML are listed below. A specific discussion
of each CityGML module is provided in the following sections.

**Multiplicities of attributes**
  Attributes with a variable amount of occurrences (*) are substituted by
  a data type enabling the storage of arbitrary values (e.g. data type
  `String` with a predefined separator) or by an array with a predefined
  amount of elements representing the number of objects that participate
  in the association. This means that object attributes can be stored in
  a single column.

**Cardinalities and types of relationships**
  n:m relations require an additional table in the database. This table
  consists of the primary keys of both element tables which forms a
  composite primary key. If the relation can be restricted to a `1:n` or
  `n:1` relationship the additional table can be avoided. Therefore, all
  `n:m` relations in CityGML were checked for a more restrictive
  definition. This results in simplified cardinalities and relations.

**Simplified treatment of recursions**
  Some recursive relations are used in the CityGML data model. Recursive
  database queries may cause high cost, especially if the amount of
  recursive steps is unknown. In order to guarantee good performance,
  implementation of recursive associations receive two additional columns
  which contain the ID of the parent and of the root element. For example,
  if all building parts related to a specific building are queried, only
  those tuples containing the ID of the building as root element have to
  be selected. Thus, typical queries concerning object geometry remain
  high-performance.

**Data type adaptation**
  Data types specified in CityGML were substituted by data types which
  allow an efficient representation in the database. Strings, for example,
  are used to represent code types and number vectors; GML geometry types
  were changed to the database geometry data type. Matrices are stored
  each one as String data type, with values listed in a row-major sequence
  separated by spaces.

**Project specific classes and class attributes**
  The 3D city database may contain some classes for representation of
  project specific metadata, version control and attributes for
  representation of additional project specific information. Since this
  information is represented in the CityGML specification differently or
  even not at all, appropriate classes and class attributes are added or
  respectively adopted.

**Simplified design of GML geometry classes**
  Spatial properties of features are represented in CityGML using GML3’s
  geometry model, which is based on the ISO 19107 standard *Spatial Schema*
  [Herr2001]_ and represents 3D geometry according to the well-known
  Boundary Representation (B-Rep, cf. [FVFH1995]_). Actually only a subset
  of the GML3 geometry package is used in CityGML. Moreover, for 2D and 3D
  surface-based geometry types a simpler but equally powerful model is
  used: These geometries are stored as polygons, which are aggregated to
  *MultiSurfaces*, *CompositeSurfaces*, *TriangulatedSurfaces*, *Solids*,
  *MultiSolids*, as well as *CompositeSolids* (see :numref:`citydb_geometric-topological_model`
  for more details).
