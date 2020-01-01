.. _webmap_chapter:

3DCityDB-Web-Map-Client
=======================

Starting from version 3.3.0, the 3DCityDB software package comes with a
software package called **3DCityDB-Web-Map-Client** (in this chapter we
simply call it “3D web client”) acting as a web-based front-end for
high-performance 3D visualization and interactive exploration of
arbitrarily large semantic 3D city models. The 3D web client has been
developed based on the Cesium Virtual Globe, which is an open source
JavaScript library developed by Analytical Graphics, Inc. (AGI) [11]_.
It utilizes HTML5 and the Web Graphics Library (WebGL) as its core for
hardware acceleration and provides cross-platform functionalities like
displaying 3D graphic contents on the web without the needs of
additional plugins.

While developing the 3D web client, various extensions have been made to
the Cesium Virtual Globe in order to facilitate users to view and
explore 3D city models conveniently. The major one among those
extensions is that the KML/glTF models exported using the Import/Export
tool can now be directly visualized along with imagery and terrain
layers within a web browser using the 3D web client, which additionally
can link the KML/glTF models with table data exported using the
Spreadsheet Generator Plugin (SPSHG) and allows querying the thematic
data of every city object. With this newly introduced 3D web client, the
functionalities of the 3DCityDB now range from high-efficient storage
and management of virtual 3D city models according to the CityGML
standard up to high-performance visualization and exploration of them on
the web.

.. toctree::
   :maxdepth: 1

   system-requirements
   installation-config
   features
   online-spreadsheet
   wms-integration
   dtm-integration
   selection
   mobile-support

.. figure:: /media/image191.png
   :name: pic_3d_web_map_railway

   Screenshot showing the example of displaying different
   CityGML top-level features (building, bridge, tunnel, water, vegetation,
   transportation etc.) in glTF format in the 3D web client

