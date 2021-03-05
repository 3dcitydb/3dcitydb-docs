.. _first_steps_system_requirements_chapter:

System requirements
-------------------

3D City Database
~~~~~~~~~~~~~~~~

Setting up an instance of the 3D City Database requires an existing
installation of a `PostgreSQL <https://www.postgresql.org/>`_  or
`Oracle <https://www.oracle.com/database/>`_ database.

PostgreSQL
  Supported versions are **PostgreSQL 9.6 and higher** with the **PostGIS
  extension 2.3 and higher**. Please also make sure to always install the
  latest patches and updates.

Oracle
  Supported versions are **Oracle 10g R2 and higher**. The 3D City
  Database requires spatial data support provided either through the
  Oracle *Spatial* or *Locator* extension. It is highly recommended to
  install available patches to avoid unexpected errors and to benefit from
  the latest functionality. For Oracle 10g R2, at least patch set
  10.2.0.4.0 is required for using the KML/COLLADA/glTF export
  capabilities.

The SQL scripts for creating the 3DCityDB schema are written to be executed
by the default command-line client of either database system – namely
**psql** for PostgreSQL and **SQL*Plus** for Oracle. The scripts
include meta commands specific to these clients and would not work
properly when using a different client software. So please make sure
**psql** or **SQL*Plus** is installed on the machine from where you want to
set up the 3D City Database.

Importer/Exporter tool
~~~~~~~~~~~~~~~~~~~~~~

The Importer/Exporter tool can run on any platform providing support for
Java 8 and higher. The recommended version for running the Importer/Exporter
is **Java 11**. The tool has been successfully tested on (but is not
limited to) the following operating systems:

-  Microsoft Windows XP, Vista, 7, 8, 10;
-  Apple Mac OS X and macOS;
-  Ubuntu Linux 9 to 20.

Prior to the setup of the Importer/Exporter tool, a Java Runtime
Environment (JRE) **must be installed on your system**. Java
installation packages are offered and maintained by different vendors.
The following list provides a non-exhaustive overview of Java distributions
that are free to download and use:

- `AdoptOpenJDK <https://adoptopenjdk.net/>`_
- `Amazon Coretto <https://aws.amazon.com/corretto/>`_
- `OpenJDK <https://openjdk.java.net/>`_


Follow the installation instructions for your operating system. Java binaries
from other vendors like Oracle Java, Azul Zulu or Red Hat OpenJDK might require
a license for commercial use or access to updates. Please carefully review
the license terms and conditions of use provided by the vendors.

The Importer/Exporter is shipped with an installer that will
guide you through the steps of the setup process. A full installation of
the Importer/Exporter including database scripts, plugins and test datasets
requires approx. 280 MB of hard disk space. Installing only the
mandatory application files will use approx. 160 MB of hard disk space.
Installation packages can be selected during the setup process.

The Importer/Exporter runs with 1 GB of main memory per default. This
setting should be reasonable on most platforms and for most
import/export procedures. If required, you can *manually* adapt the main
memory limits in the starter script of the program or by using environment
variables. Please refer to :numref:`impexp_interface_chapter` for more details.
