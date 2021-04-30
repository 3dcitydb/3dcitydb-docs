.. _citydb_docker_chapter:

###############################################################################
3D City Database using Docker
###############################################################################

.. image:: ../media/citydb_docker_logo.png
  :width: 80 px
  :align: right
  :alt: 3D City Datbase on Docker

The 3DCityDB Docker images are available for *PostgreSQL/PostGIS* and *Oracle*.
The PostgreSQL/PostGIS version is based on the official
`PostgreSQL <https://github.com/docker-library/postgres>`_ and
`PostGIS <https://github.com/postgis/docker-postgis>`_ Docker images.
The Oracle version is based on the
*Oracle Database Enterprise Edition* images available from the
`Oracle Container registry <https://container-registry.oracle.com>`_.
The images described here are available for 3DCityDB version ``v4.1.0`` and newer.
Older images, are available from
`TUM-GIS 3DCityDB Docker images <https://github.com/tum-gis/
3dcitydb-docker-postgis>`_.

When designing the images we tried to stay as close as possible to the behavior of
the base images and the :ref:`3DCityDB Shell scripts <3dcitydb_shell_scripts>`.
Thus, all configuration options you may be used to from the base images are
available for the 3DCityDB Docker images as well.

.. _citydb_docker_image_variants:

*******************************************************************************
Image variants and versions
*******************************************************************************

The images are available in various *variants* and *versions*. The
PostgreSQL/PostGIS images are available based on *Debian* and *Alpine Linux*,
the Oracles image are based on *Oracle Linux*.
:numref:`citydb_docker_tbl_images` gives an overview on the available image
versions.

.. list-table:: 3DCityDB Docker image variants and versions
  :widths: auto
  :header-rows: 1
  :stub-columns: 1
  :align: center
  :name: citydb_docker_tbl_images

  * - Tag
    - PostGIS (Debian)
    - PostGIS (Alpine)
    - Oracle
  * - edge
    - |psql-deb-build-edge| |psql-deb-size-edge|
    - |psql-alp-build-edge| |psql-alp-size-edge|
    - |ora-build-edge| |ora-size-edge|
  * - latest
    - |psql-deb-size-latest|
    - |psql-alp-size-latest|
    - |ora-size-edge|
  * - 4.1.0
    - |psql-deb-size-v4.1.0|
    - |psql-alp-size-v4.1.0|
    - |ora-size-edge|

.. _citydb_docker_image_pg:

PostgreSQL/PostGIS images
===============================================================================

The PostgreSQL/PostGIS images are available from
`3DCityDB Dockerhub <https://hub.docker.com/r/3dcitydb/3dcitydb-pg>`_ and
can be pulled like this:

.. code-block:: Shell

  docker pull 3dcitydb/3dcitydb-pg:TAG

The image tags are compose of the *base image version*, the
*3DCityDB version* and the *image variant*:

.. code-block::

  <base image version>-<3DCityDB version>-<image variant>

The base image version is inherited
from the `PostGIS Docker images <https://hub.docker.com/r/postgis/postgis/tags>`_.
Debian is the default image variant, where no image varaint is appended to the
tag. For the Alpine Linux images ``-alpine`` is appended. Currently,
following base image versions are supported:

.. list-table:: Overview on supported PostgreSQL/PostGIS versions
  :widths: auto
  :header-rows: 1
  :stub-columns: 1
  :align: center
  :name: citydb_docker_tbl_pgversions

  * - PostgreSQL/PostGIS version
    - 2.5
    - 3.0
    - 3.1
  * - 9.5
    - 9.5-2.5
    - 9.5-3.0
    -
  * - 9.6
    - 9.6-2.5
    - 9.6-3.0
    - 9.6-3.1
  * - 10
    - 10-2.5
    - 10-3.0
    - 10-3.1
  * - 11
    - 11-2.5
    - 11-3.0
    - 11-3.1
  * - 12
    - 12-2.5
    - 12-3.0
    - 12-3.1
  * - 13
    -
    - 13-3.0
    - 13-3.1

Here are some examples for full image tags:

.. code-block:: shell

  docker pull 3dcitydb/3dcitydb-pg:9.5-2.5-v4.1.0
  docker pull 3dcitydb/3dcitydb-pg:13-3.1-v4.1.0
  docker pull 3dcitydb/3dcitydb-pg:13-3.1-v4.1.0-alpine
  docker pull 3dcitydb/3dcitydb-pg:13-3.1-v4.1.0-alpine

.. _citydb_docker_image_oracle:

Oracle images
===============================================================================

Due to the current Oracle licensing conditions we cannot offer Oracle images
in a public repository like `DockerHub <https://hub.docker.com/>`_. However,
you can easily build the images yourself. A detailed description of how to
do that is available in :numref:`citydb_docker_oracle_build`.

.. _citydb_docker_config:

*******************************************************************************
Usage and configuration
*******************************************************************************

A 3DCityDB container is configured by settings environment variables inside
the container. For instance, this can be done using the ``-e VARIABLE=VALUE``
flag of `docker run <https://docs.docker.com/engine/reference/run/#env-
environment-variables>`_. The 3DCityDB Docker images introduce the variables
:option:`SRID`, :option:`HEIGHT_EPSG` and :option:`GMLSRSNAME`. Their behavior
is described here.
Fruthermore, some variables inherited from the base images offer important
configuration options, they are described below for the
:ref:`PostgreSQL/PostGIS <citydb_docker_config_psql>` and
:ref:`Oracle <citydb_docker_config_oracle>` image variants.

.. tip:: All variables besides :option:`POSTGRES_PASSWORD` and
  :option:`ORACLE_PASSWORD` are optional.

.. option:: SRID=<EPSG code>

  EPSG code for the 3DCityDB instance. If :option:`SRID` is not set,
  the 3DCityDB schema will not be setup in the default database and
  you will end up with a plain PostgreSQL/PostGIS or Oracle container.

.. option:: HEIGHT_EPSG=<EPSG code>

  EPSG code of the height system, omit or use 0 if unknown or
  :option:`SRID` is already 3D. This variable is used only for the automatic
  generation of :option:`GMLSRSNAME`.

.. option:: GMLSRSNAME=<mySrsName>

  If set, the automatically generated :option:`GMLSRSNAME` from :option:`SRID`
  and :option:`HEIGHT_EPSG` is overwritten. If not set, the variable will
  be created automatically like this:

  If only :option:`SRID` is set: :option:`GMLSRSNAME` =
  ``urn:ogc:def:crs:EPSG::SRID``

  If :option:`SRID` and :option:`HEIGHT_EPSG` are set:
  :option:`GMLSRSNAME` = ``urn:ogc:def:crs,crs:EPSG::SRID,crs:EPSG::HEIGHT_EPSG``

.. _citydb_docker_config_psql:

PostgreSQL/PostGIS environment variables
===============================================================================

The 3DCityDB PostgreSQL/PostGIS Docker images make use of the following
environment variables inherited from the official
`PostgreSQL <https://hub.docker.com/_/postgres>`_ and
`PostGIS <https://hub.docker.com/r/postgis/postgis>`_ Docker images. Refer to
the documentations of both images for much more configuration options.

.. option:: POSTGRES_DB=<database name>

  Sets name for the default database. If not set, the default database is named
  like :option:`POSTGRES_USER`.

.. option::  POSTGRES_USER=<username>

  Sets name for the database user, defaults to ``postgres``.

.. option:: POSTGRES_PASSWORD=<password>

  Sets the password for the database connection. This variable is **mandatory**.

.. option:: POSTGIS_SFCGAL=<any value>

  It set, `PostGIS SFCGAL <http://www.sfcgal.org/>`_ support is
  enabled. **Note:** SFCGAL is currently only available in the Debian image variant.
  Setting the variable on Apline images will have no effect.

.. _citydb_docker_config_oracle:

Oracle environment variables
===============================================================================

.. todo:: Describe environment variables below.

.. option:: DBUSER=<username>

  **Mandatroy:** Sets the name for the database user.


.. option:: ORACLE_PWD=<password>

  **Mandatroy:** Sets the password for the database connection.

.. option:: ORACLE_PDB=<???>

  **TODO**

.. option:: DBVERSION=<???>

  **TODO**

.. option:: VERSIONING=<???>

  **TODO**

.. code-block:: bash
   :caption: 3DCityDB Docker Linux quick start

   docker run -i -t -p 5432:5432 --name cdb \
   -e POSTGRES_DB=citydb \          # Optional: Set name for the DB. IF not set, DB is named like POSTGRES_USER (default=postgres)
   -e POSTGRES_PASSWORD=postgres \  # Required: Set DB Password
   -e SRID=25832 \                  # If SRID is not set, no 3DCityDB instance is set up in the container
   -e HEIGHT_EPSG=7837 \            # Optional: Height EPSG for auto GMLSRSNAME generation
   -e GMLSRSNAME=EPSG:25832 \       # Optional: Overwrites auto generated GMLSRSNAME from SRID and HEIGHT_EPSG
   -e POSTGIS_SFCGAL=true \         # Optional: Enable SFCGAL support, only currently available in Debian images, default = false
   3dcitydb/3dcitydb-pg

   # Behavior GMLSRSNAME

   # only SRID set              GMLSRSNAME="urn:ogc:def:crs:EPSG::$SRID"
   # SRID and HEIGHT_EPSG set   GMLSRSNAME="urn:ogc:def:crs,crs:EPSG::$SRID,crs:EPSG::$HEIGHT_EPSG"
   # GMLSRSNAME set             GMLSRSNAME=GMLSRSNAME





The Docker Container for 3D City Database is based on the Open Source
database management system PostgreSQL and the PostGIS extension for
spatial data. The image is freely available via
`DockerHub <https://hub.docker.com/u/tumgis/>`_ and can
be directly downloaded and used. The detailed documentation and source
code can be found on the GitHub project page (see below). All that is
needed is a Docker installation on your system. The time-consuming
installation of a database server, its configuration, the installation
of a database extension for spatial data and the setup of the 3D City
Database data model are a thing of the past. An example for setting up a
3DCityDB using Docker from a command line is given below:

**Windows**

.. code:: bash

    docker run -dit --name citydb-container -p 5432:5432^
        -e "SRID=31468"^
        -e "SRSNAME=urn:adv:crs:DE_DHDN_3GK4*DE_DHN92_NH"^
        tumgis/3dcitydb-postgis

**Linux**

.. code:: bash

    docker run -dit --name citydb-container -p 5432:5432 \
        -e "SRID=31468" \
        -e "SRSNAME=urn:adv:crs:DE_DHDN_3GK4*DE_DHN92_NH" \
        tumgis/3dcitydb-postgis

.. note::
   In the examples above the long commands are broken to several
   lines for readability using the Bash (\\) or CMD (^) line continuation.

The ``docker run`` command fetches the most recent version of the Docker
image from the Docker hub. This image includes a PostgreSQL/PostGIS
installation. The 3DCityDB schema is being installed and a new and empty
3DCityDB database is created using the SRID 31468 and GML SRSName
*urn:adv:crs:DE_DHDN_3GK4*DE_DHN92_NH*. After completion of the command
the user can directly start importing a CityGML file into the database
using the Importer/Exporter tool, which must have been installed
locally.


.. _citydb_docker_build:

*******************************************************************************
How to build images
*******************************************************************************

.. _citydb_docker_psql_build:

PostgreSQL/PostGIS
===============================================================================

.. _citydb_docker_oracle_build:

Oracle
===============================================================================




.. Images ---------------------------------------------------------------------

.. edge

.. |psql-deb-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/3dcitydb/psql-docker-build-edge?label=Debian&
  style=flat-square&logo=Docker&logoColor=white
  :target: https://hub.docker.com/r/3dcitydb/3dcitydb-pg

.. |psql-deb-size-edge| image:: https://img.shields.io/docker/image-size/
  3dcitydb/3dcitydb-pg/edge?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/3dcitydb-pg

.. |psql-alp-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/3dcitydb/psql-docker-build-edge?label=Alpine&
  style=flat-square&logo=Docker&logoColor=white
  :target: https://hub.docker.com/r/3dcitydb/3dcitydb-pg

.. |psql-alp-size-edge| image:: https://img.shields.io/docker/image-size/
  3dcitydb/3dcitydb-pg/edge-alpine?label=image%20size&logo=Docker&logoColor=white&
  style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/3dcitydb-pg

.. |ora-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/3dcitydb/oracle-docker-build-edge?label=Oracle%20Linux&
  style=flat-square&logo=Docker&logoColor=white
  :target: :ref:`citydb_docker_oracle_build`

.. |ora-size-edge| image:: https://img.shields.io/static/v1?label=image%20size&message=
  %3E3%20GB&color=blue&style=flat-square&logo=Docker&logoColor=white
  :target: :ref:`citydb_docker_oracle_build`

.. latest

.. |psql-deb-size-latest| image:: https://img.shields.io/docker/image-size/
  3dcitydb/3dcitydb-pg/latest?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/3dcitydb-pg

.. |psql-alp-size-latest| image:: https://img.shields.io/docker/image-size/
  3dcitydb/3dcitydb-pg/latest-alpine?label=image%20size&logo=Docker&logoColor=white&
  style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/3dcitydb-pg


.. 4.1.0

.. |psql-deb-size-v4.1.0| image:: https://img.shields.io/docker/image-size/
  3dcitydb/3dcitydb-pg/13-3.1-4.1.0?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/3dcitydb-pg

.. |psql-alp-size-v4.1.0| image:: https://img.shields.io/docker/image-size/
  3dcitydb/3dcitydb-pg/13-3.1-4.1.0-alpine?label=image%20size&logo=Docker&logoColor=white&
  style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/3dcitydb-pg
