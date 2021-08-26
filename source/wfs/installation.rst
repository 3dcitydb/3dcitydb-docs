.. _wfs_installation_chapter:

Installation
------------

The 3D City Database WFS is shipped as a Java WAR (web archive) file.
Please download the WFS distribution package from the GitHub
`release section <https://github.com/3dcitydb/web-feature-service/releases>`_
or from the 3DCityDB website at https://www.3dcitydb.org.

.. note::
   Alternatively, you may build your own WAR file from the `source code <https://github.com/3dcitydb/web-feature-service>`_
   provided on GitHub. This requires that you are experienced in building
   Java web applications from source using Gradle. No further documentation
   is provided here.

Please follow the following installation steps.

| **Step 1: Install and properly configure your Java servlet container**
| Please refer to the documentation of your servlet container for
  hints on installation and configuration. Make sure that the servlet
  container uses Java 8 (or higher) for running web applications.

.. caution::
   In contrast to previous versions, it is **no longer required** to install specific
   Java libraries such as JDBC drivers as so-called *shared* or *common libs* in a
   global context of your servlet container. All libraries required by the WFS are rather
   **automatically** deployed locally in the ``WEB-INF/lib`` folder of the web application.
   No further manual steps are needed.

   If you update a previous version of the WFS, you can choose to uninstall the global
   libraries of the previous version unless they are needed by another web application
   running on your servlet container. For Apache Tomcat 7 (or higher), you can simply delete
   the JAR files from the ``lib`` folder within the Tomcat installation directory after you have
   shut down the server. Please refer to the user manual of your previous WFS version to learn
   which JAR files need to be deleted. Uninstalling the global libraries is optional though
   and may be skipped. More details on how to manage global libraries can be found in the
   documentation of your servlet container.

| **Step 2: Configure your servlet container (optional)**
| Make sure that your servlet container has enough memory assigned
  (heap space ~ 1GB or more).

.. note::
  You may, for instance, use the Java command-line option ``-Xms``
  for this purpose.

| **Step 3: Deploy the WFS WAR file on your servlet container**
| If your servlet container is correctly set up and configured, simply
  deploy the WAR file to install the WFS web service. Again, the way to
  deploy a WAR file varies for different servlet containers. For Apache
  Tomcat servers, copy the WAR file into the ``webapps`` folder, which, by
  default, is in the installation directory of the Apache Tomcat server.
  This will automatically deploy the application. Alternatively, use the
  web-based Tomcat *manager application* to deploy WAR files on the
  server. The manager application is included in a default installation.
  For more information on deploying WAR files on Tomcat or different
  servlet containers, please refer to the corresponding documentation
  material.

.. note::
   If you use the automatic deployment feature of Tomcat as
   described above, the name of the WAR file will be used as *context path*
   in the URL for accessing the application. For example, if the WFS WAR
   file is named ``citydb-wfs.war``, then the context path of the WFS service
   will be ``http[s]://[host][:port]/citydb-wfs/``. To pick a different context
   path, simply rename the WAR file or change Tomcat’s default behavior.

| **Step 4: Configure the WFS service**
| The WFS must be configured to meet your needs. For example, this
  includes providing connection details for the 3D City Database
  instance and the definition of the feature types that shall be served
  through the interface. These settings have to be manually edited in the
  configuration file ``config.xml`` of the service. Please check :numref:`wfs_configuration_chapter`
  for how to configure the WFS.

.. note::
   Changes to the ``config.xml`` file typically *require a reload or
   restart* of the WFS web application (a restart of the servlet container
   itself is, of course, not required). Please check to documentation of
   your favorite servlet container for how to do so. In case of Apache
   Tomcat, you can simply use the *manager application* to reload web
   applications.

| **Step 5: Install ADE extensions (optional)**
| As a last step, you may install additional CityGML *ADE extensions*
  for the WFS. This step is optional and requires a compiled and
  ready-to-use ADE extension package. Simply copy the contents of the
  ADE extension package to the ``WEB-INF/ade-extensions`` directory of your
  deployed WFS application. The ``WEB-INF`` directory is typically located
  in the *application* folder, which is generally named after the WAR
  file and itself is a subfolder of the webapps folder in the Tomcat
  installation directory (see :numref:`wfs_config_file_path_fig`).

.. note::
   The CityGML ADE must also be registered in the 3DCityDB instance
   to which your WFS service shall connect.