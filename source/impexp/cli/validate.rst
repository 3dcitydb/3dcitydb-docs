.. _impexp_cli_validate_command:

Validate command
----------------

**Synopsis**

.. code-block:: bash

   impexp validate [-hV] [--ade-extensions=<folder>] [-c=<file>]
                   [--log-file=<file>] [--log-level=<level>]
                   [--pid-file=<file>] [--plugins=<folder>]
                   [@<filename>...] <file>...

**Description**

The ``validate`` command validates one ore more CityGML or CityJSON files
against the official CityGML XML and CityJSON schemas. It corresponds to the validate
functionality of the import operation offered on the *Import* tab of the
graphical user interface (see :numref:`impexp_citygml_import_chapter`).
The validation does not require internet access since the schemas are packaged
with the application. Validation errors are reported on the console.

.. note::
   CityGML ADE schemas are automatically considered in the validation process
   if the ADE has been correctly registered with the Importer/Exporter (see
   :numref:`impexp_plugin_ade_manager_chapter` for more details). This way, also
   ADE data can be validated. CityJSON Extension schemas are, however,
   not supported by the validation process. Please use an external tool like
   `cjio <https://github.com/cityjson/cjio/>`_ to validate such datasets.

.. note::
   The ``impexp`` tool terminates with exit code 1 in case the
   validation fails.

**Options**

.. option:: <file>...

   One or more input files or directories to be validated. This parameter is mandatory.
   Glob patterns are supported to specify a set of filenames using wildcard characters
   (e.g., ``/path/to/*.gml``). If a directory is provided, it will be recursively scanned
   for CityGML/CityJSON files. The supported input file formats and extensions are
   identical with those listed in :numref:`import_supported_file_formats`.

**Examples**

.. code-block:: bash

   $ impexp validate /path/to/my_city.gml /path/to/my_city.json

Validate the CityGML file ``my_city.gml`` and the CityJSON file ``my_city.json``
against the respective schema definition.