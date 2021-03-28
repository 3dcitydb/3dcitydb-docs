.. _impexp_preferences_general_proxy_chapter:

Network proxies
^^^^^^^^^^^^^^^

Some of the functionalities offered by the Importer/Exporter require
internet access. This applies, for instance, to the XML validation when
accessing XML Schema documents on the web, to the map window for the
graphical selection of bounding boxes, or to
the automatic calculation of height offsets during visualization
exports based on the *Google Elevation API* web service.

Most computers in corporate environments have no direct internet access
but must use a proxy server. The preference dialog shown below lets you
configure network proxies.

.. figure:: /media/impexp_preferences_general_network_proxies_fig.png
   :name: impexp_preferences_general_network_proxies_fig
   :align: center

   General preferences – Network proxies.

The Importer/Exporter supports *Web (HTTP)*, *Secure web (HTTPS)* and
*SOCKS* proxies. To provide proxy information for one of the protocols, simply select
the corresponding entry from the list and enter the proxy settings in
the input fields below: *Server*, *Port*, and, if the proxy requires login credentials,
also *Username* and *Password*. Default port values for each protocol are
automatically chosen (HTTP: 80; HTTPS: 443; SOCKS: 1080) and only
need to be changed if required.

It is also possible to define one single proxy for all protocols by
simply selecting the corresponding checkbox under the protocol list.
Just make sure the proxy server supports all protocols and that they can
all be routed through the given *port*. Contact your IT administrator in case
you are unsure.

Proxies are only used if the checkbox next to the protocol type is
enabled. Otherwise, the proxy configuration will be stored but remains
inactive. When the proxy for a given protocol is enabled, every outgoing
connection by the Importer/Exporter that uses the protocol will be
routed through this proxy.

In case the computer running the Importer/Exporter is directly connected
to the internet, no proxies need to be configured.