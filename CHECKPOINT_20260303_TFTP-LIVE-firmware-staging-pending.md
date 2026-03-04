# Checkpoint: TFTP Server Live — Firmware Staging Pending

**Created:** 2026-03-03 (Session KCLC-FEAT-20260302-007-SNNT-batch-firmware-upgrade-evening)
**Session:** KCLC-FEAT-20260302-007-SNNT-batch-firmware-upgrade-evening
**Model:** Claude Haiku 4.5
**Status:** IN PROGRESS — TFTP infrastructure complete, firmware files staging next

---

## Summary

TFTP server deployed to 10.1.10.175 (Windows 11 Pro) and verified listening on UDP 69. tftpd64 (64-bit) running via Chocolatey installation. TFTP root `C:\tftp\root\` created and empty, firewall opened, config file in place. **Ready to receive firmware files from MacBook Air.** MacBook online with SSH ready for file transfer. Batch upgrade (Phase 1→2→5.9.8) and Zoom ZTP provisioning can proceed tonight on-site after firmware files staged.

---

## Inherited Context

### TFTP Infrastructure — COMPLETE

| Item | Status | Details |
|------|--------|---------|
| **tftpd64 Installation** | ✅ Complete | Via Chocolatey v2.2.2, 64-bit binary at `C:\ProgramData\superopsintegrations\chocolatey\lib\tftpd32\tools\tftpd64.exe` |
| **TFTP Root Directory** | ✅ Created | `C:\tftp\root\` — empty, ready for firmware files |
| **Config File** | ✅ Created | `C:\tftp\tftpd64.ini` — ListenIP=0.0.0.0, Port=69, configured for `C:\tftp\root\` |
| **Windows Firewall** | ✅ Opened | UDP 69 inbound rule added for "TFTP Server (UDP 69)" |
| **Service Status** | ✅ Running | tftpd64.exe PID 16168 — verified listening on `10.1.10.175:69` via `nc -zu` |
| **Auto-restart** | ✅ Configured | Task Scheduler startup task created (minor script error on -RunWithHighestPrivileges parameter, but process started successfully) |

### System Facts (Inherited from Previous Checkpoints)

- **sipXcom:** ucs05.warrentek.net | CentOS 6.10 | sipXcom 19.04
- **KCLC public IP:** 72.166.63.114 (fail2ban whitelisted)
- **MacBook wired IP (on-site):** 10.1.36.71 on en6
- **Tailscale VPN:** Active — enables remote access to 10.1.12.0/24 VLAN from off-site
- **KCLC Zoom Phone — Live:** Ext 300 (Bus Station) on Poly Edge E220 / +13522680205

### Device Status (14 phones)

| Ext | Room | Model | MAC | LAN IP | Firmware | Action Tonight |
|-----|------|-------|-----|--------|----------|-----------------|
| 300 | Bus Station | VVX 400 | 0004F27BAA59 | 10.1.12.182 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 301 | Toy Shop | VVX 400 | 0004F2DD7E86 | 10.1.12.185 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 302 | Pet Shop | VVX 410 | 0004F2887947 | 10.1.12.68 | 5.0.1 ⚠️ | DO NOT TOUCH (pre-Updater era, blocked) |
| 303 | Sweet Shop | VVX 400 | 0004F2DE4E79 | 10.1.12.180 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 304 | KCLC Admin | VVX 400 | 0004F2DE46B8 | 10.1.10.176 | **5.9.8.5760 ✅** | SKIP (already done) |
| 305 | Sport Store | VVX 400 | 0004F2DDFE05 | 10.1.12.183 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 306 | Ice Cream Shop | VVX 410 | 0004F27A7B32 | 10.1.12.65 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 307 | Music Store | VVX 400 | 0004F2822926 | 10.1.12.186 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 308 | Flower Shop | VVX 400 | 0004F2C6A847 | 10.1.12.190 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 309 | Police Station | VVX 400 | 0004F268033E | 10.1.12.191 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 310 | Fire Station | VVX 410 | 64167F909AF0 | 10.1.12.64 | 5.8.0 🔄 | Phase 2 direct (skip Phase 1) |
| 312 | The Clubhouse | VVX 401 | 64167F33F295 | 10.1.12.187 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 313 | Rec Room | VVX 400 | 0004F2DE4B0E | 10.1.12.181 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |
| 314 | Media/Kitchen | VVX 401 | 64167F31A96D | 10.1.12.189 | 5.5.2 ⏳ | Phase 1→2→5.9.8 |

**Upgrade Summary:**
- ✅ 1 complete (ext 304)
- 🔄 1 Phase 2 direct (ext 310, already on 5.8.0)
- ⏳ 11 standard two-step (Phase 1→2)
- ⚠️ 1 blocked/investigate (ext 302)

### Firmware Files Needed in C:\tftp\root\

| File | Size | Location | Status |
|------|------|----------|--------|
| `3111-46157-002-573.sip.ld` | 42MB | /private/tftpboot/ on MacBook | ⏳ Pending copy |
| `3111-46157-002-598.sip.ld` | 41MB | /private/tftpboot/ on MacBook | ⏳ Pending copy |
| `3111-48400-001-573.sip.ld` | 43MB | /private/tftpboot/ on MacBook | ⏳ Pending copy |
| `3111-48400-001-598.sip.ld` | 46MB | /private/tftpboot/ on MacBook | ⏳ Pending copy |
| `3111-46162-001-573.sip.ld` | 42MB | /private/tftpboot/ on MacBook | ⏳ Pending copy |
| `3111-46162-001-598.sip.ld` | 41MB | /private/tftpboot/ on MacBook | ⏳ Pending copy |
| `sip.ver` | 10B | /private/tftpboot/ on MacBook | ⏳ Pending copy |
| `[mac].cfg` × 13 | <1MB | /private/tftpboot/ on MacBook | ⏳ Pending copy |

---

## Next Steps — IMMEDIATE (Next Session)

**Priority 1 (Remote, today):**
1. SSH into MacBook Air — verify `/private/tftpboot/` contents
2. SCP or rsync all 8 firmware files + `sip.ver` + 13 `.cfg` files to `10.1.10.175:C:\tftp\root\`
3. Verify files arrived on Windows machine
4. Test TFTP retrieval from a test phone (10.1.12.189 Media/Kitchen) — request a small file via TFTP
5. Configure all 13 Zoom Phone extensions in Zoom dashboard (DIDs, ZTP URLs) — pre-provision as much as possible

**Priority 2 (On-site tonight, after business hours):**
1. Factory reset phones that show sipXcom in boot logs (check for WTEK background image)
2. Point each phone Boot Server → TFTP `10.1.10.175`, Port 69
3. Monitor Phase 1 upgrades (5.7.3) — wait for all to complete, then change passwords
4. Edit per-device `.cfg` files: change `-573` to `-598` (Phase 2 firmware)
5. Monitor Phase 2 upgrades (5.9.8) — confirm all reach 5.9.8
6. Configure each phone for Zoom ZTP provisioning
7. Verify ext 302 (Pet Shop) can be safely upgraded or needs research

---

## Critical Notes

- **Port 69 is open** — verified `nc -zu 10.1.10.175 69` returns success
- **tftpd64 is auto-start ready** — Task Scheduler will restart it on Windows reboot
- **Config file persists** — `C:\tftp\tftpd64.ini` contains all settings, survives restart
- **Root directory empty** — only firmware files and configs need to be added, nothing else
- **Firewall is open** — UDP 69 inbound rule allows phones to reach server
- **MacBook SSH ready** — standing by for SSH connection to pull firmware files

---

## Files Created This Session

- `/Users/warrenwiemer/Desktop/Deploy-TFTP.ps1` — (unused, Bitbucket URL failed)
- `/Users/warrenwiemer/Desktop/Deploy-TFTP-v2.ps1` — (unused, Bitbucket URL failed)
- `/Users/warrenwiemer/Desktop/Configure-TFTP.ps1` — **✅ USED** — successful tftpd64 setup

---

**Checkpoint Version:** 3.0 (Inherited Context format)
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Haiku 4.5)
**Status:** Ready for next session — firmware staging is the blocker
