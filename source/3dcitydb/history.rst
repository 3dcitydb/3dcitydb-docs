Development history
===================

Version 1 - 2003 - 2007
-----------------------

The development of the 3D City Database was always closely related to
the development of the CityGML standard [KoGr2003]_. It was
started back in 2003 by *Dr. Kolbe* and *Prof. Plümer* at the *Institute
for Cartography and Geoinformation at University of Bonn*. In the period
from November 2003 to December 2005 the official virtual 3D city model
of Berlin, commissioned by *The Berlin Senate* and *Berlin Partner
GmbH*, was developed within a pilot project funded by the European Union
[PGKS2005]_. Since then, the model has been playing
a central role in the three-dimensional spatial data infrastructure of
Berlin and opened up a multitude of applications for the public and
private sector alike. As an example the virtual city model is
successfully used for presentation of the business location, its urban
development combined with application related information to
politicians, investors, and the public in order to support civic
participation, provide access to decision-making content, assist in
policy-formulation, and control implementation processes [Döllner et al.
2006]. 3DCityDB was key in demonstrating the real world usage of CityGML
to the Open Geospatial Consortium on the one hand, and the practical
usability and versatility of CityGML to the city of Berlin on the other
hand. This first develop­ment phase was carried out by University of
Bonn in collaboration with the company *lat/lon GmbH*. Oracle Spatial
was the only supported SDBMS in that phase and the next (3DCityDB
Versions 0.2 up to 1.3).


Version 2 - 2006 - 2014
-----------------------

Within the framework **Europäische Fonds für regionale Entwicklung
(**\ EFRE II) the project *Geodatenmanagement in der Berliner Verwaltung
– Amtliches 3D-Stadtmodell für Berlin* allowed for upgrading the
official 3D city model based on the former CityGML specification draft
0.4.0 in the year 2007. **The developments were carried out by the
Institute for Geodesy und Geoinformation Science (IGG) of the Berlin
University of Technology (where Kolbe became full professor for
Geoinformation Science in 2006) on behalf of the Berliner
Senatsverwaltung für Wirtschaft, Arbeit und Frauen and the Berlin
Partner GmbH (former** Wirtschaftsförderung Berlin International\ **).
The relational database model (3DCityDB versions 1.4 up to 1.8) was
implemented and evaluated in cooperation with 3DGeo GmbH (later bought
by Autodesk GmbH) in Potsdam. A special database interface for
LandXPlorer was provided by 3DGeo / Autodesk.** Later on, a first
version of the Java based CityGML Importer/Exporter was developed
[SNKK2009]_.

**In August 2008, CityGML 1.0.0 became an adopted standard of the Open
Geospatial Consortium (OGC). In the follow-up project Digitaler
Gestaltplan Potsdam starting in 2010 the 3DCityDB version 2 was
developed which brought support for all CityGML 1.0.0 feature types. The
KML/COLLADA exporter was added as well as a ‘Matching’ plugin. This
project was carried out by IGG of TU Berlin on behalf of and in
collaboration with the company virtualcitySYSTEMS (VCS) in Berlin. In
2012 the developer team at TU Berlin received the** *Oracle Spatial
Excellence Award for Education and Research* from *Oracle USA* for our
work on 3DCityDB. Also in 2012 3DCityDB was ported to PostgreSQL/PostGIS
by *Felix Kunde*, a master student from the *University of Potsdam*, who
did his master thesis in collaboration with *IGG* [Kund2013]_.

In August 2012, CityGML 2.0.0 became an adopted standard of the Open
Geospatial Consor­tium (OGC). In September 2012, Prof. Kolbe moved from
IGG, TU Berlin to the *Chair of Geoinformatics at Technische Universität
München* (TUM). The companies virtualcity­SYSTEMS GmbH in Berlin and
M.O.S.S. Computer Grafik Systeme GmbH in Taufkirchen (near Munich) have
also been using the 3D City Database in their commercial projects for a
number of years. In this context, the Chair of Geoinformatics at TUM and
the companies virtualcitySYSTEMS and M.O.S.S. signed an official
collaboration agreement on the joint further development of 3DCityDB and
its tools.


Version 3 - 2013 - 2018
-----------------------

The work on the new major release version 3.0.0 began in 2013
when Dr. Nagel finished his PhD and joined the company VCS. In Version
3.3.0 the new 3D web client was being added. The webclient was developed
by Zhihang Yao with contributions from Kanishk Chaturvedi and Son
Nguyen. In 2015 Zhihang Yao and Kanishk Chaturvedi were awarded the
first price in the 'Best Students Contribution' of the 'Web3D city
modeling competition' under the annual ACM SIGGRAPH Web3D Conference for
the 3DCityDB-Web-Map-Client.


Version 4 - since 2015
-----------------------

The work on version 4.0.0 – especially the support of CityGML ADEs –
began in 2015 in the course of the PhD work of Zhihang Yao. One part of
his PhD thesis is focusing on the model transformation of CityGML ADEs
onto spatial relational databases using pattern matching and graph
transformation rules. Support of CityGML ADEs in the Importer/Exporter
required a substantial rewriting of the citygml4j Java library, the
Importer/Exporter and WFS source code performed by Dr. Nagel starting
from 2016. Felix Kunde worked, among others, on performance improvements
and restructuring of the PL/(pg)SQL scripts. Son Nguyen added support
for mobile devices in the 3DCityDB-Web-Map-Client in 2017. Docker
support was added by Bruno Willenborg in 2018. Starting from 2017 all
partners worked on updating diverse functionalities, scripts,
documentation, and on testing.


Acknowledgements
----------------

The 3D City Database project team is grateful and appreciative for the
financial assistance and support we received from partners that
contributed to the development of version 4.0 and the work on the ADE
support.

**Government Technology Agency of Singapore**

The Government Technology Agency of Singapore (GovTech Singapore) has
been developing a 3D city standard for Singapore based on CityGML, to
establish a common 3D representation of the city-state. GovTech wanted
to extend the representation to include other city features through the
ADE approach, and had worked with virtualcitySYSTEMS GmbH to start the
development of the ADE support on 3DCityDB. The intent is to open source
the 3DCityDB ADE support to the international community, so as to
encourage wider adoption and implementation of the CityGML standard and
ADEs.

**CADFEM International GmbH**

Founded in 1985, CADFEM is one of the pioneers of numerical simulation
based on the Finite Element Method and one of the largest European
suppliers of Computer-Aided Engineering. Through the Leonard Obermeyer
Center of the Technical University Munich, CADFEM supports the research
on digital methods for the design, creation and maintenance of the built
environment and the work on the 3D City Database. Bridging the gap
between simulation systems and 3D GIS / BIM is a key requirement for
enabling multi-physics Urban Simulations and for building Digital Twins
of the urban space. The CityGML ADE mechanism supports this in two ways:
1) city features can be enriched with data that is relevant for
simulations, and 2) simulation results can be brought back into the city
model, turning it into a dynamic knowledge base. CADFEM is supporting
the 3D City Database project to leverage the adoption and usage of
CityGML ADEs in the field of Urban Simulations.

**Climate-KIC of the EIT**

Climate-KIC is a so-called ‘Knowledge and Innovation Community’ about
Climate Change and Mitigation. It is one of three Knowledge and
Innovation Communities (KICs) created in 2010 by the European Institute
of Innovation and Technology (EIT). The EIT is an EU body whose mission
is to create sustainable growth. Most 3DCityDB developments at TU Munich
were done in the context of the projects Energy Atlas Berlin, Modeling
City Systems (MCS), Smart Sustainable Districts (SSD), and Smart
District Data Infrastructure (SDDI), all financially supported by
Climate-KIC.


List of changes between software versions
-----------------------------------------

Notable changes between 4.0 and 3.3
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

New features and functionalities:

-  Importer/Exporter 4.2: Reworked Plugin API to support non-GUI
   plugins.

-  Importer/Exporter 4.2: Property projections can now also be defined
   for abstract feature types.

-  Importer/Exporter 4.1: Added support for using SQL and XML queries
   for CityGML exports to be able express more flexible and complex
   filter conditions

-  Importer/Exporter 4.1: Added support for importing CityGML data from
   (G)ZIP files and exporting CityGML content to (G)ZIP files

-  Importer/Exporter 4.1: OSM Nominatim is now used as default geocoder
   for the map window. Google Map API services can still be used for the
   map window and for KML/COLLADA exports but require an API key.

-  Management and storage of arbitrary CityGML ADEs with the 3DCityDB,
   the Importer/Exporter ADE Manager Plugin and the 3DCityDB WFS

-  New 3DCityDB Docker images to support continuous integration
   workflows

-  New metadata tables ADE, SCHEMA, SCHEMA_REFERENCING and
   SCHEMA_TO_OBJECTCLASS for registering CityGML ADEs

-  New prefilled metadata table AGGREGATION_INFO that supports the
   automatic generation of DELETE and ENVELOPE scripts

-  New function to create entries in USER_SDO_GEOM_METADATA view
   (Oracle)

-  Function objectclass_id_to_table_name now has a counterpart:
   table_name_to_objectclass_ids returning an array of objectclass ids
   (CITYDB_OBJCLASS package in Oracle, part of a data schema in
   PostgreSQL)

-  New database procedures to enable/disable foreign key constraints to
   speed up bulk write operations (CITYDB_CONSTRAINT package in Oracle,
   part of the citydb_pkg schema in PostgreSQL)

-  New SQL script to create additional data schemas in one database
   (PostgreSQL)

-  New shell and SQL scripts to grant read-only or full read-write
   access to another schema.

-  Importer/Exporter can connect to different database schemas with the
   same user

-  Enabling XSL transformations on CityGML imports and exports as well
   as WFS responses

-  New database operation panel to change the spatial reference system
   used in the database (incl. optional coordinate transformation)

-  New LoD filter for CityGML exports

-  3DCityDB WFS allows for exporting into the CityJSON format

Improved and updated features and functionalities:

-  Moved interactive prompts from SQL to batch/shell scripts for better
   setup automation

-  Added OBJECTCLASS_ID column to all feature tables to distinguish
   objects from CityGML ADEs. Also extended OBJECTCLASS table by more
   feature-specific details and inserted new entries for feature
   properties such as geometry, generic attributes etc.

-  Improved performance on stored procedures by reducing amount of
   dynamic SQL. Therefore, schema_name parameter has been removed from
   DELETE and ENVELOPE scripts. Under PostgreSQL these scripts (as well
   as the INDEX_TABLE) are now part of a data schema such as citydb.

-  DELETE and ENVELOPE are now generated automatically in order to deal
   with schema changes introduced by ADEs. Therefore, the function
   prefix has been shortened to del\_ and env\_ not hit the character
   limit under Oracle,

-  The CITYDB_DELETE_BY_LINEAGE package has been removed. The only
   function left is del_cityobjects_by_lineage which is now part of the
   DELETE package

-  Database migration scripts for version 2.1.0 or version 3.3.0 to
   version 4.0.0

-  Switching from Ant to Gradle as the new build system for the
   Importer/Exporter tools

-  Allow import of CityGML files with flat hierarchies between city
   objects

-  Added support for importing gml:MultiGeometry objects containing only
   polygons

-  Added support for exporting to glTF v2.0

-  3DCityDB WFS now supports CORS and provides a KVP over HTTP GET
   endpoint for every operation simplifying the integration with GIS and
   ETL software such as FME
