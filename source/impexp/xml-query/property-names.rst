.. _impexp_xml_query_property_names:

<propertyNames> projection clause
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``<propertyNames>`` parameter identifies a subset of optional feature
properties that shall be kept or removed in the target dataset. Property
projections can be defined for all feature types that are part of the
export, and thus not just for top-level feature types but also for
nested feature types.

The ``<propertyNames>`` parameter consists of one ore more ``<context>``
child elements, each of which must define the target feature type through
the *typeName* attribute. A context then lists the name of one ore more
feature properties and/or generic attributes. The *mode* attribute
determines the action for these properties: 1) if set to *keep*, then
only the listed properties are kept in the target dataset, and all other
properties are deleted from the feature (*default*); 2) if set to
*remove*, then only the listed properties are deleted from the feature,
and all other properties are kept.

The following listing shows an example in which only the properties
*bldg:measuredHeight* and *bldg:lod2Solid* shall be exported for
*bldg:Building* features (*mode =* keep). Note that this implies that
all other thematic and spatial properties of buildings are deleted. For
*bldg:WallSurface* features, all properties shall be kept besides the
generic measure attribute *area* (*mode =* remove).

.. code-block:: xml

    <query>
      <propertyNames>
        <context typeName="bldg:Building" mode="keep">
          <propertyName>bldg:measuredHeight</propertyName>
          <propertyName>bldg:lod2Solid</propertyName>
        </context>
        <context typeName="bldg:WallSurface" mode="remove">
          <genericAttributeName type="measureAttribute">area</genericAttributeName>
        </context>
      </propertyNames>
    </query>

The *typeName* of the target feature type must be given as *xsd:QName*.
Like for the ``<typeNames>`` parameter, the XML namespace declaration can be
skipped if XML prefixes from :numref:`impexp_toplevel_feature_types_table`
are used. Multiple ``<context>`` elements for the same *typeName* are not allowed.

Each *propertyName* must reference a valid property of the given feature
type. This includes properties that are defined for the feature type or
inherited from a parent type in the CityGML schemas, but also properties
injected through an ADE. The *propertyName* is given as *xsd:QName*.
Mandatory properties like *gml:id* cannot be removed.

Generic attributes are also referenced by their name using a
*genericAttributeName* element. The name is case sensitive and thus must
exactly match the name stored in the database. The optional *type*
attribute can be used to more precisely specify the target generic
attribute. If *type* is omitted, then all generic attributes matching
the name are kept or removed, independent of their type. If you want to
address all generic attributes of a given type but independent of their
name, then use a *propertyName* instead as illustrated below. In this
example, all *gen:stringAttributes* are removed from *bldg:Building*.

.. code-block:: xml

    <query>
      <propertyNames>
        <context typeName="bldg:Building" mode="remove">
          <propertyName>gen:stringAttribute</propertyName>
        </context>
      </propertyNames>
    </query>

The *typeName* may also point to an abstract feature type such as
*bldg:_AbstractBuilding* or *core:_CityObject*. The property projection
is then applied to all subtypes and can even be refined on the level of
individual subtypes if the value of the *mode* attribute is identical.
If *mode* differs, then the context of the subtype overrides the context
of the (abstract) supertype.

The listing below shows how to remove *gml:name* and generic attributes
of name *location* from all city objects by defining a projection
context for the abstract type *core:_CityObject*. The projection is
refined for *bldg:Building* by additionally removing
*bldg:measuredHeight*.

.. code-block:: xml

    <query>
      <propertyNames>
        <context typeName="core:_CityObject" mode="remove">
          <propertyName>gml:name</propertyName>
          <genericAttributeName>location</genericAttributeName>
        </context>
        <context typeName="bldg:Building" mode="remove">
          <propertyName>bldg:measuredHeight</propertyName>
        </context>
      </propertyNames>
    </query>

If mode would be switched to *keep* on the *bldg:Building* context in
the above example, then this would override the *core:_CityObject*
settings for buildings. Thus, buildings would only keep the
*bldg:measuredHeight* property. The *core:_CityObject* context would,
however, still apply to all other city objects besides buildings.