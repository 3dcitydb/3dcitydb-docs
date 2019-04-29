Feature overview
~~~~~~~~~~~~~~~~

Basically, the 3D web client has been developed by extending and
customizing the so-called Cesium Viewer which is a composite widget
shipped with Cesium and provides overall functionalities of a 3D globe
such as camera control, rendering geometries and materials, animation
etc. In addition, the Cesium Viewer contains a number of especially
attractive widgets and plugins providing functionalities like querying
of geocoding service, switching between different viewing modes (2D,
2.5D, and 3D view), and handling imagery and terrain layers, which are
commonly useful for a variety of GIS applications. In addition, starting
from version 1.6.0, the web client provides better support for mobile
devices, such as a more compact GUI layout as well as the ability to
interact with the web map in first-person view based on the user’s
location in real-time. All these functionalities along with the enhanced
features and functionalities developed for the 3D web client are
explained in more detail below.

|image188|

Figure 156: Relevant GUI components of the 3D web client

The 3D Globe [1] is a base Cesium widget that allows the user to
navigate through the Earth map by panning, moving, tilting, and rotating
the camera perspective using a mouse or touchscreen. In addition, the
camera perspective can also be controlled by means of the Navigation
Component [2] which is an open source Cesium plugin [12]_ and offers the
same navigation possibilities that can be achieved with mouse or
touchscreen. It consists of a group of widgets, namely a Navigator
widget for controlling the camera perspective, a North Arrows widget for
orienting the Earth map towards the north, and a Scale Bar for
estimating the distance between two points on the ground.

The Cesium Viewer provides an especially useful built-in Toolkit [3]
containing the widgets like Geocoder, HomeButton, GeolocationButton,
BaseLayerPicker, and NavigationHelpButton. The view panel of Geocoder
can be expanded by clicking on the button |image189| to display an input
filed into which the user can enter either an explicit position value in
the form of “\ *[longitude], [latitude]*\ ” or an address name to search
a particular location. After pressing the “Enter” key on the keyboard or
clicking on the button |image190|, the Geocoding process will be
performed using the Bing Maps Locations API according to the entered
location information. Once the target location has been found, the Earth
map will be automatically adjusted to the returned location and zoomed
to the bounding box with the best fit with the camera perspective. For
example, if you want to search the position (longitude = 11.56786,
Latitude = 48.14900) where the Technical University of Munich is, the
input field of the Geocoder can be filled with the text value of
“11.56786, 48.14900” and the result should look like the following
figure.

|image191|

Figure 157: Searching the main building of the Technical University of
Munich by using the Geocoder widget

The HomeButton |image192| helps the user to quickly reset the camera
perspective to the default status (cf. Figure 155). In addition, the
GeolocationButton |image193| provides some geolocation-based features
such as flying to the user’s current location on the 3D map and
displaying the first-person view in real-time on mobile devices, which
shall be explained in more details in Section 1.

In most GIS applications, the term *base layer* (or *basemap*) is
generally considered as a background layer on the map using, for
example, satellite imagery and terrain model, to help people to quickly
identify the locations and orientations from a certain camera
perspective. Per default, Cesium comes with a number of selectable
imagery layers provided by different mapping services, such as Bing
Maps, OpenStreetMap, ESRI Maps etc. In addition, a terrain layer
so-called *STK World Terrain*\  [13]_ is available for showing worldwide
3D elevation data with an average grid resolution of 30 meters. All
these base layers (imagery and terrain layers) can be controlled by the
BaseLayerPicker widget (cf. the following figure) which has a view panel
for listing all the available base layers represented by their names and
respective icons and allows the user to select the desired one. For
example, when an icon representing the OpenStreetMap is selected, a new
instance of the OpenStreetMap imagery layer will be created to replace
the imagery layer that is currently in use. Similarly, the terrain layer
can be independently selected and added to the Earth map to overlap with
the selected imagery layer.

|image194|

Figure 158: Per default available base layers listed in the
BaseLayerPicker widget

The last widget contained within the Cesium Toolkit [3] (cf. Figure 156)
is the so-called NavigationHelpButton for showing brief instructions on
how to navigate the Earth map with mouse (typically for desktop and
laptop PCs) and touchscreen (typically for smart phones and tablet PCs).
By clicking on the |image195| button, the corresponding view panel (cf.
the following figure) will be shown on the upper-right corner of the 3D
web client.

|image196|

Figure 159: The NavigationHelpButton widget showing the instructions for
navigating Earth map

The next widget is the so-called CreditContainer [4] (cf. Figure 156)
which displays a collection of credits with respect to the software and
data providers that have been involved in the development and use of the
3D web client. These credits mainly include the mapping services
(depending on the selected base layer, e.g. Bing Maps), the 3D
geo-visualization engine (Cesium Virtual Globe), and the development
provider of the 3D web client (3DCityDB), which are all represented with
their icons, descriptions, and hyperlinks referencing to their
respective homepages.

The majority of the functionalities specially provided by the 3D web
client are controlled by the Toolbox widget [5] (cf. Figure 156) which
is an extended module based on the Cesium Viewer for integrating and
controlling the user-provided data in different formats, namely KML/glTF
modes, thematic data (online spreadsheet), Web Map Service (WMS) data,
and digital terrain model (DTM) on the one hand. On the other hand, the
user interaction with 3D city models can also be aided by this Toolbox
widget which allows, for example, deselecting, shadowing, hiding and
showing 3D objects, as well as exploring them from different view
perspectives using third-party mapping services like Microsoft Bing Maps
with oblique view, Google Streetview, and a combined version (DualMaps).

.. note::
   Starting from September 2018, a Cesium ion API key or a Bing Maps API
   key is required in order to provide access to the Cesium World Terrain
   as well as the Bing Maps Services. These can be given as the parameter
   **ionToken=<your_ion_token>** and **bingToken=<your_bing_token>** in
   the client’s URL. If no valid token is present, Open Street Map shall
   be selected as the default imagery and Nominatim shall be activated as
   the default geocoder. For more information, please refer to:

-  https://cesium.com/legal/terms-of-service/

-  https://www.microsoft.com/en-us/maps/product/terms

-  https://www.openstreetmap.org/copyright/en

The visualization of the 3D city model with large data size often result
in significant performance issue in most 3D web applications. In order
to overcome this troublesome issue, a tiling strategy has been
implemented within the 3D web client to support for efficient displaying
of large pre-styled 3D visualization models in the form of tiled
datasets exported from the 3DCityDB by using the KML/COLLADA/glTF
Exporter. This tiling strategy utilizes the multi-threading capabilities
of HTML5, so that the time-costly operations such as parsing of multiple
3D objects can be delegated to a background thread running in parallel.
At the same time, for data layer, another thread monitors the
interactions with the virtual camera and takes care of determining which
the data tiles should be loaded and unloaded according to their current
visibility and the display size on the screen. Moreover, this tiling
strategy supports caching mechanism allowing the data tiles loaded from
an earlier computation to be temporarily stored in a cache, from which
the data tiles can be loaded and rendered much faster than reloading
them again from the remote server. Of course, a larger number of cached
data tiles will consume more memory and may cause a memory overflow of
the web browser. In order to avoid this, the 3D web client provides a
so-called Status Indicator widget [6] (cf. Figure 156) which can display
the real-time status of the amount of showed and cached data tiles and
can be used to help the user to conveniently monitor and control the
memory consumed by the 3D web client.

While streaming the tiled 3D visualization models, each data tile
requires at least an asynchronous HTTP (Hypertext Transfer Protocol)
request (AJAX) to fetch the corresponding KML/glTF files from the remote
data server. This server must support CORS (Cross-Origin Resource
Sharing) to get around the cross-domain restrictions.

.. note::
   Alternatively, the open specification Cesium 3D Tiles can also
   be employed to stream massive heterogeneous 3D geospatial
   datasets [14]_. This is supported in 3DCityDB Web Map Client version
   1.6.0 or later.

.. |image188| image:: media/image195.png
   :width: 6.17557in
   :height: 4.21538in

.. |image189| image:: media/image196.png
   :width: 0.18444in
   :height: 0.15678in

.. |image190| image:: media/image196.png
   :width: 0.18444in
   :height: 0.15678in

.. |image191| image:: media/image197.png
   :width: 4.2128in
   :height: 2.4564in

.. |image192| image:: media/image198.png
   :width: 0.18182in
   :height: 0.18768in

.. |image193| image:: media/image199.png
   :width: 0.18683in
   :height: 0.18898in

.. |image194| image:: media/image200.png
   :width: 6.29498in
   :height: 5.22917in

.. |image195| image:: media/image201.png
   :width: 0.15972in
   :height: 0.15972in

.. |image196| image:: media/image202.png
   :width: 6.29774in
   :height: 3.48542in
