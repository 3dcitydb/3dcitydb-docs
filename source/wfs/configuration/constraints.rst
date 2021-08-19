.. _wfs_constraints_settings_chapter:

Constraints settings
~~~~~~~~~~~~~~~~~~~~

The ``<constraints>`` element of the ``config.xml`` allows for defining
constraints on dedicated WFS operations.

.. code-block:: xml
   :name: wfs_constraints_settings_config_listing

   <constraints>
     <supportAdHocQueries>true</supportAdHocQueries>
     <countDefault>500</countDefault>
     <computeNumberMatched>true</computeNumberMatched>
     <useDefaultSorting>false</useDefaultSorting>
     <currentVersionOnly>true</currentVersionOnly>
     <exportCityDBMetadata>false</exportCityDBMetadata>
     <exportAppearance>false</exportAppearance>
     <useResultPaging>true</useResultPaging>
     <stripGeometry>false</stripGeometry>
     <lodFilter isEnabled="true" mode="or" searchMode="depth" searchDepth="1">
       <lod>1</lod>
       <lod>2</lod>
     </lodFilter>
   </constraints>

The following tables shows which WFS operation is affected by which constraint.

.. list-table::  Overview of WFS operations affected by constraints
   :name: wfs_constraints_operations_table
   :widths: 30 70
   :align: center

   * - | **Contraint**
     - | **WFS operations**
   * - | ``supportAdHocQueries``
     - | GetPropertyValue, GetFeature
   * - | ``countDefault``
     - | GetPropertyValue, GetFeature
   * - | ``computeNumberMatched``
     - | GetPropertyValue, GetFeature
   * - | ``useDefaultSorting``
     - | GetPropertyValue, GetFeature
   * - | ``currentVersionOnly``
     - | GetPropertyValue, GetFeature
   * - | ``exportCityDBMetadata``
     - | GetPropertyValue, GetFeature
   * - | ``exportAppearance``
     - | GetPropertyValue, GetFeature
   * - | ``useResultPaging``
     - | GetPropertyValue, GetFeature
   * - | ``stripGeometry``
     - | GetPropertyValue, GetFeature
   * - | ``lodFilter``
     - | GetPropertyValue, GetFeature

**supportAdHocQueries**

If the WFS shall support "ad hoc" queries besides stored queries, then the ``<supportAdHocQueries>``
constraint has to be set to true (default: true). Stored queries are predefined queries that are
stored by the server and at most allow setting predefined parameters. In contrast, ad hoc queries
enable clients to send their own queries with arbitrary filter expressions to the WFS service.

**countDefault**

The ``<countDefault>`` constraint restricts the
number of city objects to be
returned by the WFS to the user-defined value, even if the request is
satisfied by more city objects in the 3D City Database. The default
behavior is to return *all* city objects matching a request. If a
maximum count limit is defined, then this limit is automatically
advertised in the server’s capabilities document using the ``CountDefault``
constraint.

**computeNumberMatched**

By default, the WFS server determines the total number of city objects or values that match
the request and returns this number as *numberMatched* attribute in the response document.
Computing that number might take a long time on large databases. By setting the constraint
``<computeNumberMatched>`` to false (default: true), the number of matches is not computed and
*"unknown"* is returned as value for the *numberMatched* attribute.

In addition to *numberMatched*, the *numberReturned* attribute denotes the number of city objects
or values that are actually contained in the response document. This value must always be
determined for every response document. To keep response times short for large databases,
it is therefore not only recommended to set ``<computeNumberMatched>`` to false, but also to restrict
the number of returned objects or values already in the query by using, for instance, reasonable
filter expressions or the *count* and *startIndex* parameters.

**useDefaultSorting**

Using the *fes:SortBy* clause of a query, a client can request a specific sorting of the objects
in the response document. If no *fes:SortBy* clause is present, then the WFS will not  automatically
apply a default sorting. Although strictly speaking this behavior violates a conformance requirement
of the WFS 2.0 specification, requests without sorting can be processed and answered faster.

To change the default behaviour, the ``<useDefaultSorting>`` constraint has to be set to true
(default: false). The result set is then automatically sorted by the database ID of the city objects
in case the client does not specify a *fes:SortBy* clause. On the one hand, this ensures that subsequent
invocations of the same query on the same set of data result in a response document that presents the
objects in the same order. And, on the other hand, the WFS complies to the corresponding conformance
requirements.

**currentVersionOnly**

City objects can be marked as being terminated in the 3DCityDB using the
attribute *terminationDate* without having to physically delete them from
the database. This allows keeping the object history in the database and
querying previous object versions based on timestamps. The current version
of a city object can be simply identified by the fact that the column
TERMINATION_DATE of the CITYOBJECT is not set. If a client wants to explicitly request the
current or a specific previous version of the city objects, a corresponding filter expression
on *core:terminationDate* must be defined for the query. Otherwise, the response may contain
duplicates of the same city object at different *core:terminationDate* times.

Setting an appropriate filter can be quickly forgotten or might not be supported by a given
WFS client software. By default, the WFS therefore returns only the current version of a city object
in responses to *GetPropertyValue* and *GetFeature* requests by automatically adding a filter
expression on *core:terminationDate*. If you want to suppress this default behavior, you can set
the ``<currentVersionOnly>`` constraint to false (default: true). In this case a user of the WFS
must ensure to define a proper filter expression on each request. If a 3DCityDB is used without object
history, the default behavior can be safely deactivated. Clearly, without this additional filter,
requests can be answered faster by the WFS server.

**exportCityDBMetadata**

The ``<exportCityDBMetadata>`` (default: *false*) flag allows exporting
the metadata properties LINEAGE, UPDATING_PERSON, LAST_MODIFICATION_DATE
and REASON_FOR_UPDATE of city objects stored in the table CITYOBJECT.
Since these properties are not defined by CityGML, the WFS uses a
CityGML Application Domain Extension (ADE) to include the properties
in the response document.

**exportAppearance**

The WFS supports the export of appearance properties (i.e., material and texture information)
of city objects. However, this requires setting ``<exportAppearance>`` to true. The default value
for this constraint is false, so that no appearance properties are returned by the WFS by default.
The export includes both local and global appearances. Since global appearances are not stored as
inline attributes of the city objects but rather as individual top-level features, they are returned
within the ``<wfs:additionalObjects>`` element of the response document in accordance with the WFS specification.

.. note::
   Further settings for exporting appearances can be found in :numref:`wfs_server_settings_chapter`.

**useResultPaging**

Result paging is the ability of a client to scroll through a set of response features or values,
*n* features or values at a time much like one scrolls through the response from a search engine
one page at a time. In order for paging to be triggered, either the *count* parameter shall be set
on the request or the WFS server shall implement a default count value (see *countDefault* constraint).
Result paging is accomplished following the *previous* and *next* URLs defined on the response document.

Result paging is enabled by default for the WFS. To disable it, simply set the ``<useResultPaging>``
constraint to false (default: true). Whether result paging is available is also advertised in the
server’s capabilities document using the *ImplementsResultPaging* constraint.

.. note::
   Further settings in the context of result paging can be found in :numref:`wfs_server_settings_chapter`.

**stripGeometry**

When setting ``<stripGeometry>`` to *true* (default: *false*), the WFS will
remove all spatial properties from a city object before returning the
city object to the client. Thus, the client will not receive any
geometry values.

**lodFilter**

The ``<lodFilter>`` constraint defines a server-side filter on the LoD
representations of the city objects. When using this constraint, city
objects in a response document will only contain those LoD levels that
are enumerated using one or more ``<lod>`` child elements of ``<lodFilter>``.
Further LoD representations of a city object, if any, are automatically
removed. If a city object satisfies a query but does not have a geometry
representation in at least one of the specified LoD levels, it will be
skipped from the response document and thus not returned to the client.

The default behavior of the LoD filter can be adapted using attributes
on the ``<lodFilter>`` element. The *mode* attribute defines how the
selected LoDs should be evaluated and can take one of the values shown
described below.

.. list-table::  Available filter modes
   :name: wfs_lod_filter_mode_table
   :widths: 20 70

   * - | **Filter mode**
     - | **Description**
   * - | **or**
     - | City objects having a spatial representation in *at least one* of the selected LoDs will be exported. Additional LoD representations of the city object that do not match the user selection are not exported.
   * - | **and**
     - | Only city objects having a spatial representation in *all* of the selected LoDs will be exported. Additional LoD representations of the city object that do not match the user selection are not exported.
   * - | **minimum**
     - | This is a special version of the *Or* mode that only exports the lowest LoD representation from the matching ones. The exported LoD may therefore differ for each city object.
   * - | **maximum**
     - | This is a special version of the *Or* mode that only exports the highest LoD representation from the matching ones. The exported LoD may therefore differ for each city object.


The default *mode* value is *or*. When setting the *searchMode* attribute to *depth*, then you can use the
additional *searchDepth* attribute to specify how many levels of
nested city objects shall be considered when searching for matching
LoD representations. If *searchMode* is set to *all*, then all nested
city objects will be considered (default: *searchMode = depth, searchDepth = 1*).

The following example illustrates the use of the *seachDepth* attribute. Assume a *Building* feature having a nested
*BuildingInstallation* sub-feature and a nested *WallSurface* sub-feature as direct children. Moreover, the
*BuildingInstallation* itself has a nested *RoofSurface* sub-feature.

.. code-block:: xml

    <bldg:Building>
      …
      <bldg:outerBuildingInstallation>
        <bldg:BuildingInstallation>
          <bldg:boundedBy>
            <bldg:RoofSurface> … </bldg:RoofSurface>
          </bldg:boundedBy>
        </bldg:BuildingInstallation>
      </bldg:outerBuildingInstallation>
      …
      <bldg:boundedBy>
        <bldg:WallSurface> … </bldg:WallSurface>
      </bldg:boundedBy>
      …
    </bldg:Building>

When setting *search depth* to "1" in this example, not only the
*bldg:Building* but also its nested *bldg:BuildingInstallation* and
*bldg:WallSurface* are searched for a matching LoD representation, but
**not** the *bldg:RoofSurface* of the *bldg:BuildingInstallation*. This
roof surface is on the nesting depth 2 when counted from the
*bldg:Building*. Thus, *search depth* would have to be set to "2" to also
consider this *bldg:RoofSurface* feature.

.. note::
   The more levels you enter for the *searchDepth* attribute, the more
   complex the resulting SQL queries for the 3DCityDB will get.