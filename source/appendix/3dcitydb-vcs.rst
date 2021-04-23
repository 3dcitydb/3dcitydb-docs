.. _appendix_3dcitydb_vcs_chapter:

3DCityDB @ Virtual City Systems
===============================

`Virtual City Systems <https://vc.systems/>`_
has successfully applied the 3D
City Database in customer projects worldwide and also funded its
development. With the Open Source database at the core,
Virtual City Systems also offers a *3D Spatial Data Infrastructure*
solution for the *management*, *distribution*, *maintenance* and
*visualization* of massive 3D geo data. With core
developers of the 3D City Database project having joined the company,
Virtual City Systems now takes a leading role in its development.
Moreover, Virtual City Systems offers a branded version of the 3D City
database called the `VC Database <https://vc.systems/en/products/vc-database/>`_
to answer customer demands and to provide support and maintenance.

VC Database
-----------

The `VC Database <https://vc.systems/en/products/vc-database/>`_
provides enhanced database functionality as well
as plugins for the Importer/Exporter tool that support workflows for
maintaining and updating the 3D city model content. Main features are:

-  | **Data continuation and object histories**
   | The VC Database offers tools and additional database structures to manage
     updates and multiple versions of city objects in the database as well as to
     automatically track changes of city objects. Both aspects are relevant in the
     context of the continuation of city objects and entire city models. The VC
     Database offers the five operations ``Insert``, ``Delete``, ``Terminate``,
     ``Replace``, and ``Update`` that enable you to easily keep your 3D city model
     up-to-date and to get access to previous states of the model.

-  | **Integration of additional LoDs against existing city objects in the database**
   | This plugin allows for integrating city objects from an external data
     source with existing city objects stored in the database. The
     candidate objects are identified with the database objects based on
     thematic and spatial checks. Therefore, data inconsistency can easily
     be spotted and analyzed before an import. If an integration is
     performed, exiting LoDs are replaced and newly introduced LoDs are
     attached to the existing objects. Moreover, appearance information
     can be integrated without replacing the geometry.

-  | **Deletion and termination of city objects**
   | The VC Database provides a user-friendly Importer/Exporter plugin
     for deleting and terminating city objects using spatial and thematic
     filter criteria as well as delete lists.

-  | **Transactional Web Feature Service**
   | Customers of the VC Database already benefit from an
     OGC-compliant WFS 2.0 implementation that supports transactions as
     well as comprehensive spatial and thematic queries using the OGC
     Filter Encoding standard.

The VC Database is fully compliant with the 3D City Database. If
features developed for the VC Database have gained enough
maturity, Virtual City Systems will introduce them to the open source 3D
City Database project (e.g. the WFS interface).


VC Suite – The Digital Geo-Twin Platform
----------------------------------------

The VC Suite is a modular *3D Spatial Data Infrastructure*
solution to store, manage, distribute and visualize 3D geo data. Core
components are the *VC Database* and its OGC WFS interface for
accessing and editing the data, the `VC Warehouse <https://vc.systems/en/products/vc-warehouse/>`_, a data
exchange solution running on FME technology that enables users to
export 3D city model content from the VC Database into various
industry GIS and CAD formats, and the web-based authoring tool
`VC Publisher <https://vc.systems/en/products/vc-publisher/>`_
for creating high-performance 3D web maps. Based
on the Open Source 3D City Database, the VC Suite allows for
building a 3D SDI platform for Digital Geo-Twins based on open
standards and interfaces.

.. figure:: /media/appendix_vc_suite_components_fig.png

   Components of the VC Suite.

Our `VC Map <https://vc.systems/en/products/vc-map/>`_ web mapping technology
offers enhanced GIS functionality beyond pure
visualization such as measurements, real-time shadows, viewshed analysis, WFS-based
thematic and spatial queries, POI integration, data exports through a
VC Warehouse interface, and integration of external data services and sources
(e.g., WMS, WFS, GeoJSON, vector tiles) as well as meshes, point clouds and oblique imagery.
The 3D web maps are based on the Cesium WebGL virtual globe and therefore can be
displayed on modern web browsers and mobile devices such as tablets and
smartphones without the need for additional plugins.

.. figure:: /media/appendix_vcmap_berlin_3dcitymodel_fig.png

   The Berlin 3D City Model is managed based on our VC Suite. The
   Berlin Economic atlas shown above is a VC Map application that
   displays the entire city model and combines the 3D objects with business
   and POI information, see https://www.businesslocationcenter.de/wab/maps/main/#/.