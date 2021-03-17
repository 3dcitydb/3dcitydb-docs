.. _citydb_docker_chapter:

###############################################################################
Docker
###############################################################################

The 3DCityDB Docker images are available for *PostgresSQL/PostGIS* and *Oracle*.
The PostgreSQL/PostGIS version is based on the official
`PostgresSQL <https://github.com/docker-library/postgres>`_ and
`PostGIS <https://github.com/postgis/docker-postgis>`_ Docker images.
The Oracle version is based on the
*Oracle Database Enterprise Edition* images available from the
`Oracle Container registry <https://container-registry.oracle.com>`_.

When designing the images we tried to stay as close as possible to the behavior of
the base images and the :ref:`3DCityDB Shell scripts <3dcitydb_shell_scripts>`.

.. _citydb_docker_image_variants:

*******************************************************************************
Image variants and versions
*******************************************************************************

The images are available in various *variants* and *versions*. The
PostgresSQL/PostGIS images are available based on *Debian* and *Alpine Linux*,
the Oracle image versions are based on *Oracle Linux*.
:numref:`citydb_docker_tbl_images` gives an overview on the available images.

.. table:: 3DCityDB Docker image variants and versions
  :name: citydb_docker_tbl_images

  +--------+---------------------------------------------------------------------------------------------------+-------------------------------------------------+
  | Tag    | PostgreSQL/PostGIS                                                                                | Oracle                                          |
  +========+=================================================+=================================================+=================================================+
  | edge   | |psql-deb-build-edge| |psql-deb-size-edge|      | |psql-alp-build-edge| |psql-alp-size-edge|      | |ora-build-edge| |ora-size-edge|                |
  +--------+-------------------------------------------------+-------------------------------------------------+-------------------------------------------------+
  | latest | |psql-deb-build-latest| |psql-deb-size-latest|  | |psql-alp-build-latest| |psql-alp-size-latest|  | |ora-build-latest| |ora-size-edge|              |
  +--------+-------------------------------------------------+-------------------------------------------------+-------------------------------------------------+
  | v4.1.0 | |psql-deb-build-v4.1.0| |psql-deb-size-v4.1.0|  | |psql-alp-build-v4.1.0| |psql-alp-size-v4.1.0|  | |ora-deb-build-v4.1.0| |ora-deb-size-v4.1.0|    |
  +--------+-------------------------------------------------+-------------------------------------------------+-------------------------------------------------+


.. _citydb_docker_usage:

*******************************************************************************
Usage and configuration
*******************************************************************************

A 3DCityDB is configured using environment variables passed to ``docker run``.
Following variables are available:

POSTGRES_DB
  Optional: Sets name for the database. IF not set, the DB is named like
  POSTGRES_USER (default=postgres).

POSTGRES_USER
  Sets the database username (default=postgres).

POSTGRES_PASSWORD
  Mandatory: Sets the password for the database connection.

SRID
  Optional: EPSG code for the 3DCityDB instance. If SRID is not specified,
  the 3DCityDB schema will not be setup in the default database.

HEIGHT_EPSG
  Optional:

GMLSRSNAME
  Optional: If set, overwrites the automatically generated GMLSRSNAME from SRID
  and HEIGHT_EPSG.

  If only SRID is set, ``GMLSRSNAME="urn:ogc:def:crs:EPSG::$SRID"``

  If SRID and HEIGHT_EPSG are set, ``GMLSRSNAME="urn:ogc:def:crs,crs:EPSG::$SRID,crs:EPSG::$HEIGHT_EPSG"``

POSTGIS_SFCGAL
  Optional: It set, PostGIS SFCGAL support is enabled

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

PostgresSQL/PostGIS
===============================================================================

.. _citydb_docker_oracle_build:

Oracle
===============================================================================




.. Images ---------------------------------------------------------------------

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
