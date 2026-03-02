# Checkpoint: SigKey=152 Blocker — 5.8.x Intermediate Firmware Required

**Created:** 2026-03-02 08:19:05 EST
**Session:** KCLC-FEAT-20260302-004-SNNT-vvx400-firmware-verify-and-ztp
**Model:** Claude Sonnet 4.6
**Status:** BLOCKED — All 5.9.8 firmware uses SigKey=152, incompatible with 5.7.2 Updater. Need 5.8.x firmware to upgrade Updater first.

---

## Summary

Root cause of firmware upgrade failure finally identified: UC Software 5.9.8 (both available packages — split and rts63 combined) uses **SigKey=152**, a signing key introduced after April 2017. The phones' Updater component (5.7.2.21547, 29-Apr-17) doesn't have this key in its trust store → instant "Could not get application name" before any firmware is flashed. All previous TFTP approaches (filename, format, sip.ver) were irrelevant to this core issue.

MacBook Air IP changed: 10.1.10.156 → **10.1.36.71** (DHCP). Phone provisioning servers updated.

VVX 401 identified: Part **3111-48400-001** (NOT 3111-46161-001 as previously assumed). Per-device config created.

---

## Inherited Context

### System Facts

- **sipXcom server:** ucs05.warrentek.net | CentOS 6.10 | sipXcom 19.04
- **sipXcom SSH:** `sshpass -p 'wbC7PmX5YckaKa9N' ssh root@ucs05.warrentek.net`
- **KCLC public IP (fail2ban whitelisted):** 72.166.63.114
- **Zoom account:** Single account — TFH + KCLC share @thefathershouse.com
- **Zoom MCP app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Credentials in Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **ZTP provisioning URL (VVX401):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401`
- **ZTP provisioning URL (VVX400):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
- **KCLC E911 address:** 2307 South St, Leesburg, FL 34748 (Impact Center — NOT 2301 which is TFH)
- **Port status:** +13522680205 was scheduled to port 11:30 AM ET 2026-03-02 — **verify if completed**
- **Test voicemail email (ext 314):** wwiemer@warrentek.net
- **MacBook Air IP:** 10.1.36.71 (DHCP — was 10.1.10.156, changed this session)
- **TFTP root (macOS):** `/private/tftpboot/`
- **TFTP daemon:** macOS built-in — reload: `sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist`
- **HTTP server (if needed):** `cd /private/tftpboot && sudo python3 -m http.server 80`

### Phone Inventory (14 total — all on 5.5.2.8571 / Updater 5.7.2.21547)

| Model | Part # | MAC | IP | Firmware File Needed |
|-------|--------|-----|----|---------------------|
| VVX 400 (test) | 3111-46157-002 | 0004f2de46b8 | 10.1.10.176 | 3111-46157-002.sip.ld |
| VVX 401 (ext 211, Media/Kitchen) | **3111-48400-001** | 64:16:7F:31:A9:6D | 10.1.12.189 | 3111-48400-001.sip.ld |
| VVX 400 (~12 more) | 3111-46157-002 | various | various | 3111-46157-002.sip.ld |
| VVX 410 (~2) | unknown | unknown | unknown | TBD |

### TFTP Root Current State (`/private/tftpboot/`)

| File | Purpose | Status |
|------|---------|--------|
| `000000000000.cfg` | Master config, `APP_FILE_PATH="sip.ld"` | ✅ |
| `sip.ver` | Version manifest `5.9.8.5760` | ✅ |
| `sip.ld` | = 3111-46157-002.sip.ld (VVX 400 firmware, 41MB) | ✅ |
| `3111-46157-002.sip.ld` | VVX 400 split firmware | ✅ |
| `0004f2de46b8.cfg` | VVX 400 per-device config (empty shell) | ✅ |
| `64167f31a96d.cfg` | VVX 401 per-device config (empty shell) | ✅ created this session |

### Root Cause Confirmed

```
Firmware header: SigKey=152 (Aug 2023 build)
Updater version: 5.7.2.21547 (Apr 2017)
Result: Updater cannot verify firmware signature → "Could not get application name"
```

Both available 5.9.8 packages confirmed affected:
- `Poly_UC_Software_5_9_8_release_sig_split/3111-46157-002.sip.ld` → SigKey=152
- `Poly_UC_Software_5.9.8_rts63_release_sig_combined/sip.ld` → SigKey=152 (also wrong hardware: Cpu=33792 vs VVX400 Cpu=2048)

### Active Constraints

- **DO NOT TOUCH TFH extensions:** 202, 703, 230, 232, 233, 235, 239, 240, 241, 245, 246, 282
- **311 is reserved** — N11 service code
- **Zoom ZTP blocked** — requires TLS 1.2 minimum; 5.5.2 only supports TLS 1.0
- **pumpKIN** — DO NOT USE (crashes on large TFTP transfers)
- **APP_FILE_PATH** — must be relative filename, NOT full HTTP URL (causes status 53)
- **DHCP VLAN 12** — serves ucs05.warrentek.net as TFTP server; web UI Boot Server must be set to **Static**

### Key Decisions

- **Final extension mapping:** sipXcom 2XX → Zoom 3XX (+100), skip 311
- **Two-step firmware upgrade required:** 5.5.2 → 5.8.x (installs new Updater) → 5.9.8
- **Per-model firmware files:** each phone model gets its own firmware file (not generic sip.ld)
- **Multi-model provisioning:** per-device .cfg files will specify model-specific firmware paths once implemented

---

## Completed Work — DO NOT REDO

- [x] Root cause confirmed: SigKey=152 incompatible with 5.7.2 Updater
- [x] Both 5.9.8 packages checked — both use SigKey=152
- [x] MacBook Air IP updated to 10.1.36.71 in all phone provisioning servers
- [x] TFTP daemon reloaded (`sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist`)
- [x] `000000000000.cfg` corrected to attribute format (APP_FILE_PATH on APPLICATION element)
- [x] `sip.ver` added to TFTP root (5.9.8.5760)
- [x] `sip.ld` = 3111-46157-002.sip.ld (correct split firmware for VVX400)
- [x] VVX 401 identified: Part 3111-48400-001, MAC 64:16:7F:31:A9:6D, IP 10.1.12.189
- [x] Per-device config created for VVX 401 (64167f31a96d.cfg)
- [x] VVX 401 provisioning server confirmed: TFTP, 10.1.36.71, Boot Server = Static

---

## Blockers

| Item | Detail |
|------|--------|
| **SigKey=152** | All 5.9.8 firmware uses this key; 5.7.2 Updater can't verify → upgrade fails |
| **Need 5.8.x firmware** | Install 5.8.x first → new Updater included → then 5.9.8 works |
| **Zoom ZTP blocked** | TLS 1.0 (5.5.2) vs TLS 1.2 required by Zoom — can't ZTP until on 5.9.x |
| **KCLC Site not created** | Zoom site needs to exist before any ZTP provisioning |

---

## Ideas & Discoveries

- **Engineering Advisory 68081** referenced in rts63 ReadMe — "Announcing New Polycom UC Software Installation Changes" — likely documents the SigKey change and required upgrade path
- **Poly support portal** may have 5.8.x firmware available for free (VVX EOL'd)
- **Per-device firmware routing:** 000000000000.cfg could have APP_FILE_PATH="" and per-device .cfg files set `app.provisioning.upgradeServer` or similar param to specify model firmware — worth investigating for multi-model fleet
- **rts63 firmware** is for different hardware (Cpu=33792 vs VVX400's Cpu=2048) — NOT usable for our phones
- **Bootloader/BootBlock recovery** as last resort: hold keys during power-on → BootBlock accepts TFTP firmware without signature check (risky — research key combo first)

---

## Next Steps — RECOMMENDED

**Priority order for next session:**

1. **FIRST: Find and download UC Software 5.8.x** for VVX phones
   - Check: https://support.hp.com or HP Poly community (search "UC Software 5.8.0 VVX download")
   - Also check: Poly community forum — many legacy firmware files shared there
   - Target files: `sip.ld` from the 5.8.x package (combined or split)
   - Verify: check SigKey in header — should be <150 for 5.7.2 Updater compatibility
   - Note: 5.8.x may also be available via `xxd sip.ld | head -4` to confirm signing key

2. **Once 5.8.x obtained:**
   - Copy to `/private/tftpboot/sip.ld` (replacing current 5.9.8)
   - Update `sip.ver` to match 5.8.x version
   - Reboot VVX 401 (Warren is physically present) — should upgrade cleanly
   - Verify 5.8.x installs, then swap to 5.9.8 sip.ld for final upgrade

3. **Multi-model firmware setup (once upgrade path confirmed):**
   - Per-device configs specify each model's firmware file
   - VVX 400: `APP_FILE_PATH="3111-46157-002.sip.ld"`
   - VVX 401: `APP_FILE_PATH="3111-48400-001.sip.ld"`
   - VVX 410: TBD (identify part number first)

4. **After all phones on 5.9.8:**
   - Create KCLC Site in Zoom (admin.zoom.us)
   - Register MACs in Zoom
   - Set ZTP URL in each phone
   - Provision remaining 13 phones

5. **Verify port status:** +13522680205 — was scheduled 11:30 AM ET today

---

**Checkpoint Version:** 3.0 (Inherited Context format)
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-02 08:19:05 EST
