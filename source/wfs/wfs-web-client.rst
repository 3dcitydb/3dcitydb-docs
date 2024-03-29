.. _wfs_web_based_client_chapter:

Web-based WFS client
--------------------

The 3D City Database WFS is shipped with a simple web-based client that
is mainly meant to test the functionality of the server. The client is
automatically installed with the server and is available at the
following URL (cf. :numref:`wfs_service_url_chapter` for details):

::

   http[s]://[host][:port]/[context_path]/wfsclient

The screenshot below shows the user interface of the client rendered in
a standard web browser.

.. figure:: /media/wfs_web_client_fig.png
   :name: wfs_web_client_fig
   :align: center

   Web-based WFS client.

The user interface consists of two text fields. A user simply enters the
XML-encoded operation request that shall be sent to the server into the
upper text field named *WFS Request* [1]. Clicking on the *Send* button
forwards the request to the server. As soon as the response document is
received from the WFS server, it is rendered in the lower text field
named *WFS Result*.

.. caution::
   Avoid sending requests through this client that might potentially
   result in a large number of city objects contained in the response
   document. Otherwise the available main memory of the web browser is
   quickly exhausted when trying to display the response document, which
   renders the browser non-responsive or might even lead to a program
   crash.

You may want to use the *count* attribute on the
GetFeature request in order to limit the maximum number of features to
be contained in the response document. Alternatively, you can specify
the “\ *hits*\ ” value for the *resultType* attribute in order to only
receive the number of features matching your query instead of the
features themselves (cf. :numref:`wfs_getfeature_operation_chapter`).
