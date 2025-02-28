Handling digital terrain models
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Cesium offers the possibility of high-performance streaming and
rendering of Digital Terrain Models (DTM) for the realistic
representation of the Earth's surface. Cesium provides per default two
available terrain layers, which can be selected in the ``BaseLayerPicker``
(2) widget. The first one is the so-called ``WGS84 Ellipsoid`` (default
terrain layer) which approximates the Earth's surface using a smooth
ellipsoid surface with a constant height value of 0. The other one is
the so-called ``STK World Terrain`` using a worldwide 3D elevation
data with an average grid resolution of 30 meters.
However, starting from September 1st 2018,
this was replaced by ``Cesium World Terrain``, 
which requires a Cesium ion account.

For specific application cases, high-resolution Digital Terrain Models
might be required. For this case, the 3D web client provides a simple
widget to facilitate handling the terrain data that must be created in a
specific terrain format (``heightmap`` or ``quantized-mesh``) defined by
Cesium. There exists an open source software tool
`Cesium Terrain Builder <https://github.com/geo-data/cesium-terrain-builder>`_
for creating terrain data in ``heightmap`` format. The
created terrain data is generated in a hierarchical folder structure
according to the TMS tiling schema and can be easily published on the
web by uploading the terrain data files to a CORS-enabled web server.

The input panel (1) on the 3D web client for adding and removing terrain
layers can be expanded and collapsed by clicking on the ``Terrain Layer`` button.


.. figure:: /media/3d_web_client_dtm_gui.png
   :name: 3d_web_client_dtm_gui
   :align: center

   The input panel (1) for adding a new terrain layer and the
   ``BaseLayerPicker`` widget (2) where the added terrain layers will be listed
   together with the per default available base layers

For adding a new terrain layer, the input fields ``URL (*)``,
``Name (*)``, ``Icon URL`` and ``Tooltip`` in the input panel (1) are required,
which describe the location on the web, the name, URL of an icon image and a short tooltip of the terrain layer, 
respectively. When a terrain layer has been loaded, its icon image
together with its label name will be listed in the ``BaseLayerPicker``
panel (2). The tooltip will automatically appear when the mouse is moved
over the respective icon image.

**Usage example**

In this example, a high-resolution (0.5m) Digital Terrain Model provided
by the Vorarlberg State Government will be added to the 3D web client.
This terrain data was created in ``heightmap`` format using the open
source tool ``Cesium Terrain Builder``. Here, the following parameter
values should be entered into the corresponding input fields:

+--------------+-----------------------------------------------------------------------------------+
| ``URL (*)``  | https://www.3dcitydb.org/3dcitydb/fileadmin/mydata/Vorarlberg_Demo/Vorarlberg_DTM |
+--------------+-----------------------------------------------------------------------------------+
| ``Name (*)`` | ``Vorarlberg DTM``                                                                |
+--------------+-----------------------------------------------------------------------------------+
| ``Icon URL`` | https://cdn.flaggenplatz.de/media/catalog/product/all/4489b.gif                   |
+--------------+-----------------------------------------------------------------------------------+
| ``Tooltip``  | ``Digital Terrain Model of Vorarlberg``                                           |
+--------------+-----------------------------------------------------------------------------------+                          

.. figure:: /media/3d_web_client_dtm_gui_numbers.png
   :name: 3d_web_client_dtm_gui_numbers
   :align: center

   Example showing how to add a new terrain layer to the 3D web client

.. note:: 
   For your convenience, this example is readily available for access `here <https://www.3dcitydb.net/3dcitydb-web-map/2.0.0/3dwebclient/index.html?t=3DCityDB-Web-Map-Client&s=false&ts=0&la=47.250442&lo=9.863893&h=2078.724&hd=271.86&p=-1.02&r=0.01&&bm=imageryType%3Dwms%26url%3Dhttp%253A%252F%252Fvogis.cnv.at%252Fmapserver%252Fmapserv%26name%3DVorarlberg%2520Aerial%26iconUrl%3Dhttp%253A%252F%252Fcdn.flaggenplatz.de%252Fmedia%252Fcatalog%252Fproduct%252Fall%252F4489b.gif%26tooltip%3DVorarlberg%2520Aerial%2520Photography%2520taken%2520during%2520the%2520winter%25202015%26layers%3Dwi2015_20cm%26tileStyle%3D%26tileMatrixSetId%3D%26additionalParameters%3Dmap%253Di_luftbilder_r_wms.map%26proxyUrl%3D%252Fproxy%252F&tr=name%3DVorarlberg%2520DTM%26iconUrl%3Dhttps%253A%252F%252Fcdn.flaggenplatz.de%252Fmedia%252Fcatalog%252Fproduct%252Fall%252F4489b.gif%26tooltip%3DDigital%2520Terrain%2520Model%2520of%2520Vorarlberg%26url%3Dhttps%253A%252F%252Fwww.3dcitydb.org%252F3dcitydb%252Ffileadmin%252Fmydata%252FVorarlberg_Demo%252FVorarlberg_DTM&sw=showOnStart%3Dfalse>`_.

As shown in the figure above, once the parameter settings have been
completed, the terrain layer can be loaded by clicking on the 
``Add / Update Terrain Layer`` button (3) and its icon image together with its label
name (4) will be listed on the ``BaseLayerPicker`` widget. You can use the
``Geocoder`` widget (5) to zoom the Earth map to the region of Vorarlberg
state and check the loaded terrain data. Clicking on the 
``Remove terrain layer`` button (6), the terrain layer will be removed and substituted
with the default ``WGS84 Ellipsoid`` terrain layer.
