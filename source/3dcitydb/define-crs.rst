.. _citydb_crs_definition_chapter:

Definition of the CRS for a 3D City Database instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The definition of the CRS of a 3D City Database instance consists of two
components: 1) a valid *Spatial Reference Identifier* (SRID, typically
the EPSG code) and 2) an OGC GML conformant *definition identifier* *for
the CRS*. Both components are defined during the database setup (see
:numref:`first_step_3dcitydb_installation_oracle` and
:numref:`first_step_3dcitydb_installation_postgis`) and
are further stored in the table DATABASE_SRS (see :numref:`citydb_schema_metadata_diagram`).

The SRID is an integer value key pointing to spatial reference
information within Oracle’s MDSYS.CS_SRS table or PostGIS’
SPATIAL_REF_SYS table. Both DBMSs are shipped with a large number of
predefined spatial reference systems. **At setup time, the SRID chosen
as default value for the 3D City Database instance must already exist in
the mentioned tables.**

The GML conformant **CRS definition identifier** is used as value for
the gml:srsName attribute on GML geometry elements when exporting
database contents to CityGML instance documents. It should follow the
OGC recommendation for the Universal Resource Name (URN) encoding of
CRSs given in the **OGC Best Practice Paper Definition identifier URNs
in OGC namespace** [Whit2009]_. At setup time, please make sure to
provide a URN value which corresponds to the spatial reference system
identified by the default SRID of the database instance. Since CityGML
is a 3D standard, the URN encoding **shall** **always represent a
three-dimensional CRS** which, for example, can be denoted as compound
coordinate reference systems [Whit2009]_. The general syntax of a
URN encoding for a compound reference system is as follows:

.. code-block::

   urn:ogc:def:crs,crs:authority:version:code,crs:authority:version:code

Authority, version, and code depend on the information authority
providing the CRS definition (e.g. EPSG or OGC). The following example
shows a possible combination of an SRID (here referring to a 2D CRS) and
CRS URN encoding (3D) to set up an instance of the 3D City Database:

.. code-block::

   SRID: 31466
   URN: urn:ogc:def:crs,crs:EPSG:7.7:31466,crs:EPSG:7.7:5783

The example SRID is referencing a Projected CRS defined by EPSG (DHDN /
3-degree Gauss-Krüger zone 2; used in the western part of Germany;
EPSG-Code: 31466). The URN encodes a compound coordinate reference
system which adds a Vertical CRS as height reference (DHHN92 height,
EPSG-Code: 5783).
