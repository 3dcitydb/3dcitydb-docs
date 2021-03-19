.. _impexp_chapter:

Importer/Exporter
=================

The Importer/Exporter tool is a Java-based client for
the 3D City Database and allows for high-performance loading and
extracting 3D city model data.

It offers a graphical user interface (GUI) for convenient use on desktop
computers by end-users. And it can also be run without a GUI via its command-line
interface (CLI). The CLI is useful on headless systems or to embed
the Importer/Exporter in batch processing workflows and third-party applications.

This chapter explains the functionality and use of the Importer/Exporter along the
GUI of the tool. :numref:`impexp_cli_chapter` is then dedicated to a discussion of
the CLI. For system requirements and a documentation of the installation procedure,
please refer to :numref:`first_steps_system_requirements_chapter` .

.. toctree::
   :maxdepth: 1

   launching
   gui
   database
   import
   export
   export-vis
   preferences/index
   map-window
   cli

.. hint::
  The Importer/Exporter also serves as reference implementation for the
  3D City Database. So, if you wonder how to correctly populate the database tables,
  it is recommended to create simple example datasets in CityGML or CityJSON
  format and to load them into the database using the Importer/Exporter. Afterwards,
  you can inspect how the data is represented in the 3D City Database schema.