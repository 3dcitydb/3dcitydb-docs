.. _impexp_preferences_plugins:

Plugins
^^^^^^^

Plugins extend the core functionality of the Importer/Exporter and can be developed by everyone based
on a well-defined plugin API (see :numref:`impexp_plugin_mechanism_chapter`). This preferences dialog
provides an overview of all installed plugins and lets you enable or disable them. The dialog is only
available if at least one plugin has been installed.

.. figure:: /media/impexp_preferences_plugins_fig.png
   :name: impexp_preferences_plugins_fig
   :align: center

   Plugins overview and management.

Simply browse through the list of plugins to get more information about each plugin. This typically
comprises the name of the plugin, its version and a brief description of its functionality. Plugin vendors may
provide additional links to online resources such as the plugin homepage or a user manual. The *technical details*
section contains implementation details of the plugin, which are useful in case you encounter issues and
need support from the plugin vendor.

A plugin is enabled or disabled by ticking the checkbox in the list entry. Disabling unnecessary plugins can increase
the performance of Importer/Exporter operations affected by the plugin.

.. note::
   You cannot *install* or *remove* plugins using this dialog. Plugins are rather installed by copying the
   plugin files into the ``plugins`` subfolder within the installation directory of the Importer/Exporter and
   uninstalled by simply deleting these files again (see also :numref:`impexp_plugin_mechanism_chapter`).
   Both actions require a restart of the Importer/Exporter.