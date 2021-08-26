.. _impexp_cli_export_command:

Export command
--------------

**Synopsis**

.. code-block:: bash

   impexp export [-hV] [--ade-extensions=<folder>] [-c=<file>]
                 [--compressed-format=<format>] [--log-file=<file>]
                 [--log-level=<level>] -o=<file>
                 [--output-encoding=<encoding>] [--pid-file=<file>]
                 [--plugins=<folder>] [--use-plugin=<plugin[=true|false]>[,
                 <plugin[=true|false]>...]]... [--worker-threads=<threads[,
                 max]>] [[[-t=<[prefix:]name>[,<[prefix:]name>...]]...
                 [--namespace=<prefix=name>[,<prefix=name>...]]...]
                 [[-r=<version>] [-R=<timestamp[,timestamp]>]] [-i=<id>[,
                 <id>...] [-i=<id>[,<id>...]]...] [--db-id=<id>[,<id>...]
                 [--db-id=<id>[,<id>...]]...] [-b=<minx,miny,maxx,maxy[,
                 srid]> [--bbox-mode=<mode>] [-g=<rows,columns>]]
                 [[--count=<count>] [--start-index=<index>]] [-l=<0..4>[,
                 <0..4>...] [-l=<0..4>[,<0..4>...]]... [--lod-mode=<mode>]
                 [--lod-search-depth=<0..n|all>]] [[--no-appearance] |
                 -a=<theme>[,<theme>...] [-a=<theme>[,<theme>...]]...]
                 [-s=<select>] [-q=<xml>]] [[-T=<database>] -H=<host>
                 [-P=<port>] -d=<name> [-S=<schema>] -u=<name> [-p
                 [=<password>]]] [@<filename>...]

**Description**

The ``export`` command exports top-level city objects from the 3D City
Database in CityGML or CityJSON format. It corresponds to the export operation
offered on the *Export* tab of the graphical user interface (see :numref:`impexp_citygml_export_chapter`).
The command provides a range of options to adapt the export process.
If you miss settings available for the GUI version of this command,
you can use the :option:`--config` option to pass a config file
containing these settings.

**General options**

.. option:: -o, --output=<file>

   Specify the output file to use for storing the exported city objects. By default,
   CityGML is used as output format. The output format can be changed to CityJSON
   by choosing ``.json`` or ``.cityjson`` as file extension. Moreover, compressed
   exports using GZIP (``.gz``, ``.gzip``) or ZIP (``*.zip``) are possible.
   The supported formats and file extensions are also listed in :numref:`export_supported_file_formats`.

.. option:: --output-encoding=<encoding>

   Define the encoding of the output file using a IANA-based character encoding name.
   The default value is UTF-8.

.. option:: --compressed-format=<format>

   For compressed exports using GZIP or ZIP, specify the output format to use for encoding the data.
   Allowed values are ``citygml`` and ``cityjson``.

.. option:: --worker-threads=<threads[,max]>

   Number of parallel threads to use for the export process. You can also specify a
   minimum and maximum number of threads separated by commas. A general description
   of the multithreaded processing used for the export process is provided in
   :numref:`impexp_export_preferences_resources_chapter`.

**Query and filter options**

The ``export`` command offers additional options to define both thematic and spatial filters
that are used to restrict the export to a subset of the top-level city objects stored in
the 3D City Database.

.. option:: -t, --type-name=<[prefix:]name>[,<[prefix:]name>...]

   Comma-separated list of one or more names of the top-level feature types to be exported. The
   type names are case sensitive and shall match one of the official CityGML feature
   type names or a feature type name from a CityGML ADE. To avoid ambiguities,
   you can use an optional prefix for each name. The prefix must be associated with the
   official XML namespace of the feature type. You can either use the official CityGML namespace
   prefixes listed in :numref:`impexp_toplevel_feature_types_table`. Or you can use
   the :option:`--namespace` option to declare your own prefixes.

.. option:: --namespace=<prefix=name>[,<prefix=name>...]

   Used to specify namespaces and their prefixes as comma-separated list of one or more
   ``prefix=name`` pairs. The prefixes can be used in other options such as :option:`--type-name`.

.. option:: -r, --feature-version=<version>

   Specify the version of the top-level features to use for the export. Allowed values are
   ``latest``, ``at``, ``between``, ``terminated``, ``terminated_at`` and ``all``. When choosing
   ``latest``, only those features that have not been terminated in the database are exported,
   whereas ``all`` will export all features. You can also choose to export only features that were
   valid at a given timestamp using ``at`` or for a given time range using ``between``. Likewise,
   ``terminated`` will return all terminated features whereas ``terminated_at`` will select features that
   were terminated at a given timestamp. In all cases, timestamps must be provided using the
   :option:`--feature-version-timestamp` option. Further details about the feature version filter
   are available in :numref:`impexp_export_feature_version_filter`.

.. option:: -R, --feature-version-timestamp=<timestamp[,timestamp]>

   One or two timestamps to be used with the :option:`--feature-version` option. A
   timestamp can be given as date in the form ``YYYY-MM-DD`` or as date-time specified as
   ``YYYY-MM-DDThh:mm:ss[(+|-)hh:mm``. The date-time format supports an optional
   UTC offset. Use one timestamp with the ``at`` and ``terminated_at`` values and two timestamps
   separated by comma with the ``between`` value of the :option:`--feature-version` option.

.. option:: -i, --resource-id=<id>[,<id>...]

   Comma-separated list of one or more identifiers. Only top-level features having a matching
   value for their identifier attribute will be exported.

.. option:: --db-id=<id>[,<id>...]

   Comma-separated list of one or more database IDs. Only top-level features having a matching
   primary key for the ID column of the CITYOBJECT table will be exported.

.. option:: -b, --bbox=<minx,miny,maxx,maxy[,srid]>

   2D bounding box to use as spatial filter. The bounding box is given by four coordinates
   that define its lower left and upper right corner. By default, the coordinates are assumed
   to be in the same CRS that is used by the 3DCityDB instance. Alternatively, you
   can provide the database ``srid`` of the CRS associated with the coordinates as fifth value
   (e.g. ``4326`` for WGS84). All values must be separated by commas. The bounding box is evaluated
   against the GMLID column of the CITYOBJECT table.

.. option:: --bbox-mode=<mode>

   Choose the mode of the bounding box filter. Allowed values are ``overlaps`` (default) and ``within``.
   When set to ``overlaps``, all features overlapping with the bounding box are exported. Otherwise,
   features must be ``within`` the given bounding box. Can only be used together with the :option:`--bbox` option.

.. option:: -g, --bbox-tiling=<rows,columns>

   Use this option to enable tiling for the export. The bounding box given by the
   :option:`--bbox` option is split into a regular grid having the specified number
   of ``rows`` and ``columns``. Each grid cell is exported as separate tile. The values
   for ``rows`` and ``columns`` must be separated by a comma. More information about tiled exports
   is provided in :numref:`impexp_citygml_export_chapter`.

.. option:: --count=<count>

   Maximum number of top-level features to be exported.

.. option:: --start-index=<index>

   Index within the result set of all top-level features from which the export
   shall begin. The start index uses zero-based numbering. Thus, the first top-level feature is
   assigned the index 0, rather than the index 1.

.. option:: -l, --lod=<0..4>[,<0..4>...]

   Comma-separated list of LoDs that shall be exported for the city objects. Each LoD value
   must be given as integer between ``0`` and ``4``. If you provide more than one LoD,
   use the :option:`--lod-mode` option to specify how the values shall be evaluated.
   LoD representations of a city object that are stored in the database but not listed
   with this option are not exported. See :numref:`impexp_export_lod_filter` for more details.

.. option:: --lod-mode=<mode>

   Specify the LoD filter mode in case you have provided more than one LoD value for
   the :option:`--lod` option. Allowed values are ``or`` (default), ``and``, ``minimum``,
   and ``maximum``. When choosing ``or``, a city object must have a spatial representation
   in at least one of the given LoDs to be exported, whereas ``and`` requires a representation in all
   LoDs. Both ``minimum`` and ``maximum`` are special versions of ``or`` where only
   the lowest or highest LoD representation from the matching ones is exported.

.. option:: --lod-search-depth=<0..n|all>

   Number of levels of nested city objects that shall be considered by the LoD filter
   when searching for a matching LoD representation (see also :numref:`impexp_export_lod_filter`).
   Either provide a non-negative integer or ``all`` as value. The default value is 1.

.. option:: --no-appearance

   Flag to indicate that appearances of the features shall not be exported.

.. option:: -a, --appearance-theme=<theme>[,<theme>...]

   Comma-separated list of appearance themes. Only appearances having a matching theme
   will be exported. Use ``none`` as value to address appearance features lacking
   a theme attribute.

.. option:: -s, --sql-select=<select>

   Provide an SQL SELECT statement to be used as SQL filter when querying the database.
   In general, any SELECT statement can be used as long as it returns a list
   of database IDs of the selected city objects (see :numref:`impexp_export_sql_filter` for more information).
   You can also use an @-file to provide the SELECT statement (see :numref:`impexp_cli_argument_files`).

.. option:: -q, --xml-query=<xml>

   This option allows you to use a full-fledged XML query expression as filter
   for the export operation. Make sure the query expression is valid and adheres to the
   specification for XML query expressions provided in :numref:`impexp_xml_queries_chapter`.
   You can also use an @-file to provide the query expression (see :numref:`impexp_cli_argument_files`).
   This option **cannot be used with any other filter option** of the ``export`` command.

.. include:: database-options.rst

**Examples**

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password -o my_city.gml

Export the entire database content as CityGML to the output file ``my_city.gml``.
The 3DCityDB to connect to is supposed to be running on a PostgreSQL database on
the same machine. The connection will be established to the ``citydb_v4`` database
with the user ``citydb_user`` and the password ``my_password``.

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   -t Building -b 13.3508824,52.4799281,13.3578297,52.4862805,4326 \
                   -o my_city.json

Only export ``Building`` features overlapping with the provided bounding box from the
database. The coordinates of the bounding box are given in WGS84. For this reason,
the fifth value ``4326`` of the :option:`-b` option denotes the SRID that is used
by the target database for the WGS84 reference system. The output format is CityJSON.

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   -i ID_0815,ID_0816 -l 1,2,3 --lod-mode=maximum
                   --compressed-format=citygml -o my_city.zip

Export the top-level city objects with the identifiers ``ID_0815`` and ``ID_0816``
from the database if they have an LoD representation in either LoD 1, 2 or 3.
From the matching LoD representations, only export the highest LoD. The output file
will be a compressed ZIP archive containing a CityGML file with the exported
city objects.

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   -s "select cityobject_id from cityobject_genericattrib \
                       where attrname='energy_level' and realval < 12" \
                   -o my_city.zip

Export all city objects satisfying the given SQL SELECT statement.

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   -s "select cityobject_id from cityobject_genericattrib \
                       where attrname='energy_level' and realval < 12" \
                   -o my_city.zip

Export all city objects satisfying the given SQL SELECT statement.

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   @/path/to/xml-query -o my_city.gml

Export all city objects satisfying the given XML query expression.
To avoid possible system limitations on the length of the command line,
the XML query is stored in a separate argument file called ``xml-query`` which is
referenced from the command line using the @-file notation.
The content of the ``xml-query`` file is shown below:

.. code-block:: bash

   -q
   '<query xmlns="http://www.3dcitydb.org/importer-exporter/config"> \
     <typeNames> \
       <typeName xmlns:bldg="http://www.opengis.net/citygml/building/2.0">bldg:Building</typeName> \
     </typeNames> \
     <filter> \
       <bbox> \
         <envelope srid="4326"> \
           <lowerCorner>13.3508824 52.4799281</lowerCorner> \
           <upperCorner>13.3578297 52.4862805</upperCorner> \
         </envelope> \
       </bbox> \
     </filter> \
     <sortBy> \
       <sortProperty> \
         <valueReference>bldg:measuredHeight</valueReference> \
       </sortProperty> \
     </sortBy> \
     <limit> \
       <count>20</count> \
     </limit> \
   </query>'

According to this query expression, only the first 20 buildings satisfying the provided
bounding box filter and sorted by their *bldg:measuredHeight* attribute will be exported.

.. note::
   For the above @-file ``xml-query`` to work, the following requirements must be met:

   - The entire XML query expression is the value of the :option:`-q` option and, thus,
     must be put on a single line in the argument file. Either avoid line breaks
     in your XML or escape them using a backslash ``\`` character like
     in the above example.
   - Since the XML query expression contains whitespace, it must be put in
     double or single quotes. When using double quotes, all double quotes of
     the query expression itself must be escaped. To avoid escaping, the above
     example uses single quotes.


