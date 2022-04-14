.. _wfs_docker_chapter:

###############################################################################
Web Feature Service using Docker
###############################################################################

The 3DCityDB Web Feature Service (WFS) Docker image exposes the capabilities
of the :ref:`wfs_chapter` for dockerized applications and workflows.
Using the WFS Docker you can expose the features stored in a 3DCityDB instance
through an `OGC WFS <https://www.ogc.org/standards/wfs>`_ interface offering a
rich set of features like advanced filter capabilities. For a basic configuration
just the connection credentials of the 3DCityDB (``CITYDB_*`` environment variables) have to
be specified.
All WFS :ref:`functionalities <wfs_basic_functionality_chapter>` are supported by
the images.

.. rubric:: Synopsis

.. code-block:: bash
  :name: wfs_docker_code_synopsis

  docker run --name wfs [-d] -p 8080:8080 \
      [-e CITYDB_TYPE=postgresql|oracle] \
      [-e CITYDB_HOST=the.host.de] \
      [-e CITYDB_PORT=thePort] \
      [-e CITYDB_NAME=theDBName] \
      [-e CITYDB_SCHEMA=theCityDBSchemaName] \
      [-e CITYDB_USERNAME=theUsername] \
      [-e CITYDB_PASSWORD=theSecretPass] \
      [-e WFS_CONTEXT_PATH=wfs-context-path] \
      [-e WFS_ADE_EXTENSIONS_PATH=/path/to/ade-extensions/] \
      [-e WFS_CONFIG_FILE=/path/to/config.xml] \
      [-v /my/data/config.xml:/path/to/config.xml] \
    3dcitydb/wfs[:TAG]

When running containers with default settings, the
:ref:`WFS <wfs_basic_functionality_chapter>` will listen at following URL.
Note that the web root is used as context path in this case.

.. code-block:: bash

  http[s]://[host][:port]/wfs

The :ref:`Web-based client <wfs_web_based_client_chapter>` is available here:

.. code-block:: bash

  http[s]://[host][:port]/wfsclient

.. _wfs_docker_image_variants:

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
The **latest** and **release** image versions  are only built
when a new release is published on Github. The **latest** tag will point to
the most recent release version.

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
  * - 5.0.0
    - |deb-size-v5.0.0|
    - |alp-size-v5.0.0|

.. note::
  | Minor releases are not listed in this table.
  | The latest 3DCityDB WFS version is: |version-badge-github|
  | The latest image version on DockerHub is: |version-badge-dockerhub|

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
  docker pull 3dcitydb/wfs:5.0.0
  docker pull 3dcitydb/wfs:5.0.0-alpine

.. _wfs_docker_image_usage:

*******************************************************************************
Usage and configuration
*******************************************************************************

A 3DCityDB WFS Docker container is configured using environment variables and
a WFS ``config.xml`` file.
The easiest way of using the WFS Docker is to use the default ``config.xml``
shipped inside the image and by setting the database connection details
and/or the web context path through environment variables.
The default config file exposes all filter capabilities and feature types from the
connected database to the WFS and should be suitable for most situations.

If you require more specific settings, get a copy of
:download:`default-config.xml <https://raw.githubusercontent.com/3dcitydb/web-feature-service/master/resources/docker/default-config.xml>`
and build your own config file (see :ref:`wfs_configuration_chapter`).
Mount your custom config file to the container at runtime
(see `docker run docs <https://docs.docker.com/engine/reference/run/>`_).
To apply the custom config file set the :option:`WFS_CONFIG_FILE` option.

All available environment variables are listed and described below.

.. note:: The environment variables are *optional*. If you do not provide them, make sure that your ``config.xml``
   file contains all settings (including database connection details) required to run the service. Otherwise, the
   WFS will throw error messages when starting the container. If you use environment variables though, they
   **always take precedence** over corresponding settings in the config.xml file. Thus, you can create custom config
   files and use them with different databases by overwriting the settings with the environment variables.

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

.. option:: WFS_CONFIG_FILE=</path/to/custom/config.xml>

  Path of the WFS config file to use. See :ref:`above <wfs_docker_image_usage>`
  how to create and use a custom config file.

.. option:: WFS_CONTEXT_PATH=<wfs-context-path>

  The URL subpath where the WFS is served (see :numref:`wfs_service_url_chapter`). The default value is
  ``ROOT``, for serving from the web root. **Note:** Nested paths are currently not supported.
  For instance, set ``WFS_CONTEXT_PATH=citydb-wfs`` to serve from
  ``http[s]://my-domain/citydb-wfs/``.

.. option:: WFS_ADE_EXTENSIONS_PATH=</path/to/ade-extension/>

  Allows for providing an alternative directory where the WFS service shall
  search for ADE extensions (default: *ade-extensions* folder is the *WEB-INF* directory).
  The WFS service must have read access to this directory (see
  :numref:`wfs_configuration_chapter` for more details).

.. _wfs_docker_build:

*******************************************************************************
Build your own images
*******************************************************************************

3DCityDB WFS images can easily be built on your own. The images support the
following build arguments:

.. option:: BUILDER_IMAGE_TAG=<11.0.12-jdk-slim'>

  Tag of the builder base image, https://hub.docker.com/_/openjdk.

.. option:: RUNTIME_IMAGE_TAG=<9-alpine>

  Tag of the runtime image, https://hub.docker.com/_/tomcat.

.. option:: DEFAULT_CONFIG=</path/to/default/config.xml>

  Name of the default config file shall that shall be copied into the image and used by default
  when running a container. The config file must be located inside the *resources/docker* folder
  (default: `default-config.xml`).

.. option:: TOMCAT_USER=<tomcat>

  Name of the user running the Tomcat service inside the container (default: *tomcat*).
  Note that the user is assigned the fixed UID = 1000.

.. option:: TOMCAT_GROUP=<tomcat>

  Name of the group that the user shall be assigned to (default: *tomcat*).
  Note that the group is assigned the fixed GID = 1000.

.. rubric:: Build process

1. Clone the `WFS Github repository <https://github.com/3dcitydb/
   web-feature-service>`_ and navigate to the cloned repo:

   .. code-block:: bash

    git clone https://github.com/3dcitydb/web-feature-service.git
    cd web-feature-service

2. Build the image using `docker build <https://docs.docker.com
   /engine/reference/commandline/build/>`_:

  .. code-block:: bash

    # Debian variant
    docker build . \
      -t 3dcitydb/wfs:edge

    # Alpine variant
    docker build . \
      -t 3dcitydb/wfs:edge-alpine \
      -f Dockerfile.alpine

.. _wfs_docker_examples:

*******************************************************************************
Examples
*******************************************************************************

This example shows how to bring up a 3DCityDB WFS with the Importer/Exporter and
3DCityDB Docker images. In this example we are going to provide the
:download:`LoD3 Railway dataset <https://github.com/3dcitydb/importer-exporter/raw/92e08aa306611ee850e065bb542bb3d60791a54f/resources/samples/Railway%20Scene/Railway_Scene_LoD3.zip>`
via WFS and run some example queries.

.. rubric:: Database creation and data import

.. note:: A more detailed example on importing data using the 3DCityDB Docker images
  is available :ref:`here <impexp_docker_example_link_citydb>`.

1. Download the dataset, create a folder and put the downloaded file in the new folder.
   In the following we assume the file is at ``/my/data/Railway_Scene_LoD3.zip``.

2. Create a `Docker network <https://docs.docker.com/network/>`_ and a
   :ref:`3DCityDB Docker <citydb_docker_chapter>` container for our dataset:

  .. code-block:: bash

    docker network create citydb-net

    docker run -d --name citydb \
      --network citydb-net \
      -e "POSTGRES_PASSWORD=changeMe" \
      -e "SRID=3068" \
    3dcitydb/3dcitydb-pg:latest-alpine

3. Import the dataset using the
   :ref:`3DCityDB Importer/Exporter Docker <impexp_docker_chapter>`:

  .. code-block:: bash

    docker run -i -t --rm --name impexp \
        --network citydb-net \
        -v /my/data:/data \
      3dcitydb/impexp:latest-alpine import \
        -H citydb \
        -d postgres \
        -u postgres \
        -p changeMe \
        /data/Railway_Scene_LoD3.zip


.. rubric:: WFS configuration and testing

Start a 3DCityDB WFS container. We are going to expose port 8080 to the host system
for the service and serve WFS content from ``/citydb-wfs``.

.. code-block:: bash

  docker run -d --name wfs \
      -p 8080:8080 \
      --network citydb-net \
      -e CITYDB_HOST=citydb \
      -e CITYDB_NAME=postgres \
      -e CITYDB_USERNAME=postgres \
      -e CITYDB_PASSWORD=changeMe \
      -e WFS_CONTEXT_PATH=citydb-wfs \
    3dcitydb/wfs:latest-alpine

.. note:: The 3DCityDB, Importer/Exporter and WFS Docker containers are attached to the same
  Docker network ``citydb-net`` we created in the beginning. Thus, container names (e.g. ``citydb``)
  can be use as hostnames for communication between the containers. See
  `Docker network docs <https://docs.docker.com/network/>`_ for more Docker networking options.

Now the WFS should be up and running. Let's check if the service started using
``docker logs``:

.. code-block:: console

  $ docker logs -n 5 wfs

  03-Sep-2021 12:24:14.036 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/usr/local/tomcat/webapps/host-manager]
  03-Sep-2021 12:24:14.049 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/tomcat/webapps/host-manager] has finished in [13] ms
  03-Sep-2021 12:24:14.052 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
  03-Sep-2021 12:24:14.058 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["ajp-nio-8009"]
  03-Sep-2021 12:24:14.061 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in 515 ms

If you see output similar to this, the service started successfully.

.. rubric:: Get WFS capabilities

The service is listening on port 8080 on our local machine, the Web-based client
can be accessed from a browser:

* WFS service endpoint: ``http://localhost:8080/citydb-wfs/wfs``
* WFS Web-based client: ``http://localhost:8080/citydb-wfs/wfsclient``

Let's query the capabilities document to check what our WFS can do. We are going to use
`curl <https://de.wikipedia.org/wiki/CURL>`_ for this:

.. code-block:: bash

  serviceURL='http://localhost:8080/citydb-wfs/wfs?'
  query='SERVICE=WFS&REQUEST=GetCapabilities'
  curl -v "$serviceURL$query"

The capabilities document returned looks like this:

.. code-block:: xml

  <?xml version="1.0" standalone="yes"?>
  <wfs:WFS_Capabilities xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:gml="http://www.opengis.net/gml" xmlns:wtr="http://www.opengis.net/citygml/waterbody/2.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:veg="http://www.opengis.net/citygml/vegetation/2.0" xmlns:tran="http://www.opengis.net/citygml/transportation/2.0" xmlns:dem="http://www.opengis.net/citygml/relief/2.0" xmlns:grp="http://www.opengis.net/citygml/cityobjectgroup/2.0" xmlns:bldg="http://www.opengis.net/citygml/building/2.0" xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:tun="http://www.opengis.net/citygml/tunnel/2.0" xmlns:frn="http://www.opengis.net/citygml/cityfurniture/2.0" xmlns:gen="http://www.opengis.net/citygml/generics/2.0" xmlns:brid="http://www.opengis.net/citygml/bridge/2.0" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:luse="http://www.opengis.net/citygml/landuse/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/wfs/2.0 http://schemas.opengis.net/wfs/2.0/wfs.xsd" version="2.0.0">
    <ows:ServiceIdentification>
      <ows:Title>3DCityDB Web Feature Service</ows:Title>
      <ows:ServiceType>WFS</ows:ServiceType>
      <ows:ServiceTypeVersion>2.0.0</ows:ServiceTypeVersion>
    </ows:ServiceIdentification>
    <ows:ServiceProvider>
      <ows:ProviderName/>
      <ows:ServiceContact/>
    </ows:ServiceProvider>
    <ows:OperationsMetadata>
      <ows:Operation name="GetCapabilities">
        <ows:DCP>
          <ows:HTTP>
            <ows:Get xlink:href="http://localhost:8080/citydb-wfs/wfs"/>
            <ows:Post xlink:href="http://localhost:8080/citydb-wfs/wfs"/>

  <!-- ... -->
  <!-- ... -->

        </fes:SpatialOperators>
      </fes:Spatial_Capabilities>
    </fes:Filter_Capabilities>
  </wfs:WFS_Capabilities


.. rubric:: Example query: Feature by ID

Now let's query a feature by ID (``GMLID_BUI46739_1739_10911``) from the WFS.

The WFS request for this looks like this and is stored in ``request.xml``:

.. code-block:: xml
  :caption: request.xml

  <?xml version="1.0" encoding="UTF-8"?>
  <wfs:GetFeature service="WFS" version="2.0.0" xmlns:wfs="http://www.opengis.net/wfs/2.0">
    <wfs:StoredQuery id="http://www.opengis.net/def/query/OGC-WFS/0/GetFeatureById">
      <wfs:Parameter name="id">GMLID_BUI46739_1739_10911</wfs:Parameter>
    </wfs:StoredQuery>
  </wfs:GetFeature>


Let's send a POST request with the content from ``request.xml`` to the WFS and
and write the output to ``building.gml``:

.. code-block:: bash

  curl -v \
    -X POST \
    -H 'Content-Type: text/xml' \
    -d "@request.xml" \
    "http://localhost:8080/citydb-wfs/wfs" > building.gml

The shortened and beautified content of ``building.gml`` looks like this:

.. code-block:: xml

  <?xml version="1.0" standalone="yes"?>
  <bldg:Building gml:id="GMLID_BUI46739_1739_10911">
    <gml:description>Simple Chapel with a recess/loggia</gml:description>
    <gml:name>Chapel KIT/KHH-1</gml:name>
    <gml:boundedBy>
      <gml:Envelope srsName="urn:ogc:def:crs:EPSG::3068" srsDimension="3">
        <gml:lowerCorner>-299.374655062533 575.1129259060015 103.648365247638</gml:lowerCorner>
        <gml:upperCorner>-272.47917424008 596.1169211194645 121.04746928772363</gml:upperCorner>
      </gml:Envelope>
    </gml:boundedBy>
    <core:creationDate>2021-09-03</core:creationDate>
    <core:relativeToTerrain>entirelyAboveTerrain</core:relativeToTerrain>
    <bldg:outerBuildingInstallation>
      <bldg:BuildingInstallation gml:id="UUID_071439a3-5cd7-4ace-b0cb-4cedec5a6540">
        <gml:name>Tower</gml:name>
        <core:creationDate>2021-09-03</core:creationDate>
        <core:relativeToTerrain>entirelyAboveTerrain</core:relativeToTerrain>
        <bldg:function>1040</bldg:function>
        <bldg:lod3Geometry>
          <gml:MultiSurface gml:id="UUID_87c65640-96ad-42d2-aa2d-367245f4a865">

  <!-- ... -->
  <!-- ... -->

.. rubric:: Shutdown and cleanup

When the services and network are no longer required, they can be removed:

.. code-block:: bash

  docker rm citydb wfs
  docker network remove citydb-net

.. Images ---------------------------------------------------------------------

.. version badges

.. |version-badge-github| image:: https://img.shields.io/github/v/release/3dcitydb/web-feature-service?label=Github&logo=github
  :target: https://github.com/3dcitydb/web-feature-service/releases

.. |version-badge-dockerhub| image:: https://img.shields.io/docker/v/3dcitydb/wfs?label=Docker%20Hub&logo=docker&logoColor=white&sort=semver
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags

.. edge

.. |deb-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/web-feature-service/docker-build-edge?
  style=flat-square&logo=Docker&logoColor=white
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |alp-build-edge| image:: https://img.shields.io/github/workflow/status/
  3dcitydb/web-feature-service/docker-build-edge-alpine?
   style=flat-square&logo=Docker&logoColor=white
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |deb-size-edge| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/edge?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |alp-size-edge| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/edge-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. latest

.. |deb-size-latest| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/latest?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |alp-size-latest| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/latest-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. 5.0.0

.. |deb-size-v5.0.0| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/5.0.0?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated

.. |alp-size-v5.0.0| image:: https://img.shields.io/docker/image-size/
  3dcitydb/wfs/5.0.0-alpine?label=image%20size&logo=Docker&logoColor=white&style=flat-square
  :target: https://hub.docker.com/r/3dcitydb/wfs/tags?page=1&ordering=last_updated