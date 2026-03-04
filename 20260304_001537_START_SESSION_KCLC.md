KCLC-FEAT-20260304-013-SNNT-ztp-provisioning-and-zoom-registration

Phase 2 (5.9.8) upgrade unblocked and confirmed on 2 phones; remaining 8 initiated. Root cause was TFTP serving from C:\tftp\ not C:\tftp\root\ — batch fix applied. All 10 phones now have correct -598 cfg files.

Checkpoint: [CHECKPOINT_20260304_0015_phase2-5.9.8-upgrade-complete-ztp-next.md](projects/Zoom-Phone-Migration-KCLC/CHECKPOINT_20260304_0015_phase2-5.9.8-upgrade-complete-ztp-next.md)
Model: sonnet — multi-step workflow with known patterns

Priorities:
1. Verify all 8 remaining phones show UC Software = 5.9.8.5760, Updater = 5.9.8.4127 via web UI
2. ZTP test on 10.1.12.189 (ext 314): Settings → Provisioning Server → HTTPS → provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400 → Save → Reboot → confirm registers as ext 314
3. If ZTP confirmed → batch remaining 9 phones to Zoom provisioning URL

Critical facts:
- TFTP root is C:\tftp\ (NOT C:\tftp\root\) — always edit files there
- All 10 phones: admin password = 2307
- ZTP URL: provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400
- Bus Station (10.1.12.182), Fire Station (10.1.12.64), Pet Shop (10.1.12.68) = on-site only, do not attempt remotely
