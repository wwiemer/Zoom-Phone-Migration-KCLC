KCLC-IMPL-20260301-003-CLDE-onsite-provisioning-and-full-buildout

Ext 314 (Media/Kitchen) fully configured in Zoom — MAC registered, provisioning URL saved on phone. Warren driving on-site to physically factory reset VVX401 phones. All remaining Zoom changes (12 renumbers, call queue, KCLC site, Twilio forward) still pending.

Checkpoint: [CHECKPOINT_20260301_2044_ext314-configured-onsite-provisioning-pending.md](projects/Zoom-Phone-Migration-KCLC/CHECKPOINT_20260301_2044_ext314-configured-onsite-provisioning-pending.md)
Model: sonnet — well-defined execution steps, API calls, portal work

Priorities:
1. On-site: factory reset VVX401 at 10.1.12.189 → verify ext 314 comes online → test call to +1 (352) 782-0626
2. If test passes: execute all 12 remaining Zoom renumbers via API (IDs in checkpoint), register MACs via portal
3. Create KCLC Zoom Site (2307 South St E911), create call queue (300-304 simultaneous), then set Twilio forward tonight

Critical facts:
- 311 is reserved N11 — final mapping: 200-210→300-310, 211→314, 212→312, 213→313
- MAC registration requires admin.zoom.us portal (phone:write:device:admin scope unavailable via S2S OAuth)
- All 14 phones are Poly vvx401 (End of Life in Zoom — still functional)
- KCLC E911 = 2307 South St (NOT 2301 — that is TFH church); site + E911 address not yet created in Zoom
- Port: +13522680205 ports tomorrow 2026-03-02 at 11:30 AM ET — Twilio forward must be set TONIGHT after phones confirmed working
- Zoom common area IDs for all 13 remaining extensions are in the checkpoint
