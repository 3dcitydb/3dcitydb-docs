3D City Database
================

This chapter gives an in-depth presentation and explanation of the relational schema of
the 3D City Database. In :numref:`chapter_citydb_UML_class_diagram`, it first discusses
the database design along the UML data model of CityGML and its mapping and adaptation to
a platform-independent conceptual model for the 3D City Database. This database design is
also realized as UML diagrams and forms the basis for the derivation of the
relational database schema for a specific database system. The resulting relational schema
is then discussed in :numref:`citydb_schema_chapter` together with the rules and conventions
applied in the mapping process. The relational schema itself is illustrated using
entity-relationship diagrams.

The remaining sections of this chapter are dedicated to different aspects of the
database-side implementation and work with the 3D City Database.

.. toctree::
   :maxdepth: 1

   uml/index
   schema/index
   define-crs
   multi-schema
   sproc/index
   docker
