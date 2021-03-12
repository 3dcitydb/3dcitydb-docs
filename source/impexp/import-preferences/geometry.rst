.. _impexp_import_preferences_geometry:

Geometry
^^^^^^^^

Before importing the city objects into the 3D City Database, the
Importer/Exporter can apply an affine coordinate transformation to all
geometry objects. This option is disabled by default though.

.. figure:: /media/impexp_import_preferences_geometry_fig.png
   :name: impexp_import_preferences_geometry_fig
   :align: center

   Import preferences – Geometry.

An affine transformation (cf. [Weis2015]_) is any transformation that preserves
collinearity (i.e., points initially lying on a line still lie on a line
after transformation) and ratios of distances (e.g., the midpoint of a
line segment remains the midpoint after transformation). It will move
lines into lines, polylines into polylines and polygons into polygons
while preserving all their intersection properties. Geometric
contraction, expansion, dilation, reflection, rotation, skewing,
similarity transformations, spiral similarities, and translation are all
affine transformations, as are their combinations.

The affine transformation is defined as the result of the multiplication
of the original coordinate vectors by a matrix plus the addition of a
translation vector.

.. math:: {\overrightarrow{p}}^{'} = A \bullet \overrightarrow{p} + \overrightarrow{b}

In matrix form using homogenous coordinates:

.. math::

   \begin{bmatrix}
   x^{'} \\
   y^{'} \\
   z^{'} \\
   \end{bmatrix} = \begin{bmatrix}
   m_{11} & m_{12} & m_{13} & m_{14} \\
   m_{21} & m_{22} & m_{23} & m_{24} \\
   m_{31} & m_{32} & m_{33} & m_{34} \\
   \end{bmatrix} \bullet \begin{bmatrix}
   x \\
   y \\
   z \\
   1 \\
   \end{bmatrix}

The coefficients of this matrix and translation vector can be entered in
this preferences dialog (cf. :numref:`impexp_import_preferences_geometry_fig`).
The first three columns define
any linear transformation; the fourth column contains the translation
vector. The affine transformation does neither affect the dimensionality
nor the associated reference system of the geometry object, but only
changes its coordinate values. It is applied the same to all coordinates
in all objects in the input file. This also includes all
matrices in the data like the 2x2 matrices of GeoreferencedTextures (CityGML only),
the 3x4 transformation matrices of TexCoordGen elements (CityGML only) used for texture
mapping and the 4x4 transformation matrices for ImplicitGeometries.

.. caution::
   An affine transformation cannot be undone or reversed after the
   import using the Importer/Exporter.

Two elementary affine transformations are predefined: 1) *Identity
matrix* (leave all geometry coordinates unchanged), which serves as an
explanatory example of how values in the matrix should be set, and 2)
*Swap X/Y*, which exchanges the values of *x* and *y* coordinates in all
geometries (and thus performs a 90 degree rotation around the z axis).
The latter is very helpful in correcting CityGML datasets that have
northing and easting values in wrong order.

**Example:** For an ordinary translation of all city objects by 100
meters along the x-axis and 50 meters along the y-axis (assuming all
coordinate units are given in meters), the *identity matrix* must be
applied together with the translation values set as coefficients in the
translation vector:

.. math::

   {\overrightarrow{p}}^{'} = \begin{bmatrix}
   1 & 0 & 0 & 100 \\
   0 & 1 & 0 & 50 \\
   0 & 0 & 1 & 0 \\
   \end{bmatrix} \bullet \overrightarrow{p}