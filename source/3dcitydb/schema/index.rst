.. _citydb_schema_chapter:

Relational database schema
--------------------------

The simplified UML database model defined in the previous :numref:`chapter_citydb_UML_class_diagram`
has been mapped to a relational schema based on some general
mapping rules and conventions. The following sections discuss
these rules and present the relational schema in detail using
entity-relationship diagrams. Based on this work, the platform-specific
SQL scripts for setting up the 3D City Database on PostgreSQL and Oracle
have been derived automatically.

.. toctree::
   :maxdepth: 1

   mapping-rules
   core
   geometry
   appearance
   building
   bridge
   city-furniture
   generics
   land-use
   relief
   transportation
   tunnel
   vegetation
   water-body
   sequences
   managing-ade-schemas

The figures for the entity-relationship diagrams are taken from Oracle JDeveloper (v12.2.1),
which is used to model the database schema and extract SQL DDL
scripts automatically for Oracle databases. It is a freeware IDE by
Oracle and can be downloaded at
https://www.oracle.com/application-development/technologies/jdeveloper.html.

For PostgreSQL databases, the open source tool pgModeler (v0.8.2) is
used to maintain the schema. The source code is available on GitHub
at https://github.com/pgmodeler/pgmodeler. Pre-built binaries can be
purchased from the official website at https://www.pgmodeler.io/.

Starting from version 3.0.0 of the 3DCityDB, the corresponding schema
modelling projects are shipped with the release and can be edited by the
user to create customized SQL scripts. However, note that the 3DCityDB
Importer/Exporter tool only supports the default schema, unless it is not
reprogrammed against the user’s new database schema.