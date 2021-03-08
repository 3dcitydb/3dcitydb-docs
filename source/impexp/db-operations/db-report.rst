.. _impexp-db-report:

Database report
^^^^^^^^^^^^^^^

A database report is a list of all tables of the 3D City Database
together with their total number of rows. This operation therefore
provides a quick overview of the contents of the 3D City Database.
The report is printed to the console window.

.. figure:: /media/impexp_gui_database_report_fig.png
   :name: impexp_gui_database_report_fig
   :align: center

   Generating a database report.

If the database is version-enabled (Oracle only), the database report
can be created for the contents of a specific *workspace* [1] at a given
*timestamp* [2]. If no workspace is specified, the default *LIVE* workspace is
chosen per default. If the workspace does not exist, a
corresponding error message is provided.

.. note::
  Workspaces are not a feature of
  the 3D City Database but are managed through the *Oracle Workspace
  Manager* tool. Please refer to the Oracle database documentation for
  details. Since PostgreSQL currently does not support workspaces, the
  corresponding input fields are disabled when connecting to a 3D City
  Database running on PostgreSQL.
