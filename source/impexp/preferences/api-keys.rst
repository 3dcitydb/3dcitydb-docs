.. _impexp_preferences_general_apiKeys_chapter:

API Keys
^^^^^^^^

The Importer/Exporter uses external web services offered by third party
providers for different tasks and functionalities. Some of these
services are open and free to use, whereas others are more restrictive
and require passing an API key to use the service. With the *API Keys*
preference dialog, you can provide your API keys for different services.

.. figure:: /media/impexp_preferences_general_apikeys_fig.png
   :name: impexp_preferences_general_apikeys_fig
   :align: center

   General preferences – API keys.

The *Google Maps API* services can be used by the Importer/Exporter for
two different tasks: 1) the *Geocoding API* is used for geocoding
addresses and address lookups in the map window (cf. :numref:`impexp_preferences_map_window_chapter`), and
2) the *Maps Elevation API* is used in visualization exports for
retrieving height values from the Google Earth terrain model (cf.
:numref:`impexp_kml_export_terrain_preferences_chapter`).
If you want to use one of these services, you
must enter your corresponding API key in the above dialog. Otherwise the
services will respond with an error message that will be displayed by
the Importer/Exporter. Please visit the Google Maps API website if you
do not have an API key yet.

.. note::
   Google has changed the usage and pricing policies for the
   above-mentioned services starting from July 16, 2018. Thus, in previous
   versions of the Importer/Exporter, the services could be used without
   entering an API key.