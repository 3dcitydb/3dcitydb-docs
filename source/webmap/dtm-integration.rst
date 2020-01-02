Handling Digital Terrain Models
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Cesium offers the possibility of high-performance streaming and
rendering of Digital Terrain Models (DTM) for the realistic
representation of the Earth’s surface. Cesium provides per default two
available terrain layers, which can be selected in the *BaseLayerPicker*
[2] widget. The first one is the so-called *WGS84 Ellipsoid* (default
terrain layer) which approximates the Earth’s surface using a smooth
ellipsoid surface with a constant height value of 0. The other one is
the so-called *STK World Terrain* (Replaced by Cesium World Terrain
starting from September 1st 2018) using a worldwide 3D elevation
data with an average grid resolution of 30 meters, which is sufficient
in many use cases.

For specific application cases, high-resolution Digital Terrain Models
might be required. For this case, the 3D web client provides a simple
widget to facilitate handling the terrain data that must be created in a
specific terrain format (*heightmap* or *quantized-mesh*) defined by
Cesium. There exists an open source software tool
`Cesium Terrain Builder <https://github.com/geo-data/cesium-terrain-builder>`_
for creating terrain data in *heightmap* format. The
created terrain data is generated in a hierarchical folder structure
according to the TMS tiling schema and can be easily published on the
web by uploading the terrain data files to a CORS-enabled web server.

The input panel [1] on the 3D web client for adding and removing terrain
layers can be expanded and collapsed by clicking on the *Add
Terrain-Layer* button.


.. figure:: /media/3d_web_client_dtm_gui.png
    :name: 3d_web_client_dtm_gui

    The input panel [1] for adding a new terrain layer and the
    BaseLayerPicker widget [2] where the added terrain layers will be listed
    together with the per default available base layers

For adding a new terrain layer, the input fields *name(*)*,
*iconUrl(*)*, and *tooltip(*)* in the input panel [1] have to be filled
with a proper label name, an URL of an icon image, and a short tooltip
respectively. When a terrain layer has been loaded, its icon image
together with its label name will be listed in the *BaseLayerPicker*
panel [2]. The tooltip will automatically appear when the mouse is moved
over the respective icon image. The *url* parameter points to the URL of
the web server folder where the terrain data are stored.

**Usage example**

In this example, a high-resolution (0.5m) Digital Terrain Model provided
by the Vorarlberg State Government will be added to the 3D web client.
This terrain data was created in *heightmap* format using the open
source tool *Cesium Terrain Builder*. Here, the following parameter
values should be entered into the corresponding input fields:

-  **name**:                           Vorarlberg_DTM
-  **iconUrl**:                        https://cdn.flaggenplatz.de/media/catalog/product/all/4489b.gif
-  **tootip**:                          Digital Terrain Model of Vorarlberg
-  **url**:                             https://www.3dcitydb.org/3dcitydb/fileadmin/mydata/Vorarlberg_Demo/Vorarlberg_DTM

.. figure:: /media/3d_web_client_dtm_gui_numbers.png
   :name: 3d_web_client_dtm_gui_numbers

    Example showing how to add a new terrain layer to the 3D web client

As shown in the figure above, once the parameter settings have been
completed, the terrain layer can be loaded by clicking on the *Add
Terrain layer* button [3] and its icon image together with its label
name [4] will be listed on the *BaseLayerPicker* widget. You can use the
*Geocoder* widget [5] to zoom the Earth map to the region of Vorarlberg
state and check the loaded terrain data. Clicking on the *Remove Terrain
layer* button [6], the terrain layer will be removed and substituted
with the *WGS84 Ellipsoid* terrain layer.
