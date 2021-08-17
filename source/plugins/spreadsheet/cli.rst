Command-line interface
----------------------

**Synopsis**

.. code-block:: bash

   impexp export-table [-hV] [--ade-extensions=<folder>] [-c=<file>]
                       [-D=<char>] -l=<file> [--log-file=<file>]
                       [--log-level=<level>] -o=<file> [--pid-file=<file>]
                       [--plugins=<folder>] [--use-plugin=<plugin
                       [=true|false]>[,<plugin[=true|false]>...]]... [[[-t=<
                       [prefix:]name>[,<[prefix:]name>...]]...
                       [--namespace=<prefix=name>[,<prefix=name>...]]...]
                       [[-r=<version>] [-R=<timestamp[,timestamp]>]]
                       [-i=<id>[,<id>...] [-i=<id>[,<id>...]]...] [-b=<minx,
                       miny,maxx,maxy[,srid]>] [-s=<select>]]
                       [[-T=<database>] -H=<host> [-P=<port>] -d=<name>
                       [-S=<schema>] -u=<name> [-p[=<password>]]]
                       [@<filename>...]

**Description**

The ``export-table`` command exports attributes of the
city objects stored in the 3D City Database in tabular form.
It corresponds to the table export operation
offered on the *Table Export* tab of the graphical user interface (see :numref:`impexp_plugin_spshg_export_chapter`).
The command provides a range of options to adapt the export process.
In addition, you can also use the global options that are available
for all commands of the Importer/Exporter command-line interface
(see :numref:`impexp_cli_chapter`).

**General options**

.. option:: -o, --output=<file>

   Specify the output file to use for storing the exported attribute data.
   Use ``.csv`` as file extension to export the data as comma-separated values (CSV)
   file, which is also the default output format. Alternatively, you can export
   the data as Microsoft Excel (XLSX) file by choosing ``.xslx`` as file extension.

.. option:: -l, --template=<file>

   Provide the template file to use for the export. The template file
   defines the layout and content for the output file. See :numref:`impexp_plugin_spshg_template_files`
   for more information.

.. option:: -D, --delimiter=<char>

   Delimiter to use for separating values in the output CSV file. By default,
   a comma ``,`` is used as delimiter. This option is ignored when exporting as XLSX file.

**Query and filter options**

The ``export-table`` command offers additional options to define both thematic and spatial filters
that are used to restrict the export to a subset of the top-level city objects stored in
the 3D City Database.

.. option:: -t, --type-name=<[prefix:]name>[,<[prefix:]name>...]

   Comma-separated list of one or more names of the top-level feature types to be exported. The
   type names are case sensitive and shall match one of the official CityGML feature
   type names. To avoid ambiguities, you can use an optional prefix for each name. The prefix
   must be associated with the official XML namespace of the feature type. You can either use
   the official CityGML namespace prefixes listed in :numref:`impexp_toplevel_feature_types_table`.
   Or you can use the :option:`--namespace` option to declare your own prefixes.

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
   are available in :numref:`impexp_plugin_spshg_feature_version_filter`.

.. option:: -R, --feature-version-timestamp=<timestamp[,timestamp]>

   One or two timestamps to be used with the :option:`--feature-version` option. A
   timestamp can be given as date in the form ``YYYY-MM-DD`` or as date-time specified as
   ``YYYY-MM-DDThh:mm:ss[(+|-)hh:mm``. The date-time format supports an optional
   UTC offset. Use one timestamp with the ``at`` and ``terminated_at`` values and two timestamps
   separated by comma with the ``between`` value of the :option:`--feature-version` option.

.. option:: -i, --resource-id=<id>[,<id>...]

   Comma-separated list of one or more identifiers. Only top-level features having a matching
   value for their identifier attribute will be exported.

.. option:: -b, --bbox=<minx,miny,maxx,maxy[,srid]>

   2D bounding box to use as spatial filter. The bounding box is given by four coordinates
   that define its lower left and upper right corner. By default, the coordinates are assumed
   to be in the same CRS that is used by the 3DCityDB instance. Alternatively, you
   can provide the database ``srid`` of the CRS associated with the coordinates as fifth value
   (e.g. ``4326`` for WGS84). All values must be separated by commas. The bounding box is evaluated
   against the GMLID column of the CITYOBJECT table.

.. option:: -s, --sql-select=<select>

   Provide an SQL SELECT statement to be used as SQL filter when querying the database.
   In general, any SELECT statement can be used as long as it returns a list
   of database IDs of the selected city objects (see :numref:`impexp_export_sql_filter` for more information).
   You can also use an @-file to provide the SELECT statement (see :numref:`impexp_cli_argument_files`).

.. include:: ../../impexp/cli/database-options.rst

**Examples**

.. code-block:: bash

   $ impexp export-table -H localhost -d citydb_v4 -u citydb_user -p my_password \
                         -l my_template.txt -o my_attributes.xslx

Export attributes according to the provided ``my_template.txt`` file
(see :numref:`impexp_plugin_spshg_template_files`) for
all top-level city objects stored in the database. The attribute data
is stored in the ``my_attributes.xslx`` file using XLSX as output format.
The 3DCityDB to connect to is supposed to be running on a PostgreSQL database on
the same machine. The connection will be established to the ``citydb_v4`` database
with the user ``citydb_user`` and the password ``my_password``.

.. code-block:: bash

   $ impexp export-table -H localhost -d citydb_v4 -u citydb_user -p my_password \
                         -t Building -b 13.3508824,52.4799281,13.3578297,52.4862805,4326 \
                         -D ; -l my_template.txt -o my_attributes.csv

Only export attributes of ``Building`` features overlapping with the provided bounding box from the
database. The coordinates of the bounding box are given in WGS84. For this reason,
the fifth value ``4326`` of the :option:`-b` option denotes the SRID that is used
by the target database for the WGS84 reference system. The output format is CSV and
a semicolon ``;`` is used as delimiter.

.. code-block:: bash

   $ impexp export -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   -s "select cityobject_id from cityobject_genericattrib \
                       where attrname='energy_level' and realval < 12" \
                   -l my_template.txt -o my_attributes.csv

Export attributes of all city objects satisfying the given SQL SELECT statement.