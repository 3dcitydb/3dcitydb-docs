.. _first_steps_docker_chapter:

###############################################################################
Docker Images
###############################################################################

`Docker <https://docker.com>`_ is a widely used virtualization technology
that makes it possible to pack an application with all its required resources
into a standardized unit - the *Docker Container*. Software encapsulated in
this way can run on Linux, Windows, macOS and most cloud services
without any further changes or setup process. Docker containers are lightweight
compared to traditional virtualization environments that emulate an entire
operating system because they contain only the application and all the
tools, program libraries, and files it requires.

Docker images are available for the following tools of the 3DCityDB software
suite:

- :doc:`3D City Database <../3dcitydb/docker>`
- :doc:`3D City Database Importer/Exporter <../impexp/docker>`
- :doc:`3D Web Map Client <../webmap/docker>`
- :doc:`3DCityDB Web Feature Service (WFS) <../wfs/docker>`

All images are available from
`3DCityDB DockerHub <https://hub.docker.com/orgs/3dcitydb>`_.

`Docker Compose <https://hub.docker.com/u/tumgis/>`_ files are available
for orchestrating the individual services. This allows for example,
that a single command call can be used to create a 3DCityDB linked to a
3DCityDB WFS, which makes the data from the database accessible via a
standardized web interface.

*******************************************************************************
Getting started
*******************************************************************************
In this section you will find *quick start* code snippets for all 3DCityDB Docker
images to get you running in a few seconds.
For a more comprehensive documentation please visit the individual site of each
image.

3DCityDB Docker
===============================================================================

To run a PostgreSQL/PostGIS 3DCityDB container the only required settings are
a database password (``POSTGRES_PASSWORD``) the EPSG code of the coordinate
reference system (``SRID``) of the 3DCityDB instance. Use the
`docker run -p <https://docs.docker.com/engine/reference/run/>`_ switch to
define which port to expose to the host system for connections to the DB.

The detailed documentation for the 3DCityDB Docker image is available
:doc:`here <../3dcitydb/docker>`.

.. code-block:: bash
   :caption: 3DCityDB Docker Linux BASH quick start

   docker run -d -p 5432:5432 --name cdb \
     -e POSTGRES_PASSWORD=changeMe! \
     -e SRID=25832 \
   3dcitydb/3dcitydb-pg

.. code-block:: bash
   :caption: 3DCityDB Docker Windows CMD quick start

   docker run -d -p 5432:5432 --name cdb ^
     -e POSTGRES_PASSWORD=changeMe! ^
     -e SRID=25832 ^
   3dcitydb/3dcitydb-pg

Importer/Exporter Docker
===============================================================================

The detailed documentation for the 3DCityDB Importer/Exporter image is
available here.

.. code-block:: bash
   :caption: Importer/Exporter Docker Linux quick start

   docker run -i -t --name impexp --rm \
    -u $(id -u):$(id -g) \
    -v /local/share/dir:/share \
   3dcitydb/importer-exporter COMMAND OPTS ARGS

.. code-block:: bash
   :caption: Importer/Exporter Docker Windows quick start

   Quick Start code windows

3D-Web-Map-Client Docker
===============================================================================

The detailed documentation for the 3DCityDB 3D-Web-Map-Client Docker image is available here.

.. code-block:: bash
   :caption: 3D-Web-Map-Client Docker Linux quick start

   Quick Start code Linux

.. code-block:: bash
   :caption: 3D-Web-Map-Client Docker Windows quick start

   Quick Start code windows


Web-Feature-Service (WFS) Docker
===============================================================================

The detailed documentation for the 3DCityDB WFS Docker image is available here.

.. code-block:: bash
   :caption: WFS Docker Linux quick start

   Quick Start code Linux

.. code-block:: bash
   :caption: WFS Docker Windows quick start

   Quick Start code windows
