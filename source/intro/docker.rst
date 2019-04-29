3DCityDB Docker Images
======================

|image224|\ Docker is a widely used virtualization technology that makes
it possible to pack an application with all its required resources into
a standardized unit - the *Docker Container*. Software encapsulated in
this way can run on Linux, Windows, macOS and most cloud services
without any further changes. Docker containers are lightweight compared
to traditional virtualization environments that emulate an entire
operating system because they contain only the application and all the
tools, program libraries, and files it requires.

Getting started
---------------

The Docker Container for 3D City Database is based on the Open Source
database management system PostgreSQL and the PostGIS extension for
spatial data. The image is freely available via DockerHub [21]_ and can
be directly downloaded and used. The detailed documentation and source
code can be found on the GitHub project page (see below). All that is
needed is a Docker installation on your system. The time-consuming
installation of a database server, its configuration, the installation
of a database extension for spatial data and the setup of the 3D City
Database data model are a thing of the past. An example for setting up a
3DCityDB using Docker from a command line is given below:

*Windows*

*docker run -dit --name citydb-container -p 5432:5432^*

*-e "SRID=31468"^*

*-e "SRSNAME=urn:adv:crs:DE_DHDN_3GK4*DE_DHN92_NH"^*

*tumgis/3dcitydb-postgis*

*Linux*

*docker run -dit --name citydb-container -p 5432:5432 \\*

*-e "SRID=31468" \\*

*-e "SRSNAME=urn:adv:crs:DE_DHDN_3GK4*DE_DHN92_NH" \\*

*tumgis/3dcitydb-postgis*

.. note::
   In the examples above the long commands are broken to several
   lines for readability using the Bash (\) or CMD (^) line continuation.

The docker run command fetches the most recent version of the Docker
image from the Docker hub. This image includes a PostgreSQL/PostGIS
installation. The 3DCityDB schema is being installed and a new and empty
3DCityDB database is created using the SRID 31468 and GML SRSName
“urn:adv:crs:DE_DHDN_3GK4*DE_DHN92_NH”. After completion of the command
the user can directly start importing a CityGML file into the database
using the Importer/Exporter tool, which must have been installed
locally.

Further images
--------------

In addition to the Docker Image for the 3D City Database, Docker Images
for the *3DCityDB Web-Feature-Service (WFS)* and the *3DCityDB
3D-Web-Map-Client* are also available.

Docker Compose [22]_ files are available for orchestrating the
individual services. This allows for example, that a single command call
can be used to create a 3DCityDB linked to a 3DCityDB WFS, which makes
the data from the database accessible via a standardized web interface.

**Downloads, documentation and source code**

The documentation and source code for the individual images can be found
on the Github project pages listed below. If you experience any problems
or want to contribute, please submit an Github issue or pull request.

*3DCityDB PostGIS*

-  Documentation, source code
   https://github.com/tum-gis/3dcitydb-docker-postgis

-  Image download https://hub.docker.com/r/tumgis/3dcitydb-postgis/

*3DCityDB Web Feature Service (WFS)*

-  Documentation, source code
   https://github.com/tum-gis/3dcitydb-wfs-docker

-  Image download https://hub.docker.com/r/tumgis/3dcitydb-wfs/

*3DCityDB 3D Web Map Client*

-  Documentation, source code
   https://github.com/tum-gis/3dcitydb-web-map-docker

-  Image download https://hub.docker.com/r/tumgis/3dcitydb-web-map/

*3DCityDB Docker Compose service orchestration*

-  Download, Documentation, and source code

..

   https://github.com/tum-gis/3dcitydb-docker-compose

.. |image224| image:: media/image234.png
   :width: 1.29921in
   :height: 1.59142in
