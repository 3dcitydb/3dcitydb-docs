.. _ade_manager_plugin_agg_rules:

Graph transformation rules
--------------------------

The realization of the model transformation process is mainly based on
the concept of *"Graph transformation"* and implemented using the
open source graph transformation engine
`Attributed Graph Grammar (AGG) <http://www.user.tu-berlin.de/o.runge/agg>`_.
The AGG transformation tool comes with a graphical editor that
allows users to define an arbitrary number of graph-structured
transformation rules for mapping complex object-oriented models onto a
compact relational database models. The graph transformation process
implemented for the ADE Manager plugin as well as the most relevant
transformation rules are discussed in detail in [YaKo2017]_.

While developing the ADE Manager plugin, around 50 default mapping rules
have been defined and tested. Using these predefined mapping
rules, well-known CityGML ADEs like the Energy ADE, i-UR ADE, Noise ADE,
UtilityNetwork ADE, Dynamizer ADE, IMGeo3D ADE and further custom ADEs
could be successfully and correctly transformed to compact relational schemas
for the 3DCityDB. Thus, typically, there is no need for users to change or
customize the default rules used by the transformation operation.

If you nevertheless want to modify these default rules or even add your
own additional rules, you have to build a customized version of
the ADE Manager plugin. For this purpose, clone the source code
of the ADE Manager plugin from its GitHub repository at
https://github.com/3dcitydb/plugin-ade-manager to a folder
in your local file system.

The graphical editor of the AGG tool can be started with the
runnable JAR file ``AggV21Build.jar`` that can be found in the
``lib`` subfolder of the cloned repository. On most systems,
double-clicking this JAR file will launch the AGG editor.
If this does not work for you, you can execute the AGG editor from
the command line with the following command.

.. code:: bash

   $ java -jar AggV21Build.jar

Once the AGG tool has started, use *File -> Open* from the main
menu bar of the user interface to load the default transformation rules
that are used by the ADE Manager plugin. The AGG workspace file you have to load is
called ``Working_Graph.ggx`` and is located in the subfolder
``src/main/resources/org/citydb/plugins/ade_manager/graph``. Modify this file
and the contained rules according to your needs.

.. figure:: /media/ade_manager_plugin_AGG_user_interface.png
   :name: ade_manager_plugin_AGG_user_interface
   :align: center

   AGG graph editor for defining model transformation rules for the ADE Manager plugin.

When you have completed the work with the AGG editor, you have to
compile your customized version of the ADE Manager plugin.
The plugin uses `Gradle <https://gradle.org/>`_ as build system.
To build the plugin from source, open a terminal on your local machine and
change to the folder where you have cloned the repository of the plugin.
Afterwards, run the following command in this folder.

.. code:: bash

   $ gradlew installDist

The script automatically downloads all required dependencies for building
the ADE Manager plugin. So make sure you are connected to the internet.
The build process runs on all major operating systems and only requires a
Java 8 JDK or higher to run. The build process will produce the plugin
software package under ``build/install``. Simply copy the contents of
this folder into the ``plugins`` folder of your Importer/Exporter installation
to use the plugin.