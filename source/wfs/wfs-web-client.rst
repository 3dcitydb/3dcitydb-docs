Web-based WFS client
--------------------

The 3D City Database WFS is shipped with a simple web-based client that
is mainly meant to test the functionality of the server. The client is
automatically installed with the server and is available at the
following URL (cf. chapter 7.4.1.2 for details):

.. code-block:: http

   http[s]://[host][:port]/[context_path]/wfsclient

The screenshot below shows the user interface of the client rendered in
a standard web browser.

|image183|

Figure 152: Web-based WFS client.

The user interface consists of two text fields. A user simply enters the
XML-encoded operation request that shall be sent to the server into the
upper text field named *WFS Request* [1]. Clicking on the *Send* button
forwards the request to the server. As soon as the response document is
received from the WFS server, it is rendered in the lower text field
named *WFS Result*.

.. warning::
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
features themselves (cf. chapter 7.4.6).

.. |image183| image:: ../media/image190.png
   :width: 5.42953in
   :height: 4.58446in
