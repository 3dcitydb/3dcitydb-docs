.. _impexp_import_bbox_filter:

Bounding box filter
-------------------

The bounding box filter takes a 2D bounding box as parameter that is given by the
coordinate values of its lower left (x\ :sub:`min`, y\ :sub:`min`) and upper right (x\ :sub:`max`, y\
:sub:`max`) corner. The bounding box is evaluated against the gml:boundedBy property (CityGML)
respectively the "geographicalExtent" property (CityJSON) of the input features.
You can choose whether features *overlapping* with the provided bounding box are to be
imported, or whether features must be *inside* of it.

.. figure:: /media/impexp_import_bbox_filter.png
   :name: impexp_import_bbox_filter_fig
   :align: center

   Bounding box filter for import operations.

Make sure to choose a *coordinate reference system* from the drop-down list that matches the
provided coordinate values. Otherwise, the spatial filter may not work
as expected. The coordinate reference system list can be augmented with
user-defined reference systems (see :numref:`impexp_crs_management_chapter` for more information).

The coordinate values of the bounding box filter can either be entered
manually or chosen interactively in a 2D map window. To open the map
window, click on the map button |map_select|.

.. figure:: /media/impexp_bbox_selection_map_window_fig.png
   :name: impexp_bbox_selection_map_window_fig
   :align: center

   Bounding box selection using the 2D map window.

In the map window, keep the left mouse button clicked while holding the
``ALT`` key. This lets you draw a bounding box on the map. In order to move
the map to a specific location or address, simply enter the location or
address in the input field on top of the map and click the search button |map_search|
or use the map navigation controls. If you are happy with the bounding
box selection, click the *Apply* button. This will close the map window
and copy the coordinate values of the selected area into the
corresponding fields of the bounding box filter and set the reference system
to WGS 84. Click *Cancel* if you want to close the map window but skip your selection.
A more comprehensive guide on how to use the map window is provided in chapter
:numref:`impexp_preferences_map_window_chapter`.

With the |bbox_copy| button on the bounding box filter dialog, you can copy a bounding
box to the clipboard, while the |bbox_paste|
button pastes a bounding box from the clipboard to the input fields of
the bounding box filter (or use the right-click context menu).

.. |bbox_copy| image:: /media/bbox_copy.svg

.. |bbox_paste| image:: /media/bbox_paste.svg

.. |map_select| image:: /media/map_select.svg

.. |map_search| image:: /media/map_search.svg