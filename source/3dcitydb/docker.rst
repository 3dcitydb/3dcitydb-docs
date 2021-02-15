.. _citydb_docker_chapter:

###############################################################################
Docker
###############################################################################

.. warning:: THIS PAGE IS WORK IN PROGRESS


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
“urn:adv:crs:DE_DHDN_3GK4*DE_DHN92_NH”. After completion of the command
the user can directly start importing a CityGML file into the database
using the Importer/Exporter tool, which must have been installed
locally.