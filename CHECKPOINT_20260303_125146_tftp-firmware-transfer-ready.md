# Checkpoint: TFTP Firmware Transfer Ready

**Created:** 2026-03-03 12:51:46 EST
**Session:** KCLC-FEAT-20260303-008-SNNT-tftp-firmware-staging
**Model:** Claude Haiku 4.5
**Status:** READY FOR EXECUTION — MacBook Air hostname confirmed, firmware staging next

---

## Summary

Session focused on locating TFTP firmware files and MacBook Air access details. Located 5.9.8 firmware checkpoint with complete device inventory and upgrade procedure. MacBook Air hostname confirmed as "MacBookAir" on local network. Ready to proceed with firmware file verification and SCP transfer to Windows TFTP server.

---

## Inherited Context

### Critical System Facts

- **TFTP Server (Windows):** 10.1.10.175 (SolarWinds TFTP, tftpd64 running via Chocolatey)
- **TFTP root (Windows):** `C:\tftp\root\`
- **MacBook Air hostname:** `MacBookAir` (on local network, wired connection preferred)
- **MacBook Air TFTP directory:** `tftp-root/` (not `/private/tftpboot/`)
- **Firmware files (8 total, 40-46MB each):**
  - `3111-46157-002-573.sip.ld` (VVX 400, Phase 1)
  - `3111-46157-002-598.sip.ld` (VVX 400, Phase 2)
  - `3111-48400-001-573.sip.ld` (VVX 401, Phase 1)
  - `3111-48400-001-598.sip.ld` (VVX 401, Phase 2)
  - `3111-46162-001-573.sip.ld` (VVX 410, Phase 1)
  - `3111-46162-001-598.sip.ld` (VVX 410, Phase 2)
  - `sip.ver` (5.9.8.5760 permanently)
  - All `.cfg` files (per-device configs, 13 phones)

### Phones Pending Upgrade (13 total)

| Ext | Room | Model | Current | Action |
|-----|------|-------|---------|--------|
| 300–301, 303, 305–309, 312–314 | (11 phones) | VVX 400/410 | 5.5.2 | Phase 1→2→5.9.8 |
| 310 | Fire Station | VVX 400 | 5.8.0 | Phase 2 direct (skip Phase 1) |
| 302 | Pet Shop | VVX 400 | 5.0.1 | ❌ BLOCKED — do not touch |
| 304 | KCLC Admin | VVX 400 | 5.9.8 | ✅ Complete — skip |

### Key Constraints

- **sipXcom config in flash:** All phones may need factory reset before TFTP works
- **WiFi TFTP fails:** Use wired connection (10.1.36.71 on en6 is old reference—MacBook may be on different IP now)
- **Fire Station exception:** Already on 5.8.0, skip Phase 1, go straight to 5.9.8
- **Pet Shop blocked:** On 5.0.1.4068/BootROM 5.2.1 (pre-Updater), upgrade path unknown

### Previous Session Results (2026-03-02)

✅ **TFTP infrastructure rebuilt** with 8 version-tagged firmware files
✅ **Per-device configs created** for all 13 phones (edit `.cfg` to advance Phase 1→2)
✅ **Full upgrade path validated** on VVX 400: 5.5.2 → 5.7.3 → 5.9.8
✅ **SolarWinds TFTP at 10.1.10.175** identified as permanent TFTP server (setup pending)

---

## Next Steps — IMMEDIATE

1. **SSH to MacBook Air:**
   ```bash
   ssh wwiemer@MacBookAir
   ```

2. **Verify firmware files exist:**
   ```bash
   ls -lh ~/tftp-root/
   # Should show: 3111-46157-002-{573,598}.sip.ld, 3111-48400-001-{573,598}.sip.ld,
   #             3111-46162-001-{573,598}.sip.ld, sip.ver, all .cfg files
   ```

3. **SCP all files to Windows TFTP:**
   ```bash
   scp ~/tftp-root/* wwiemer@10.1.10.175:C:/tftp/root/
   ```

4. **Verify transfer on Windows:**
   ```powershell
   dir C:\tftp\root\
   # Confirm all 8 firmware files + sip.ver + .cfg files present
   ```

5. **Test TFTP from phone:**
   - Boot a phone with TFTP boot server set to 10.1.10.175:69
   - Verify sip.ver downloads successfully

6. **Zoom pre-provisioning** (after TFTP verified)
   - Configure all 13 extensions in Zoom dashboard
   - Set DIDs and ZTP URLs

---

## Git Status

No changes to commit (SOP execution silent).

---

**Checkpoint Version:** 3.0 (Inherited Context format)
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Haiku 4.5)
**Created:** 2026-03-03 12:51:46 EST
