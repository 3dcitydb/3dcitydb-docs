.. _impexp_preferences_export_cityobjectgroup:

CityObjectGroup
^^^^^^^^^^^^^^^

When exporting city object groups, also group members are written to the
target dataset (cf. :numref:`impexp_export_feature_type_filter`).
Group members are always exported *by reference*.
For CityGML, this means that the ``grp:member`` property of a CityObjectGroup does
not contain the member inline but uses an *xlink:href* reference to point
to the member. In CityJSON, the ``"members"`` property of a CityObjectGroup
is per definition just an array of the identifiers of the group members.
In both cases, the group members themselves are therefore exported as separate
top-level features in the same dataset.

.. figure:: /media/impexp_export_preferences_cityobjectgroup_fig.png
   :name: impexp_export_preferences_cityobjectgroup_fig
   :align: center

   Export preferences – CityObjectGroup.

By default, group members are only exported if they satisfy the export
filter settings. This behavior can be changed using this preference dialog. When
checking the option *Export all group members as references only*,
then a reference is created for each group member of a CityObjectGroup defined
in the database, no matter whether this group member is also exported or
skipped due to filter settings. Thus, the consistency of the
references is not checked, and some references might not be
resolvable in the final dataset. The benefit of skipping this check is
that the overall performance of the export is increased.