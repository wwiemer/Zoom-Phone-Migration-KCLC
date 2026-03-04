KCLC-FEAT-20260303-012-SNNT-phase2-5.9.8-upgrade-and-ztp

**Previous Session:** KCLC-FEAT-20260303-011-SNNT-tftp-firmware-transfer-verify
**Checkpoint:** CHECKPOINT_20260303_223656_phase1-complete-phase2-in-progress.md
**Model Recommendation:** Sonnet 4.6
**Location:** Windows TFTP server at 10.1.10.175 (KCLC) + phones at 10.1.12.x

---

## Status

✅ **Stage 1 COMPLETE:** 10 phones at 5.7.3 + password changed to 2307
⏳ **Stage 2 IN PROGRESS:** Phase 2 cfg deployed (-598), but phones reading cached cfg
🔧 **Active Fix:** Provisioning server swap on 10.1.12.189 to force cache clear
⏳ **Stage 3 NOT STARTED:** Zoom ZTP provisioning

---

## Critical Facts (Inherited)

- **TFTP server:** Windows 10.1.10.175 — `tftpd64.exe -i C:\tftp\root -l C:\tftp\tftpd64.log`
- **All 10 phones:** admin password = 2307
- **Phase 2 cfg:** All 10 files updated to -598.sip.ld (UTF-8 no-BOM verified)
- **Cache bust fix:** Change provisioning server IP → Save → Reboot → change back → Save → Reboot
- **First ZTP phone:** 10.1.12.189 (ext 314) — already built out in Zoom
- **Zoom ZTP URL:** `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`

---

## Immediate Actions

1. **Complete cache-bust on 10.1.12.189** (if not done):
   - Web UI → Settings → Provisioning Server → change to `10.1.10.176` → Save → Reboot
   - After boot: change back to `10.1.10.175` → Save → Reboot
   - Watch for "Updating Software" on screen → 5.9.8 downloading

2. **Verify 5.9.8:** UC Software = 5.9.8.5760, Updater = 5.9.8.4127

3. **Repeat for remaining 9 phones** (same provisioning swap)

4. **ZTP test on 10.1.12.189:**
   - Settings → Provisioning Server → HTTPS → `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
   - Confirm registers as ext 314

5. **If ZTP works → batch all 10 phones to Zoom**

---

## On-Site List (do not attempt remotely)
- Bus Station (10.1.12.182) — 5.5.2, Edge E220 temp, needs full Phase 1→2→ZTP on-site
- Fire Station (10.1.12.64) — unknown password, TFTP cfg set for 5.9.8 direct
- Pet Shop (10.1.12.68) — 5.0.1 blocked, needs intermediate firmware research

---

**See checkpoint for full details.**
