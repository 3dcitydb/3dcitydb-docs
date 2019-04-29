Geometric-topological Model
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The geometry model of CityGML consists of primitives, which may be
combined to form complexes, composite geometries or aggregates. A
zero-dimensional object is modelled as a *Point*, a one-dimensional as a
\_\ *Curve.* A curve is restricted to be a straight line, thus only the
GML3 class *LineString* is used.

Combined geometries can be aggregates, complexes or composites of
primitives (see illustration in figure 1). In an *Aggregate*, the
spatial relationship between components is not restricted. They may be
disjoint, overlapping, touching, or disconnected. GML3 provides a
special aggregate for each dimension, a *MultiPoint*, a *MultiCurve, a
MultiSurface* or a *MultiSolid*. In contrast to aggregates, a *Complex*
is topologically structured: its parts must be disjoint, must not
overlap and are allowed to touch, at most, at their boundaries or share
parts of their boundaries. A *Composite* is a special complex provided
by GML3. It can only contain elements of the same dimension. Its
elements must be disjoint as well, but they must be topologically
connected along their boundaries. *A Composite* can be a
*CompositeSolid,* a *CompositeSurface, or CompositeCurve*.

|image4|

Figure 1: Different types of aggregated geometries [Gröger et al., 2012]

The modelling of two-dimensional and three-dimensional geometry types is
handled in a simplified way. All surface-based geometries are stored as
polygons, which are aggregated to *MultiSurfaces*, *CompositeSurfaces*,
*TriangulatedSurfaces*, *Solids*, *MultiSolids*, as well as
*CompositeSolids* accordingly. This simplification substitutes the more
complex representation used for those GML geometry classes in grey
blocks in Figure 2. Mapping the UML diagram to the relational schema now
requires only one table (SURFACE_GEOMETRY), which is explained in
chapter 2.3.3.3.

|image5|

| Figure 2: Geometrical-topographical model.
| For simplification the geometry classes in the grey block are
  substituted by the construct in the orange block

In order to implement topology, CityGML uses the XML concept of *XLinks*
provided by GML. Each geometry object that should be shared by different
geometric aggregates or different thematic features is assigned a unique
identifier, which may be referenced by a GML geometry property using a
*href* attribute. The XLink topology is simple and flexible and nearly
as powerful as the explicit GML3 topology model. However, a disadvantage
of the XLink topology is that navigation between topologically connected
objects can only be performed in one direction (from an aggregate to its
components), not (immediately) bidirectional, as it is the case for
GML’s built-in topology.

Implicit Geometry
~~~~~~~~~~~~~~~~~

The concept of implicit geometries is an enhancement of the GML3
geometry model.

An implicit geometry is a geometric object, where the shape is stored
only once as a prototypical geometry, for example a tree or other
vegetation objects, a traffic light or traffic sign. This prototypic
geometry object is re-used or referenced many times, wherever the
corresponding feature occurs in the 3D city model. Each occurrence is
represented by a link to the prototypic shape geometry (in a local
Cartesian coordinate system), by a transformation matrix that is
multiplied with each 3D coordinate of the prototype, and by an anchor
point denoting the base point of the object in the world coordinate
reference system. The concept of implicit geometries is similar to the
well-known concept of **primitive instancing** used for the
representation of **scene graphs** in the field of computer graphics
[Foley et al. 1995].

|image6|

Figure 3: Implicit Geometry model

Implicit geometries may be applied to features from different thematic
fields in order to geometrically represent the features within a
specific level of detail (LOD). Thus, each CityGML thematic extension
module (like *Building*, *Bridge*, and *Tunnel* etc.) may define spatial
properties providing implicit geometries for its thematic classes.

The shape of an implicit geometry can be represented in an external file
with a proprietary format, e.g. a VRML file, a DXF file, or a 3D Studio
MAX file. The reference to the implicit geometry can be specified by an
URI pointing to a local or remote file, or even to an appropriate web
service. Alternatively, a GML3 geometry object can define the shape.
This has the advantage that it can be stored or exchanged inline within
the CityGML dataset. Typically, the shape of the geometry is defined in
a local coordinate system where the origin lies within or near to the
object’s extent. If the shape is referenced by an URI, also the MIME
type of the denoted object has to be specified (e.g. “model/vrml” for
VRML models or “model/x3d+xml” for X3D models).

The implicit representation of 3D object geometry has some advantages
compared to the explicit modelling, which represents the objects using
absolute world coordinates. It is more space-efficient, and thus more
extensive scenes can be stored or handled by a system. The visualization
is accelerated since 3D graphics hardware supports the scene graph
concept. Furthermore, the usage of different shape versions of objects
is facilitated, e.g. different seasons, since only the library objects
have to be exchanged.

.. |image4| image:: media/image14.png
   :width: 6.10416in
   :height: 2.16666in

.. |image5| image:: media/image15.png
   :width: 6.26702in
   :height: 5.94783in

.. |image6| image:: media/image16.png
   :width: 6.29931in
   :height: 1.16319in
