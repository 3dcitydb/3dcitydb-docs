.. _citydb_crs_definition_chapter:

Definition of the CRS for a 3D City Database instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The definition of the CRS of a 3D City Database instance consists of two
components: 1) a valid *Spatial Reference Identifier* (SRID, typically
the EPSG code) and 2) an OGC GML conformant *definition identifier* *for
the CRS*. Both components are defined during the database setup (see
:numref:`3dcitydb_setup_schema_chapter`) and
are further stored in the table DATABASE_SRS (see :numref:`citydb_schema_metadata_diagram`).

The **SRID** is an integer value key pointing to spatial reference
information within the SPATIAL_REF_SYS table (PostGIS) or the
MDSYS.CS_SRS table (Oracle). Both DBMSs are shipped with a large number of
predefined spatial reference systems.

.. note::
  When defining the default SRID during setup of the 3D City Database instance,
  the chosen value must already exist in the mentioned tables.

The GML conformant **CRS definition identifier** should follow the
OGC recommendation for the Universal Resource Name (URN) encoding of
CRSs given in the **OGC Best Practice Paper Definition identifier URNs
in OGC namespace** [Whit2009]_. At setup time, please make sure to
provide a URN value which corresponds to the spatial reference system
identified by the default SRID of the database instance. Since CityGML
is a 3D standard, the URN encoding **should always represent a
three-dimensional CRS**.

.. note::
  The CRS definition identifier is used as value for the ``gml:srsName`` attribute
  on GML geometry elements when exporting data in CityGML format. Software
  consuming the exported data will rely on this information to be able to automatically
  apply the correct spatial reference. So please make sure that the CRS
  identifier is correct. The identifier is, however, neither required nor
  evaluated when executing spatial operations inside the 3DCityDB itself.

An identifier for a three-dimensional CRS can, for example, be denoted as compound
coordinate reference systems according to [Whit2009]_. The general syntax of a
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

.. note::
  The 3DCityDB is shipped with a database script that allows you to change the SRID and/or the
  CRS definition identifier at any time for a given 3DCityDB instance
  (see :numref:`citydb_sproc_srs_chapter`).
  This functionality is helpful, for instance, in case the 3DCityDB was set up with a
  wrong SRID by mistake that does not match the imported data. You can
  quickly change the SRID so that spatial functions of the database
  work correctly. Or you can even use this functionality to reproject an
  entire 3DCityDB instance to a new CRS.

  The Importer/Exporter also offers a convenient way to execute this
  script via its graphical user interface (see :numref:`impexp-db-change-crs`).