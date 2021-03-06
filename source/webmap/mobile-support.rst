.. _webmap_mobile_support_chapter:

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

**A more lightweight graphical user interface**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to make the best use of the limited screen real-estate
available on mobile devices, some elements are removed or hidden from
the web client, such as credit texts and logos, as well as some of
Cesium's built-in navigation controls that can easily be manipulated
using touch gestures (see :numref:`3d_web_client_mobile_gui`).

The main toolbox now scales to fit to the screen size. In case of
excess lines/length, the toolbox becomes scrollable (see :numref:`3d_web_client_mobile_main_toolbox`).

The infobox displayed when a city object (e.g. building) is clicked
is now displayed in fullscreen with scrollable contents, as
illustrated in :numref:`3d_web_client_mobile_infobox` below.

.. figure:: /media/webmap_mobile_gui_fig.PNG
   :name: 3d_web_client_mobile_gui
   :scale: 50 %
   :align: center

   The 3DCityDB Web Map Client on mobile devices

.. figure:: /media/webmap_mobile_main_toolbox_fig.PNG
   :name: 3d_web_client_mobile_main_toolbox
   :scale: 50 %
   :align: center
   
   The main toolbox on mobile devices

.. figure:: /media/webmap_mobile_infobox_fig.PNG
   :name: 3d_web_client_mobile_infobox
   :scale: 50 %
   :align: center
   
   The infobox on mobile devices

**Geolocation-based features**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The web client contains a new GPS button (located on the top right
corner in the view toolbar) providing new functionalities involving
user's current location and orientation (see :numref:`3d_web_client_mobile_symbols` and :numref:`3d_web_client_mobile_first_person_view`). Namely:

*  Location "snapshot" (single-click): shows the user's current position and orientation.

*  Real-time Orientation Tracking (double-click): periodically shows the
   user's current orientation with fixed location.

*  Real-time Compass Tracking + Position (triple-click) or the
   "First-person View" mode: periodically shows the user's current
   orientation and position.

.. figure:: /media/webmap_mobile_symbols_fig.png
   :name: 3d_web_client_mobile_symbols
   :scale: 30 %
   :align: center

   From left to right, the 3 modes of geolocation-based features: 
   Location snapshot, Real-time orientation tracking and First-person view

.. figure:: /media/webmap_mobile_first_person_view_fig.PNG
   :name: 3d_web_client_mobile_first_person_view
   :scale: 50 %
   :align: center
   
   Real-time orientation tracking and First-person View on
   mobile devices

To disable real-time tracking, simply either click on the button
again to return to "snapshot" mode or hold the button for 1 second,
the camera will then ascend to a higher altitude of the current
location.

.. note::
   The mobile extension makes use of the Geolocation API and the
   *DeviceOrientation* API in HTML5. The Geolocation API only works via
   HTTPS since Google Chrome 50. Therefore, make sure the client is called
   from a secured page (via SSL/HTTPS). Additionally, permission to
   retrieve current orientation and location must be granted by the user.
