.. _first_steps_system_requirements_chapter:

System requirements
-------------------

3D City Database
~~~~~~~~~~~~~~~~

Setting up an instance of the 3D City Database requires an existing
installation of a `PostgreSQL <https://www.postgresql.org/>`_,
`Oracle <https://www.oracle.com/database/>`_, or
`PolarDB <https://www.alibabacloud.com/de/product/polardb>`_ database.

PostgreSQL with PostGIS extension
  Supported versions are **PostgreSQL 11 and higher** with **PostGIS 2.5 and higher**.
  Make sure to check the `PostgreSQL versioning policy <https://www.postgresql.org/support/versioning/>`_
  to find out which PostgreSQL versions are actively maintained or have reached end-of-life.
  The `PostGIS support matrix <https://trac.osgeo.org/postgis/wiki/UsersWikiPostgreSQLPostGIS>`_
  shows which versions of PostgreSQL are supported by which versions of PostGIS
  and whether a specific PostGIS version has reached end-of-life.

Oracle
  Supported versions are **Oracle 19c and higher**. Make sure to check the
  `Oracle lifetime support policy <https://www.oracle.com/support/lifetime-support/resources.html>`_
  to find out which Oracle versions are actively maintained or have reached end-of-life.

PolarDB for PostgreSQL with Ganos extension
  Supported versions are **PolarDB 1.1 and higher** with **Ganos 4.6 and higher**. Make
  sure to check the `PolarDB documentation <https://www.alibabacloud.com/de/product/polardb/>`_
  to find out which PolarDB and Ganos versions are actively maintained or have reached end-of-life.

.. note::
  It is recommended that you always install the latest patches, minor releases, and
  security updates for your database system. Database versions that have reached
  end-of-life are no longer supported by the 3D City Database.

The SQL scripts for creating the 3D City Database schema are written to be executed
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

-  Microsoft Windows 7, 8, 10, 11;
-  Apple Mac OS X and macOS;
-  Ubuntu Linux 9 to 22.

Prior to the setup of the Importer/Exporter tool, a Java Runtime
Environment (JRE) **must be installed on your system**. Java
installation packages are offered and maintained by different vendors.
The following list provides a non-exhaustive overview of Java distributions
that are free to download and use:

- `Eclipse Temurin <https://adoptium.net/>`_
- `Amazon Coretto <https://aws.amazon.com/corretto/>`_
- `OpenJDK <https://openjdk.java.net/>`_
- `Oracle Java 17 LTS <https://www.oracle.com/java/technologies/downloads/>`_

Follow the installation instructions for your operating system. Note that
starting from Java 17 long-term support (LTS) version, `Oracle Java` is
again released under a no-fee and free-to-use license, while use of previous versions
of Oracle Java remains restricted to a fee-based subscription license. Likewise, Java binaries
from other vendors like Azul Zulu or Red Hat OpenJDK might require
a license for commercial use or access to updates. Please carefully review
the license terms and conditions of use provided by the vendors.

The Importer/Exporter is shipped with an installer that will
guide you through the steps of the setup process. A full installation of
the Importer/Exporter including database scripts, plugins and test datasets
requires approx. 280 MB of hard disk space. Installing only the
mandatory application files will use approx. 160 MB of hard disk space.
Installation packages can be selected during the setup process.

The Importer/Exporter runs with 1 GB of main memory per default. The maximum
available main memory is controlled by JVM default options. This
setting should be reasonable on most platforms and for most
import/export processes. If required, you can *manually* adapt the main
memory limits in the starter script of the program or by using environment
variables. Please refer to :numref:`impexp_launching_chapter` for more details.
