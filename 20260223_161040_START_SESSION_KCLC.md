KCLC-IMPL-20260223-007-CLDE-zoom-provisioning

Discovery complete. 9/14 rooms confirmed in Zoom. 5 questions answered by Warren. Ready to provision.

Checkpoint: [CHECKPOINT_20260223_1610_zoom-discovery-complete.md](CHECKPOINT_20260223_1610_zoom-discovery-complete.md)
Model: sonnet — API calls + provisioning steps, well-defined scope

Priorities:
1. Create 3 missing common area phones: Bus Station, The Market, The Paint Shop
2. Register all 14 MACs to their Zoom common area phone accounts
3. Assign DID +13522680205 to KCLC auto-receptionist or appropriate destination
4. Test ZTP: reboot one phone, confirm it auto-configures via Zoom

Critical facts:
- Cutover Feb 28, 2026 — HARD DEADLINE
- Single account confirmed — TFH/KCLC ext-to-ext calling WORKS
- 9 rooms already in Zoom (ext 705–715): Pet Shop, Sweet Shop, Ice Cream Shop, Sports Store, Toy Shop, Music Store, Flower Shop, Club house, Media Center
- 5 questions from discovery MUST be answered by Warren before provisioning (see checkpoint)
- DID +13522680205 is PENDING/unassigned — needs routing decision
- Common area phone MACs: ALL 14 still unregistered in Zoom
- ZTP URL: https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}
