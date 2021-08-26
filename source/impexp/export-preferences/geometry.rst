.. _impexp_export_preferences_geometry:

Geometry
^^^^^^^^

The Importer/Exporter can apply an affine coordinate transformation to all
geometry objects that are exported from the database. This option is disabled by default.

.. figure:: /media/impexp_export_preferences_geometry_fig.png
   :name: impexp_export_preferences_geometry_fig
   :align: center

   Export preferences – Geometry.

You define the affine transformation by entering the coefficients of the transformation matrix in
this preferences dialog (cf. :numref:`impexp_export_preferences_geometry_fig`). Please refer to
:numref:`impexp_import_preferences_geometry` for a general discussion on affine transformations and
the meaning of the coefficients.

The export operation applies the affine transformation to all coordinates
of all city objects. This also includes all matrices in the data like the 2x2 matrices of
GeoreferencedTextures (CityGML only), the 3x4 transformation matrices of TexCoordGen elements (CityGML only)
used for texture mapping and the 4x4 transformation matrices for ImplicitGeometries.

**Example:** For an ordinary translation of all city objects by 100
meters along the x-axis and 50 meters along the y-axis (assuming all
coordinate units are given in meters), you have to enter the coefficients of the
following transformation matrix:

.. math::

   {\overrightarrow{p}}^{'} = \begin{bmatrix}
   1 & 0 & 0 & 100 \\
   0 & 1 & 0 & 50 \\
   0 & 0 & 1 & 0 \\
   \end{bmatrix} \bullet \overrightarrow{p}