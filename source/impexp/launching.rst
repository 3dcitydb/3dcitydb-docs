.. _impexp_launching_chapter:

Launching the Importer/Exporter
-------------------------------

The 3D City Database Importer/Exporter offers both a graphical user
interface (GUI) and a command-line interface (CLI). The CLI allows for
embedding the tool in batch processing workflows and third-party
applications. The usage of the CLI is documented in :numref:`impexp_cli_chapter`.

To launch the GUI, simply use the starter script located in the
installation directory of the Importer/Exporter. A desktop icon as well as shortcuts in the start menu
of your operating system will additionally be available in case you
chose to create shortcuts during setup. Depending on your platform, one
of the following starter scripts is provided in the installation directory:

- ``3DCityDB-Importer-Exporter.bat`` (Microsoft Windows family)
- ``3DCityDB-Importer-Exporter`` (UNIX/Linux/Mac OS family)

On most platforms, double-clicking the starter script or its shortcut
runs the Importer/Exporter.

For some UNIX/Linux distributions, you will have to run the starter
script from within a shell environment though. Please open your
favourite shell and first check whether execution rights are correctly
set on the starter script. If not, change to the installation folder and
enter the following command to make the starter script executable for
the owner of the file:

.. code:: bash

   $ chmod u+x 3DCityDB-Importer-Exporter

Afterwards, simply run the software by issuing the following command:

.. code:: bash

   $ ./3DCityDB-Importer-Exporter

.. note::
   With every release, the README.txt file in the installation
   folder provides version-specific information on how to
   run the Importer/Exporter.

**Using environment variables in the launch process**

The Importer/Exporter is launched with default options for the
Java Virtual Machine (JVM) that runs the application. You can override
these default options in the launch process by using the environment
variable ``JAVA_OPTS``.

For example, ``JAVA_OPTS`` can be used to control the amount of main
memory that the Importer/Exporter can use. By default, the tool runs with
at minimum 1 GB of main memory. The maximum available main memory depends
on your Java version. When running on Java 11, for instance,
the Importer/Exporter can use 25% of the available physical main memory
of your machine with an upper limit of 25 GB. This value should be reasonable
on most platforms and for most import/export processes. However, when
working with very large large CityGML top-level features or CityJSON
input files, you might need to increase the memory limits
to avoid out-of-memory exceptions. This can be done with the JVM parameters
``-Xms`` for the initial memory size and ``-Xmx`` for the maximum memory
size (please check the documentation of your Java installation for more details).

The following snippet shows how to use ``JAVA_OPTS`` to set the memory
limits to an initial size of 1 GB and a maximum size of 8 GB under Windows.

.. code:: bash

  set JAVA_OPTS="-Xms1G -Xmx8GB"

For UNIX/Linux, it looks almost the same:

.. code:: bash

  export JAVA_OPTS="-Xms1G -Xmx8GB"

Simply copy this line into the start script ``3DCityDB-Importer-Exporter``
and make sure to put it before the last line in this script. Note that you
can set any other JVM option as well.

Instead of ``JAVA_OPTS`` you can also use ``IMPEXP_OPTS`` as environment variable.
Both are supported by the Importer/Exporter and evaluated in the launch process.

**Adapting the CLI start script**

The start script ``3DCityDB-Importer-Exporter`` is only a wrapper and invokes
the CLI script ``impexp`` of the Importer/Exporter, which
is located in the ``bin`` folder of the installation directory. To get more fine-grained
control over the launch process, you can also edit this CLI start script directly.
Good knowledge in shell scripting is required when doing so.