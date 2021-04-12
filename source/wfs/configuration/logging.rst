.. _wfs_logging_settings_chapter:

Logging settings
~~~~~~~~~~~~~~~~

The WFS service logs messages and errors that occur during operations to
a dedicated log file. Entries in the log file are associated with a
timestamp, the severity of the event and the IP address of the client
(if available). Per default, the log is stored in the file
``WEB-INF/wfs.log`` within the *application folder* of the WFS web
application.

The ``<logging>`` element in the ``config.xml`` file is used to adapt these
default settings. The attribute *logLevel* on the ``<file>`` child element
lets you change the severity level for log messages to *debug*, *info*,
*warn*, or *error* (default: *info*). Additionally, you can provide an
alternative absolute path and filename where to store the log messages.

.. note::
   A web application typically has limited access to the file
   system for security reasons. Thus, make sure that the log file is
   accessible for the WFS web application. Check the documentation of your
   servlet container for details.

If you want log messages to be additionally printed to the console, then
simply include the ``<console>`` child element as well. The ``<console>``
element also provides a *logLevel* attribute to define the severity
level.

.. code-block:: xml
   :name: wfs_logging_settings_config_listing

   <logging>
     <console logLevel="info"/>
     <file logLevel="info">
       <fileName>path/to/your/wfs.log</fileName>
     </file>
   </logging>

.. caution::
   Log messages are continuously written to the same log file. The
   WFS application does not include any mechanism to truncate or rotate the
   log file in case the file size grows over a certain limit. So make sure
   you configure log rotation on your server.