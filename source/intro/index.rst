First steps
===========

The 3D City Database comes with SQL scripts for setting up an instance
of the relational schema on a spatial database system (Oracle
Spatial/Locator or PostgreSQL/PostGIS) and with a database loading and
extracting tool called Importer/Exporter. Installers are available for
download at http://www.3dcitydb.org. The source code of the 3D City
Database project is hosted on https://github.com/3dcitydb. Please follow
the instructions on the next pages to complete a proper installation.

.. toctree::
   :maxdepth: 1

   system-requirements
   install-impexp
   3dcitydb-scripts
   db-setup-oracle
   db-setup-postgis
   upgrade
   v2-v4-migrate-oracle
   v2-v4-migrate-postgis
   docker


The individual components of the 3D City Database are also available as
images for the Docker virtualization technology. This makes it possible
to install and configure a 3D City Database with a single command line
statement in almost any runtime environment. See chapter 9 for more
details.