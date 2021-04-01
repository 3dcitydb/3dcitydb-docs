.. _impexp_cli_import_command:

Import command
---------------

**Synopsis**

.. code-block:: bash

   impexp import [-hV] [--ade-extensions=<folder>] [-c=<file>]
                 [--import-log=<file>] [--input-encoding=<encoding>]
                 [--log-file=<file>] [--log-level=<level>]
                 [--pid-file=<file>] [--plugins=<folder>]
                 [--worker-threads=<threads[,max]>] [[[-t=<[prefix:]name>[,<
                 [prefix:]name>...]]... [--namespace=<prefix=name>[,
                 <prefix=name>...]]...] [-i=<id>[,<id>...] [-i=<id>[,
                 <id>...]]...] [-b=<minx,miny,maxx,maxy[,srid]>
                 [--bbox-mode=<mode>]] [[--count=<count>]
                 [--start-index=<index>]] [[--no-appearance]]]
                 [[-T=<database>] -H=<host> [-P=<port>] -d=<name>
                 [-S=<schema>] -u=<name> [-p[=<password>]]]
                 [@<filename>...] <file>...

**Description**

The ``import`` command imports one or more CityGML or CityJSON files
into the 3D City Database. It corresponds to the import operation
offered on the *Import* tab of the graphical user interface (see :numref:`impexp_citygml_import_chapter`).
The command provides a range of options to adapt the import process.
If you miss settings available for the GUI version of this command,
you can use the :option:`--config` option to pass a config file
containing these settings.

**General options**

.. option:: <file>...

   One or more input files or directories to be imported. This parameter is mandatory.
   Glob patterns are supported to specify a set of filenames using wildcard characters
   (e.g., ``/path/to/*.gml``). If a directory is provided, it will be recursively scanned
   for CityGML/CityJSON files. The supported input file formats and extensions are
   identical with those listed in :numref:`import_supported_file_formats`.

.. option:: --input-encoding=<encoding>

   Specify the encoding of the input file(s). For XML documents, the encoding is automatically
   parsed from the XML declaration at the beginning of the dataset if present. Provide an official
   IANA-based character encoding name. The default value is UTF-8.

.. option:: --import-log=<file>

   If you want an import log to be created for all top-level features loaded in the database,
   provide the path to the import log file with this option. More information about the
   import log can be found in :numref:`impexp_import_preferences_import_log`.

.. option:: --worker-threads=<threads[,max]>

   Number of parallel threads to use for the import process. You can also specify a
   minimum and maximum number of threads separated by commas. A general description
   of the multithreaded processing used for the import process is provided in
   :numref:`impexp_import_preferences_resources_chapter`.

**Import filter options**

The ``import`` command offers additional options to define both thematic and spatial filters
that are used to narrow down the set of top-level city objects to be imported from the
input file(s).

.. option:: -t, --type-name=<[prefix:]name>[,<[prefix:]name>...]

   Comma-separated list of one or more names of the top-level feature types to be imported. The
   type names are case sensitive and shall match one of the official CityGML feature
   type names or a feature type name from a CityGML ADE. To avoid ambiguities,
   you can use an optional prefix for each name. The prefix must be associated with the
   official XML namespace of the feature type. You can either use the official CityGML namespace
   prefixes listed in :numref:`impexp_toplevel_feature_types_table`. Or you can use
   the :option:`--namespace` option to declare your own prefixes.

.. option:: --namespace=<prefix=name>[,<prefix=name>...]

   Used to specify namespaces and their prefixes as comma-separated list of one or more
   ``prefix=name`` pairs. The prefixes can be used in other options such as :option:`--type-name`.

.. option:: -i, --resource-id=<id>[,<id>...]

   Comma-separated list of one or more identifiers. Only top-level features having a matching
   value for their identifier attribute will be imported.

.. option:: -b, --bbox=<minx,miny,maxx,maxy[,srid]>

   2D bounding box to use as spatial filter. The bounding box is given by four coordinates
   that define its lower left and upper right corner. By default, the coordinates are assumed
   to be in the same CRS that is used by the 3DCityDB instance. Alternatively, you
   can provide the database ``srid`` of the CRS associated with the coordinates as fifth value
   (e.g. ``4326`` for WGS84). All values must be separated by commas. The bounding box is evaluated
   against the gml:boundedBy property (CityGML) respectively the “geographicalExtent” property
   (CityJSON) of the input features.

.. option:: --bbox-mode=<mode>

   Choose the mode of the bounding box filter. Allowed values are ``overlaps`` (default) and ``within``.
   When set to ``overlaps``, all features overlapping with the bounding box are imported. Otherwise,
   features must be ``within`` the given bounding box. Can only be used together with the :option:`--bbox` option.

.. option:: --count=<count>

   Maximum number of top-level features to be imported.

.. option:: --start-index=<index>

   Index within the set of all top-level features over all input file(s) from which the import
   shall begin. The start index uses zero-based numbering. Thus, the first top-level feature is
   assigned the index 0, rather than the index 1.

.. option:: --no-appearance

   Flag to indicate that appearances of the features shall not be imported.

.. include:: database-options.rst

**Examples**

.. code-block:: bash

   $ impexp import -H localhost -d citydb_v4 -u citydb_user -p my_password my_city.gml

Import the dataset ``my_city.gml`` into the 3DCityDB. The 3DCityDB is supposed
to be running on a PostgreSQL database on the same machine. The connection will
be established to the ``citydb_v4`` database with the user ``citydb_user`` and the
password ``my_password``.

.. code-block:: bash

   $ impexp import -H localhost -d citydb_v4 -p -u citydb_user \
                   -t Building,CityFurniture /path/to/**/*.json

The folder ``/path/to`` is recursively scanned for files with the extension ``.json``.
From these files, only ``Building`` and ``CityFurniture`` features will be imported.
Note that the password option :option:`-p` is provided without a value and, thus,
the user will be prompted for the password.

.. code-block:: bash

   $ impexp import -H localhost -d citydb_v4 -p -u citydb_user \
                   -b 13.3508824,52.4799281,13.3578297,52.4862805,4326 \
                   --bbox-mode=within my_city.gml

Import all city objects from the ``my_city.gml`` file that are ``within`` the provided
bounding box. The coordinates of the bounding box are given in WGS84. For
this reason, the fifth value ``4326`` of the :option:`-b` option denotes the SRID
that is used by the target database for the WGS84 reference system.

.. code-block:: bash

   $ impexp import -T oracle -H localhost -S other_user -d citydb_v4 -p -u citydb_user \
                   --count=10 --start-index=20 /my/city/model/files/

Recursively scan the folder ``/my/city/model/files/`` for all supported input files and import
them into a 3DCityDB running on Oracle. Only 10 features will be imported starting
from the 20th feature in the set. Note that the connection is established with the
user ``citydb_user`` but the data will be imported into the schema of the user
``other_user``. The ``citydb_user`` must therefore have been granted sufficient privileges.