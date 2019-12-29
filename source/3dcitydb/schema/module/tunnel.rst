Tunnel Model
^^^^^^^^^^^^

|image53|

Figure 47: Tunnel database schema

The tunnel model, described in paragraph 2.2.4.9 at the conceptual
level, is realised by the tables shown in Figure 47. The relational
schema is identical to the building and bridge schema for the most parts
except for the naming. Please, refer to the explanation of the building
schema on the previous pages for a complete understanding. The main
differences to the building schema are the following:

-  Tunnels cannot be modelled in LoD 0. Therefore, no corresponding
      columns appear in the TUNNEL table.

-  The CityGML feature *HollowSpace* can be seen analogue to the feature
      *Room* of a building or a bridge

-  CityGML features of tunnels, such as boundary surfaces,
      installations, openings, hollow spaces and furniture, are mapped
      to separate specific tables and are not stored in already existent
      ones (e.g. THEMATIC_SURFACE, OPENING). The reason for this is to
      provide a schema that is as close to the UML model as possible.
      There are slight differences between the building and the tunnel
      model that would lead to ambiguous references e.g. a boundary
      surface of the building namespace cannot reference to a tunnel
      feature.

-  OBJECTCLASS_ID of table TUNNEL_THEMATIC_SURFACE allows the values:

   -  89 (*TunnelCeilingSurface*),

   -  90 (*InteriorTunnelWallSurface*)

   -  91 (*TunnelFloorSurface*),

   -  92 (*TunnelRoofSurface*),

   -  93 (*TunnelWallSurface*),

   -  94 (*TunnelGroundSurface*),

   -  95 (*TunnelClosureSurface*),

   -  96 (*OuterTunnelCeilingSurface*),

   -  97 (*OuterTunnelFloorSurface*).

-  In the TUNNEL_INSTALLATION table external tunnel installations can be
      identified by the OBJECTCLASS_ID 86 and internal ones by 87.

-  The OBJECTCLASS_ID column in table BRIDGE_OPENING can be of integer
   value 100 (*BridgeDoor*) or 99 (*BridgeWindow*). They are associated
   to entries in the table TUNNEL_THEMATIC_SURFACE via the
   TUNNEL_OPEN_TO_THEM_SRF link table.

-  If a CityGML ADE is used that extends any of the named classes above,
   further values for OBJECTCLASS_ID may be added by the ADE manager.
   Their concrete numbers depend on the ADE registration (cf. section
   6.3.3.1).

-  In contrast to the building model tunnels and tunnel openings do not
   have addresses.

.. |image53| image:: ../../media/image64.png
   :width: 10.31169in
   :height: 9.13115in
