.. _impexp_xml_query_appearance:

<appearance> parameter
^^^^^^^^^^^^^^^^^^^^^^

The ``<appearance>`` parameter filters appearances by their theme. To keep
an appearance in the target dataset, the value of its *app:theme*
attribute simply has to be enumerated using a ``<theme>`` subelement. The
string values must match exactly.

The *app:theme* attribute in CityGML is optional and thus can be null.
To be able to also express whether appearances having a *null* theme
should be exported, the ``<appearance>`` parameter offers another subelement
``<nullTheme>``, which is of type Boolean. If set to *true*, appearances
with a null theme are exported, otherwise not (default).

The following query exports road features and appearances with theme
*summer* and *winter*. Since ``<nullTheme>`` is set to *false*, appearances
lacking an *app:theme* attribute are not exported.

.. code-block:: xml

    <query>
      <typeNames>
        <typeName>tran:Road</typeName>
      </typeNames>
      <appearance>
        <nullTheme>false</nullTheme>
        <theme>summer</theme>
        <theme>winter</theme>
      </appearance>
    </query>