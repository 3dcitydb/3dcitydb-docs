Introduction
============

Virtual 3D city and landscape models are provided for an increasing
number of cities, regions, states, and even countries. They are created
and maintained by public authorities like national and state mapping
agencies as well as by cadastre institutions and private companies. The
3D topography of urban and rural areas is essential for both visual
exploration and a range of different analyses in, for example, the urban
planning, environmental, energy, transportation, and facility management
sectors.

3D city models are nowadays used as an integrative information backbone
representing the relevant urban entities along with their spatial,
semantic, and visual properties. They are often created and maintained
with full coverage of entire cities and even countries, i.e. all real
world objects of a specific type like buildings, roads, trees, water
bodies, and the terrain are explicitly represented. In most cases the 3D
city model objects have well-defined identifiers, which are kept stable
during the lifetime of the real world objects and their virtual
counterparts. Such complete 3D models are a good basis to organize
different types of data and sensors within Smart City projects as they
build a stable platform for information linking and enrichment.

In order to establish a common understanding and interpretation of the
urban objects and to achieve interoperable access and exchange of
complete 3D models including the geometric, topologic, visual, and
semantic data, the Open Geospatial Consortium (OGC) has issued the
CityGML standard [Kolbe 2009]. CityGML defines a feature catalogue and
data model for the most relevant 3D topographic elements like buildings,
bridges, tunnels, roads, railways, vegetation, water bodies, etc. The
data model is mapped to an XML-based exchange format using OGC’s
Geography Markup Language (GML).

The 3D City Database (3DCityDB) is a free Open Source package consisting
of a database schema and a set of software tools to import, manage,
analyse, visualize, and export virtual 3D city models according to the
CityGML standard. The database schema results from a mapping of the
object oriented data model of CityGML 2.0 to the relational structure of
a spatially-enhanced relational database management system (SRDBMS). The
3DCityDB supports the commercial SRDBMS Oracle (with ‘Spatial’ or
‘Locator’ license options) and the Open Source SRDBMS PostGIS (which is
an extension to the free RDBMS PostgreSQL). 3DCityDB makes use of the
specific representation and processing capabilities of the SRDBMS
regarding the spatial data elements. It can handle also very large
models in multiple levels of details consisting of millions of 3D
objects with hundreds of millions of geometries and texture images.

3DCityDB is in use in real life production systems in many places around
the world and is also being used in a number of research projects. For
example, the cities of Berlin, Potsdam, Munich, Frankfurt, Zurich,
Rotterdam, Singapore all keep and manage their virtual 3D city models
within an instance of 3DCityDB. The companies virtualcitySYSTEMS (VCS)
and M.O.S.S., who are also partners in development, use 3DCityDB at the
core of their commercial products and services to create, maintain,
visualize, transform, and export virtual 3D city models (see Appendix B,
Appendix C, and Appendix D for examples how and where TUM,
virtualcitySYSTEMS, and M.O.S.S. employ 3DCityDB in their projects).
Furthermore, the state mapping agencies of all 16 states in Germany
store and manage the state-wide collected 3D building models in CityGML
LOD1 and LOD2 using 3DCityDB. In 2012 the previous version of 3DCityDB
and the developer team received the Oracle Spatial Excellence Award,
issued by Oracle USA.

Since 3DCityDB is based on CityGML, interoperable data access from user
applications to the database can be achieved in at least two ways:

1) by using the included high-performance CityGML Import/Export tool or
      the included basic Web Feature Service 2.0 in order to exchange
      the data in CityGML format (Version 2.0 or 1.0), and

2) by directly accessing the database tables whose relational structures
   are fully explained in detail within this document. It is easy to
   enrich a 3D city model by adding information to the database tables
   in some user application (using e.g. the database APIs of programming
   language like C++, Java, Python, or of ETL tools like the Feature
   Manipulation Engine from Safe Software). The enriched dataset then
   can be exchanged or archived by exporting the city model to CityGML
   without information loss. Analogously, 3DCityDB can be used to import
   a CityGML dataset and then access and work with the city model by
   directly accessing the database tables from some application programs
   or ETL software.

The Import/Export tool also provides functionalities for the direct
export of 3D visualization models in KML, COLLADA, and glTF formats. A
tiling strategy is supported which allows to visualize even very large
3D city and landscape models in geoinformation systems (GIS) or digital
virtual globes like Google Earth or CesiumJS Virtual Globe. The
Import/Export tool comes with an API to create further importers,
exporters, and database administration tools. One export plugin coming
with the software installer package is the so-called ‘Spreadsheet
Generator Plugin’ (SPSHG) which allows to export thematic data of 3D
objects into tables in CSV and Microsoft Excel format that can be easily
uploaded to and published as online spreadsheets, for instance, within
the Google Cloud. Starting from release 3.3.0, the 3DCityDB software
package comes with the CesiumJS-based 3D viewer called
“3DCityDB-Web-Map-Client” which can link the 3D visualization models
with online spreadsheets and facilitates interactive visualization and
exploration of 3D city models over the Internet within web browsers on
desktop and mobile computers. The most significant new functionality in
release 4.0.0 is the support of CityGML Application Domain Extensions
(ADEs). ADEs extend the CityGML datamodel by domain specific object
types, attributes, and relations.

**This document describes the design and the components of the 3D City
Database as well as their usage for the new major release 4.0.0 which
has been developed and implemented by the three partners in development,
namely the Chair of Geoinformatics at Technische Universität München,
virtualcitySYSTEMS, and M.O.S.S. The development is continuing the
previous work carried out at the Institute for Geodesy und
Geoinformation Science (IGG) of the Berlin University of Technology and
the Institute for Cartography and Geoinformation (IKG) of the University
of Bonn.**

**This document has been completely reworked, integrated, extended, and
edited from the previous 3DCityDB documentations (version 3.3.0, version
2.0.1, and the documentation addendum on 3DCityDB version 2.1.0 and the
Importer/Exporter tool version 1.6.0). Some figures and texts are cited
from the** OpenGIS City Geography Markup Language (CityGML) Encoding
Standard, Version 2.0.0 **[Gröger et al. 2012].**

Main features of 3DCityDB
-------------------------

Many (but not all) of the features referring to object modelling and
representation are implied by following the CityGML standard 2.0.0
issued by the Open Geospatial Consortium.

-  **CityGML 2.0.0 and 1.0.0 compliant database**: The implementation
   defines the classes and relations for the most relevant topographic
   objects in cities and regional models with respect to their
   geometrical, topological, semantical, and appearance properties.
   Included are generalization hierarchies between thematic classes,
   aggregations, relations between objects, and spatial properties.
   These thematic information go beyond graphic exchange formats and
   allow to employ virtual 3D city models for sophisticated analysis
   tasks in different application domains.

-  **Implementation on the basis of a spatially-enhanced relational
   database manage­ment system (Oracle 10G R2 or higher with
   Spatial/Locator option; PostgreSQL 9.1 or higher with PostGIS
   extension 2.0 or higher):** For the representation of all vector and
   grid geometry the built-in data types provided by the SRDBMS are used
   exclusively. This way, special solutions are avoided and different
   geoinformation systems, CAD/BIM systems, and ETL software systems can
   directly access (read and write) the geometry objects stored in the
   SRDBMS.

-  **Support for CityGML Application Domain Extensions (ADEs):**
   Semantic 3D city models are employed for many different applications
   from diverse domains like energetic, environmental, driving, and
   traffic simulations, as-built building infor­mation modeling
   (as-built BIM), asset management, and urban information fusion. In
   order to store and exchange application specific data aligned and
   integrated with the 3D city objects, the CityGML datamodel can be
   extended by new feature types, attributes, and relations using the
   CityGML ADE mechanism. ADEs are specified as (partial) GML
   application schemas using the modeling language XML Schema. Starting
   from release 4.0.0 the 3DCityDB database schema can be dynamically
   extended by arbitrary ADEs like the Energy ADE, UtilityNetwork ADE,
   Dynamizer ADE, or national CityGML extensions like IMGeo3D (from The
   Netherlands). Since ADEs can define an arbitrary number of new
   elements with all types and numbers of spatial properties, a
   transformation method has been developed to automatically derive the
   relational database schemas for arbitrary ADEs from the ADE XML
   schema files. Since we intended to follow similar rules in the
   mapping of the object-oriented ADE models onto relational models as
   we used for the (manual) mapping of the CityGML datamodel onto the
   3DCityDB core schema, the Chair of Geoinformatics at TUM developed a
   new transformation method based on graph transformation systems. This
   method is described in detail in [Yao & Kolbe 2017] and is
   implemented within the “ADE Manager” plugin for the Importer/Exporter
   software tool. It performs a sophisticated analysis of the XML schema
   files of an ADE, the automatic derivation of additional relational
   table structures, and the registration of the ADE within the
   3DCityDB. Furthermore, SQL scripts are generated for each ADE for
   e.g. the deletion of ADE objects and attributes from the database.
   Please note that in order to support also the import and export of
   CityGML datasets with ADE contents, a Java library for the specific
   ADE has to be implemented. This library has to perform the handling
   of the CityGML ADE XML elements and the reading from and writing into
   the respective ADE database tables using JDBC and SQL. An example how
   to develop such a Java library is given for a Test ADE in the
   3DCityDB github repository [1]_.

-  **Tool for importing and exporting CityGML data:** The included
   Importer/Exporter software tool allows for high performance importing
   and exporting of CityGML datasets according to CityGML versions 2.0
   and 1.0. The tool allows processing of very large datasets (>> 4 GB),
   even if they include XLinks between CityGML features or XLinks to 3D
   GML geometry objects. The multi-threaded programming exploits
   multiprocessor systems or multikernel CPUs to speed up the processing
   of complex XML-structures, resulting in high performance database
   access. Objects can be filtered during import or export according to
   spatial regions (bounding box), their object IDs, feature types,
   names, and levels of detail. Bounding boxes can be interactively
   selected using a map window based on OpenStreetMap (OSM). A tiling
   strategy is implemented in order to support the export of very large
   datasets. In case of a very high number of texture images they can be
   automatically distributed in a configurable number of subdirectories
   in order to avoid large directories with millions of files which can
   render a Microsoft Windows operating systems unresponsive. The
   Importer can also validate CityGML files and can be configured to
   only import valid features. It considers CityGML ADE contents, if the
   ADEs have been registered in the database and specific Java libraries
   for reading/writing the ADE contents from/into the ADE database
   tables is provided (see above). The Importer/Exporter tool can be run
   in interactive or batch mode.

-  **Tool for exporting visualization models in KML, COLLADA, and glTF
   formats:** This tool exports city models from the 3D city database in
   KML, COLLADA, and glTF formats which can directly be viewed and
   interactively explored in geoinformation systems (GIS) or digital
   virtual globes like Google Earth or Cesium WebGL Virtual Globe. A
   tiling strategy is supported where only tiles in the vicinity of the
   viewer’s location are being loaded facilitating the visualization of
   even very large 3D city and landscape models. Information balloons
   for all objects can be configured by the user. The exported models
   are especially suited to be visualized using the
   3DCityDB-Web-Map-Client (see below), an Open Source 3D web viewer
   that is based on the CesiumJS Webglobe framework with many functional
   extensions.

-  **Tool for exporting data to spreadsheets:** The ‘Spreadsheet
   Generator’ (SPSHG) allows exporting thematic data of 3D objects into
   tables in CSV and Microsoft Excel format which can be uploaded to a
   Google Spreadsheet within the Google Document Cloud. For every
   selected geoobject one row is being exported where the first column
   always contains the GMLID value of the respective object. The further
   columns can be selected by the user. This tool can be used to export
   attribute data from e.g. buildings like the class, function, usage,
   roof type, address, and further generic attributes that may contain
   information like the building energy demand, potential solar energy
   gain, noise level on the facades etc. The spreadsheet rows can be
   linked to the visualization model generated by the KML/COLLADA/glTF
   Exporter. This is illustrated in Appendix B.

-  **Tool for 3D visualization and interactive exploration of 3D models
   on the web:** The ‘3DCityDB-Web-Map-Client’ is a WebGL-based 3D web
   viewer which extends the Cesium Virtual Globe to support efficient
   displaying, caching, prefetching, dynamic loading and unloading of
   arbitrarily large pre-styled 3D visualization models in the form of
   tiled KML/glTF datasets generated by the KML/COLLADA/glTF Exporter.
   It provides an intuitive user interface to facilitate rich
   interaction with 3D visualization models by means of the enhanced
   functionalities like highlighting the objects of interests on
   mouseover and mouseclick as well as hiding, showing, and shadowing
   them. Moreover, the 3DCityDB-Web-Map-Client is able to link the 3D
   visualization model with an online spreadsheet (Google Fusion Table)
   in the Google Cloud and allows viewing and querying the thematic data
   of every city object according to its GMLID. For details see also
   [Chaturvedi et al. 2015, Yao et al. 2016].

-  **Web Feature Service (WFS) 2.0:** The 3DCityDB comes with an OGC
   compliant implementation of a basic WFS 2.0 allowing web-based access
   to the 3D city objects stored in the database. WFS clients can
   directly connect to this interface and retrieve 3D content for a wide
   variety of purposes. The implementation currently satisfies the
   *Simple WFS* conformance class. The WFS considers CityGML ADE
   contents, if the ADEs have been registered in the database and
   specific Java libraries for reading/ writing the ADE contents
   from/into the ADE database tables is provided (see above). An
   implementation of a full, transactional WFS is commercially available
   from one of the development partners, see Appendix C.

-  **Support of different kinds of multi-representations: Levels of
   detail, different appearances, (and with Oracle RDBMS only) planning
   versions and history: Every geoobject as well as the DTM can be
   represented in five different resolution or fidelity steps (Levels of
   Detail, LOD). With increasing LOD, objects do not only obtain a more
   precise and finer geometry, but do also gain a thematic refinement.**

**Different appearance data may be stored for each city object.
Appearance relates to any surface-based theme, e.g. infrared radiation
or noise pollution, not just visual properties. Consequently, data
provided by appearances can be used as input for both presentation and
analysis of virtual 3D city models. The database supports feature
appearances for an arbitrary number of themes per city model. Each LOD
of a feature can have individual appearances. Appearances can represent
– among others – textures and georeferenced textures. All texture images
can be stored in the database.**

**The version and history management employs Oracle’s Workspace Manager
and, hence, is only available for 3DCityDB instances running on an
Oracle RDBMS. It is largely transparent to application programs that
work with the database. Procedures saved within the database (Stored
Procedures) are provided, which allow for the management of planning
alternatives and versions via application programs.**

-  **Complex digital terrain models:** DTMs may be represented in four
   different ways in CityGML and therefore also in the 3D city database:
   regular grids, triangular irregular networks (TINs), 3D mass points
   and 3D break lines. For every level of detail, a complex DTM
   consisting of any number of DTM components and DTM types can be
   defined. Besides, it is possible to combine certain kinds of DTM
   representations for the same geographic area with each other (e.g.
   mass points and break lines or grids and break lines). In Oracle
   Spatial (but not Locator) Grid-based DTMs may be of arbitrary size
   and are composed from separate tiles to a single overall grid using
   the Oracle GeoRaster functionality. Please note that the
   Import/Export tool provides functions to read and write TIN, mass
   point, and break line DTM components, but not for raster based DTMs.
   GeoRaster data would have to be imported and exported using other
   tools from e.g. Oracle, ESRI, or Safe Software.

-  **Complex city object modelling**: The representation of city objects
   in the 3D city database ranges from coarse models to geometrically
   and semantically fine grained structures. The underlying data model
   is a complete realization of the CityGML data model for the levels of
   detail (LOD) 0 to 4. For example, buildings can be represented by
   simple, monolithic objects or can consist of an aggregation of
   building parts. Extensions of buildings, like balconies and stairs,
   can be classified thematically and provided with attributes just as
   single surfaces can be. LOD4 completes a LOD3 model by adding
   interior structures for 3D objects. For example, LOD4 buildings are
   composed of rooms, interior doors, stairs, and furniture. This allows
   among other things to select the floor space of a building, so that
   it can later be used e.g. to derive SmartBuildings or to form 3D
   solids by extrusion [Döllner et al. 2005]. Buildings can be assigned
   addresses that are also stored in the 3D city database. Their
   implemen­tation refers to the OASIS xAL Standard, which maps the
   address formats of the different countries into a unified XML schema.
   In order to model whole complexes of buildings, single buildings can
   be aggregated to form special building groups. The same complex
   modelling applies to the other CityGML feature types like bridges,
   tunnels, transportation and vegetation objects, and water bodies.

-  **Representation of generic and prototypical 3D objects:** Generic
   objects enable the storage of 3D geoobjects that are not explicitly
   modelled in CityGML yet, for example dams or city walls, or that are
   available in a proprietary file format only. This way, files from
   other software systems like architecture or computer graphics
   programs can be imported directly into the database (without
   interpretation). However, application systems that would like to use
   these data must be able to interpret the corresponding file formats
   after retrieving them back from the 3D geodatabase.

Prototypical objects are used for memory-efficient management of objects
that occur frequently in the city model and that do not differ with
respect to geometry and appearance. Examples are elements of street
furniture like lanterns, road signs or benches as well as vegetation
objects like shrubs, certain tree types etc. Every instance of a
prototypical object is represented by a reference to the prototype, a
base point and a transformation matrix for scaling, rotating and
translating the prototype.

The geometries (and appearances like textures, colors etc.) of generic
objects as well as prototypes can be stored either using the geometry
datatype of the spatial database management system (Oracle
Spatial/Locator or PostGIS) or in proprietary file formats. In the
latter case a single file may be saved for every object, but the file
type (MIME type), the coordinate transformation matrix that is needed to
integrate the object into the world coordinate reference system (CRS)
and the target CRS have to be specified.

-  **Extendable object attribution:** All objects in the 3D geodatabase
   can be augmented with an arbitrary number of additional generic
   attributes. This way, it is possible to add further thematic
   information as well as further spatial properties to the objects at
   any time. In combination with the concept of generic 3D objects this
   provides a highly flexible storage option for object types which are
   not explicitly defined in the CityGML standard. Every generic
   attribute consists of a triple of attribute name, data type, and
   value. Supported data types are: string; integer and floating-point
   numbers; date; time; binary object (BLOB, e.g. for storing a file);
   geometry object according to the specific geometry data type of
   Oracle or PostGIS respectively; simple, composite, or aggregate 3D
   solids or surfaces. Please note that generic attributes of type BLOB
   or geometry are not allowed as generic attributes in CityGML (and
   will, thus, not be exported by the CityGML exporter). However, it may
   be useful to store binary data associated with the individual city
   objects, for example, to store derived 3D computer graphics
   representations.

-  **Free, also recursive grouping of geoobjects:** Geoobjects can be
   grouped arbitrarily. The aggregates can be named and may also be
   provided with an arbitrary number of generic attributes (see above).
   Object groups may also contain object groups, which leads to nested
   aggregations of arbitrary depth. In addition, for every object of an
   aggregation, its role in the group can be specified explicitly
   (qualified association).

-  **External references for all geoobjects:** All geoobjects can be
   provided with an arbitrary number of references to corresponding
   objects in external data sources (i.e. hyperlinks / linked data). For
   example, in case of building objects this allows to store e.g. the
   IDs of the corresponding objects in official cadasters, digital
   landscape models (DLM), or Building Information Models (BIM). Each
   reference consists of an URI to the external data store or database
   and the corresponding object ID or URI within that external data
   store or database.

-  **Flexible 3D geometries:** The geometry of most 3D objects can be
   represented through the combination of solids and surfaces as well as
   any - also recursive - aggregation of these elements. Each surface
   may has attached different textures and colors on both its front and
   back face. It may also comprise information on transparency.
   Additional geometry types (any geometry type supported by the spatial
   database management system Oracle Spatial/Locator or PostGIS) can be
   added to the geoobjects by using generic attributes.

-  **Open Source and Platform Independence:** The entire software is
   freely accessible to the interested public. The 3DCityDB is licensed
   under the Apache License, Version 2.0, which allows including
   3DCityDB in commercial systems. You may obtain a copy of the Apache
   License at http://www.apache.org/licenses/LICENSE-2.0. Both the
   Importer/Exporter tool and the Web Feature Service are imple­mented
   in Java and can be run on different platforms and operating systems.

-  **Docker support:** We now provide Docker images for 1) a complete
   3DCityDB installation pre-installed within a PostGIS SRDBMS, 2) a
   webserver with an installed 3DCityDB-Web-Map-Client, 3) a 3DCityDB
   WFS. We also provide a Docker-compose script to launch all three
   Docker containers in a linked way with just a single command. Details
   are given in Section 9 and in the respective github
   repositories [2]_. Docker is a runtime environment for
   virtualization. Docker encapsulates individual software applications
   in so-called containers, which are – in contrast to virtual machines
   – light-weight and can be deployed, started and stopped very quickly
   and easily. Using our Docker images a 3DCityDB can be installed by a
   single command.

System and design decisions
---------------------------

The 3D City Database is implemented as a relational database schema
using the spatial datatypes provided by a spatially-enhanced relational
database management system (SRDBMS). Above, external software
applications and database stored procedures are provided working on this
database schema. Since only Oracle with the Spatial or Locator licensing
option (10G R2 or higher) and PostgreSQL (9.3 or higher) with PostGIS
extension (2.0 or higher) offer comprehensive support for 3D spatial
data, the 3D City Database schema is being provided for these two
systems only.

**In addition to the general advantages arising from the usage of a
widely used relational database management system (RDBMS), both Oracle
Spatial/Locator and PostgreSQL/ PostGIS offer some important performance
characteristics that allow an efficient imple­men­tation of the required
functionalities:**

-  Both RDBMS support spatial data types with coordinates ranging from
      2D to 4D. Spatial indexes and filters can be 2D or 3D allowing for
      efficient spatial selections in very large city models.
      Furthermore, the spatial data types are supported by a number of
      commercial and Open Source GIS that provide a database connection
      as for example ESRI’s ArcGIS/ArcSDE or Safe Software’s Feature
      Manipulation Engine (FME). This enables such systems to directly
      access the data stored in the 3D geodatabase.

-  Rules can be implemented using stored procedures and trigger
   mechanisms which propagate updates of objects to likewise affected
   objects in the database (transparent for the user).

**The data model of the 3D City Database is based on the CityGML 2.0
standard. The object-oriented data model of CityGML has been mapped to a
purely relational data model with the exception that geometry objects
are mapped to the spatial datatypes provided by the SDBMS. In order to
achieve high performance for data manipulations and queries the mapping
was done manually with a number of optimizations. A few simplifying
assumptions where made regarding the usage of the CityGML concepts in
the real world helping to increase performance. These are documented in
chapter** **2.1.**

**Surface-based geometries like Polygons, TINs, MultiSurfaces as well as
Solids are stored in a special way: they are decomposed into their
primitive surfaces and each surface is stored as an individual tuple in
one big surface table. The reason for this is that each surface can be
assigned multiple appearances (e.g. textures) in CityGML and, thus, each
appearance must be explicitly linkable to the corresponding surface. For
Solids also the solid geometry objects are stored in addition to their
decomposed boundary surfaces allowing to apply spatial operations on
them like the computation of the volume.**

**The provided software tools like the Importer/Exporter application are
implemented in the Java language in order to be platform independent.
The tools have been confirmed to run under Microsoft Windows, Linux, and
Apple Mac OS X. High performance is achieved by exploiting
multi-threading on multiprocessor or multi-core CPU systems.**