Thematic model
~~~~~~~~~~~~~~

The thematic model consists of the class definitions for the most
important types of objects within virtual 3D city models. Most thematic
classes are (transitively) derived from the basic classes *Feature* and
*FeatureCollection*, the basic notions defined in ISO 19109 and GML3 for
the representation of features and their aggregations. Features contain
spatial as well as non-spatial attributes, which are mapped to GML3
feature properties with corresponding data types. Geometric properties
are represented as associations to the geometry classes described in
:numref:`citydb_geometric-topological_model` The thematic model also comprises different types of
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

.. toctree::
   :maxdepth: 1

  core
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