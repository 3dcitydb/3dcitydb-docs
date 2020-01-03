Data Modelling and Database Design
==================================

In this section the slightly simplified data model with respect to
CityGML is described at the conceptual level using UML class diagrams.
These diagrams form the basis for the implementation-dependent
realization of the model with a relational database system which is
presented in :doc:`database schema <schema/index>` section.
However, UML diagrams may also form the basis for other implementations
e.g. for the definition of an exchange format based on XML or GML. The
UML diagrams of the 3D city model are depicted in
:doc:`UML sub chapter <uml/index>`.

Simplification compared to CityGML 2.0.0
----------------------------------------

CityGML is a common information model for 3D urban objects and provides
a comprehensive and extensible representation of the objects. It is
explained in detail in the CityGML specification [GKCN2008]_, [GKNH2012]_
and [Kolb2009]_. An analysis of the previous versions of the 3D City
Database indicated that for the data collected and processed a less
complex schema is sufficient. Using a simplified schema usually allows
improving system performance. Therefore, the first task was related to
database design aspects with respect to adjusting the comprehensive
CityGML features.

As result a simplified database schema was generated, allowing an
optimized workflow and guaranteeing efficient processing time. The
related UML-diagrams were discussed and coordinated with the project
partners and translated into the relational schema. Based on this work
the SQL scripts for setting up the Oracle and PostgreSQL database
schema were generated.

.. note::

   All test CityGML datasets (versions 1.0.0 and 2.0.0) from the
   `CityGML homepage <http://www.citygml.org>`_
   (and others) can be stored and managed without restrictions
   with this simplified database schema.

Multiplicities of attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Attributes with a variable amount of occurrences (*) are substituted by
a data type enabling the storage of arbitrary values (e.g. data type
`String` with a predefined separator) or by an array with a predefined
amount of elements representing the number of objects that participate
in the association. This means that object attributes can be stored in
a single column.

Cardinalities and types of relationships
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

n:m relations require an additional table in the database. This table
consists of the primary keys of both elements’ tables which form a
composite primary key. If the relation can be restricted to a `1:n` or
`n:1` relationship the additional table can be avoided. Therefore, all
`n:m` relations in CityGML were checked for a more restrictive
definition. This results in simplified cardinalities and relations.

Simplified treatment of recursions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Some recursive relations are used in the CityGML data model. Recursive
database queries may cause high cost, especially if the amount of
recursive steps is unknown. In order to guarantee good performance,
implementation of recursive associations receive two additional columns
which contain the ID of the parent and of the root element. For example,
if all building parts related to a specific building are queried, only
those tuples containing the ID of the building as root element have to
be selected. Thus, typical queries concerning object geometry remain
high-performance.

Data type adaptation
~~~~~~~~~~~~~~~~~~~~

Data types specified in CityGML were substituted by data types which
allow an effective representation in the database. Strings for example
are used to represent code types and number vectors; GML geometry types
were changed to the database geometry data type. Matrices are stored
each one as String data type, with values listed in a row-major sequence
separated by spaces.

Project specific classes and class attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The 3D city database may contain some classes for representation of
project specific metadata, version control and attributes for
representation of additional project specific information. Since this
information is represented in the CityGML specification differently or
even not at all, appropriate classes and class attributes are added or
respectively adopted.

Simplified design of GML geometry classes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Spatial properties of features are represented by objects of GML3’s
geometry model based on the ISO 19107 standard *Spatial Schema*
[Herr2001]_, representing 3D geometry according to the well-known
Boundary Representation (B-Rep, cf. [FVFH1995]_). Actually only a subset
of the GML3 geometry package is used. Moreover, for 2D and 3D
surface-based geometry types a simpler but equally powerful model is
used: These geometries are stored as polygons, which are aggregated to
*MultiSurfaces*, *CompositeSurfaces*, *TriangulatedSurfaces*, *Solids*,
*MultiSolids*, as well as *CompositeSolids*.
