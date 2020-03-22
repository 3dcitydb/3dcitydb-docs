System requirements
-------------------

The 3D City Database WFS is implemented as *Java web application* based
on the *Java Servlet* technology. It therefore must be run in a Java
servlet container on a web server. The following minimum *software
requirements* have to be met:

-  **Java servlet container** supporting the **Java Servlet 3.1 / 3.0**
   (or higher) specification

-  **Java 8** Runtime Environment (Java 7 or earlier versions are not
   supported)

The WFS implementation has been successfully deployed and tested on
**Apache Tomcat 9** (http://tomcat.apache.org/). This is also the
*recommended* servlet container. *Apache Tomcat 8 and 7* are also
supported, whereas any previous version of the Apache Tomcat server will
not work.

.. note::
   Neither Java nor a servlet container are part of the WFS
   distribution package and therefore must be properly installed and
   configured before deploying the WFS. Please refer to the documentation
   of your favorite servlet container for more information.

*Hardware requirements* for the web server running the WFS depend on the
intended use and number of concurrent accesses. There are no minimum
requirements to be met, so make sure your system setup meets your needs.
Also note that the WFS **does not provide its own security layer**
(e.g., to limit access to specific networks or users). So, it is your
responsibility to take any reasonable physical, technical and
administrative measures to secure the WFS service and the access to the
3D City Database.

*WFS clients* connecting to the WFS interface of the 3D City Database
must support the *OGC WFS standard version 2.0*. Moreover, they must
be capable of consuming 3D data encoded in CityGML or CityJSON, which is
delivered by the WFS server.
