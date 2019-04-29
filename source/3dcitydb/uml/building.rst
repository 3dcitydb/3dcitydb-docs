Building model
^^^^^^^^^^^^^^

Buildings can be represented in five levels of detail (LoD0 to LoD4).
The building model allows the representation of simple buildings that
consist of only one component, as well as the representation of complex
relations between parts of a building, e.g. a building consisting of
three parts – a main house, a garage and an extension. The parts can
again consist of parts etc. The subclasses *Building* and *BuildingPart*
of \_\ *AbstractBuilding* enable these modelling options.

|image9|

Figure 6: Example of buildings consisting of one and two building parts
[Gröger et al., 2008]

In the case of a simple, one-piece house there is only one *Building*
which inherits all attributes and relations from *\_AbstractBuilding*
(cf. **Fehler! Verweisquelle konnte nicht gefunden werden.**). However,
such a *Building* can also comprise *BuildingParts* which likewise
inherit all properties from *\_AbstractBuilding*: the building’s class,
function (e.g. residential, public, or industry), usage, year of
construction, year of demolition, roof type, measured height, and the
number and individual heights of all its storeys above and below ground
(cf. Figure 7).

|image10|

Figure 7: UML diagram of *Building* model

Furthermore, *Addresses* can be assigned to *Buildings* or
*BuildingParts*. In particular, *BuildingParts* may again comprise
*BuildingParts* as components, because the composition relation is
inherited. This way a tree-like hierarchy can be created whose root
object is a *Building* and whose non-root nodes are *BuildingParts*. The
attribute values are generally filled in the lower hierarchy level,
because basically every part can have its own construction year and
function. However, the function can also be defined in the root of the
hierarchy and therefore span the whole building. The individual
*BuildingParts* within a *Building* must not penetrate each other and
must form a coherent object.

The geometric representation of an \_\ *AbstractBuilding* is
successively refined from LOD0 to LOD4. Therefore, a single building can
have multiple spatial representations in different levels of detail at
the same time by *Solid*, *MultiSurface*, and/or *MultiCurve* (cf.
Figure 7).

In LoD0, the building can be represented by horizontal, 3-dimentional
surfaces describing the footprint and the roof edge. In LoD1, a building
model consists of a geometric representation of the building volume.
Optionally, a *MultiCurve* representing the *TerrainIntersectionCurve*
can be specified. This geometric representation is refined in LoD2 by
additional *MultiSurface* and *MultiCurve* geometries, used for
modelling architectural details like a roof overhang, columns, or
antennas. In LoD2 and higher LoDs the outer facade of a building can
also be differentiated semantically by the classes \_\ *BoundarySurface*
and *BuildingInstallation*. A \_\ *BoundarySurface* is a part of the
building’s exterior shell with a special function like wall
(*WallSurface*), roof (*RoofSurface*), ground plate (*GroundSurface*),
or closing surface (*ClosureSurface*) as shown in Figure 8. Closure
surfaces can be used to virtually seal open buildings as for example
hangars, allowing e.g. volume calculation. The *BuildingInstallation*
class is used for building elements like balconies, chimneys, dormers,
or outer stairs, strongly affecting the outer appearance of a building.
A *BuildingInstallation* is used for the representation of chimneys,
stairs, balconies etc. and optionally has the attributes *class*,
*function,* and *usage*.

|image11|

Figure 8: Boundary surfaces

In LoD3, the openings in \_\ *BoundarySurface* objects (doors and
windows) can be represented as thematic objects. In LoD4, the highest
level of resolution, also the interior of a building, composed of
several rooms, is represented in the building model by the class *Room*.
The aggregation of rooms according to arbitrary, user-defined criteria
(e.g. for defining the rooms corresponding to a certain storey) is
achieved by employing the general grouping concept provided by CityGML.
Interior installations of a building, i.e. objects within a building
which (in contrast to furniture) cannot be moved, are represented by the
class *IntBuildingInstallation*. If an installation is attached to a
specific room (e.g. radiators or lamps), they are associated with the
*Room* class, otherwise (e.g. in case of rafters or pipes) with
*\_AbstractBuilding.* A *Room* may have the attributes *class*,
*function,* and *usage* referenced to external code lists. The *class*
attribute allows a classification of rooms with respect to the stated
function, e.g. commercial or private rooms, and occurs only once. The
*function* attribute is intended to express the main purpose of the
room, e.g. living room, kitchen. **The attribute usage can be used if
the object’s usage differs from its function.** Both attributes can
occur multiple times.

The visible surface of a room is represented geometrically as a *Solid*
or *MultiSurface*. Semantically, the surface can be structured into
specialised \_\ *BoundarySurfaces*, representing floor (*FloorSurface*),
ceiling (*CeilingSurface*), and interior walls (*InteriorWallSurface*)
(cf. Figure 8). Room furniture, like tables and chairs, can be
represented in the CityGML building model with the class
*BuildingFurniture*. A *BuildingFurniture* may have the attributes
*class*, *function,* and *usage*.

.. |image9| image:: media/image19.png
   :width: 5.0849in
   :height: 2.13945in

.. |image10| image:: media/image20.png
   :width: 6.27707in
   :height: 7.43396in

.. |image11| image:: media/image21.png
   :width: 5.78302in
   :height: 2.34483in
