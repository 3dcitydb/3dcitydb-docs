3DCityDB @ TU München
=====================

The Chair of Geoinformatics [23]_ at Technische Universität München
(TUM) took over the further development of the 3D City Database from TU
Berlin (TUB) when Prof. Kolbe moved from TUB to TUM in 2012. 3DCityDB is
being used at TUM in teaching courses on spatial databases and 3D city
modeling, in student projects and master theses, and in many past and
ongoing research projects.

.. _webclient:

Interactive Cloud-based 3D Webclient
------------------------------------

Besides the Open Source 3DCityDB-Web-Map-Client as described in chapter
8 the Chair of Geoinformatics has also developed a “Professional
Version” of the interactive 3D web client. This version links 3D
visualization models exported in KML/glTF from 3DCityDB with table data
exported using the 3DCityDB Spreadsheet Generator and allows viewing,
editing, and querying objects and their thematic data [Herreruela et al.
2012; Yao et al. 2014; Chaturvedi et al. 2015]. The configuration of a
3D webclient project (information about each layer, thematic data,
preferences, spatial bookmarks) is also stored in the Cloud as a Google
Spreadsheet. The following image shows a screenshot of a tool created by
TUM for the Energy Atlas Berlin that is based on the “3D Webclient
Professional”. It estimates building energy demands based on the German
standard DIN 18599 and the 3D building models in CityGML and allows to
interactively explore retrofitting potentials for single or sets of
buildings [Kaden & Kolbe 2014]. Thematic data are stored in Google
Spreadsheets, where spreadsheet formulas are employed to implement
ad-hoc computation of energy values and their changes according to
retrofit measures. Also the costs of the retrofitting measures are
estimated for each building individually.

|image225|


.. _research:

Research Projects in which 3DCityDB is being used
-------------------------------------------------

Semantic 3D city modeling, city system modeling, and indoor navigation
are major research fields of the Chair of Geoinformatics at TUM. We have
been driving the international development of CityGML and IndoorGML
within the OGC. We are partners in and/or coordinators of projects on
Smart Cities, Sustainable Urban Development, and Strategic Energy
Planning funded by the `Climate-KIC <http://www.climate-kic.org/>`__ of
the European Institute of Innovation & Technology (EIT). Projects using
3DCityDB are: `Energy Atlas
Berlin <http://www.gis.bgu.tum.de/en/projects/energieatlas-berlin/>`__\  [24]_,
Neighborhood Demonstrators, `Smart Sustainable
Districts <https://www.gis.bgu.tum.de/en/projects/smart-sustainable-districts-ssd/>`__\  [25]_,
`Modeling City
Systems <https://www.gis.bgu.tum.de/en/projects/modeling-city-systems-mcs/>`__\  [26]_,
and `Smart District Data
Infrastructure <https://www.gis.bgu.tum.de/en/projects/smart-district-data-infrastructure/>`__\  [27]_.
3DCityDB has also been used in the `OGC Future Cities
Pilot <https://www.gis.bgu.tum.de/en/projects/future-cities-pilot-phase-1/>`__\  [28]_,
and ‘\ `3D
Tracks <https://www.gis.bgu.tum.de/en/projects/3dtracks/>`__\  [29]_ -
Collaborative Subway Track Planning in Multi-Scale 3D City and Building
Models’ [Borrmann et al. 2015] funded by the German Science Foundation
(DFG) and was used in projects on deriving 3D DLM from 2D DLM and
DTM/DSM [Fiutak et al. 2018].


.. _development:

Current and future work on 3DCityDB
-----------------------------------

The team at the Chair of Geoinformatics is currently working on the
following tools and extensions to 3DCityDB. Most of them will be made
available as Open Source software within the 3DCityDB repository as soon
as they are finished and tested:

**Support of the Dynamizer ADE:** Dynamizers extend CityGML to support
the representation and exchange of time-varying attribute values for all
CityGML feature properties using timeseries. Support in 3DCityDB is
facilitated by 1) provision of the Java library for importing and
exporting CityGML Dynamizer ADE contents, and 2) provision of a new web
service, the so-called *InterSensor Service*, which will give access to
the timeseries data stored in the 3DCityDB according to the OGC Sensor
Web Enablement standards.

**Update Manager:** This tool will provide a check-out / check-in
functionality for parts of stored 3D city models for the purpose of
editing and updating. It will automatically detect changes made on the
previously exported (checked-out) CityGML dataset and create WFS as well
as direct database transactions that will update the 3DCityDB contents
according to the identified changes (check-in).

**Solar potential analysis:** This tool computes the solar energy of
direct and diffuse irradiation on building walls and roofs. The
computation considers shadow casting by buildings, vegetation, a Digital
Surface Model and the Digital Terrain Model. The monthly energy and
irradiation values as well as the sky view factors are attached as
generic attributes to wall and roof surface objects and in aggregated
form to buildings. The software is implemented in Java and directly
connects to the 3DCityDB. It has been employed to estimate the solar
potentials in the official Energy Atlas of the city of Helsinki,
Finland.

.. |image225| image:: media/image235.jpg
   :width: 6.3in
   :height: 3.48194in

