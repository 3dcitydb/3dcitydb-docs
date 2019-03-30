3DCityDB @ virtualcitySYSTEMS
=============================

|image226|\ virtualcitySYSTEMS [30]_ has successfully applied the 3D
City Database in customer projects worldwide and also funded its
development. With the Open Source database at the core,
virtualcitySYSTEMS also offers a *3D Spatial Data Infrastructure*
solution for the *management*, *distribution*, *maintenance* and
*visualization* of massive 3D geo data (see next page). As leading
developers of the 3D City Database joined the company,
virtualcitySYSTEMS now takes an active role in its development.
Moreover, virtualcitySYSTEMS offers a branded version of the 3D City
database called the *virtualcityDATABASE* to answer customer demands and
to provide support and maintenance.


virtualcityDATABASE
-------------------

The virtualcityDATABASE provides enhanced database functionality as well
as plugins for the Importer/Exporter tool that support workflows for
maintaining and updating the 3D city model content. Main features are:

-  **Integration of additional LoDs against existing city objects in the
   database**

..

   This plugin allows for integrating city objects from an external data
   source with existing city objects stored in the database. The
   candidate objects are identified with the database objects based on
   thematic and spatial checks. Therefore, data inconsistency can easily
   be spotted and analyzed before an import. If an integration is
   performed, exiting LoDs are replaced and newly introduced LoDs are
   attached to the existing objects. Moreover, appearance information
   can be integrated without replacing the geometry.

-  **Deletion of entire city objects or single LoDs representations**

..

   The 3D City Database provides a low-level API for deleting city
   objects. This API has been extended in the virtualcityDATABASE to
   also delete single LoDs of city objects. A graphical user dialog
   realized as a plugin for the Importer/Exporter allows users to easily
   delete city objects based on comprehensive thematic filter criteria.

-  **Adding material appearances for buildings**

..

   This plugin helps to define constant material information for
   building surfaces based on thematic properties (e.g., to colorize
   roofs according to their solar potential).

-  **Transactional Web Feature Service**

..

   Customers of the virtualcityDATABASE already benefit from an
   OGC-compliant WFS 2.0 implementation that supports transactions as
   well as comprehensive spatial and thematic queries using the OGC
   Filter Encoding standard.

The virtualcityDATABASE is fully compliant with the 3D City Database. If
features developed for the virtualcityDATABASE have gained enough
maturity, virtualcitySYSTEMS will introduce them to the Open Source 3D
City Database project (e.g. the WFS interface).


virtualcitySUITE – The 3D City Platform
---------------------------------------

The virtualcitySUITE is a modular *3D Spatial Data Infrastructure*
solution to store, manage, distribute and visualize 3D geo data. Core
components are the *virtualcityDATABASE* and its OGC WFS interface for
accessing and editing the data, the *virtualcityWAREHOUSE*, a data
distribution solution running on FME technology that enables users to
export 3D city model content from the virtualcityDATABASE into various
industry GIS and CAD formats, and the web-based authoring tool
*virtualcityPUBLISHER* for creating high-performance 3D web maps. Based
on the Open Source 3D City Database, the virtualcitySUITE allows for
building a 3D SDI platform for virtual 3D city models based on open
standards and interfaces.

|3DGDI_EN_2015|

Figure 188: Components of the virtualcitySUITE.

Our 3D web maps offer enhanced GIS functionality beyond pure 3D
visualization including 3D measurements, real-time shadows, WFS-based
thematic and spatial queries, POI integration, data exports through a
virtualcityWAREHOUSE interface, and integration of external WMS and WFS
data sources as well as pointcloud data and oblique imagery. The 3D web
maps are based on the Cesium WebGL virtual globe and therefore can be
displayed on modern web browsers and mobile devices such as tablets and
smartphones without the need for additional plugins.

|image228|

Figure 189: The Berlin 3D City Model consisting of more than 500,000
fully textured buildings is managed based on our virtualcitySUITE. The
Berlin Economic atlas shown above is a 3D web map application that
displays the entire city model and combines the 3D objects with business
and POI information, see
http://www.businesslocationcenter.de/wab/maps/main/.
