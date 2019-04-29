Mobile Support Extension
~~~~~~~~~~~~~~~~~~~~~~~~

Starting from version 1.6.0, the 3DCityDB-Web-Map-Client is equipped
with an extension that provides better support for mobile devices. The
extension comes with a built-in mobile detector, which can automatically
detect and adjust the client's behaviors accordingly to whether the 3D
web client is operating on a mobile device. The extension has been
tested on several smartphones and tablets running Android and iOS.

Some of the most important mobile features enabled by this extension are
listed as follows:

1. **A more lightweight graphical user interface**

..

   In order to make the best use of the limited screen real-estate
   available on mobile devices, some elements are removed or hidden from
   the web client, such as credit texts and logos, as well as some of
   Cesium's built-in navigation controls that can easily be manipulated
   using touch gestures (see Figure 182).

   The main toolbox now scales to fit to the screen size. In case of
   excess lines/length, the toolbox becomes scrollable (see Figure 183).

   The infobox displayed when a city object (e.g. building) is clicked
   is now displayed in fullscreen with scrollable contents, as
   illustrated in Figure 184 below.

|image219|

Figure 182: The 3DCityDB Web Map Client on mobile devices

|image220|

Figure 183: The main toolbox on mobile devices

|image221|

Figure 184: The infobox on mobile devices

2. **Geolocation-based features**

..

   The web client contains a new GPS button (located on the top right
   corner in the view toolbar) providing new functionalities involving
   user's current location and orientation (see Figure 185 and Figure
   186). Namely:

-  Location "snapshot" (single-click): shows the user's current position
      and orientation.

-  Real-time Orientation Tracking (double-click): periodically shows the
      user's current orientation with fixed location.

-  Real-time Compass Tracking + Position (triple-click) or the
      "First-person View" mode: periodically shows the user's current
      orientation and position.

..

   |image222|

| Figure 185: From left to right, the 3 modes of geolocation-based
  features:
| Location snapshot, Real-time orientation tracking and First-person
  view

|image223|

Figure 186: Real-time orientation tracking and First-person View on
mobile devices

   To disable real-time tracking, simply either click on the button
   again to return to "snapshot" mode or hold the button for 1 second,
   the camera will then ascend to a higher altitude of the current
   location.

Note that the mobile extension makes use of the Geolocation API and the
*DeviceOrientation* API in HTML5. The Geolocation API only works via
HTTPS since Google Chrome 50. Therefore, make sure the client is called
from a secured page (via SSL/HTTPS). Additionally, permission to
retrieve current orientation and location must be granted by the user.

.. |image219| image:: media/image229.PNG
   :width: 2.73236in
   :height: 4.86021in

.. |image220| image:: media/image230.PNG
   :width: 2.87402in
   :height: 5.11164in

.. |image221| image:: media/image231.PNG
   :width: 2.87402in
   :height: 5.11219in

.. |image222| image:: media/image232.png
   :width: 3.7037in
   :height: 1.15434in

.. |image223| image:: media/image233.PNG
   :width: 3.03478in
   :height: 5.39815in
