Sequences
~~~~~~~~~

:numref:`citydb_schema_sequences_diagram` lists predefined
sequences from which multiple users may
generate unique integers for primary keys automatically. Sequences help
to coordinate primary keys across multiple rows and tables. For
instance, the ID values of the BUILDING table are generated from the
CITYOBJECT_SEQ sequence. The sequences are defined to start with 1 and
to be incremented by 1 when a sequence number is generated. It is highly
recommended to generate ID values for all tables by using the predefined
sequences only.

The sequence GRID_COVERAGE_RDT_SEQ does not exist in the PostgreSQL
version as the corresponding table does not exist.

.. figure:: ../../media/citydb_schema_sequences_diagram.png
   :name: citydb_schema_sequences_diagram

   Overview of all sequences used in 3DCityDB

