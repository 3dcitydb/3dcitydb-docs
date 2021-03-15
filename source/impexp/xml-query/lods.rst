.. _impexp_xml_query_lods:

<lods> parameter
^^^^^^^^^^^^^^^^

The ``<lods>`` parameter lists the level of details (LoD) that shall be
exported for the requested feature types.

The LoDs to be exported are given as list of one or more ``<lod>`` elements
having an integer value between 0 and 4. The optional *mode* attribute
specifies whether a feature must have a spatial representation in all of
the enumerated LoDs to be exported (mode = ``and``), or whether it is
enough that the feature has a spatial representation in at least one LoD
from the list (mode = ``or``) (default). The modes ``minimum`` and ``maximum`` are
special cases of the ``or`` mode, for which only the lowest respectively highest of the
matching LoDs is exported. If a feature has additional
spatial representations in LoDs that are not listed, then these
representations are not exported. If a feature does not satisfy the LoD
filter condition at all, then it is skipped from the export.

Many feature types in CityGML can have nested sub-features. In such
cases, the top-level feature itself is not required to have a spatial
property, but the geometry can be modelled for its nested sub-features.
For example, a *bldg:Building* feature does not need to provide an LoD 2
geometry through its own *bldg:lod2Solid* or *bldg:lod2MultiSurface*
properties. Instead, it can have a list of nested boundary surfaces such
as *bldg:WallSurface* and *bldg:RoofSurface* features that have own LoD
2 representations. Nevertheless, in this case the *bldg:Building* is
considered to be represented in LoD 2.

To handle these cases, the ``<lods>`` parameter offers the optional
*searchMode* attribute. When set to *all*, then all nested features are
recursively scanned for having a spatial representation in the provided
list of LoDs. If an LoD representation is found for any (transitive)
sub-feature, then the top-level feature is considered to satisfy the
filter condition. The *all* mode is, however, expensive because it
requires many joins and sub-queries on the database level. When setting
*searchMode* to *depth* instead, you can use the additional
*searchDepth* attribute to specify the maximum depth to which nested
sub-features are searched for LoD representations.

For example, the following *bldg:Building* feature has a nested
*bldg:BuildingInstallation* sub-feature and a nested *bldg:WallSurface*
sub-feature. Moreover, the *bldg:BuildingInstallation* itself has a
nested *bldg:RoofSurface* sub-feature.

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

When setting *searchDepth* to 1 in this example, then not only the
*bldg:Building* but also its nested *bldg:BuildingInstallation* and
*bldg:WallSurface* are searched for a matching LoD representation, but
**not** the *bldg:RoofSurface* of the *bldg:BuildingInstallation*. This
roof surface is on the nesting depth 2 when counted from the
*bldg:Building*. Thus, *searchDepth* would have to be set to 2 to also
consider this *bldg:RoofSurface* feature.

Per default, *searchMode* is set to *depth* with a *searchDepth* of 1.

The following listing exemplifies the use of the ``<lods>`` parameter. In
this example, all tunnels shall be exported that have either an LoD 2 or
LoD 3 representation. LoD representations are also searched on
sub-features up to a nesting depth of 2.

.. code-block:: xml

    <query>
      <typeNames>
        <typeName>tun:Tunnel</typeName>
      </typeNames>
      <lods mode="or" searchMode="depth" searchDepth="2">
        <lod>2</lod>
        <lod>3</lod>
      </lods>
    </query>