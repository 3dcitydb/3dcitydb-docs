.. _wfs_configuration_chapter:

Configuring the WFS
-------------------

.. toctree::
   :hidden:

   capabilities
   feature-types
   operations
   filter-capabilities
   constraints
   post-processing
   database
   server
   logging

After deploying but before using the WFS service, you need to edit the
``config.xml`` file to make the service run properly. The ``config.xml`` file is
located in the ``WEB-INF`` directory of the WFS web application. If you use Apache
Tomcat, ``WEB-INF`` is a subfolder of the *application* folder, which is generally
named after the WAR file and itself is a subfolder of the ``webapps`` folder in the
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
elements ``<capabilities>``, ``<featureTypes>``, ``<operations>``, ``<filterCapabilities>``,
``<constraints>``, ``<postProcessing>``, ``<database>``, ``<server>``, and ``<logging>``.
The discussion of the settings follows this organization in the subsequent clauses.

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
   * - | :ref:`wfs_filter_capabilities_settings_chapter`
     - | Define the filter operations that shall be supported in queries.
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

**Environment variables**

In addition to the ``config.xml`` file, the WFS supports the following environment
variables to configure further settings. The variables must have been set prior to
starting the service. They **always take precedence** over corresponding settings in the
``config.xml`` file.

.. list-table::  Environment variables supported by the WFS service
   :name: wfs_supported_env_variables_table
   :widths: 30 70

   * - | **Environment variable**
     - | **Description**
   * - | ``CITYDB_TYPE``
     - | Used to specify the database system of the 3DCityDB the WFS service shall connect to. Allowed values are *postgresql* for PostgreSQL/PostGIS databases (default) and *oracle* for Oracle Spatial databases.
   * - | ``CITYDB_HOST``
     - | Host name or IP address of the server on which the database is running.
   * - | ``CITYDB_PORT``
     - | Port of the database server to connect to. Default value is *5432* for PostgreSQL and *1521* for Oracle, depending on the setting for ``CITYDB_TYPE``.
   * - | ``CITYDB_NAME``
     - | Used to specify the name of the 3DCityDB instance to connect to. When connecting to an Oracle database, provide the database SID or service name as value.
   * - | ``CITYDB_SCHEMA``
     - | Schema to use when connecting to the database. The defaults are *citydb* for PostgreSQL and the *username* specified through ``CITYDB_USERNAME`` for Oracle, depending on the setting for ``CITYDB_TYPE``.
   * - | ``CITYDB_USERNAME``
     - | Connect to the database sever with this user.
   * - | ``CITYDB_PASSWORD``
     - | The password to use when connecting to the database server.
   * - | ``WFS_CONFIG_FILE``
     - | With this variable, you can specify a configuration file that shall be used instead of the default ``config.xml`` file in the ``WB-INF`` directory when starting the WFS service. The variable must provide the full path to the configuration file. The WFS service must have read access to this file.
   * - | ``WFS_ADE_EXTENSIONS_PATH``
     - | Allows for providing an alternative directory where the WFS service shall search for ADE extensions (default: ``ade-extensions`` folder in the ``WEB-INF`` directory). The WFS service must have read access to this directory.