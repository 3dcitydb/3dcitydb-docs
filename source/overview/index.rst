Overview
========

The aim of the 3D City Database is to provide a high-performance and scalable
datastore for virtual 3D City Models also known as Digital (Geo-)Twins. The
database schema implements and is fully compliant with the conceptual data
model of the OGC standard CityGML 2.0. This allows you to store, manage,
analyze and even visualize your 3D geodata based on an open and interoperable
standard rather than to seal it in proprietary data silos.

The 3D City Database and all bundled tools are free and open source software.
The database schema is available for the open source database PostgreSQL/PostGIS
and the commercial Oracle Spatial Database solution. Both database systems
are highly optimized, de-facto industry standards for storing and querying
3D spatial data. The geometry data types and spatial functions offered by both systems
follow common OGC and GIS standards. This way the data stored in the 3D City Database
can be easily accessed and consumed by both open source and commercial GIS tools
such as QGIS, ESRI ArcGIS or Safe Software's Feature Manipulation Engine (FME).

This chapter introduces the key aspects and main features of the 3D City Database
and provides a brief development history as well as acknowledgements to supporters
of the project. Contributions to the 3D City Database project are always welcome!

.. toctree::
   :maxdepth: 1

   introduction
   main-features
   history
   acknowledgements
   license