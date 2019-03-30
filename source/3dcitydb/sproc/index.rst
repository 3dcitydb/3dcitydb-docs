Stored procedures and additional features
=========================================

The 3D City Database is shipped with a set of stored procedures referred
to as the CITYDB package (formerly known as the GEODB package in v2.x).
They are automatically installed during the setup procedure of the 3D
City Database. For the Oracle version, it comprises of eight PL/SQL
packages. In the PostgreSQL version, functions are written in PL/pgSQL
and stored either in their own database schema called ‘citydb_pkg’ or as
part of an instance schema like ‘citydb’. Many of these functions and
procedures expose certain tasks on the database side to the
Importer/Exporter client. When calling stored procedures, the package
name has to be included for the Oracle version. With PostgreSQL, the
‘citydb_pkg’ schema has not to be specified as prefix since it is put on
the database *search path* during setup.

|image60|\ |image61|

Figure 54: Graphical database client connected to the 3D City Database
(left: SQL Developer (Oracle), right: pgAdmin 4 (PostgreSQL)

.. toctree::
   :maxdepth: 1

   udt
   citydb-util
   citydb-constraint
   citydb-idx
   citydb-srs
   citydb-stat
   citydb-objclass
   citydb-delete
   citydb-envelope
