.. _citydb_multiple_database_schemas_chapter:

Working with multiple database schemas
--------------------------------------

Most users rarely work with only one 3D City Database. They maintain
multiple instances for each data set, for different city projects or
user groups and probably for various test demos. The new ability to
manage CityGML ADEs sets the ground for even more experiments. This
chapter explains how to manage multiple 3D City Databases in separate
schemas.

Create and address database schemas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Databases and schemas in PostgreSQL**

PostgreSQL provides a clustering concept for database schemas that
allows users to group multiple instances of the 3D City Database. This
means within one database object a user can create more schemas like in
the ‘citydb’ schema, that store the table layout of the 3D City
Database. They can be regarded as separate namespaces. To address the
different namespaces, dot notation should be used in queries. Note, if
tables are not schema-qualified the first namespace in the database
search path (see :numref:`first_step_3dcitydb_installation_postgis`)
that contains the tables will be used.
One advantage of using multiple schemas instead of many databases is the
ability to join tables from different namespaces. Cross-database queries
are not directly possible in PostgreSQL (see *postgres_fdw* extension).

To create an additional 3D City Database instance within a given
database run the CREATE_SCHEMA shell script and define a name for the
new schema. The new instance will obtain the CRS from the ‘citydb’
schema, which can be changed later (see chapter :numref:`citydb_sproc_srs_chapter`).
To drop a schema, call the DROP_SCHEMA shell script.

**Oracle user schemas**

In Oracle, schemas are bound to one user. All user schemas belong to one
database. There is no clustering concept like in PostgreSQL, so a
CREATE_SCHEMA script would not make too much sense. In fact, a new
instance should be created with a new user and the CREATE_DB script.
Like with PostgreSQL schemas, it is possible to join tables from
different user namespaces if sufficient privileges were granted (see
next section). As another alternative Oracle databases can be set under
version control with the Oracle Workspace Manager so that a user can
also work with multiple versions of a city model in separate
*workspaces*. To change the workspace a user must execute the
DBMS_WM.GotoWorkspace procedure.

Read and write access to a schema
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A shell script called GRANT_ACCESS is provided to grant either READ-ONLY
(RO) or READ-WRITE (RW) access rights to a 3D City Database instance.
The user who acts as the grantor must be specified in the
CONNECTION_DETAILS file. The user name of the grantee must be entered
when executing the script.

**Read-only access rights**

Granting only read access is useful if you want to protect your data
from unauthorized or accidental modification. This is the default
setting in the GRANT_ACCESS script. Read-only users will be allowed to:

-  connect to the given database schema and use its objects (tables,
   views, sequences, types etc.),

-  export data in both CityGML and KML/COLLADA formats,

-  generate database reports, query the index status and calculate
   envelopes.

But they can neither import new data into the 3DCityDB nor alter the
data already stored in the tables in any way (incl. updating envelopes,
dropping and creating indexes).

**Read and write access rights**

By choosing the RW option in the GRANT_ACCESS script the grantee will
also be able to perform UPDATE and DELETE operations against the schema
content. This is especially useful for Oracle users, who want to manage
different database schemas with primarily one user. In PostgreSQL
however, one user can be the owner of multiple schemas. Still, write
access can be interesting in a multi-editor scenario.

.. note::
   Dropping and creating indexes is not possible in PostgreSQL, if
   you’re not the owner of the table.

**Revoke access**

Like with the GRANT_ACCESS script, access rights can also be revoked, of
course. Simply call the REVOKE_ACCESS script and enter the user name of
the grantee and the schema name from which the rights shall be revoked
from.

Schema support in stored procedures
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Since v3.0.0, most stored procedures of the 3D City Database offer an
input argument to specify the schema name against which the operation
will be executed. The default for Oracle is the schema of the currently
connected user, for PostgreSQL it is \`citydb`. For v4.0 this parameter
has been removed for those type of stored procedures that operate on the
logical level of the database, because managing different ADEs in
separate schemas can result in a different table structure. E.g. one
central delete script is not guaranteed to work against every schema.
Thus, for PostgreSQL these procedures are now part of an instance schema
such as ‘citydb’ (see also :numref:`citydb_sproc_chapter`). Instead of calling a delete
function from the central ‘citydb_pkg’ schema like this:

.. code:: sql

    SELECT citydb_pkg.delete_cityobject(1, ‘my_schema’);

you now have to schema-qualify the function itself:

.. code:: sql

    SELECT my_schema.delete_cityobject(1);

In Oracle, every stored procedure could be called this way, as every
user schema stores the PL/SQL packages.
