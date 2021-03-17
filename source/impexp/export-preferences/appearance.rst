.. _impexp_export_preferences_appearance_chapter:

Appearance
^^^^^^^^^^

The appearance export preferences control how appearance information of
city objects is written to the output datasets.

.. figure:: /media/impexp_export_preferences_appearance_fig.png
   :name: impexp_export_preferences_appearance_fig
   :align: center

   Export preferences – Appearance.

By default, both appearance information and texture image files
associated with the city objects in the 3D City Database are exported
[1]. Alternatively, the user can choose to only export the appearance
information without textures or to skip appearances completely.

When exporting texture files, the additional options *Overwrite existing
texture files* and *Generate unique texture filenames* influence the way
in which texture files are written to the file system [1].

1) *Overwrite existing texture files*:
   Texture files are stored in a separate folder of the file system.
   Before exporting a texture image file into this folder, the
   Importer/Exporter can check whether a file of the same filename
   already exists in this folder. In this case, the existing file will
   be kept if this option is *not enabled*. Otherwise, and by default,
   there is no check and a texture file of the same name will be
   overwritten.

2) *Generate unique texture filenames*:
   Often filenames for texture images are automatically created from a
   naming scheme involving some counter (e.g., a prefix “\ *tex*\ ”
   followed by a number incremented by 1 for each new image). It thus
   can happen that two city objects within the same or different
   instance documents are assigned a texture image file of the same
   name but with different content. In the 3D City Database, texture
   images are stored in separate records and thus duplicate filenames
   are not an issue. When exporting the data, however, two texture
   files of the same name might be written to the same target folder,
   in which case one is replaced with the other. This will obviously
   lead to false visualizations and issues in workflows consuming the
   exported data. For this reason, checking this option
   will force the export process to generate unique and stable
   filenames for each texture file.

The location where to store the texture files can be defined by the user
[2]. The default option is to pick a folder below the export directory
and thus relative to the output file. The default folder name is
*"appearance"*. Instead of a local path, also an absolute path can
be provided. In this case, the same folder will be used in subsequent
exports from the 3D City Database.

.. note::
  When the data is exported into a ZIP archive, texture files are
  also stored inside the archive if a local path is chosen.

When appearances are chosen to be exported but the *Do not store texture
files* option [1] is checked, then appearance information is generated
for the city objects in the output file, but the texture files are
not stored in the file system. However, since the texture path is part
of the appearance information, the directory settings [2] and whether to
generate unique texture filenames [1] still has an impact on the
generated appearance information. The *Do not store texture files*
option is useful, for example, if the texture files have already been
exported to an absolute directory in a previous run of the export
operation.

Especially when running the Importer/Exporter on a Windows machine,
placing a large number of files into the
same folder might lead to severe I/O lags. This might
negatively affect the performance for large exports. For this reason,
the Importer/Exporter can automatically distribute the texture files
over additional subfolders that are automatically created. Simply check
the option *Automatically place texture files in additional subfolders*
and provide the number of subfolders to be used.