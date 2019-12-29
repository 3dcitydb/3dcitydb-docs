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
[FVFH1995]_.

.. figure:: ../../media/citydb_implicit_geometry_model.png
   :name: citydb_implicit_geometry_model

   Implicit Geometry model

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
