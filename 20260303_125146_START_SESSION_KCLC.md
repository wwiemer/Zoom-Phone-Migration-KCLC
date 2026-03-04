# KCLC-FEAT-20260303-009-SNNT-tftp-firmware-transfer

**Previous Session:** KCLC-FEAT-20260303-008-SNNT-tftp-firmware-staging
**Checkpoint:** [CHECKPOINT_20260303_125146_tftp-firmware-transfer-ready.md](CHECKPOINT_20260303_125146_tftp-firmware-transfer-ready.md)
**Model Recommendation:** Haiku (file transfer + verification, straightforward)

---

## Status

✅ **MacBook Air hostname:** `MacBookAir` (confirmed)
✅ **TFTP directory:** `tftp-root/` (8 firmware files + sip.ver + .cfg files)
⏳ **Next action:** SSH to MacBook Air, verify files, SCP to Windows 10.1.10.175:C:\tftp\root\

---

## Critical Facts

- **Windows TFTP server:** 10.1.10.175 (SolarWinds tftpd64, running)
- **Windows TFTP root:** `C:\tftp\root\`
- **MacBook Air:** `MacBookAir` (wired preferred, SSH ready)
- **Firmware:** 8 × 40-46MB .ld files, `sip.ver` (5.9.8.5760), 13 .cfg files
- **13 phones pending:** Phase 1→2→5.9.8 (Fire Station 310 direct Phase 2)
- **2 phones skipped:** Ext 302 blocked (5.0.1), Ext 304 done (5.9.8)

---

## Immediate Actions

1. SSH to MacBook Air and verify tftp-root directory
2. SCP all firmware files to Windows 10.1.10.175
3. Verify transfer on Windows
4. Test TFTP from a phone
5. Configure Zoom pre-provisioning

---

**Files to copy:** 8 × .ld + sip.ver + all .cfg
**Source:** ~/tftp-root/ (MacBook Air)
**Destination:** C:\tftp\root\ (Windows 10.1.10.175)
**Test phone:** 10.1.12.189 (request sip.ver to confirm TFTP works)
