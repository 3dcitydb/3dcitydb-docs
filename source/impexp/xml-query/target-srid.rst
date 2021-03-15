.. _impexp_xml_query_target_srid:

targetSrid attribute
^^^^^^^^^^^^^^^^^^^^

The ``<query>`` element offers an optional *targetSrid* attribute. If
*targetSrid* is provided, all exported geometries will be
transformed into the target coordinate reference system. The
*targetSrid* attribute must reference an SRID available in the underlying
database. The transformation is performed using corresponding database functions.

.. code-block:: xml

    <query targetSrid="25832">
      …
    </query>