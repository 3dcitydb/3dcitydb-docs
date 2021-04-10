Installation
------------

The *ADE Manager* plugin is packaged with the Importer/Exporter installer.
When using the GUI-based setup wizard for installation (see :numref:`impexp_gui_installation_chapter`),
you can simply select the plugin from the list of available software
packages as shown below.

.. figure:: /media/ade_manager_plugin_gui_installation.png
   :name: ade_manager_plugin_gui_installation
   :align: center

   Installation wizard of the Importer/Exporter tool.

If you have not installed the plugin together with the Importer/Exporter,
it is also possible to install it at any later time with the following steps:

1.  Download the ADE Manager plugin as ZIP file from the official
    3D City Database website at https://www.3dcitydb.org
    or from the releases section of the GitHub repository used for maintaining the plugin
    at https://github.com/3dcitydb/plugin-ade-manager.

    .. caution::
       Make sure the version of the ADE Manager plugin that you
       want to download can be used together with the version of your
       Importer/Exporter installation.

2.  Open the ``plugins`` folder within the installation directory of your
    Importer/Exporter and unzip the ZIP file of the ADE Manager plugin
    there. If the ``plugins`` folder does not exist, then create it first.
    After unzipping, a new subfolder ``plugin-ade-manager`` should
    have been created containing all files required by the plugin.

3.  Run the *Importer/Exporter*. The ADE Manager plugin should
    be automatically detected and loaded.

If you have successfully installed the plugin, the *ADE Manager* operation tab
is available on the operations window of the Importer/Exporter as illustrated
below.

.. figure:: /media/ade_manager_plugin_user_interface.png
   :name: ade_manager_plugin_user_interface
   :align: center

   The "ADE Manager" operation tab of the ADE Manager plugin.
