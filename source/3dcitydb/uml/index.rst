.. _chapter_citydb_UML_class_diagram:

UML database design
-------------------

CityGML is a common information model for 3D urban objects and provides
a comprehensive and extensible representation of the objects. It is
explained in detail in the CityGML specification [GKNH2012]_, [GKCN2008]_
and [Kolb2009]_. Most thematic
classes are (transitively) derived from the basic classes *Feature* and
*FeatureCollection*, the basic notions defined in ISO 19109 and GML3 for
the representation of features and their aggregations. Features contain
spatial as well as non-spatial attributes, which are mapped to GML3
feature properties with corresponding data types. Geometric properties
are represented as associations to the geometry classes described in
:numref:`citydb_geometric-topological_model`. The thematic model also comprises different types of
interrelationships between Feature classes like aggregations,
generalizations, and associations.

The aim of the explicit modelling is to reach a high degree of semantic
interoperability between different applications. By specifying the
thematic concepts and their semantics along with their mapping to UML
and GML3, different applications can rely on a well-defined set of
*Feature* types, attributes, and data types with a standardised meaning
or interpretation. In order to allow also for the exchange of objects
and/or attributes that are not explicitly modelled in CityGML, the
concepts of *GenericCityObjects* and *GenericAttributes* have been
introduced.

This chapter discusses the mapping of the CityGML data model to
a general database design for the 3D City Database on a conceptual level
using UML diagrams. The following pages cite several parts of the
CityGML specification which are necessary for a better understanding.
Main focus is put on explaining the simplifications and customizations of
the CityGML data model and the resulting differences to the 3D
City Database design. Design decisions in the model are explicitly
visualised within the UML diagrams.

.. note::
   For intuitive understanding, classes that are merged to a single
   table in the relational schema are shown as orange blocks in the UML
   diagrams. n:m relations between UML classes that require an additional
   table are represented as green blocks.

.. toctree::
   :maxdepth: 1

   citygml-simplification
   core
   geometry
   appearance
   building
   bridge
   city-furniture
   generic-city-object
   land-use
   relief
   transportation
   tunnel
   vegetation
   water-body


