.. _wfs_docker_chapter:

###############################################################################
Web-Feature-Service using Docker
###############################################################################

The 3DCityDB Web-Feature-Service (WFS) Docker images expose the capabilities
of the :ref:`wfs_chapter` for dockerized applications and workflows.
All :ref:`functionalities <wfs_basic_functionality_chapter>` are supported by
the images.

.. rubric:: Synopsis

.. code-block:: bash
  :name: wfs_docker_code_synopsis

  docker run --name wfs [-d] -p 8080:8080 \
      [-e CITYDB_TYPE=PostGIS|Oracle] \
      [-e CITYDB_HOST=the.host.de] \
      [-e CITYDB_PORT=5432] \
      [-e CITYDB_NAME=theDBName] \
      [-e CITYDB_SCHEMA=theCityDBSchemaName] \
      [-e CITYDB_USERNAME=theUsername] \
      [-e CITYDB_PASSWORD=theSecretPass] \
      [-e WFS_CONTEXT_PATH=wfs-context-path] \
      [-e WFS_CONFIG_FILE=/path/to/config.xml] \
      [-e WFS_ADE_EXTENSIONS_PATH=/path/to/ade-extension] \
    3dcitydb/wfs[:TAG]

*******************************************************************************
Image variants and versions
*******************************************************************************

The WFS Docker images are based on the official `Apache Tomcat images <https://hub.
docker.com/_/tomcat>`_ and are available as Debian and Alpine
Linux variants. :numref:`wfs_docker_tbl_images` gives an overview on the images
available. Currently, Tomcat 9 images are used as base images for the WFS.

The **edge** images are automatically built and published on every push to the
*master* branch of the `3DCityDB WFS Github repository <https://
github.com/3dcitydb/web-feature-service>`_ using the latest stable version of
the base images.
The **latest** and **release** (e.g. ``4.3.0``) image versions  are only built
when a new release is published on Github. The **latest** tag will point to
the most recent release version, e.g. ``latest = 4.3.0``.

.. list-table:: 3DCityDB WFS Docker image variants and versions
  :widths: auto
  :header-rows: 1
  :stub-columns: 1
  :align: center
  :name: wfs_docker_tbl_images

  * - Tag
    - Debian
    - Alpine
  * - edge
    - |deb-build-edge| |deb-size-edge|
    - |alp-build-edge| |alp-size-edge|
  * - latest
    - |deb-size-latest|
    - |alp-size-latest|
  * - 4.3.0
    - |deb-size-v4.3.0|
    - |alp-size-v4.3.0|

The images are available on `3DCityDB DockerHub <https://hub.docker.com/r/
3dcitydb/>`_ and can be pulled like this:

.. code-block:: bash

  docker pull 3dcitydb/wfs:TAG

The image *tag* is composed of the WFS version and the image
variant. Debian is the default image variant, where no image variant is
appended to the tag. For the Alpine Linux images ``-alpine`` is appended.
The full list of available tags can be found on `DockerHub <https://hub.
docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated>`_.
Here are some examples of full image tags:

.. code-block:: shell

  docker pull 3dcitydb/wfs:edge
  docker pull 3dcitydb/wfs:edge-alpine
  docker pull 3dcitydb/wfs:latest-alpine
  docker pull 3dcitydb/wfs:4.3.0
  docker pull 3dcitydb/wfs:4.3.0-alpine


*******************************************************************************
Usage and configuration
*******************************************************************************

A 3DCityDB WFS Docker container is configured using environment variables and
WFS ``config.xml`` file.
The easiest way of using the WFS Docker is to use the default ``config.xml`` we
ship with the container and overwrite the 3DCityDB connection credentials
and/or the web context path using environment variables.
This config file exposes all filter capabilities and feature types and from the
connected database to the WFS and should be suitable for most situations.

If you require more specific settings, get a copy of the
:download:`default-config.xml <https://raw.githubusercontent.com/3dcitydb/web-feature-service/master/resources/docker/default-config.xml>`
and build your own config file (see :ref:`wfs_configuration_chapter`).
Add you new config file to the container during build time, as described in
:ref:`building your own images <wfs_docker_build>` or mount it to the container
on runtime (see `docker run docs <https://docs.docker.com/engine/reference/run/>`_).
To apply the custom config file set the :option:`WFS_CONFIG_FILE` option.

.. note:: The environment variables always take precedence over the settings provided
  in ``config.xml``. Thus, you can create custom config files and use them with
  different databases by overwriting the settings with the environment variables.

Environment variables
===============================================================================

All environment variables are optional. If you do not provide the database
connection details via env variables (``CITYDB_*``), they must be provided in
the ``config.xml`` file. Otherwise you will get error messages when starting
the service.

.. option:: CITYDB_TYPE=<postgresql|oracle>

  The type of the 3DCityDB to connect to. *postgresql* is the default.

.. option:: CITYDB_HOST=<hostname or ip>

  Name of the host or IP address on which the 3DCityDB is running.

.. option:: CITYDB_PORT=<port>

  Port of the 3DCityDB to connect to. Default is *5432* for PostgreSQL and *1521*
  for Oracle, depending on the setting of :option:`CITYDB_TYPE`.

.. option:: CITYDB_NAME=<dbName>

  Name of the 3DCityDB database to connect to.

.. option:: CITYDB_SCHEMA=<citydb>

  Schema to use when connecting to the 3DCityDB. The defaults are *citydb* for
  PostgreSQL, *username* for Oracle, depending on the setting of
  :option:`CITYDB_TYPE`.

.. option:: CITYDB_USERNAME=<username>

  Username to use when connecting to the 3DCityDB

.. option:: CITYDB_PASSWORD=<thePassword>

  Password to use when connecting to the 3DCityDB

.. option:: WFS_CONTEXT_PATH=<wfs-context-path>

  The URL subpath where the WFS is served. The default setting is ``ROOT``, for
  serving from the web root. **Note:** Nested paths are currently not supported.
  For instance, set ``WFS_CONTEXT_PATH=citydb-wfs`` to serve from
  ``http[s]://my-domain/wfs-client/``.

.. option:: WFS_CONFIG_FILE=</path/to/custom/config.xml>

  Path of the WFS config file to use.

.. option:: WFS_ADE_EXTENSIONS_PATH=</path/to/ade-extension>

  ?

.. _wfs_docker_build:

*******************************************************************************
Build your own images
*******************************************************************************



.. Images ---------------------------------------------------------------------

.. |deb-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/importer-exporter/docker-build-edge?
  style=flat-square&logo=Docker&logoColor=white
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |alp-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/importer-exporter/docker-build-edge-alpine?
   style=flat-square&logo=Docker&logoColor=white
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |deb-size-edge| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/edge?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |alp-size-edge| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/edge-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |deb-size-latest| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/latest?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |alp-size-latest| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/latest-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |deb-size-v4.3.0| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/4.3.0?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |alp-size-v4.3.0| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/4.3.0-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated
