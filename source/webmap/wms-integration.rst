Handling imagery layers
~~~~~~~~~~~~~~~~~~~~~~~

Cesium supports adding additional imagery layer to the Earth map by
using the OGC compliant Web Map Service (WMS).
Starting from version ``2.0.0``, the 3D web client also supports 
Web Map Tile Service (WMTS). The 3D web client
provides a simple widget panel which allows the user to easily add and
remove any imagery layers. The widget panel (1) (marked in
the following figure) can be expanded and collapsed by clicking on the
``Imagery Layer`` button on the widget panel.

.. figure:: /media/3d_web_client_wms.png
   :name: 3d_web_client_wms
   :align: center

   The input panel (1) for adding a new imagery layer and the
   ``BaseLayerPicker`` widget (2) where the added imagery layers will be listed
   together with the per default available imagery layers

The parameters required for defining such imagery layers are described as follows:

+----------------------+------------------------------------------------------------------------------------------------+
| ``Type``             | ``WMS`` for Web Map Service or ``WMTS`` for Web Map Tile Service                               |
+----------------------+------------------------------------------------------------------------------------------------+
| ``URL (*)``          | The location of the dataset on the web                                                         |
+----------------------+------------------------------------------------------------------------------------------------+
| ``Name (*)``         | The name of the imagery layer to be displayed in the ``BaseLayerPicker`` panel (2)             |
+----------------------+------------------------------------------------------------------------------------------------+
| ``Sub-layers (*)``   | An imagery layer may contain one or more sublayers, whose names must be separated by comma     |
+----------------------+------------------------------------------------------------------------------------------------+
| ``Style``            | (Only for WMTS) The style identifier available in the imagery dataset                          |
+----------------------+------------------------------------------------------------------------------------------------+
| ``tileMatrixSetID``  | (Only for WMTS) The ``TileMatrixSet`` identifier listed in the imagery dataset                 |
+----------------------+------------------------------------------------------------------------------------------------+
| ``Icon URL``         | The URL of the icon used for representing this imagery layer in the ``BaseLayerPicker``        |
+----------------------+------------------------------------------------------------------------------------------------+
| ``Tooltip``          | A text to be displayed when the mouse pointer is hovering over the layer's icon                |
+----------------------+------------------------------------------------------------------------------------------------+
| ``Other parameters`` | Additional parameters can be specified as ``key=value`` pairs separated by the ``&`` character |
+----------------------+------------------------------------------------------------------------------------------------+
| ``Proxy URL``        | Location of the proxy server used for cross-domain issues, such as ``/proxy/``                 |
+----------------------+------------------------------------------------------------------------------------------------+

The ``Proxy URL`` parameter
helps the 3D web client to get around the cross-domain issue when
performing HTTP requests. Since most of the WMS/WMTS server do not support
CORS, a proxy running behind the 3D web client is required. If you use
the JavaScript-based HTTP server shipped with the 3D web client 
or if you are using the 3D web client hosted on the 3DCityDB server, you
do not need to change the default value, since there already exists a
built-in proxy running with the relative path ``/proxy/``. Otherwise, this
parameter value must be adjusted according to the path of the proxy in
use.

**Usage example:**

In this example, a WMS imagery layer provided by the
`Vorarlberg State Government <https://vorarlberg.at>`_
will be added to and displayed in the 3D web client.
The following parameter values should be entered into the corresponding
input fields:                    

+----------------------+----------------------------------------------------------------+
| ``Type``             | ``WMS``                                                        |
+----------------------+----------------------------------------------------------------+
| ``URL (*)``          | http://vogis.cnv.at/mapserver/mapserv                          |
+----------------------+----------------------------------------------------------------+
| ``Name (*)``         | ``Vorarlberg Aerial``                                          |
+----------------------+----------------------------------------------------------------+
| ``Sub-layers (*)``   | ``wi2015_20cm``                                                |
+----------------------+----------------------------------------------------------------+
| ``Icon URL``         | http://cdn.flaggenplatz.de/media/catalog/product/all/4489b.gif |
+----------------------+----------------------------------------------------------------+
| ``Tooltip``          | ``Vorarlberg Aerial Photography taken during the winter 2015`` |
+----------------------+----------------------------------------------------------------+
| ``Other parameters`` | ``map=i_luftbilder_r_wms.map``                                 |
+----------------------+----------------------------------------------------------------+
| ``Proxy URL``        | ``/proxy/``                                                    |
+----------------------+----------------------------------------------------------------+

.. figure:: /media/3d_web_client_wms_gui.png
   :name: 3d_web_client_wms_gui
   :align: center

   Example showing how to add a new WMS layer to the 3D web client

.. note:: 
   For your convenience, this example is readily available for access `here <https://www.3dcitydb.net/3dcitydb-web-map/2.0.0/3dwebclient/index.html?t=3DCityDB-Web-Map-Client&s=false&ts=0&la=47.241792&lo=9.744878&h=91437.295&hd=360&p=-90&r=0&&bm=imageryType%3Dwms%26url%3Dhttp%253A%252F%252Fvogis.cnv.at%252Fmapserver%252Fmapserv%26name%3DVorarlberg%2520Aerial%26iconUrl%3Dhttp%253A%252F%252Fcdn.flaggenplatz.de%252Fmedia%252Fcatalog%252Fproduct%252Fall%252F4489b.gif%26tooltip%3DVorarlberg%2520Aerial%2520Photography%2520taken%2520during%2520the%2520winter%25202015%26layers%3Dwi2015_20cm%26tileStyle%3D%26tileMatrixSetId%3D%26additionalParameters%3Dmap%253Di_luftbilder_r_wms.map%26proxyUrl%3D%252Fproxy%252F&sw=showOnStart%3Dfalse>`_.

As shown in the figure above, once the parameter settings have been
completed, the WMS layer can be loaded by clicking on the 
``Add / Update Imagery Layer`` button (3). 
Its icon image together with its label name (4)
will be listed on the ``BaseLayerPicker`` widget (2). You can use the
``Geocoder`` widget (5) to zoom the Earth map to the region of ``Vorarlberg``
state and check the added WMS layer. Clicking on the ``Remove imagery layer``
button (6) will remove the WMS layer and replace it with the default imagery layer 
that is the first item listed on the ``BaseLayerPicker`` widget (2).
