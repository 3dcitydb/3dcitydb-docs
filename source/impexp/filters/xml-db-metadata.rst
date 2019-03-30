3DCityDB metadata
^^^^^^^^^^^^^^^^^

The 3DCityDB stores database-specific
metadata with every city object using the columns
LAST_MODIFICATION_DATE, UPDATING_PERSON, REASON_FOR_UPDATE and LINEAGE
of the CITYOBJECT table. In order to make these metadata properties
available in filter expressions, the 3DCityDB ADE injects them into the
CityGML *core:_CityObject* feature.

==================================== ============= ===============================
**ADE property                       **Data type** **Column of the ADDRESS table**
(injected into core:_CityObject)**                
**citydb:** **lastModificationDate** **xs:string** **LAST_MODIFICATION_DATE**
**citydb:** **updatingPerson**       **xs:string** **UPDATING_PERSON**
**citydb:** **reasonForUpdate**      **xs:string** **REASON_FOR_UPDATE**
**citydb:** **lineage**              **xs:string** **LINEAGE**
==================================== ============= ===============================

Table 36: 3DCityDB ADE properties for accessing address information.

The properties can also be used in filter expressions. For instance, the
query below fetches all bridges that have been modified in the database
after *2018-01-01*.

| <query>
| <typeNames>
| <typeName>brid:Bridge</typeName>
| </typeNames>
| <filter>
| <propertyIsGreaterThan>
| <valueReference>citydb:lastModificationDate</valueReference>
| <literal>2018-01-01</literal>
| </propertyIsGreaterThan>
| </filter>
| </query>
