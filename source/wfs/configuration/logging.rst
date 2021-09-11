.. _wfs_logging_settings_chapter:

Logging settings
~~~~~~~~~~~~~~~~

The WFS service logs messages and errors that occur during operations to
a dedicated log file by default. Entries in the log file are associated with a
timestamp, the severity of the event and the IP address of the client
(if available). The log is normally stored in the file
``WEB-INF/wfs.log`` within the *application folder* of the WFS web
application.

The ``<logging>`` element in the ``config.xml`` file is used to adapt these
default settings. The attribute *logLevel* on the ``<file>`` child element
lets you change the severity level for log messages that shall be recorded in the log file
to *debug*, *info*, *warn*, or *error* (default: *info*). Additionally, the ``<fileName>``
element lets you define an alternative absolute path and filename where to store the log file.

.. note::
   A web application typically has limited access to the file
   system for security reasons. Thus, make sure that the log file is
   accessible for the WFS web application. Check the documentation of your
   servlet container for details.

If you want log messages to be printed to the console via ``STDOUT`` and ``STDERR``,
then simply set the ``<console>`` child element. The ``<console>`` element also provides
a *logLevel* attribute to define the severity level. You can pick different log levels
for the console and the log file. Printing log messages to the console is useful,
for example, when the 3DCityDB WFS is running in a Docker container (see :numref:`wfs_docker_chapter`)
or a debug environment.

Logging for the 3DCityDB WFS can be configured to either use a log file, or the console,
or both.

.. code-block:: xml

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