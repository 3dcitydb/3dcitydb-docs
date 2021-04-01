.. _impexp_cli_delete_command:

Delete command
---------------

**Synopsis**

.. code-block:: bash

   impexp delete [-hvV] [--ade-extensions=<folder>] [-c=<file>]
                 [--log-file=<file>] [--log-level=<level>] [-m=<mode>]
                 [--pid-file=<file>] [--plugins=<folder>] [[-g]] [[[-t=<
                 [prefix:]name>[,<[prefix:]name>...]]...
                 [--namespace=<prefix=name>[,<prefix=name>...]]...]
                 [-r=<version> [-R=<timestamp[,timestamp]>]] [-i=<id>[,
                 <id>...] [-i=<id>[,<id>...]]...] [--db-id=<id>[,<id>...]
                 [--db-id=<id>[,<id>...]]...] [-b=<minx,miny,maxx,maxy[,
                 srid]> [--bbox-mode=<mode>]] [[--count=<count>]
                 [--start-index=<index>]] [-s=<select>] [-q=<xml>]]
                 [-f=<file> [--delete-list-encoding=<encoding>] [-n=<name>]
                 [-I=<index>] [-C=<type>] [--[no-]header] [-D=<string>]
                 [-Q=<char>] [--quote-escape=<char>] [-M=<char>] [-w]]
                 [[-T=<database>] -H=<host> [-P=<port>] -d=<name>
                 [-S=<schema>] -u=<name> [-p[=<password>]]] [@<filename>...]

**Description**

The ``delete`` command deletes top-level city objects from the 3D City Database.
The city objects can either be physically deleted or just terminated. In the latter
case, the objects remain in the database and their *terminationDate* attribute
is set to the time of the delete operation. This way, terminated objects can be
easily identified and filtered in, for instance, export operations using a
feature version filter.

The ``delete`` command supports both thematic and spatial filters to specify the
features to be deleted. In addition, you can also use a delete list. Delete lists
are CSV files that contain a list of identifiers (and possibly further data). Each city object in the database
whose identifier matches an entry in the delete list will be deleted. For example,
the import log file created when importing CityGML/CityJSON files into
the database (see :numref:`impexp_import_preferences_import_log`) can be used as
delete list. This comes in very handy when you want to "rollback" an import process,
for instance, because it aborted due to errors.

A corresponding delete operation is not offered by the graphical user interface.

**General options**

.. option:: -m, --delete-mode=<mode>

   Specify the delete mode. Allowed values are ``delete`` and ``terminate``.
   Be careful to make the right choice here. When choosing ``delete``,
   the city objects will be permanently removed from the database. This
   operation cannot be undone. Terminated objects remain in the database.
   However, if you only ``terminate`` but never ``delete``, your database
   will grow over time. The default mode is ``delete``.

   .. note::
      City objects that are already terminated in the database will not be terminated
      again when running in ``terminate`` mode. Thus, their *terminationDate*
      will not be updated. However, terminated city objects can, of course, be
      deleted from the database.

.. option:: -v, --preview

   This flag allows you to tun the ``delete`` command in preview mode.
   The delete operation will print the number and types of features
   that would be affected by the delete operation as summary overview
   to the console.

.. option:: -g, --cleanup-global-appearances

   Flag to indicate that global appearances should be cleaned up after
   having deleted the city objects. Global appearances are not automatically
   deleted because they can reference more than one city object. If this
   option is set, the delete operation will search the database for global appearances
   that do not reference any city object anymore. Only these appearances
   will be removed from the database.

**Query and filter options**

The ``delete`` command offers additional options to define both thematic and spatial filters
that are used to more precisely specify the top-level city objects to be deleted from
the 3D City Database.

.. option:: -t, --type-name=<[prefix:]name>[,<[prefix:]name>...]

   Comma-separated list of one or more names of the top-level feature types to be deleted. The
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

   Specify the version of the top-level features to use for the delete operation. Allowed values are
   ``latest``, ``at``, ``between``, and ``all``. When choosing ``latest``, only those features that have
   not been terminated in the database are deleted, whereas ``all`` will delete all features.
   You can also choose to delete only features that were valid at a given timestamp using ``at``
   or for a given time range using ``between``. In both cases, the timestamps must be
   provided using the :option:`--feature-version-timestamp` option.

.. option:: -R, --feature-version-timestamp=<timestamp[,timestamp]>

   One or two timestamps to be used with the :option:`--feature-version` option. A
   timestamp can be given as date in the form ``YYYY-MM-DD`` or as date-time specified as
   ``YYYY-MM-DDThh:mm:ss[(+|-)hh:mm``. The date-time format supports an optional
   UTC offset. Use one timestamp with the ``at`` value and two timestamps separated by comma
   with the ``between`` value of the :option:`--feature-version` option.

.. option:: -i, --resource-id=<id>[,<id>...]

   Comma-separated list of one or more identifiers. Only top-level features having a matching
   value for their identifier attribute will be deleted.

.. option:: --db-id=<id>[,<id>...]

   Comma-separated list of one or more database IDs. Only top-level features having a matching
   primary key for the ID column of the CITYOBJECT table will be deleted.

.. option:: -b, --bbox=<minx,miny,maxx,maxy[,srid]>

   2D bounding box to use as spatial filter. The bounding box is given by four coordinates
   that define its lower left and upper right corner. By default, the coordinates are assumed
   to be in the same CRS that is used by the 3DCityDB instance. Alternatively, you
   can provide the database ``srid`` of the CRS associated with the coordinates as fifth value
   (e.g. ``4326`` for WGS84). All values must be separated by commas. The bounding box is evaluated
   against the GMLID column of the CITYOBJECT table.

.. option:: --bbox-mode=<mode>

   Choose the mode of the bounding box filter. Allowed values are ``overlaps`` (default) and ``within``.
   When set to ``overlaps``, all features overlapping with the bounding box are deleted. Otherwise,
   features must be ``within`` the given bounding box. Can only be used together with the :option:`--bbox` option.

.. option:: --count=<count>

   Maximum number of top-level features to be deleted.

.. option:: --start-index=<index>

   Index within the result set of all top-level features from which the delete operation
   shall begin. The start index uses zero-based numbering. Thus, the first top-level feature is
   assigned the index 0, rather than the index 1.

.. option:: -s, --sql-select=<select>

   Provide an SQL SELECT statement to be used as SQL filter when querying the database.
   In general, any SELECT statement can be used as long as it returns a list
   of database IDs of the selected city objects (see :numref:`impexp_export_sql_filter` for more information).
   You can also use an @-file to provide the SELECT statement (see :numref:`impexp_cli_argument_files`).

.. option:: -q, --xml-query=<xml>

   This option allows you to use a full-fledged XML query expression as filter
   for the delete operation. Make sure the query expression is valid and adheres to the
   specification for XML query expressions provided in :numref:`impexp_xml_queries_chapter`.
   You can also use an @-file to provide the query expression (see :numref:`impexp_cli_argument_files`).
   This option **cannot be used with any other filter option** of the ``delete`` command.

**Delete list options**

You can also pass a delete list to the ``delete`` command to control which
city objects should be deleted. A delete list can be used in addition to or
instead of the above filter options.

Delete lists are simple comma-separated values (CSV) files that provide the
identifiers of the city objects to be deleted. Each identifier must be put on a separate
line (row) of the file, and each line may contain additional values (columns) separated
by a delimiter (typically a single reserved character such as comma, semicolon, tab, etc.).
The first record may be reserved as header containing a list of column names. Usually,
every row has the same sequence of columns. If a line starts with a predefined comment marker
(typically a single reserved character such as ``#``), the entire row is ignored
and skipped.

Due to their simple structure, delete lists can be easily created with external tools
and processes. The following snippet shows an example of a simple delete list that
can be used with the ``delete`` command. It just provides an identifier per row. The first
line is used as header.

.. code-block:: bash
   :linenos:

   GMLID
   ID_0815
   ID_0816
   ID_0817
   ID_0818
   ...

The following options let you define the layout and reserved characters
of the delete list you want to use with the ``delete`` command.

.. option:: -f, --delete-list=<file>

   Specify the path to the delete list file to use in the delete operation.

.. option:: --delete-list-encoding=<encoding>

   Define the encoding of the delete list using a IANA-based character encoding name.
   UTF-8 is assumed as default encoding.

.. option:: -n, --id-column-name=<name>

   Name of the column that holds the identifier value. Using this option only makes
   sense if the delete list contains a header line. Otherwise, use the
   :option:`--id-column-index` option to specify the column index.

.. option:: -I, --id-column-index=<index>

   Index of the column that holds the identifier value. The first column of a
   row has the index 1. If this option is omitted, the value of the first column
   of each row will be used as identifier by default. This option is mutually
   exclusive with the :option:`--id-column-name` option (only specify one).

.. option:: -C, --id-column-type=<type>

   Specify the type of the identifier. Allowed values are ``resource`` and ``db``.
   When choosing ``resource``, the identifiers provided in the delete list are interpreted
   as object identifiers (e.g., *gml:id* in CityGML) and are therefore matched
   against the GMLID column of the CITYOBJECT table. In contrast, the identifiers
   are taken as database IDs and checked against the ID column of the CITYOBJECT table
   when selecting ``db`` instead. The default value is ``resource``.

.. option:: --[no-]header

   Define whether or not the delete list uses a header line. By default, the
   delete operations assumes that the first line contains header information.

.. option:: -D, --delimiter=<string>

   Specify the delimiter used for separating values in the delete list.
   By default, a comma ``,`` is assumed as delimiter. The provided delimiter
   may consist of more than one character.

.. option:: -Q, --quote=<char>

   The values in the delete list may be quoted (i.e., enclosed by a reserved
   character). This option lets you define the character used as quote (default: ``"``).
   Only single characters are allowed as value.

.. option:: --quote-escape=<char>

   If the delete list contains quoted values, define the character used for
   escaping embedded quote characters. The default escape character is ``"``.
   Only single characters are allowed as value.

.. option:: -M, --comment-marker=<char>

   Specify the character used as comment marker in the delete list. Any
   line starting with this comment marker is interpreted as comment and,
   thus, will be ignored. The default comment marker is ``#``.
   Only single characters are allowed as value.

.. option:: -w, --delete-list-preview

   Use this option to get a preview of the first few lines of the
   delete list when applying the provided options for parsing
   and interpreting the delete list. This preview is very helpful to adapt
   and specify the delimiter character(s), quoting rules, header
   information, identifier column name or index, etc. The
   preview is printed to the console.

.. include:: database-options.rst

**Examples**

.. code-block:: bash

   $ impexp delete -H localhost -d citydb_v4 -u citydb_user -p my_password

Delete all city objects from the database. The 3DCityDB to
connect to is supposed to be running on a PostgreSQL database on
the same machine. The connection will be established to the ``citydb_v4`` database
with the user ``citydb_user`` and the password ``my_password``.

.. caution::
  Be very careful with the above example and only use it in case
  you are absolutely sure. **There will be no confirmation prompt
  and no undo operation** (which is also true for all further examples...).
  If you really want to clean the entire
  database, the ``cleanup_schema`` database function provided by
  the 3DCityDB should be your preferred choice simply because it is much faster.

.. code-block:: bash

   $ impexp delete -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   -m terminate -v \
                   -t Building -b 13.3508824,52.4799281,13.3578297,52.4862805,4326 \
                   --bbox-mode=within

Only terminate ``Building`` features ``within`` the given bounding box. The
affected buildings are kept in database but marked as terminated by setting
their *terminationDate* attribute. The entire process is run in preview mode.
Thus, a summary overview of the number of affected feature is printed to
the console but nothing will be committed to the database.

.. code-block:: bash

   $ impexp delete -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   -m terminate -g \
                   -s "select cityobject_id from cityobject_genericattrib \
                       where attrname='energy_level' and realval < 12" \
                   --count 20

Terminate the first 20 top-level city objects satisfying the given SQL SELECT
statement. Afterwards, global appearances not referencing a city object anymore
will be deleted from the database.

.. code-block:: bash

   $ impexp delete -H localhost -d citydb_v4 -u citydb_user -p my_password \
                   -f imported-features.log -I 3

Only delete those top-level city objects having an identifier that is contained
in the provided delete list ``imported-features.log``.
The command uses default values for parsing and interpreting
the delete list except for the index of the column that holds the identifier values,
which is set to 3.

The above example has been chosen deliberately because it illustrates how
to use an import log file created by a CityGML/CityJSON import
operation as delete list for the ``delete`` command. The general layout
and content of an import log file is discussed in :numref:`impexp_import_preferences_import_log`.
The snippet below shows an example.

.. code-block:: bash
   :linenos:

   #3D City Database Importer/Exporter, version "4.3.0"
   #Imported top-level features from file: C:\data\citygml\berlin\gasometer\tile_0_0\Gasometer.gml
   #Database connection string: citydb_user@localhost:5432/citydb_v4
   #Timestamp: 2021-03-12 15:17:50
   FEATURE_TYPE,CITYOBJECT_ID,GMLID_IN_FILE
   Building,2,BLDG_00030000001065f4
   Building,13,BLDG_0003000a000afacf
   Building,17,BLDG_0003000a000afa94
   Building,25,BLDG_0003000e009b5355
   Building,33,BLDG_0003000b003d0d1b
   Building,45,BLDG_0003000000107172
   Building,51,BLDG_0003000a000afb14
   Building,1,BLDG_0003000000106562
   Building,9,BLDG_0003000a000afb3b
   Building,21,BLDG_0003000000106543
   Building,28,BLDG_0003000a0019a1c6
   Building,36,BLDG_0003000b003d0d16
   Building,41,BLDG_0003000f000f7460
   Building,49,BLDG_0003000e0097f53f
   Building,57,BLDG_0003000000106686
   #Import successfully finished.

The import log uses a comma ``,`` as delimiter. Lines 1-4 and line 21 are
comments starting with the comment marker ``#`` and should therefore be ignored.
Line 5 is used as header to provide column names for the subsequent rows.
The actual content is therefore provided in lines 6-20. Each row consists
of three columns, and the third column ``GMLID_IN_FILE`` contains the object
identifiers of the imported top-level features.

As you can see from the above example, the default CSV options of the
``delete`` command are already suited to correctly identify the comments, the header, and
the separate columns of the import log file. As mentioned above, we only
have to specify the correct column index of the identifier values on the command line.

.. note::
   The second column ``CITYOBJECT_ID`` of the import log file holds the database IDs of the
   imported city objects. To use the database IDs in the delete process, you have to use
   ``-I 2`` to specify the second column and ``-C db`` to denote that the identifiers
   stored in this column are database IDs. **This is even the preferred way for using an import log
   as delete list** because database IDs are guaranteed to be unique.






