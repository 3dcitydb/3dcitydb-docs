.. _impexp_import_bbox_filter:

Bounding box filter
-------------------

**Bounding box filter**: This filter takes a 2D bounding box as parameter that is given by the
coordinate values of its lower left (x\ :sub:`min`, y\ :sub:`min`) and upper right corner (x\ :sub:`max`, y\
:sub:`max`) [5]. The bounding box is evaluated against the gml:boundedBy property of the CityGML input features.
You can choose whether features *overlapping* with the provided bounding box are to be
imported, or whether features must be *inside* of it.

.. figure:: /media/impexp_import_bbox_filter.png
   :name: impexp_import_bbox_filter_fig
   :align: center

   Bounding box filter for import operations.

For the *bounding box filter*, make sure that you choose a *coordinate
reference system* from the drop-down choice list that matches the
provided coordinate values. Otherwise, the spatial filter may not work
as expected. The coordinate reference system list can be augmented with
user-defined reference systems (see :numref:`impexp_crs_management_chapter` for more information).

The coordinate values of the bounding box filter can either be entered
manually or chosen interactively in a 2D map window. To open the map
window, click on the map button |map_select| [6].

.. figure:: /media/impexp_bbox_selection_map_window_fig.png
   :name: impexp_bbox_selection_map_window_fig
   :align: center

   Bounding box selection using the 2D map window.

In the map window, keep the left mouse button clicked while holding the
ALT key. This lets you draw a bounding box on the map. In order to move
the map to a specific location or address, simply enter the location or
address in the input field on top of the map and click the *Go* button
or use the map navigation controls. If you are happy with the bounding
box selection, click the *Apply* button. This will close the map window
and carry the coordinate values of the selected area into the
corresponding fields of the bounding box filter [5]. Click *Cancel* if
you want to close the map window but skip your selection. A more
comprehensive guide on how to use the map window is provided in chapter
:numref:`impexp_preferences_map_window_chapter`.

With the |bbox_copy| button on the bounding box filter dialog [6], you can copy a bounding
box to the clipboard, while the |bbox_paste|
button pastes a bounding box from the clipboard to the input fields of
the bounding box filter [5] (or use the right-click context menu).

.. |bbox_copy| image:: /media/bbox_copy.png
   :width: 0.16667in
   :height: 0.16667in

.. |bbox_paste| image:: /media/bbox_paste.png
   :width: 0.16667in
   :height: 0.16667in

.. |map_select| image:: /media/map_select.png
   :width: 0.16667in
   :height: 0.16667in