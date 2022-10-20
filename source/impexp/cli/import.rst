.. _impexp_cli_import_command:

Import command
---------------

**Synopsis**

.. code-block:: bash

   impexp import [-hV] [--[no-]fail-fast] [--ade-extensions=<folder>]
                 [-c=<file>] [--duplicate-log=<file>] [--import-log=<file>]
                 [--input-encoding=<encoding>] [--log-file=<file>]
                 [--log-level=<level>] [-o=<mode>] [--pid-file=<file>]
                 [--plugins=<folder>] [--use-plugin=<plugin[=true|false]>[,
                 <plugin[=true|false]>...]]... [--worker-threads=<threads[,
                 max]>] [[--creation-date=<mode>]
                 [--termination-date=<mode>] [--lineage=<lineage>]
                 [--updating-person=<name>] [--reason-for-update=<reason>]
                 [--use-metadata-from-file]] [[[-t=<[prefix:]name>[,<
                 [prefix:]name>...]]... [--namespace=<prefix=name>[,
                 <prefix=name>...]]...] [-i=<id>[,<id>...] [-i=<id>[,
                 <id>...]]...] [-b=<minx,miny,maxx,maxy[,srid]>
                 [--bbox-mode=<mode>]] [[--count=<count>]
                 [--start-index=<index>]] [[--no-appearance]]] [-f=<file>[,
                 <file>...] [-f=<file>[,<file>...]]... [-m=<mode>] [-w]
                 [[-n=<name>] [-I=<index>] [--[no-]header] [-D=<string>]
                 [-Q=<char>] [--quote-escape=<char>] [-M=<char>]
                 [--csv-encoding=<encoding>]]] [[-T=<database>] [-H=<host>]
                 [-P=<port>] [-d=<name>] [-S=<schema>] [-u=<name>] [-p
                 [=<password>]]] [@<filename>...] <file>...

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

.. option:: -o, --import-mode=<mode>

   Defines the mode for handling conflicting top-level city objects and for avoiding duplicate objects in
   the database. Allowed values are ``import_all`` (default), ``skip``, ``delete``, and ``terminate``.
   With ``import_all``, all top-level city objects from the input file(s) are imported, regardless of whether
   this results in duplicate objects. For the other modes, the object identifier of each top-level city object
   to be imported is queried in the database to check whether a city object with identical identifier already
   exists in the database. If no duplicate is found, the city object is directly imported. Otherwise, ``skip``
   means that the city object from the input file is skipped and not imported. Thus, the object in the database
   takes precedence. With ``delete``, the city object from the input file takes precedence and overwrites the
   duplicate object in the database. For this purpose, the duplicate is first *physically deleted* before
   importing the object from the input file. When choosing ``terminate`` instead of ``delete``, the duplicate
   object is only *terminated*.

.. option:: --import-log=<file>

   If you want an import log to be created for all top-level features loaded in the database,
   provide the path to the import log file with this option. Note that the log file will be *truncated*
   in case it already exists. More information about the import log can be found in
   :numref:`impexp_import_preferences_import_logs`.

.. option:: --duplicate-log=<file>

   This option lets you create a duplicate log that records all objects in the database sharing an
   identical identifier with a top-level city object from the input file(s). Note that the log file will be
   *truncated* in case it already exists. More information about the duplicate log can be found in
   :numref:`impexp_import_preferences_import_logs`.

.. option:: --[no-]fail-fast

   Flag to indicate whether to fail fast on errors and to immediately cancel the import
   process (default: true).

.. option:: --worker-threads=<threads[,max]>

   Number of parallel threads to use for the import process. You can also specify a
   minimum and maximum number of threads separated by commas. A general description
   of the multithreaded processing used for the import process is provided in
   :numref:`impexp_import_preferences_resources_chapter`.

**Metadata options**

The ``import`` command allows for specifying metadata that is assigned to every city object
during import and stored in columns of the table CITYOBJECT.

.. option:: --creation-date=<mode>

   Specifies how to set the `creationDate` attribute for city objects. Allowed values are
   ``replace`` (default), ``complement`` and ``inherit``. If the creation date
   is not available from the city object during import, it
   may either be set to the import date (``complement``) or be inherited from the parent
   object, if available (``inherit``). Alternatively, the user can choose to replace all
   creation dates from the input files with the import date (``replace``).

.. option:: --termination-date=<mode>

   Specifies how to set the `terminationDate` attribute for city objects. Allowed values are
   ``replace`` (default), ``complement`` and ``inherit``.  If the termination date
   is not available from the city object during import, it
   may either be set to NULL (``complement``) or be inherited from the parent object, if
   available (``inherit``). Alternatively, the user can choose to replace all termination
   dates in the input files with NULL (``replace``).

.. option:: --lineage=<lineage>

   Value to store as lineage of the city objects.

.. option:: --updating-person=<name>

   Name of the user responsible for the import. By default, the name of the database
   user is chosen.

.. option:: --reason-for-update=<reason>

   Reason for importing the data.

.. option:: --use-metadata-from-file

   Flag to indicate that values for lineage, updating person and reason for update
   stored for the city objects in the input file should take precedence over the
   values defined on the command line.

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

**Import list options**

You can also pass an import list to the ``import`` command to control which
city objects should be imported or skipped during import. Please refer to
:numref:`impexp_import_list_filter` for a description of the import
list filter. The following options let you define the layout and reserved characters
of the import list.

.. option:: -f, --import-list=<file>

   Specify the path to the import list file to use in the import operation.

.. option:: -m, --import-list-mode=<mode>

   Specify the mode of the import list filter. Allowed values are ``import`` and
   ``skip``. When choosing ``import``, only city objects having an identifier that
   is contained in the import list will be imported. In ``skip`` mode, matching city objects
   will not be imported. The default mode is ``import``.

.. option:: -w, --import-list-preview

   Use this option to get a preview of the first few lines of the
   import list when applying the provided options for parsing
   and interpreting the import list. This preview is very helpful to adapt
   and specify the delimiter character(s), quoting rules, header
   information, identifier column name or index, etc. The
   preview is printed to the console.

.. option:: -n, --id-column-name=<name>

   Name of the column that holds the identifier value. Using this option only makes
   sense if the import list contains a header line. Otherwise, use the
   :option:`--id-column-index` option to specify the column index.

.. option:: -I, --id-column-index=<index>

   Index of the column that holds the identifier value. The first column of a
   row has the index 1. If this option is omitted, the value of the first column
   of each row will be used as identifier by default. This option is mutually
   exclusive with the :option:`--id-column-name` option (only specify one).

.. option:: --[no-]header

   Define whether or not the import list uses a header line. By default, the
   import operation assumes that the first line contains header information.

.. option:: -D, --delimiter=<string>

   Specify the delimiter used for separating values in the import list.
   By default, a comma ``,`` is assumed as delimiter. The provided delimiter
   may consist of more than one character.

.. option:: -Q, --quote=<char>

   The values in the import list may be quoted (i.e., enclosed by a reserved
   character). This option lets you define the character used as quote (default: ``"``).
   Only single characters are allowed as value.

.. option:: --quote-escape=<char>

   If the import list contains quoted values, define the character used for
   escaping embedded quote characters. The default escape character is ``"``.
   Only single characters are allowed as value.

.. option:: -M, --comment-marker=<char>

   Specify the character used as comment marker in the import list. Any
   line starting with this comment marker is interpreted as comment and,
   thus, will be ignored. The default comment marker is ``#``.
   Only single characters are allowed as value.

.. option:: --csv-encoding=<encoding>

   Define the encoding of the import list using a IANA-based character encoding name.
   UTF-8 is assumed as default encoding.

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
                   -o delete
                   -b 13.3508824,52.4799281,13.3578297,52.4862805,4326 \
                   --bbox-mode=within my_city.gml

Import all city objects from the ``my_city.gml`` file that are ``within`` the provided
bounding box. The coordinates of the bounding box are given in WGS84. For
this reason, the fifth value ``4326`` of the :option:`-b` option denotes the SRID
that is used by the target database for the WGS84 reference system. The import operation uses
``delete`` as import mode. Thus, the identifiers of the top-level city objects from ``my_city.gml``
satisfying the bounding box filter are queried in the database. If objects sharing the same identifier
are found in the database, they are first deleted before importing the city objects from ``my_city.gml``.

.. code-block:: bash

   $ impexp import -T oracle -H localhost -S other_user -d citydb_v4 -p -u citydb_user \
                   --count=10 --start-index=20 /my/city/model/files/

Recursively scan the folder ``/my/city/model/files/`` for all supported input files and import
them into a 3DCityDB running on Oracle. Only 10 features will be imported starting
from the 20th feature in the set. Note that the connection is established with the
user ``citydb_user`` but the data will be imported into the schema of the user
``other_user``. The ``citydb_user`` must therefore have been granted sufficient privileges.

.. code-block:: bash

   $ impexp import -H localhost -d citydb_v4 -p -u citydb_user \
                   -f imported-features.log -m skip -I 2 \
                   /my/city/model/files/

Use an import list filter to *skip* all city objects from the input files in ``/my/city/model/files/``
whose identifier matches a value from the import list ``imported-features.log``.
The command uses default values for parsing and interpreting the import list except
for the index of the column that holds the identifier values, which is set to 2.

The above command has been chosen deliberately to illustrate how you can **resume** an
import operation that was aborted or failed due to errors. Of course, you must have enabled
import logs for this to work. The import log will contain the identifiers of those city objects
that were successfully imported before the operation failed. Thus, by using the filter mode ``skip``
they will be skipped when re-running the import operation with the above command.