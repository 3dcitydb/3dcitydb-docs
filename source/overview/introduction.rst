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
`CityGML standard <https://www.opengeospatial.org/standards/citygml>`_ [Kolb2009]_.
CityGML defines a feature catalogue and data model for the most relevant
3D topographic elements like buildings, bridges, tunnels, roads,
railways, vegetation, water bodies, etc. The data model is mapped to an
XML-based exchange format using OGC’s Geography Markup Language (GML).

The 3D City Database (3DCityDB) is a free and open source package consisting
of a database schema and a set of software tools to import, manage,
analyse, visualize, and export virtual 3D city models according to the
CityGML standard [YNKH2018]_. The database schema results from a mapping of the
object oriented data model of CityGML 2.0 to the relational structure of
a spatially-enhanced relational database management system (SRDBMS). The
3DCityDB supports the open source SRDBMS PostgreSQL (with
*PostGIS* extension) and the commercial SRDBMS Oracle (with *Spatial* or
*Locator* license options). The 3DCityDB makes use of the
specific representation and processing capabilities of the SRDBMS
regarding the spatial data elements. It can handle also very large
models in multiple levels of details consisting of millions of 3D
objects with hundreds of millions of geometries and texture images.

The 3DCityDB is in use in real life production systems in many places around
the world and is also being used in a number of research projects. For
example, the cities of Berlin, Potsdam, Munich, Frankfurt, Zurich,
Rotterdam, Singapore all keep and manage their virtual 3D city models
within an instance of the 3DCityDB. The companies Virtual City Systems (VCS)
and M.O.S.S., who are also partners in development, use the 3DCityDB at the
core of their commercial products and services to create, maintain,
visualize, transform, and export virtual 3D city models (see Appendix B,
Appendix C, and Appendix D for examples how and where TUM,
Virtual City Systems, and M.O.S.S. employ the 3DCityDB in their projects).
Furthermore, the state mapping agencies of all 16 states in Germany
store and manage the state-wide collected 3D building models in CityGML
LOD1 and LOD2 using the 3DCityDB. In 2012 the 3DCityDB
and the developer team received the Oracle Spatial Excellence Award,
issued by Oracle USA.

Since the 3DCityDB is based on CityGML, interoperable data access from user
applications to the database can be achieved in at least two ways:

1) by using the included high-performance CityGML Importer/Exporter tool or
   the included basic Web Feature Service 2.0 in order to exchange the
   data in CityGML format (version 2.0 or 1.0), and

2) by directly accessing the database tables whose relational structures
   are fully explained in detail within this document. It is easy to
   enrich a 3D city model by adding information to the database tables
   in some user application (using e.g. the database APIs of programming
   language like C++, Java, Python, or of ETL tools like the Feature
   Manipulation Engine from Safe Software). The enriched dataset then
   can be exchanged or archived by exporting the city model to CityGML
   without information loss. Analogously, the 3DCityDB can be used to import
   a CityGML dataset and then access and work with the city model by
   directly accessing the database tables from some application programs
   or ETL software.

The Importer/Exporter tool also provides functionalities for the direct
export of 3D visualization models in KML, COLLADA, and glTF formats. A
tiling strategy is supported which allows to visualize even very large
3D city and landscape models in geoinformation systems (GIS) or digital
virtual globes like Google Earth or CesiumJS Virtual Globe.

Starting from release 3.3.0, the 3DCityDB software bundle contains the
CesiumJS-based 3D viewer called “3DCityDB-Web-Map-Client” which facilitates
the interactive visualization and exploration of 3D city models over the
internet within web browsers on desktop and mobile computers. The most
significant new functionality in release 4.0.0 is the support of CityGML
Application Domain Extensions (ADEs). ADEs extend the CityGML data model
by domain specific object types, attributes, and relations.

The Importer/Exporter provides a Plugin API to create further importers,
exporters, and database administration tools. The software is shipped with
the two optional plugins: 1) The "Spreadsheet Generator Plugin" (SPSHG) to export
thematic data of 3D objects into tables in CSV and Microsoft Excel format
that can be easily published as online spreadsheets (e.g., using Google Docs)
and linked with the 3DCityDB-Web-Map-Client, and 2) the "ADE Manager Plugin" to
dynamically extend the 3DCityDB core schema with tables and objects
for storing and managing CityGML ADEs.

This documentation describes the design and the components of the 3D City
Database as well as their usage for the major release 4 which
has been developed and implemented by the three partners in development,
namely the `Chair of Geoinformatics <https://www.gis.bgu.tum.de/en/home/>`_
at Technische Universität München, `Virtual City Systems <https://www.vc.systems/>`_,
and `MOSS <https://www.moss.de/>`_.

The development is continuing the previous work carried out at the
`Institute for Geodesy and Geoinformation Science <https://www.igg.tu-berlin.de/menue/institut_fuer_geodaesie_und_geoinformationstechnik/parameter/en/>`_
of the Berlin University of Technology and the
`Institute for Cartography and Geoinformation <https://www.geoinfo.uni-bonn.de/en>`_
of the University of Bonn.

Some figures and texts are cited from the OpenGIS City Geography Markup
Language (CityGML) Encoding Standard, Version 2.0.0 [GKNH2012]_.
