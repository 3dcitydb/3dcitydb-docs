.. _webmap_mobile_support_chapter:

Mobile support extension
~~~~~~~~~~~~~~~~~~~~~~~~

Starting from version ``1.6.0``, the 3DCityDB-Web-Map-Client is equipped
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

The web client contains a new geolocation button (located on the top right
corner in the view toolbar) providing new functionalities involving
user's current location and orientation. These are described as follows:

.. |3d_web_client_gps_main| image:: /media/GPS_main.png
   :height: 50px

.. |3d_web_client_gps_single| image:: /media/GPS_single.png
   :height: 50px

.. |3d_web_client_gps_on_ori| image:: /media/GPS_on_ori.png
   :height: 50px

.. |3d_web_client_gps_on_pos_ori| image:: /media/GPS_on_pos_ori.png
   :height: 50px

.. |3d_web_client_gps_off| image:: /media/GPS_off.png
   :height: 50px

* |3d_web_client_gps_main| Main geolocation button by default (before activation)
* |3d_web_client_gps_single| A single "snapshot" of the device's current position and orientation
* |3d_web_client_gps_on_ori| Live-tracking of the device's orientation with fixed position
* |3d_web_client_gps_on_pos_ori| Live-tracking of the device's orientation and position (first-person view)
* |3d_web_client_gps_off| Option to disable the live-tracking functions / first-person view

.. note::
   The mobile extension makes use of the Geolocation API and the
   *DeviceOrientation* API in HTML5. The Geolocation API only works via
   HTTPS since Google Chrome 50. Therefore, make sure the client is called
   from a secured page (via SSL/HTTPS). Additionally, permission to
   retrieve current orientation and location must be granted by the user.
