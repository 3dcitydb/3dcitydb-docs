.. _wfs_constraints_settings_chapter:

Constraints settings
~~~~~~~~~~~~~~~~~~~~

The ``<constraints>`` element of the ``config.xml`` allows for defining
constraints on dedicated WFS operations.

.. code-block:: xml
   :name: wfs_constraints_settings_config_listing

   <constraints>
     <countDefault>10</countDefault>
     <currentVersionOnly>true</currentVersionOnly>
     <exportCityDBMetadata>false</exportCityDBMetadata>
     <stripGeometry>false</stripGeometry>
     <lodFilter mode="and" searchMode="depth" searchDepth="2">
       <lod>2</lod>
       <lod>3</lod>
     </lodFilter>
   </constraints>

**countDefault**

The ``<countDefault>`` constraint restricts the
number of city objects to be
returned by the WFS to the user-defined value, even if the request is
satisfied by more city objects in the 3D City Database. The default
behavior is to return *all* city objects matching a request. If a
maximum count limit is defined, then this limit is automatically
advertised in the server’s capabilities document using the ``CountDefault``
constraint.

**currentVersionOnly**

City objects can be marked as being terminated in the 3DCityDB using the
attribute *terminationDate* without having to physically delete them from
the database. This allows keeping the object history in the database and
querying previous object versions based on timestamps. The current version
of a city object can be simply identified by the fact that the column
TERMINATION_DATE of the CITYOBJECT is not set. By default, the WFS only
returns the current version of a city object
in responses to *GetFeature* requests by automatically adding a filter
expression on the *terminationDate* attribute. If you want to change
this default behaviour, set the ``<currentVersionOnly>`` flag to *false*
(default: *true*).

**exportCityDBMetadata**

The ``<exportCityDBMetadata>`` (default: *false*) flag allows exporting
the metadata properties LINEAGE, UPDATING_PERSON, LAST_MODIFICATION_DATE
and REASON_FOR_UPDATE of city objects stored in the table CITYOBJECT.
Since these properties are not defined by CityGML, the WFS uses a
CityGML Application Domain Extension (ADE) to include the properties
in the response document.

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
on the ``<lodFilter>`` element. The *mode* attribute defines whether a city
object must have a spatial representation in all (*and*) or just one
(*or*) of the provided LoD levels. If setting *searchMode* to
*depth*, then you can use the additional *searchDepth* attribute
to specify how many levels of nested city objects shall be considered
when searching for matching LoD representations. If *searchMode* is set
to *all*, then all nested city objects will be considered
(default: *searchMode = depth, searchDepth = 1*).

.. note::
   The more levels you enter for the *searchDepth* attribute, the more
   complex the resulting SQL queries for the 3DCityDB will get.