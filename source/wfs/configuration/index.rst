.. _wfs_configuration_chapter:

Configuring the WFS
-------------------

.. toctree::
   :hidden:

   capabilities
   feature-types
   operations
   constraints
   post-processing
   database
   server
   logging

After deploying but before using the WFS service, you need to edit the
``config.xml`` file to make the service run properly. The ``config.xml`` file is
in the ``WEB-INF`` directory of the WFS web application. The ``WEB-INF`` is a
subfolder of the *application* folder, which is generally named after
the WAR file and itself is a subfolder of the ``webapps`` folder in the
Tomcat installation directory. This may be different if you use another
servlet container.

For example, assume that the WFS web application was deployed under the
context name ``citydb-wfs``. Then the location of the ``WEB-INF`` folder and the
``config.xml`` file in a default Apache Tomcat installation is shown below.

.. figure:: /media/wfs_config_file_path_fig.png
   :name: wfs_config_file_path_fig
   :align: center

   Location of the WEB-INF folder and the config.xml file.

Open the ``config.xml`` file with a text or XML editor of your choice and
manually edit the settings. In the ``config.xml`` file, the WFS settings are organized into the main XML
elements ``<capabilities>``, ``<featureTypes>``, ``<operations>``, ``<constraints>``,
``<postProcessing>``, ``<database>``, ``<server>``, and ``<logging>``. The discussion of the
settings follows this organization in the subsequent clauses.

.. list-table::  Configuration settings of the WFS service
   :name: wfs_configuration_settings_table
   :widths: 30 70

   * - | **Settings**
     - | **Description**
   * - | :ref:`wfs_capabilities_settings_chapter`
     - | Define service metadata that shall be used in the capabilities document of the WFS service.
   * - | :ref:`wfs_feature_type_settings_chapter`
     - | Control which feature types shall be advertised and served by the WFS service.
   * - | :ref:`wfs_operations_settings_chapter`
     - | Define the operation-specific behaviour of the WFS.
   * - | :ref:`wfs_constraints_settings_chapter`
     - | General constraints that influence the capabilities of the WFS service and of the advertised operations.
   * - | :ref:`wfs_postprocessing_settings_chapter`
     - | Allow for specifying XSLT transformations to be applied to the CityGML data before sending the response to the client.
   * - | :ref:`wfs_database_settings_chapter`
     - | Connection details to use for connecting to a 3D City Database instance.
   * - | :ref:`wfs_server_settings_chapter`
     - | Server-specific options and parameters.
   * - | :ref:`wfs_logging_settings_chapter`
     - | Logging-specific settings like the log level and output file to use.

.. caution::
   An XML Schema for validating the contents of
   the ``config.xml`` file is provided as file ``config.xsd`` in the subfolder
   ``schemas``. **After every edit** to the ``config.xml`` file, **make sure** that
   the it **validates against this schema** before reloading
   the WFS web application. Otherwise, the application might refuse to
   load, or unexpected behavior may occur.