Interaction with 3D objects
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The 3D web client supports rich model interaction such as highlighting
of 3D objects on mouse over and mouse click. More than one 3D object can
be selected by Ctrl-clicking on them and can also be hidden and
redisplayed in the 3D web client interactively. Besides, the user is
able to create a screenshot image of the current map view (including the
highlighted and hidden 3D objects) or print it directly via the web
browser. Moreover, when a 3D object is selected, it can be visually
inspected in other third-party mapping applications (*Bing Maps, Google
Streetview, OpenStreetMap* and *DualMaps*) from multiple view
perspectives such as oblique view, street view, or a combined version.

For the sake of clarity, the above mentioned functionalities will be
illustrated with the help of a number of screenshots generated based on
the online demo **Semantic 3D City Model of Berlin** which shows all
Berlin’s buildings (> 550,000) with textured 3D geometries and many
thematic attributes in the 3D web client. You can find the link of this
demo via the following web page:

::

    https://github.com/3dcitydb/3dcitydb-web-map

Once the demo was opened in your web browser, you may need to use the
*Geocoder* widget to zoom the Earth map to the building object with the
GMLID “\ **BLDG_0003000b0009a940**\ ”.

.. figure:: /media/webmap_clicked_object_attribute_table_fig.PNG
   :name: 3d_web_client_clicked_object_attribute_table
   :align: center

   By clicking on a building object it will automatically be
   highlighted and its attribute information will be queried from a Google
   Fusion Table and displayed in tabular form on the right side of the 3D
   web client


.. figure:: /media/3d_web_client_object_external_maps.png
   :name: 3d_web_client_object_external_maps
   :align: center

   By clicking on the dropdown list *Show the selected object
   in External Maps*, the user can select one of the given options to
   explore the selected building object in the chosen mapping application
   which will be opened in a new browser window or tab

.. figure:: /media/webmap_dualmaps_fig.png
   :name: 3d_web_client_object_dual_maps
   :align: center

   If the option *DualMaps* has been chosen, the selected
   building will be shown in a so-called mash-up web application linking
   different view perspectives, e.g. Google 2D map view, Google Streetview,
   and Bing Maps oblique view

.. figure:: /media/webmap_multiple_object_highlighting_fig.PNG
   :name: 3d_web_client_object_group
   :align: center

   A group of building objects can be interactively selected by
   Ctrl-clicking. Deactivating the selection of a certain building object
   can be done by Ctrl-clicking on it again

.. figure:: /media/3d_web_client_object_highlight_and_hide.png
   :name: 3d_web_client_object_highlight_and_hide
   :align: center

   The selected building objects can be hidden by clicking on
   the button *Hide selected Objects.* The GMLIDs of the selected
   (highlighted) and hidden building objects can be explored by clicking
   the drop-down buttons *Choose highlighted Object* and *Choose hidden
   Object* respectively

.. figure:: /media/3d_web_client_object_show_hidden.png
   :name: 3d_web_client_object_show_hidden
   :align: center

   The hidden objects can be shown on the 3D web client again
   by clicking on the button *Show Hidden Objects*

.. figure:: /media/3d_web_client_object_clear_highlighting.png
   :name: 3d_web_client_object_clear_highlighting
   :align: center

   The objects selection and along with the highlighting effect
   can be deactivated by clicking on the button *Clear Highlighting*

.. figure:: /media/3d_web_client_object_print_view.png
   :name: 3d_web_client_object_print_view
   :align: center

   A screenshot of the current view can be created directly
   within the 3D web client by clicking on the button *Create Screenshot*
   or *Print current view*

.. figure:: /media/webmap_screenshot_print_fig.png
   :name: 3d_web_client_object_print_view_options
   :align: center

   Once the button *Print current view* has been clicked on, a
   printer settings dialog (differs for different web browsers) will appear
   giving a preview of the screenshot file to be printed

.. figure:: /media/3d_web_client_object_shadow.png
   :name: 3d_web_client_object_shadow
   :align: center

   Shadow visualization of the 3D city models can also be
   activated and deactivated by clicking the *Toggle Shadows* button

.. figure:: /media/3d_web_client_object_scene_link.png
   :name: 3d_web_client_object_scene_link
   :align: center

   It is possible to create a scene link saving the current
   status of the 3D web client by clicking on the *Generate Scene Link*
   button. This scene link encodes the information about the title of the
   web site, activation status of the shadow visualization, parameters of
   the current loaded layers, the camera perspective etc. The created scene
   link can be stored as a browser bookmark or favorite and can also be
   sent e.g. by email to friends, colleagues, project partners etc. When
   they open the link, the same scene will open in their browsers.