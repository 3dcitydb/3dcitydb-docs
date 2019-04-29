Vegetation Model
^^^^^^^^^^^^^^^^

The vegetation model of CityGML distinguishes between solitary
vegetation objects like trees and vegetation areas, which represent
biotopes like forests or other plant communities. Single vegetation
objects are modelled by the class *SolitaryVegetationObject*, while for
areas filled with specific vegetation the class *PlantCover* is used.

|vegetation1|

Figure 23: Image illustrates objects of the vegetation model

(from: [Gröger et al., 2008])

The geometry representation of a *PlantCover* feature may be a
*MultiSurface* or a *MultiSolid*, depending on the vertical extent of
the vegetation. For example, regarding forests, a *MultiSolid*
representation might be more appropriate (cf. Figure 23).

The UML diagram of the vegetation model is depicted in Figure 24. A
*SolitaryVegetation­Object* may have the attributes *class* (e.g. tree,
bush, grass), *species* (species’ name, e.g. Abies alba), *usage*, and
*function* (e.g. botanical monument)\ *, height,* *trunkDiameter* and
*crownDiameter*. A *PlantCover* feature may have the attributes *class*
(plant community)\ *, usage, function* (e.g. national forest) and
*averageHeight*. Since both *SolitaryVegetationObject* and *PlantCover*
are *CityObjects*, they inherit all attributes of a city object, in
particular its name (*gml:name)* and an *ExternalReference* to a
corresponding object in an external information system, which may
contain botanical information from public environmental agencies.

|image27|

Figure 24: Vegetation Model

The geometry of a *SolitaryVegetationObject* may be defined in LoD 1-4
by absolute coordinates, or prototypically by an *ImplicitGeometry*.
Season dependent appearances may be mapped using *ImplicitGeometries.*
For visualisation purposes, only the content of the library object
defining the object’s shape and appearance has to be swapped.

A *SolitaryVegetationObject* or a *PlantCover* may have a different
geometry in each LoD. Whereas a *SolitaryVegetationObject* is associated
with the \_\ *Geometry* class representing an arbitrary GML geometry (by
the relation *lodXGeometry*), a *PlantCover* is restricted to be either
a *MultiSolid* or a *MultiSurface*.

.. |vegetation1| image:: media/image36.png
   :width: 3.6in
   :height: 2.7in

.. |image27| image:: media/image37.png
   :width: 6.3in
   :height: 2.39583in
