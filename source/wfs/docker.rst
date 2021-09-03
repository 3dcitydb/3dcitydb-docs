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
    3dcitydb/wfs[:TAG]

*******************************************************************************
Image variants and versions
*******************************************************************************

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

*******************************************************************************
Usage and configuration
*******************************************************************************

A 3DCityDB WFS Docker container is configured using environment variables.

Environment variables
===============================================================================

.. option:: CITYDB_TYPE=<postgresql|oracle>

  The type of the 3DCityDB to connect to. *postgresql* is the default.

.. option:: CITYDB_HOST=<hostname or ip>

  Name of the host or IP address on which the 3DCityDB is running.

.. option:: CITYDB_PORT=<port>

  Port of the 3DCityDB to connect to. Default is *5432* for PostgreSQL and *1521* for Oracle.

.. option:: CITYDB_NAME=<dbName>

  Name of the 3DCityDB database to connect to.

.. option:: CITYDB_SCHEMA=<citydb>

  Schema to use when connecting to the 3DCityDB (default: *citydb | username*).

.. option:: CITYDB_USERNAME=<username>

  Username to use when connecting to the 3DCityDB

.. option:: CITYDB_PASSWORD=<thePassword>

  Password to use when connecting to the 3DCityDB

.. option:: WFS_CONTEXT_PATH=<wfs-context-path>

  The URL subpath where the WFS is served. The default setting is ``ROOT``, for
  serving from the web root. **Note:** Nested paths are currently not supported.
  For instance, set ``WFS_CONTEXT_PATH=citydb-wfs`` to serve from
  ``http[s]://my-domain/wfs-client/``.

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
