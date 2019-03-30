Metadata Model
^^^^^^^^^^^^^^

An overview of the relational structure of the *Metadata Module* is
shown in Figure 28. The table ADE serves as a central registry for all
the registered CityGML ADEs each of which corresponds to a table row and
the relevant ADE metadata attributes are mapped onto the respective
columns. For example, each registered ADE shall own a globally unique ID
value for identification purpose. This ID value could be a UUID
(Universally Unique Identifier) which can be automatically generated and
stored in the column ADEID while registering the ADE. The columns NAME
and DESCRIPTION are mainly used for storing the basic description
information of each ADE. The column VERSION denotes the version number
of an ADE and allows to distinguish different release versions. In the
3DCityDB database schema, the database objects like tables, indexes,
foreign key constrains, and sequences of a certain ADE shall be named by
starting with a unique prefix. This allows applications to rapidly fetch
out the database schema of a certain ADE using a wildcard filter. In
this way, it is possible to automatically perform some kinds of
statistics on the ADE data contents stored in the individual tables. In
addition, the column XML_SCHEMAMAPPING_FILE is used to store the
XML-formatted schema mapping information of each ADE and is henced
defined with the CLOB data type. Another CLOB-typed column is
DROP_DB_SCRIPT where the SQL statements for dropping the individual ADE
database schema is saved and can be easily retrieved and carried out at
the database side. Moreover, the CREATION_DATE and CREATION_PERSON are
two application-specific attribute columns for providing the information
about who and when have operated the ADE registration process. This
meta-information is typically helpful for 3DCityDB users to accomplish
the administration work e.g. searching and cleaning up those ADEs that
are outdated or registered by certain database users.

|image31|

Figure 28: Technical implementation of the 3DCityDB Metadata Module in a
relational diagram

A CityGML ADE may consist of multiple application schemas one of which
should be the root schema referencing the others. Such dependency
information along with the meta-information of the individual schema are
stored in two tables, namely SCHEMA and SCHEMA_REFERENCING. The
SCHEMA_REFERENCING table is an associative table which contains two
foreign key columns REFERENCED_ID and REFERENCING_ID to link the
respective referencing and referenced schemas. In the table SCHEMA, the
flag attribute IS_ADE_ROOT is used for denoting the root schema that
directly or indirectly references all the other ADE schemas of an ADE.
In this way, the dependency hierarchy of the ADE schemas can be fully
represented in a relational model to facilitate the reconstruction of
the original schema relations through user applications. For each
schema, its meta-information such as the schema location, namespace,
namespace prefix, source XML schema definition file, as well as the file
type (e.g. plain XML text or archived) of the schema can also be stored
in the further columns of the SCHEMA table. The column CITYGML_VERSION
refers to the consideration that an ADE schema may have two different
versions, because they can be defined based on both CityGML version
1.0.0 and 2.0.0 at the same time.

The table OBJECTCLASS is a central registry for enumerating not only the
standard CityGML classes but also the classes of the registered ADEs.
Each class is assigned with a globally unique numeric ID for querying
and accessing the class-related information. As explained in the section
2.3.1.2, the ID values ranging from 0 to 113 have already been reserved
for the standard CityGML classes. Thus, the ID values of the registered
ADE classes must be larger than 113. Concerning the situation that more
additional feature classes might be introduced into the future versions
of the CityGML standard, a certain range of integer values must be
preserved and shall not be used for ADEs. Therefore, for each ADE, it is
recommended to assign its classes with a set of relatively large integer
values which can be incrementally sequenced with an initial value of
**10000**. In order to avoid the class ID conflict, each ADE shall own a
certain large value range which can be centrally maintained and
organized by an official community like the 3DCityDB group. The
OBJECTCLASS table also contains a few additional columns like the
IS_ADE_CLASS which is a flag attribute to denote which classes are
belonging to ADEs. Another column named TABLENAME refers to the table
name of a CityGML or ADE class and provides the basic information about
model mapping. The last two columns SUPERCLASS_ID and BASECLASS_ID are
two foreign key columns of the ID column for representing the
inheritance hierarchy of all the CityGML and ADE classes in a relational
structure.

In addition to the inheritance relationship, the aggregation
relationship between CityGML and ADE classes can also be represented
within a 3DCityDB instance by means of the table AGGREGATION_INFO. Its
first two columns CHILD_ID and PARENT_ID are two foreign key columns
which point to the primary key column of the table OBJECTCLASS to
reflect the two related classes. The aggregation or composition
relationship between each pair of classes can be distinguished by using
the flag attribute IS_COMPOSITE whose value can either be 0
(aggregation) or 1 (composition). In 3DCityDB, each
aggregation/composition is logically mapped onto a foreign key column or
an associative table for joining the two respective class tables. This
meta-information can also be stored in the table AGGREGATION_INFO using
its column JOIN_TABLE_OR_COLUMN_NAME. In addition, the multiplicity of
the individual aggregation/composition are stored in the two numeric
columns MIN_OCCURS and MAX_OCCURS. In case of a 0..\* relationship where
the value of the multiplicity end is unbounded, the value in the column
MAX_OCCURS shall be set NULL.
