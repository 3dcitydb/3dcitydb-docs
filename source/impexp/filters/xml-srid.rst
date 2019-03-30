*targetSrid* attribute
^^^^^^^^^^^^^^^^^^^^^^

The <query> element offers an optional *targetSrid* attribute. If
*targetSrid* is present, then all exported geometries will be
transformed into the target coordinate reference system. The
*targetSrid* attribute must reference an SRID defined in the underlying
database. The transformation is performed using corresponding functions
of the database system.

| <query targetSrid="25832">
| …
| </query>
