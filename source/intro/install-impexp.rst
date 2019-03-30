Installation of the Importer/Exporter
-------------------------------------

Download the universal installer from the 3DCityDB website at
http://www.3dcitydb.org or at
https://github.com/3dcitydb/importer-exporter/releases and save it to
your local file system. The installer is shipped as an executable Java
Archive (JAR) file. To run the installation wizard, simply double-click
on the *3DCityDB-Importer-Exporter-4.1.0-Setup.jar* file.

|image57|

Figure 51: Installation wizard of Import/Export tool (Step 5).

After accepting the license agreement and specifying an installation
directory, you can choose the software packages to be installed. It is
recommended to at least select the packages ‘\ *3D City Database*\ ’ and
‘\ *Documentation’*. The ‘\ *3D City Database*\ ’ package contains all
SQL scripts that are required for setting up an instance of the 3D City
Database on your spatial database system. Please refer to chapter 3.3
for a step-by-step guide on how to use the SQL scripts. The package
‘\ *Sample CityGML and KML/COLLADA datasets*\ ’ contains license-free
sample data that may be used in first tests.

The option ‘\ *Plugins*\ ’ allows a user to install plugins for the
Importer/Exporter, which add further functionality to the tool. This
release is shipped with the ‘\ *Spreadsheet Generator Plugin*\ ’ and the
‘\ *ADE Manager Plugin*\ ’. A documentation of both plugins is provided
in chapters 6.2 and 6.3. More plugins may be added in future releases.

The ‘\ *3D Web Map Client*\ ’ is a web-based viewer for 3DCityDB content
and provides high-performance 3D visualization and interactive
exploration of arbitrarily large semantic 3D city models on top of the
open source Cesium Virtual Globe (refer to chapter 8 for the complete
documentation).

After successful installation, the contents of all selected installation
packages are available in the installation directory. To run the
Importer/Exporter, simply use the starter script in the *bin* subfolder
(refer to chapter 5.1 for more information).

.. note::
   Before the Importer/Exporter can connect to an Oracle/PostgreSQL
   database, **the 3D City Database schema must have been set up**. Please
   follow the instructions provided in the next chapter.

The installation directory contains the following subfolders:

================= ============ ===================================================================================================================================================================================================================
**Folder**        **Optional** **Explanation**
3dcitydb          **x**        **Contains all SQL scripts and stored procedures for operating the 3DCityDB**
3d-web-map-client **x**        **Contains a ZIP archive containing all files required to install the 3D Web Map Client on a web server**
ade-extensions                 **Contains extension packages to support CityGML ADEs.** ADE extensions only must be copied to this directory to make them available in the program.
bin                            **Platform-specific starter scripts to launch the Importer/Exporter. For instance, under Windows, double-click on 3DCityDB-Importer-Exporter.bat to run the program**
contribs                       **Third-party tools required by the Importer/Exporter (e.g. collada2gltf converter binaries)**
lib                            Contains all libraries required by the Importer/Exporter
licence                        Contains the license documents for Importer/Exporter
manual            x            Contains the documentation for the **3DCityDB and the tools**
plugins                        Contains plugins of the Importer/Exporter. Plugins only have to be copied to this directory to make them available in the program.
samples           x            Contains CityGML and KML/COLLADA test datasets
templates                      Contains HTML templates for information balloons for KML/COLLADA exports, a selection of coordinate reference systems in the form of XML documents, and example XSLT stylesheets to be used in imports and exports.
uninstaller                    Contains a JAR executable that uninstalls the Importer/Exporter
README.txt                     A brief information about the application
================= ============ ===================================================================================================================================================================================================================

Table 18: Contents of the installation directory.
