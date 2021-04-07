Development history
===================

**Version 1 (2003 - 2007)**

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
policy-formulation, and control implementation processes [DKLS2006]_.

The 3DCityDB was key in demonstrating the real world usage of CityGML
to the Open Geospatial Consortium on the one hand, and the practical
usability and versatility of CityGML to the city of Berlin on the other
hand. This first development phase was carried out by University of
Bonn in collaboration with the company *lat/lon GmbH*. Oracle Spatial
was the only supported SDBMS in 3DCityDB versions 0.2 up to 1.3.

**Version 2 (2006 - 2014)**

Within the framework *Europäische Fonds für regionale Entwicklung*
(EFRE II) the project *Geodatenmanagement in der Berliner Verwaltung
– Amtliches 3D-Stadtmodell für Berlin* allowed for upgrading the
official 3D city model based on the former CityGML specification draft
0.4.0 in the year 2007. The developments were carried out by the
*Institute for Geodesy und Geoinformation Science (IGG) of the Berlin
University of Technology* (where Kolbe became full professor for
Geoinformation Science in 2006) on behalf of the *Berliner
Senatsverwaltung für Wirtschaft, Arbeit und Frauen and the Berlin
Partner GmbH* (former Wirtschaftsförderung Berlin International).
The relational database model (3DCityDB versions 1.4 up to 1.8) was
implemented and evaluated in cooperation with *3DGeo GmbH* (later bought
by *Autodesk GmbH*) in Potsdam. A special database interface for
LandXPlorer was provided by 3DGeo / Autodesk. Later on, a first
version of the Java based CityGML Importer/Exporter was developed
[SNKK2009]_.

In August 2008, CityGML 1.0.0 became an adopted standard of the Open
Geospatial Consortium (OGC). In the follow-up project *Digitaler
Gestaltplan Potsdam* starting in 2010 the 3DCityDB version 2 (cf. [KKNS2009]_ and [NaSt2008]_) was
developed which brought support for all CityGML 1.0.0 feature types. The
KML/COLLADA exporter was added as well as a ‘Matching’ plugin. This
project was carried out by *IGG of TU Berlin* on behalf of and in
collaboration with the company Virtual City Systems (VCS) in Berlin. In
2012, the developer team at TU Berlin received the *Oracle Spatial
Excellence Award for Education and Research* from *Oracle USA* for our
work on the 3DCityDB. Also in 2012, the 3DCityDB was ported to PostgreSQL/PostGIS
by *Felix Kunde*, a master student from the *University of Potsdam*, who
did his master thesis in collaboration with *IGG* [Kund2013]_.

In August 2012, CityGML 2.0.0 became an adopted standard of the Open
Geospatial Consortium (OGC). In September 2012, Prof. Kolbe moved from
IGG, TU Berlin to the *Chair of Geoinformatics at Technische Universität
München* (TUM). The companies Virtual City Systems in Berlin and
M.O.S.S. Computer Grafik Systeme GmbH in Taufkirchen (near Munich) have
also been using the 3D City Database in their commercial projects for a
number of years. In this context, the Chair of Geoinformatics at TUM and
the companies Virtual City Systems and M.O.S.S. signed an official
collaboration agreement on the joint further development of the 3DCityDB and
its tools.

**Version 3 (2013 - 2018)**

The work on the new major release version 3.0.0 bringing support for CityGML 2.0
began in 2013 when Dr. Nagel finished his PhD and joined the company VCS. In version
3.3.0 the new 3D web client was being added. The webclient was developed
by Zhihang Yao with contributions from Kanishk Chaturvedi and Son
Nguyen. In 2015 Zhihang Yao and Kanishk Chaturvedi were awarded the
first price in the 'Best Students Contribution' of the 'Web3D city
modeling competition' under the annual ACM SIGGRAPH Web3D Conference for
the 3DCityDB-Web-Map-Client.

**Version 4 (since 2015)**

The work on version 4 – especially the support of CityGML ADEs –
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

**Version 5 (under development)**

The next major version 5 of the 3DCityDB is intended to bring support
for CityGML 3.0. CityGML 3.0 itself is a major update of the CityGML
standard with many new features and capabilities, and is currently in
the process for adoption as OGC standard.

Support for CityGML 3.0 will require substantial rework of the
3DCityDB database schema, scripts and all tools. We are planning
to kick-off the development work in Q2 2021. Stay tuned on our GitHub page at
https://github.com/3dcitydb for early results and prototypes.
We are looking for feedback, discussions, and contributions from
the 3DCityDB community.