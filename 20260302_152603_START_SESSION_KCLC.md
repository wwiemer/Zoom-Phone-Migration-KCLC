KCLC-FEAT-20260302-007-SNNT-batch-firmware-upgrade-evening

Full upgrade path confirmed (5.5.2→5.7.3→5.9.8) on first VVX 400. TFTP infrastructure complete with version-tagged files for all models. 13 phones pending. SolarWinds TFTP at 10.1.10.175 identified as permanent server option for evening batch run.

Checkpoint: [CHECKPOINT_20260302_1522_5.9.8-confirmed-tftp-complete-sipxcom-flash-pattern.md](projects/Zoom-Phone-Migration-KCLC/CHECKPOINT_20260302_1522_5.9.8-confirmed-tftp-complete-sipxcom-flash-pattern.md)
Model: sonnet — hands-on phone provisioning, multi-device batch work

Priorities:
1. Set up SolarWinds TFTP at 10.1.10.175 — copy all firmware + per-device configs from /private/tftpboot/. Verify it serves correctly. Use as permanent TFTP for batch.
2. Factory reset + upgrade all accessible phones. Most need factory reset first (sipXcom in flash — look for WTEK background in logs). Fire Station (310) first — already prepped, straight to 5.9.8.
3. Pet Shop (302) on 5.0.1 — research upgrade path before touching. BootROM 5.2.1, pre-Updater era.

Critical facts:
- ALL phones need factory reset before TFTP if they see WTEK background in boot log or register to sipXcom
- Advance Phase 1→2: edit /private/tftpboot/[mac].cfg, change -573 to -598. No file copying.
- sip.ver = 5.9.8.5760 permanently — never change
- TFTP must use wired IP 10.1.36.71 (or 10.1.10.175 if SolarWinds ready) — WiFi TFTP fails cross-subnet
- VVX 410 part # = 3111-46162-001 (all three confirmed). Fire Station already on 5.8.0 — skip Phase 1.
