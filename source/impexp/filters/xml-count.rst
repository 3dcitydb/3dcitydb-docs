<count> parameter
^^^^^^^^^^^^^^^^^

The <count> parameter limits the number of explicitly requested
top-level city objects in the export dataset.

The mandatory <upperLimit> element denotes the number of city objects to
be exported. When combined with the optional <lowerLimit> element, then
the range of city objects from the *lowerLimit* position to the
*upperLimit* position in the result set are exported. Note that both
*lowerLimit* and *upperLimit* are inclusive in this case.

The following query shows how to export at maximum 10 buildings from the
database, even if more buildings satisfy the query expression.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <count>
| <upperLimit>10</upperLimit>
| </count>
| </query>

The following query would export at maximum 11 buildings (from the
10\ :sup:`th` to the 20\ :sup:`th` building in the result set). If the
result set contains less buildings, then the export dataset will, of
course, also contain less buildings.

| <query>
| <typeNames>
| <typeName>bldg:Building</typeName>
| </typeNames>
| <count>
| <lowerLimit>10</lowerLimit>
| <upperLimit>20</upperLimit>
| </count>
| </query>
