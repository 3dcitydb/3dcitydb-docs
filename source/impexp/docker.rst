.. _impexp_docker_chapter:

###############################################################################
Using the Importer/Exporter with Docker
###############################################################################
The 3D City Database (3DCityDB) Importer/Exporter Docker images expose the
capabilities of the :ref:`Importer/Exporter CLI <impexp_cli_chapter>` for
dockerized applications and workflows. All CLI commands despite the
:ref:`GUI command<impexp_cli_gui_command>` are supported.

.. rubric:: Synopsis

.. code-block:: bash
  :caption: 3D City Database Importer/Exporter Docker synopsis
  :name: impexp_docker_code_synopsis

  docker run --rm -i -t --name impexp \
    [-v /my/data/:/data] \
  3dcitydb/impexp COMMAND

*******************************************************************************
Image variants and versions
*******************************************************************************
The 3D City Database (3DCityDB) Importer/Exporter Docker images are available
based on Debian and Alpine Linux. The Debian version is based on the
`OpenJDK <https://hub.docker.com/_/openjdk>`_ images, the Alpine Linux
variant is based on the non-official images from
`AdoptOpenJDK <https://hub.docker.com/r/adoptopenjdk/openjdk11/>`_.
The images are going to use the latest LTS JDK version available, when a new
Importer/Exporter version is released. For version 4.3.0, this is JDK 11.0.11 LTS.
:numref:`impexp_docker_tbl_images` gives an overview on the available images.

.. list-table:: 3DCityDB Importer/Exporter Docker image variants and versions
  :widths: auto
  :header-rows: 1
  :stub-columns: 1
  :align: center
  :name: impexp_docker_tbl_images

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

The **edge** images are automatically built and published on every push to the
*master* branch of the `3DCityDB Importer/Exporter Github repository <https://
github.com/3dcitydb/importer-exporter>`_
using the latest stable version of the base images.
The **latest** and **release** (e.g. ``4.3.0``) image versions  are only built
when a new release is published on Github. The **latest** tag will point to
the most recent release version, e.g. ``latest = 4.3.0``.

The images are available on `3DCityDB DockerHub <https://hub.docker.com/r/
3dcitydb/>`_ and can be pulled like this:

.. code-block:: bash

  docker pull 3dcitydb/impexp:TAG

The image *tag* is composed of the Importer/Exporter version and the image
variant. Debian is the default image variant, where no image varaint is
appended to the tag. For the Alpine Linux images ``-alpine`` is appended.
The full list of available tags can be found on `DockerHub <https://hub.
docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated>`_.
Here are some examples for full image tags:

.. code-block:: shell

  docker pull 3dcitydb/impexp:edge
  docker pull 3dcitydb/impexp:latest-apline
  docker pull 3dcitydb/4.3.0
  docker pull 3dcitydb/4.3.0-alpine


*******************************************************************************
Usage and configuration
*******************************************************************************

The 3DCityDB Importer/Exporter Docker images do not require configuration
and allows the usage of the Importer/Exporter CLI out of the box. Simply append
the :ref:`Importer/Exporter CLI <impexp_cli_chapter>` command you want to use
to the `docker run <https://docs.docker.com/engine/reference/commandline/run/>`_
command line:

.. code-block:: bash

  docker run -i -t --rm --name impexp 3dcitydb/impexp COMMAND

All import and export operations require a mounted directory for
exchanging data between the host system and the container. Use the
`docker run <https://docs.docker.com/engine/reference/commandline/run/>`_
``-v`` or ``--mount`` options to mount a directory:

.. code-block:: bash

  # mount /my/data/ on the host system to /data inside the container
  docker run -i -t --rm --name impexp \
    -v /my/data/:/data \
  3dcitydb/impexp COMMAND

  # Mount the current working directory on the host system to /data
  # inside the container
  docker run -i -t --rm --name impexp \
      -v $(pwd):/data \
    3dcitydb/impexp COMMAND

.. tip:: Watch out for **correct paths** when working with mounts!
  All paths passed to the Importer/Exporter CLI have to be specified from
  the containers perspective. If you are not familiar with how Docker
  manages volumes and mounts go through the `Docker volume guide <https:
  //docs.docker.com/storage/volumes/>`_.



.. code-block:: shell

    RUN set -x && \
      addgroup -g 1000 -S impexp && \
      adduser -u 1000 -S impexp -G impexp
    USER impexp


.. _impexp_docker_build:

*******************************************************************************
Build own images
*******************************************************************************





.. _impexp_docker_examples:

*******************************************************************************
Examples
*******************************************************************************

For the follow examples we assume that a 3DCityDB instance with the following
settings is running:

.. code-block:: text
  :name: impexp_docker_code_exampledb
  :caption: Example 3DCityDB instance

  HOSTNAME      my.host.de
  PORT          5432
  DB TYPE       postgresql
  DB DBNAME     citydb
  DB USERNAME   postgres
  DB PASSWORD   changeMe!

.. _impexp_docker_example_import_citygml:

Importing CityGML
===============================================================================

This section provides some examples for importing CityGML datasets. Refer to
:ref:`impexp_cli_import_command` for a detailed description of the
Importer/Exporter CLI import command.

Import the CityGML dataset ``/home/me/mydata/bigcity.gml`` on you host system
into the DB given in :numref:`impexp_docker_code_exampledb`:

.. code-block:: bash

  docker run -i -t --rm --name impexp \
      -v /home/me/mydata/:/data \
    3dcitydb/impexp import \
      -H my.host.de -d citydb -u postgres -p changeMe! \
      /data/bigcity.gml

Import all CityGML datasets from ``/home/me/mydata/`` on your host system
into the DB given in :numref:`impexp_docker_code_exampledb`:

.. code-block:: bash

  docker run -i -t --rm --name impexp \
      -v /home/me/mydata/:/data \
    3dcitydb/impexp import \
      -H my.host.de -d citydb -u postgres -p changeMe! \
      /data/

.. _impexp_docker_example_import_citygml:

Exporting CityGML
===============================================================================

This section provides some examples for exporting CityGML datasets. Refer to
:numref:`impexp_cli_export_command` for a detailed description of the
Importer/Exporter CLI export command.



.. Images ---------------------------------------------------------------------

.. |deb-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/importer-exporter/docker-build-edge?
  style=flat-square&logo=Docker&logoColor=white
  :target: https://hub.docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated

.. |alp-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/importer-exporter/docker-build-edge-alpine?
   style=flat-square&logo=Docker&logoColor=white
  :target: https://hub.docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated

.. |deb-size-edge| image:: https://img.shields.io/docker/image-size/
  3dcitydb/impexp/edge?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated

.. |alp-size-edge| image:: https://img.shields.io/docker/image-size/
  3dcitydb/impexp/edge-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated

.. |deb-size-latest| image:: https://img.shields.io/docker/image-size/
  3dcitydb/impexp/latest?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated

.. |alp-size-latest| image:: https://img.shields.io/docker/image-size/
  3dcitydb/impexp/latest-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated

.. |deb-size-v4.3.0| image:: https://img.shields.io/docker/image-size/
  3dcitydb/impexp/4.3.0?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated

.. |alp-size-v4.3.0| image:: https://img.shields.io/docker/image-size/
  3dcitydb/impexp/4.3.0-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated
