Preferences
-----------

In addition to the settings on the Import, Export, KML/COLLADA/glTF
Export and Database tabs, more preferences affecting the separate
operations of the Importer/Exporter are available on the Preferences tab
shown below.

.. figure:: /media/preferences_main_gui.png
   :name: pic_preferences_main_gui
   :align: center

   The preferences dialog 

The preferences are structured in a tree view [1] on the left side of
the dialog with the following main nodes:

.. toctree::
   :maxdepth: 1

   citygml-export
   kml-collada-gltf-export
   manage-crs
   general

Below these main nodes, further subnodes organize the preferences into
separate topics. When selecting a node in the tree view, the associated
settings dialog is displayed on the right side [2]. Changes made to the
settings of the selected node are applied through the *Apply* button
[3]. The buttons *Restore* and *Default* allow for resetting the
preferences to their previous state or to their default values.

The preferences (including the settings on the separate operation tabs)
are automatically stored in the config file of the Importer/Exporter and
are restored from this file upon program start. Via the Project menu
available from the menu bar of the Importer/Exporter, the preferences
can optionally be stored in or loaded from user-defined config files
(cf. :numref:`impexp_gui_chapter`).
