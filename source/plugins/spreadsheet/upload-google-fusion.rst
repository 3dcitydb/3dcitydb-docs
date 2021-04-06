Upload XLSX files to Google Fusion Table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here is a step-by-step guide for uploading a XLSX file to the Google
Fusion Tables which a cloud-based web application that allows for
storing, showing, and sharing large data tables.

Open a web browser (you can use, for example, Google Chrome or Mozilla
Firefox\ **, but we recommend not to use Microsoft Internet Explorer**)
and type the following address into the address bar.

https://www.google.com/fusiontables/data?dsrcid=implicit

When you go to this page, you will be asked to log in by using your
Google account.

Enter your Email address and the password of your Google account into
the corresponding input fields

After logging in, an **Import new table** dialog window will be
displayed like in the screenshot below:

.. figure:: /media/impexp_plugin_spshg_fusiontable_choose_file.png
   :name: pic_plugin_spreadsheet_csv_choose_file
   :align: center

Click the **Choose File** button to open a file selection window

Navigate to the system path of your created Excel file and select it.
The following screenshot show an example Excel file.

.. figure:: /media/impexp_plugin_spshg_csv_table.png
   :name: pic_plugin_spreadsheet_csv_table
   :align: center

After selecting the Excel file, click the **Next** button to continue

The contents of the selected table is displayed in the dialog window
(see the screenshot below)

.. figure:: /media/impexp_plugin_spshg_csv_select.png
   :name: pic_plugin_spreadsheet_csv_select
   :align: center

Briefly check the table contents again and then click the **Next**
button

In the following dialog window (see the screenshot below), enter a table
name (for example “\ *Berlin_Buildings_Attributes*\ ”) into the input
field **Table name** and click the **Finish** button

.. figure:: /media/impexp_plugin_spshg_csv_select_fields.png
   :name: pic_plugin_spreadsheet_csv_select_fields
   :align: center

Now, your Excel file has been successfully uploaded to the Google Cloud
Service and a Google Fusion Table instance has been created (see the
screenshot below).

.. figure:: /media/impexp_plugin_spshg_csv_output.png
   :name: pic_plugin_spreadsheet_csv_output
   :align: center

We would like to share our created online spreadsheet with other people.
Here we need to change the sharing settings of the Google Fusion Table
by completing the following steps:

Choose the **File Share…** from the menu bar at the top of the online
spreadsheet window

.. figure:: /media/impexp_plugin_spshg_csv_share.png
   :name: pic_plugin_spreadsheet_csv_share
   :align: center

In the **Sharing settings** window, click on **Change…** button (see the
screenshot below)

.. figure:: /media/impexp_plugin_spshg_csv_share_link.png
   :name: pic_plugin_spreadsheet_csv_share_link
   :align: center

In the **Link sharing** window (see the figure below), choose the second
radio button **On – Anyone with the link**

.. figure:: /media/impexp_plugin_spshg_csv_share_options.png
   :name: pic_plugin_spreadsheet_csv_share_options
   :align: center

Click the **Save** button to save the settings and close the share
settings window

Now, the spreadsheet is being shared and can be accessed by anybody who
has its URL that can be easily obtained from the address bar of the web
browser (marked in the screenshot below). With this URL and the first
column (GMLID) in the table, the attribute information stored in the
spreadsheet are able to be queried and displayed on the
3DCityDB-Web-Map-Client when a city object is clicked on
(see :numref:`webmap_chapter` for more details).

.. figure:: /media/impexp_plugin_spshg_csv_share_results.png
   :name: pic_plugin_spreadsheet_csv_share_results
   :align: center