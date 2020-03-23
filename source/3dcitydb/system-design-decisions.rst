System and design decisions
---------------------------

The 3D City Database is implemented as a relational database schema
using the spatial datatypes provided by a spatially-enhanced relational
database management system (SRDBMS). Above, external software
applications and database stored procedures are provided working on this
database schema. Since only Oracle with the Spatial or Locator licensing
option (**10G R2 or higher**) and PostgreSQL (**9.6 or higher**) with
PostGIS extension (**2.3 or higher**) offer comprehensive support for
3D spatial data, the 3D City Database schema is being provided for these
two systems only.

In addition to the general advantages arising from the usage of a widely
used relational database management system (RDBMS), both Oracle
Spatial/Locator and PostgreSQL/ PostGIS offer some important performance
characteristics that allow an efficient imple­men­tation of the required
functionalities:

-  Both RDBMS support spatial data types with coordinates ranging from
   2D to 4D. Spatial indexes and filters can be 2D or 3D allowing for
   efficient spatial selections in very large city models.

-  The spatial data types are supported by a number of commercial and
   Open Source GIS that provide a database connection as for example
   ESRI’s ArcGIS/ArcSDE or Safe Software’s Feature Manipulation Engine
   (FME). This enables such systems to directly access the data stored
   in the 3D geodatabase.

-  Rules can be implemented using stored procedures and trigger
   mechanisms which propagate updates of objects to likewise affected
   objects in the database (transparent for the user).

The data model of the 3D City Database is based on the **CityGML 2.0**
standard. The object-oriented data model of CityGML has been mapped to a
purely relational data model with the exception that geometry objects
are mapped to the spatial datatypes provided by the SDBMS. In order to
achieve high performance for data manipulations and queries the mapping
was done manually with a number of optimizations. A few simplifying
assumptions where made regarding the usage of the CityGML concepts in
the real world helping to increase performance. These are documented in
the :doc:`data modelling <data-modelling>` chapter.

Surface-based geometries like Polygons, TINs, MultiSurfaces as well as
Solids are stored in a special way: they are decomposed into their
primitive surfaces and each surface is stored as an individual tuple in
one big surface table. The reason for this is that each surface can be
assigned multiple appearances (e.g. textures) in CityGML and, thus, each
appearance must be explicitly linkable to the corresponding surface. For
Solids also the solid geometry objects are stored in addition to their
decomposed boundary surfaces allowing to apply spatial operations on
them like the computation of the volume.

The provided software tools like the Importer/Exporter application are
implemented in the Java language in order to be platform independent.
The tools have been confirmed to run under Microsoft Windows, Linux, and
Apple Mac OS X. High performance is achieved by exploiting
multi-threading on multiprocessor or multi-core CPU systems.