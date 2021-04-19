Installation and configuration
------------------------------

For convenient use, there is an official web link (see the link below)
that can be called to directly run the 3D web client on your web
browser.

.. code-block::

    https://www.3dcitydb.org/3dcitydb-web-map/1.9.0/3dwebclient/index.html

.. note::
   The number **1.9.0** in URL denotes the version number of the 3D
   web client. Once the 3D web client has been upgraded in the future, this
   version number will be adapted to conform to the current release of the
   3D web client. Web links pointing to the previous software versions will
   remain valid and accessible online.

The 3D web client is a static web application purely written in HTML and
JavaScript and can therefore be easily deployed by uploading its files
to a simple web server. A zip file for the 3D web client can be found in
the installation directory of the Import/Export tool within the
subfolder *3d-web-map-client* or downloaded via the following GitHub
link:

.. code-block::

    https://github.com/3dcitydb/3dcitydb-web-map/releases

The extracted contents of the zip file should look something like the
screenshot below.

.. figure:: /media/webmap_content_files_fig.png
   :name: pic_3d_web_map_installation
   :align: center

The 3D web client comes with a lightweight JavaSript-based HTTP server
(the file with the name “\ *server*\ ”) that is mainly meant to test the
functionality of the 3D web client on your local machine. For running
this web server, the open source JavaScript runtime environment Node.js
is required to be installed on your machine. The latest version of
Node.js can be download via the web link below:

.. code-block::

    https://nodejs.org/en/

Once the Node.js program has been installed, you need to open a shell
environment on your operating system and navigate to the folder where
the *server.js* file is located, then simply run the following command
to launch the server:

.. code:: bash

    node server.js

.. figure:: /media/webmap_cli_running_web_server_fig.png
   :name: pic_3d_web_map_installation_nodejs
   :align: center

   Example of running the JavaScript-based web server

Now, the 3D web client is available via the URL below and its user
interface should look like in the following figure:

.. code-block::

    http://localhost:8000/3dwebclient/index.html

.. figure:: /media/webmap_user_interface_fig.png
   :name: pic_3d_web_map_installation_default
   :align: center

   User interface of the 3D web client
