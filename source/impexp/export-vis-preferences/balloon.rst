.. _impexp_export_vis_preferences_balloon_chapter:

Balloon
^^^^^^^

KML offers the possibility to enrich a placemark with a pop-up window
containing additional information about the placemark. This pop-up window is
called *balloon* and is shown when clicking on the placemark. The visualization export
operation supports adding balloons to the exported city objects in
all display forms.

.. note::
   The 3D web map client shipped with the 3D City Database (see :numref:`webmap_chapter`)
   **does not support KML balloons**. If you want to use this viewer,
   disable the creation of balloons. The 3D web map client offers alternative
   ways for showing additional information about a city object when clicking
   on it. For instance, you can display attributes coming from an online
   spreadsheet (e.g. Google Docs).

Balloons can be defined for each top-level feature type separately on
different subnodes of the *Balloon* preference node. This allows to
configure different balloon contents for the different feature types,
and some feature types may even be exported without balloons.
If an ADE extension is registered with the Importer/Exporter that supports exporting
ADE features for visualization, additional subnodes for each
ADE top-level feature type are automatically added and available.

The balloon preferences are illustrated in the following figure
for the *Building* feature type. The dialogs for *GenericCityObject*
and ADE top-level features additionally offer the possibility
to create balloons for their point and curve representations. The settings
for these additional balloons are, however, identical to the ones shown below.

.. figure:: /media/impexp_kml_export_preferences_balloon_building_fig.png
   :name: pic_kml_collada_gltf_preferences_balloon_building
   :align: center

   Visualization export preferences – Balloon settings.

In general, the content to be displayed inside a KML balloon can be
a simple string or an HTML document. The HTML content can be styled
using external Cascading Style Sheet (CSS) and may contain JavaScript
code.

The balloon options offer two different ways to provide the balloon
content for a city object. First, you can choose to *use the content
from the generic attribute* **Balloon_Content**. This option offers a lot
of flexibility because the content can be different for each city object.
Second, you can *use the content from a file*. This way you can define
a balloon template in an external file, and this template is used for all city objects.
Thus, all balloons will share the same layout and content. Simply provide
the path to the template file in the corresponding text field or click
the *Browse* button to open a file selection dialog. Example HTML template
files are provided in the subfolder ``templates/balloons`` of the
Importer/Exporter installation directory.

Both options can also be combined such that the external template file
is *only used as fallback* in case a city object lacks the
the generic attribute *Balloon_Content*.

By default, the balloons are included in the KML output files generated
by the export operation for each visualization model. Alternatively, you
can choose to export them into a separate file per feature by enabling
the corresponding option. In the latter case, they are stored inside a
``balloons`` directory relative to the visualization model.
When storing the balloons in separate files, you must make sure that these
files can be accessed from your viewer. For example, when using Google Earth,
access to local files must be granted in the Tools -> Options -> General
settings.

.. note::
   When using Google Earth as viewer and *COLLADA/glTF* as display form,
   it is recommended to use a *highlight style* for those feature types
   that shall receive a balloon. The balloon is then attached to the
   highlight geometry. In constrast to the COLLADA model, the
   highlight geometry is directly clickable in the scene.

The balloon content does not need to be static. Instead, it can contain
references to data stored in the 3DCityDB for each individual city object.
These references are dynamically resolved at export time and
replaced with the actual value from the database.
The references must be put inside a ``<3DCityDB>`` element within the balloon template
so that they can be recognized by the export operation. This works both in
case the content is stored in the generic attribute or an external file.
As mentioned above, balloon content provided in HTML may also contain JavaScript code.
If you use JavaScript for your balloon, you can also use ``<3DCityDB>``
placeholders inside your code.

The ``<3DCityDB>`` element offer a rich set of expressions and keywords
that can be used to precisely describe the data you want to fetch from the database.
The use of these expression to create dynamic balloon content
is discussed in detail in :numref:`impexp_dynamic_balloon_content_chapter`.
The following figure shows an example of a balloon that contains
feature-specific attributes and information that are dynamically
populated during the export using ``<3DCityDB>`` references.

.. figure:: /media/impexp_kml_export_balloon_dynamic_contents_fig.png
   :name: pic_kml_collada_gltf_preferences_balloon_dynamic
   :align: center

   Dynamic balloon content showing feature-specific attributes and information.