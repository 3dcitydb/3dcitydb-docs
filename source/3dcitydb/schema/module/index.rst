Database schema
---------------

In the following paragraph, the tables of the relational schema are
displayed graphically and described in detail. The description is based
on the remarks on UML charts in chapter 2.2. Focus is put on situations
where the conversion into tables leads to changes in the model.

.. toctree::
   :maxdepth: 1

   metadata
   core
   geometry
   appearance
   building
   bridge
   city-furniture
   generics
   land-use
   relief
   transportation
   tunnel
   vegetation
   water-body
   sequence

The figures are taken from Oracle JDeveloper, which allows to design
different diagrams and reuse already defined tables. JDeveloper
(v12.2.1) was used to design the database schema and extract SQL DDL
scripts automatically for Oracle databases. It is a freeware IDE by
Oracle and can be downloaded at:
http://www.oracle.com/technetwork/developer-tools/jdev.

For PostgreSQL databases the Open Source tool pgModeler (v0.8.2) has
been used to maintain the schema. Packed installers can be purchased at
http://pgmodeler.com.br/ or the user compiles the software from the
source code available at GitHub
(https://github.com/pgmodeler/pgmodeler).

Starting from version 3.0.0 of the 3DCityDB the corresponding schema
modelling projects are shipped with the release and can be edited by the
user to create customized SQL scripts. However, the 3DCityDB
Import/Export tool only supports the default schema, unless it is not
reprogrammed against the user’s new database schema.
