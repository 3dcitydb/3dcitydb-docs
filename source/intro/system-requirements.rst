System requirements
-------------------

3D City Database
~~~~~~~~~~~~~~~~

Setting up an instance of the 3D City Database requires a running
installation of an Oracle or PostgreSQL database server.

**Oracle**

Supported version are **Oracle 10g R2** **or higher**. The 3D City
Database requires spatial data support provided either through the
Oracle *Spatial* or *Locator* extension. It is highly recommended to
install available patches to avoid unexpected errors and to benefit from
the latest functionality. For Oracle 10g R2, at least patch set
10.2.0.4.0 is required for using the KML/COLLADA/glTF export
capabilities.

**PostgreSQL**

Supported versions are **PostgreSQL 9.3 or higher** with the **PostGIS
extension 2.0 or higher**. Please also make sure to always install the
latest patches and updates.

The SQL scripts to create the database schema are written to be executed
by the default command-line-based client interface of the DBMS – which
is **SQL*Plus** for Oracle and **psql** for PostgreSQL. The scripts
include meta commands specific to these clients and would not work
properly when using a different client software. So please make sure
SQL*Plus or psql is installed on the machine from where you want to
setup the 3D City Database.

Importer/Exporter Tool
~~~~~~~~~~~~~~~~~~~~~~

The Importer/Exporter tool can run on any platform providing support for
Java 8 (or higher). It has been successfully tested on (but is not
limited to) the following operating systems:

-  Microsoft Windows XP, Vista, 7, 8, 10;

-  Apple Mac OS X and macOS;

-  Ubuntu Linux 9 to 18.

Prior to the setup of the Importer/Exporter tool, the **Java 8 Runtime
Environment** (or higher) **must be installed on your system**. The
installation package can be obtained from
http://www.java.com/en/download. Follow the installation instructions
for your operating system.

The Importer/Exporter is shipped with a universal installer that will
guide you through the steps of the setup process. A full installation of
the Importer/Exporter including documentation and example CityGML files
requires approx. 505 MB of hard disk space. Installing only the
mandatory application files will use approx. 350 MB of hard disk space.
Installation packages can be selected during the setup process.

The Importer/Exporter runs with 1 GB of main memory per default. This
setting should be reasonable on most platforms and for most
import/export procedures. If required, you can *manually* adapt the main
memory limits in the starter script of the program. Please refer to
chapter 5.1 for more details.
