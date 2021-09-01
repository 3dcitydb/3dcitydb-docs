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
  :name: impexp_docker_code_synopsis

  docker run --rm --name impexp [-i -t] \
      [-e CITYDB_TYPE=PostGIS|Oracle] \
      [-e CITYDB_HOST=the.host.de] \
      [-e CITYDB_PORT=5432] \
      [-e CITYDB_NAME=theDBName] \
      [-e CITYDB_SCHEMA=theCityDBSchemaName] \
      [-e CITYDB_USERNAME=theUsername] \
      [-e CITYDB_PASSWORD=theSecretPass] \
      [-v /my/data/:/data] \
    3dcitydb/impexp[:TAG] COMMAND

*******************************************************************************
Image variants and versions
*******************************************************************************
The Importer/Exporter Docker images are available based on Debian and Alpine
Linux. The Debian version is based on the `OpenJDK <https://hub.docker.com/_
/openjdk>`_ images, the Alpine Linux variant is based on the non-official images
from `AdoptOpenJDK <https://hub.docker.com/r/adoptopenjdk/openjdk11/>`_.
The images are going to use the latest LTS JRE version available at the time a new
Importer/Exporter version is released. :numref:`impexp_docker_tbl_images` gives
an overview on the images available.

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
variant. Debian is the default image variant, where no image variant is
appended to the tag. For the Alpine Linux images ``-alpine`` is appended.
The full list of available tags can be found on `DockerHub <https://hub.
docker.com/r/3dcitydb/impexp/tags?page=1&ordering=last_updated>`_.
Here are some examples for full image tags:

.. code-block:: shell

  docker pull 3dcitydb/impexp:edge
  docker pull 3dcitydb/impexp:latest-apline
  docker pull 3dcitydb/impexp:4.3.0
  docker pull 3dcitydb/impexp:4.3.0-alpine


*******************************************************************************
Usage and configuration
*******************************************************************************

The 3DCityDB Importer/Exporter Docker images do not require configuration for
most use cases and allow the usage of the Importer/Exporter CLI out of the box.
Simply append the :ref:`Importer/Exporter CLI command <impexp_cli_chapter>` you
want to execute to the ``docker run`` command line.

.. code-block:: bash

  docker run --rm --name impexp 3dcitydb/impexp COMMAND

However, the database credentials can be passed to the Importer/Exporter
container using environment variables as well, as described in
:numref:`impexp_docker_env_vars_section`.

All import and export operations require a mounted directory for
exchanging data between the host system and the container. Use the
``-v`` or ``--mount`` options of the ``docker run`` command to mount a
directory or file.

.. code-block:: bash

  # mount /my/data/ on the host system to /data inside the container
  docker run --rm --name impexp \
    -v /my/data/:/data \
    3dcitydb/impexp COMMAND

  # Mount the current working directory on the host system to /data
  # inside the container
  docker run --rm --name impexp \
      -v $(pwd):/data \
      3dcitydb/impexp COMMAND

.. note:: The default working directory inside the container is ``/data``.

.. tip:: Watch out for **correct paths** when working with mounts!
  All paths passed to the Importer/Exporter CLI have to be specified from
  the containers perspective. If you are not familiar with how Docker
  manages volumes and bind mounts go through the
  `Docker volume guide <https://docs.docker.com/storage/volumes/>`_.

In order to allocate a console for the container process, you must use
``-i`` ``-t`` together. This comes in handy, for instance, if you don't
want to pass the password for the 3DCityDB connection on the command
line but rather want to be prompted to enter it interactively on the console.
You must use the ``-p`` option of the Importer/Exporter CLI without a
value for this purpose (see :numref:`impexp_cli_chapter`) as shown in
the example below.
Note that the ``-i`` ``-t`` options of the ``docker run`` commandare often
combined and written as ``-it``.

.. code-block:: bash

  docker run -it --rm --name impexp \
      3dcitydb/impexp import \
      -H my.host.de -d citydb -u postgres -p \
      bigcity.gml

The ``docker run`` command offers further options to configure the
container process. Please check the `official reference <https://docs.docker.
com/engine/reference/run/>`_ for more information.

.. _impexp_docker_env_vars_section:

Environment variables
===============================================================================

The Importer/Exporter Docker images support the following environment variables
to set the credentials for the connection to a 3DCityDB instance.

.. warning::

  When running the Importer/Exporter on the command line, the values of these
  variables will be used as input if a corresponding CLI option is **not** available.
  Thus, the CLI options always take precedence.

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

User management and file permissions
===============================================================================

When exchanging files between the host system and the Importer/Exporter
container it is import to make sure that files and directories have permissions
set correctly.
For security reasons (see `here <https://docs.docker.com/develop/develop-images
/dockerfile_best-practices/#user>`_) the Importer/Exporter runs as non-root user
by default inside the container.
The default user is named ``impexp`` with user and group identifier (uid, gid)
= ``1000``.

.. code-block:: console

  $ docker run --rm --entrypoint bash 3dcitydb/impexp \
      -c "cat /etc/passwd | grep impexp"

  impexp:x:1000:1000::/data:/bin/sh

As 1000 is the default uid/gid for the first user on many Linux
distributions in most cases you won't notice this, as the user on the
host system is going to have the same uid/gid as inside the container.
However, if you are facing file permission issues, you can run the
Importer/Exporter container as another user with the
``-u`` option of the ``docker run`` command. This way you can make sure,
that the right permissions are set on generated files in the mounted directory.

The following example illustrates how to use the ``-u`` option to pass the
user ID of your current host's user.

.. code-block:: bash
  :name: impexp_docker_code_uid

  docker run --rm --name impexp \
      -u $(id -u):$(id -g) \
      -v /my/data/:/data \
      3dcitydb/impexp COMMAND

.. _impexp_docker_build:

*******************************************************************************
Build your own images
*******************************************************************************

3DCityDB Importer/Exporter images are easily built on your own. The images
support two build arguments:

.. option:: BUILDER_IMAGE_TAG=<tag of the builder image>

  Set the tag of the builder image, which is ``openjdk`` for the Debian and
  ``adoptopenjdk/openjdk11`` for the Alpine image variant. This base image is
  only used for building the Importer/Exporter from source.

.. option:: RUNTIME_IMAGE_TAG=<tag of the runtime image>

  Set the tag of the runtime image, which is ``openjdk`` for the Debian and
  ``adoptopenjdk/openjdk11`` for the Alpine image variant. This is the base
  image the container runs with.

.. rubric:: Build process

1. Clone the `Importer/Exporter Github repository <https://github.com/3dcitydb/
   importer-exporter>`_ and navigate to the cloned repo:

   .. code-block:: bash

    git clone https://github.com/3dcitydb/importer-exporter.git
    cd importer-exporter

2. Build the image using `docker build <https://docs.docker.com
   /engine/reference/commandline/build/>`_:

  .. code-block:: bash

    # Debian variant
    docker build . \
      -t 3dcitydb/impexp

    # Alpine variant
    docker build . \
      -t 3dcitydb/impexp \
      -f Dockerfile.alpine

.. _impexp_docker_examples:

*******************************************************************************
Examples
*******************************************************************************

For the following examples we assume that a 3DCityDB instance with the following
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

**Importing CityGML**

This section provides some examples for importing CityGML datasets. Refer to
:numref:`impexp_cli_import_command` for a detailed description of the
Importer/Exporter CLI import command.

Import the CityGML dataset ``/home/me/mydata/bigcity.gml`` on you host system
into the DB given in :numref:`impexp_docker_code_exampledb`:

.. code-block:: bash

  docker run --rm --name impexp \
      -v /home/me/mydata/:/data \
    3dcitydb/impexp import \
      -H my.host.de -d citydb -u postgres -p changeMe! \
      bigcity.gml

.. note:: Since the host directory ``/home/me/mydata/`` is mounted to the default
   working directory ``/data`` inside the container, you can simply
   reference your input file by its filename instead of using an absolute path.

Import all CityGML datasets from ``/home/me/mydata/`` on your host system
into the DB given in :numref:`impexp_docker_code_exampledb`:

.. code-block:: bash

  docker run --rm --name impexp \
      -v /home/me/mydata/:/data \
    3dcitydb/impexp import \
      -H my.host.de -d citydb -u postgres -p changeMe! \
      /data/

**Exporting CityGML**

This section provides some examples for exporting CityGML datasets. Refer to
:numref:`impexp_cli_export_command` for a detailed description of the
Importer/Exporter CLI export command.

Export all data from the DB given in :numref:`impexp_docker_code_exampledb` to
``/home/me/mydata/output.gml``:

.. code-block:: bash

  docker run --rm --name impexp \
      -v /home/me/mydata/:/data \
    3dcitydb/impexp export \
      -H my.host.de -d citydb -u postgres -p changeMe! \
      -o output.gml

.. impexp_docker_example_link_citydb

Importer/Exporter Docker combined with 3DCityDB Docker
===============================================================================

This example shows how to use the 3DCityDB and Importer/Exporter Docker images
in conjuction. Let's assume we have a CityGML containing a few buildings
file on our Docker host at: ``/d/temp/buildings.gml``

First, let's bring up a 3DCityDB instance using the
:ref:`3DCityDB Docker images <citydb_docker_chapter>`.
As the emphasized line shows, we name the container ``citydb``.

.. code-block:: bash
  :emphasize-lines: 1

  docker run -d --name citydb \
      -e POSTGRES_PASSWORD=changeMe! \
      -e SRID=25832 \
    3dcitydb/3dcitydb-pg:edge-alpine

The next step is to :ref:`import <impexp_cli_import_command>` our data to
the 3DCityDB container. Therefore, we need to mount our data directory to
the container, as shown in line 3.
The emphasized lines show how to use the container name from the first step
as hostname by using Docker `legacy container links <https://docs.docker.com/
network/links/>`_ (``--link``).

.. note:: There are many other networking options to connect Docker containers.
  Take a look at the Docker `networking overview <https://docs.docker.com/
  network/>`_ to learn more.

.. code-block:: bash
  :linenos:
  :emphasize-lines: 2,5

  docker run -i -t --rm --name impexp \
      --link citydb \
      -v /d/temp:/data \
    3dcitydb/impexp:edge-alpine import \
      -H citydb \
      -d postgres \
      -u postgres \
      -p changeMe! \
      /data/building.gml

Now, with our data inside the 3DCityDB, let's use the Importer/Exporter to
create a :ref:`visualization export <impexp_cli_export_vis_command>`.
We are going to export all Buildings in LoD 2 as binary glTF with embedded
textures and draco compression enabled. All Buildings will be translated to
elevation 0 to fit in a visualization without terrain model.

.. code-block:: bash

  docker run -i -t --rm --name impexp \
      --link citydb \
      -v /d/temp:/data \
    3dcitydb/impexp:edge-alpine export-vis \
      -H citydb \
      -d postgres \
      -u postgres \
      -p changeMe! \
      -l 2 \
      -D collada \
      -G \
      --gltf-binary \
      --gltf-embed-textures \
      --gltf-draco-compression \
      -O globe \
      -o /data/building_glTf.kml

The export file are now available in ``/d/temp``.

.. code-block:: console

  $ ls -lhA /d/temp

  drwxrwxrwx 1 theUser theUser 4.0K May  6 17:51 Tiles/
  -rwxrwxrwx 1 theUser theUser 1.4K May  6 17:55 building_glTf.kml*
  -rwxrwxrwx 1 theUser theUser  310 May  6 17:55 building_glTf_collada_MasterJSON.json*
  -rwxrwxrwx 1 theUser theUser 3.2M May  5 16:25 buildings.gml*

As we are done now, the 3DCityDB container is no longer needed and can be removed:

.. code-block:: bash

  docker rm -f -v citydb

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
