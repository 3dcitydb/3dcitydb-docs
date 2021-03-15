.. _impexp_export_sql_filter:

SQL filter
-----------

The SQL filter offers a powerful way to query top-level features based on
a user-defined SELECT statement.

.. figure:: /media/impexp_SQL_query_dialog_fig.png
   :name: impexp_SQL_query_dialog_fig
   :align: center

   SQL filter for export operations.

The SQL query is entered in [1]. The ``+`` and ``-`` buttons [2] on the right
side of the input field allow for increasing or reducing the size of the
input field.

In general, any SELECT statement supported by the underlying database
system can be used as SQL filter. The query may operate on all tables
and columns of the database and may involve any database
function or operator. The SQL filter therefore provides a high degree of
flexibility for querying content from the 3DCityDB based on your
filter criteria.

The only mandatory requirement is that the SQL query must return a list
of database ids of the selected city objects. Put differently, the result
set returned by the query may only contain a single column with
references to the ID column of the CITYOBJECT table. The name of the
result column can be freely chosen, and the result set may contain
duplicate ID values. Of course, it must also be ensured that the SELECT
statement follows the specification of the database system.

The following example shows a simple query that selects all city objects
having a generic attribute of name *energy_level* with a double value
less than 12.

.. code-block:: SQL

   select
       cityobject_id
   from
       cityobject_genericattrib
   where
       attrname='energy_level' and realval < 12

The CITYOBJECT_ID column of CITYOBJECT_GENERICATTRIB stores foreign keys
to the ID column of CITYOBJECT. The return set therefore fulfills the
above requirement.

Note that you do not have to care about the type of the city objects
belonging to the ID values in the return set. Since the SQL filter is
evaluated together with all other filter settings on the *Export* tab, the
export operation will automatically make sure that only top-level
features in accordance with the *feature type filter* are exported. For
example, the above query might return ID values of buildings, city
furniture, windows or traffic surfaces. If, however, only buildings
have been chosen in the feature type filter, then all ID values in the
result set not belonging to buildings will be ignored. This allows
writing generic queries that can be reused in different filter
combinations. Of course, you may also limit the result set to specific
city objects if you like.

The following example illustrates a more complex query selecting all
buildings having at least one door object.

.. code-block:: SQL

   select
        t.building_id
   from
        thematic_surface t
   inner join
        opening_to_them_surface o2t on o2t.thematic_surface_id = t.id
   inner join
        opening o on o.id = o2t.opening_id
   where
        o.objectclass_id = 39
   group by
        t.building_id
   having
        count(distinct o.id) > 0

.. caution::
  Other statements than SELECT such as UPDATE, DELETE or
  DDL commands will be rejected and yield an error message. However, in
  principle, it is possible to create database functions that can be
  invoked with a SELECT statement and that delete or change content in the
  database. An example are the DELETE functions offered by the 3DCityDB
  itself (cf. :numref:`citydb_sproc_delete_chapter`). For this reason, the export operation scans
  the SQL filter statement for these well-known DELETE functions and refuses to
  execute them. However, similar functions can also be
  created after setting up the 3DCityDB schema and thus are not known to
  the export operation a priori. If such functions exist and a user of the
  Importer/Exporter shall not be able to accidentally invoke them
  through an SQL query, it is **strongly recommended** that the user
  may only connect to the 3DCityDB instance via a *read-only user* (cf.
  :numref:`citydb_schema_rw_access_chapter`).