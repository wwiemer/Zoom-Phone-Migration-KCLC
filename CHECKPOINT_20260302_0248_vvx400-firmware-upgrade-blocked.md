# Checkpoint: VVX400 Firmware Upgrade Blocked — TFTP Provisioning Failing

**Created:** 2026-03-02 02:48:00 EST
**Session:** KCLC-FEAT-20260302-001-SNNT-vvx400-firmware-tftp-upgrade
**Model:** Claude Sonnet 4.6
**Status:** BLOCKED — Research required before proceeding

---

## Summary

Spent ~6 hours attempting to upgrade VVX400 firmware from 5.5.2.8571 → 5.9.8 via TFTP (pumpKIN). Core blocker: phone runs TLS 1.0, Zoom provisioning requires TLS 1.2. Setup a TFTP server with 5.9.8 Combined firmware. Phone connects to TFTP server and downloads config files but **never requests sip.ld** and fails with provisioning error before reaching firmware download stage. Research into Polycom's official VVX400 provisioning parameters required before continuing.

**Port deadline:** 2026-03-02 at 11:30 AM ET — approximately 9 hours from this checkpoint.

---

## Inherited Context

### System Facts

- **sipXcom server:** ucs05.warrentek.net | CentOS 6.10 | sipXcom 19.04
- **sipXcom SSH:** `sshpass -p 'wbC7PmX5YckaKa9N' ssh root@ucs05.warrentek.net`
- **KCLC public IP (fail2ban whitelisted):** 72.166.63.114
- **Zoom account:** Single account — TFH + KCLC share @thefathershouse.com
- **Zoom MCP app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Credentials in Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **Zoom OAuth token:** Refresh via: `curl -X POST "https://zoom.us/oauth/token?grant_type=account_credentials&account_id=VOUi3C_NSjK1JNc6a1kWpQ" -H "Authorization: Basic $(echo -n 'WwXeCJPMQWSudkZqNaLsrA:91rlSEZCC1VIPyrY4Y4dPrbUZFquqGdY' | base64)"`
- **ZTP provisioning URL (VVX401):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401`
- **KCLC E911 address:** 2307 South St, Leesburg, FL 34748 (Impact Center — NOT 2301 which is TFH)
- **Port completing:** 2026-03-02 at 11:30 AM ET — number +13522680205
- **Test voicemail email (ext 314):** wwiemer@warrentek.net

### Phone Inventory (14 total)

| Model | Count | Part # | Notes |
|-------|-------|--------|-------|
| VVX 400 | ~12 | 3111-46157-002 (and others) | Most common model |
| VVX 401 | ~1 | 3111-46161-001 | Used for ext 314 (test) |
| VVX 410 | ~2 | unknown | |

**Test phone currently working on:**
- Model: VVX 400 (web UI shows "VVX 400" — physically the same form factor as 401)
- Part Number: 3111-46157-002 Rev:A
- MAC: 0004f2de46b8
- IP: 10.1.10.176 (Static TFTP, gets DHCP otherwise)
- Current firmware: 5.5.2.8571 (TLS 1.0 only)
- Target firmware: 5.9.8 (TLS 1.2 — needed for Zoom provisioning)

### Active Constraints

- **DO NOT TOUCH TFH extensions:** 202 (Stephen O'Neal), 703 (Tanya), 230, 232, 233, 235, 239, 240, 241, 245, 246, 282
- **311 is reserved** — N11 service code, cannot be used
- **phone:write:device:admin scope not available** — MAC registration must be done manually via admin.zoom.us
- **Poly VVX401 is "End of Life"** in Zoom — will still provision and work
- **KCLC Site NOT yet created** in Zoom
- **Twilio forward pending:** +13522680205 → +13527820706 (before 11:30 AM ET port)

### Key Decisions

- **Final extension mapping:** sipXcom 2XX → Zoom 3XX (+100), skip 311 (N11 reserved)
- **214→314 correction:** Media/Kitchen was at sipXcom ext 211 → Zoom ext 314 (not 311)
- **Bus Station (ext 300)** needs renumber from 718

---

## Completed Work This Session

- [x] Verified Zoom MCP server deployment working (list_users, list_phone_numbers tested)
- [x] Diagnosed root cause: VVX400 firmware 5.5.2.8571 uses TLS 1.0; Zoom provisioning requires TLS 1.2
- [x] Confirmed error from phone logs: `error:1409442E:ssl3_read_bytes:tlsv1 alert protocol version`
- [x] Set up pumpKIN TFTP server on MacBook Air (10.1.10.156, port 69)
- [x] TFTP root: `/Users/wwiemer/Library/CloudStorage/Dropbox/TFTP-Root/`
- [x] Downloaded and loaded Polycom UC Software 5.9.8 **Combined** package into TFTP root
- [x] Updated `000000000000.cfg` to include VVX300/400/401/410 model entries pointing to sip.ld
- [x] Renamed `0004f2de46b8-phone.cfg` → `0004f2de46b8.cfg` (correct filename the phone requests)
- [x] Created `0004f2de46b8-phone.cfg` as copy (phone requests both)
- [x] Created XML-formatted license placeholder files (000000000000-license.cfg, 0004f2de46b8-license.cfg)
- [x] Fixed device config syntax: `key>value` → `key=value`
- [x] Set pumpKIN write behavior to "Take all files"
- [x] Confirmed phone successfully connects to pumpKIN and downloads all config files
- [x] Confirmed phone is NOT requesting sip.ld — firmware download never happens

---

## Current TFTP Directory State

**Location:** `/Users/wwiemer/Library/CloudStorage/Dropbox/TFTP-Root/`

| File | Size | Notes |
|------|------|-------|
| sip.ld | 398 MB | 5.9.8 Combined SIP firmware |
| 3111-17823-001.dect.ld | 27 MB | DECT module firmware |
| sip.ver | 10 B | Firmware version/checksum |
| dect.ver | 92 B | DECT version |
| 000000000000.cfg | ~2 KB | Updated — VVX models added |
| 000000000000-directory.xml | 640 B | From firmware package |
| 000000000000-license.cfg | ~40 B | XML placeholder |
| 0004f2de46b8.cfg | 336 B | Device config (syntax fixed) |
| 0004f2de46b8-phone.cfg | 336 B | Copy of device config |
| 0004f2de46b8-web.cfg | 342 B | Phone-generated config |
| 0004f2de46b8-license.cfg | ~40 B | XML placeholder |
| Config/ | dir | From firmware package |
| languages/ | dir | From firmware package |
| VVXLocalization/ | dir | From firmware package |
| Audio files | - | LoudRing, Polycom-hold, Warble, Welcome |

**pumpKIN settings:**
- TFTP root: `/Users/wwiemer/Library/CloudStorage/Dropbox/TFTP-Root/`
- Read behavior: Give all files
- Write behavior: Take all files ✓
- Log file: `/Users/wwiemer/Library/CloudStorage/Dropbox/TFTP-Root/pumpKIN-Logs/Untitled`

---

## The Blocking Error

Every reboot produces this sequence in logs:

```
cfg  |Prov|Starting to provision
cfg  |Prov|cfgProvStatusSet: Unknown status 53
cfg  |Prov|Could not get application name
cfg  |Prm|Not proceeding with reboot/restart, as device parameters needs to be updated, before making a decision
cfg  |Prov|Finished updating configuration
```

**Key finding:** pumpKIN logs confirm phone downloads config files but **never requests sip.ld**. The firmware download never begins because provisioning validation fails before reaching that step.

---

## What Needs to Be Researched

### Primary Research Question

**What are the correct Polycom VVX400 device config parameters to enable firmware update via TFTP?**

- The error "Could not get application name" suggests the phone's provisioning state machine can't identify the firmware profile
- "Unknown status 53" is an internal provisioning state code — need to know what it means
- Official Polycom admin guide (UC Software 5.x Admin Guide) should have the config parameter reference

### Specific Questions

1. **Is there an intermediate firmware version required?**
   - Can 5.5.2.8571 upgrade directly to 5.9.8, or must it go through an intermediate version first (e.g., 5.7.x or 5.8.x)?
   - Polycom sometimes requires staged upgrades for major version jumps

2. **Does 5.9.8 Combined support TLS 1.2?**
   - Confirm 5.9.8 includes TLS 1.2 support for Zoom ZTP provisioning URL

3. **Should we use Split instead of Combined?**
   - Combined has one monolithic sip.ld (398 MB)
   - Split has model-specific files (e.g., 3111-46157-002.sip.ld)
   - Phone's boot log tried to request `3111-46157-002.sip.ld` first, fell back to `sip.ld`
   - Split might be more compatible with this provisioning flow

4. **What does "Unknown status 53" mean in Polycom provisioning?**
   - This is a numeric state code in the Polycom provisioning state machine
   - Might indicate the phone is in an unexpected state that blocks updates

5. **Are there required device config parameters missing?**
   - The `0004f2de46b8.cfg` currently has minimal parameters
   - Polycom VVX Admin Guide should list required parameters for firmware provisioning

### Suggested Resources

- Polycom UC Software 5.x Administrator Guide (PDF) — available on Polycom/HP support site
- Polycom Zero Touch Provisioning guide
- Polycom Community forums for "Unknown status 53"
- Polycom Partner Portal for firmware upgrade paths

---

## Remaining Tasks (KCLC Migration)

1. **IMMEDIATE: Fix VVX400 firmware upgrade** — Research above, then retry
2. **After firmware on all phones:** Set ZTP provisioning URL in each phone's web UI → Zoom URL
3. **Before 11:30 AM ET:** Set Twilio forward: +13522680205 → +13527820706
4. **Create KCLC Site in Zoom** with 2307 South St E911 address
5. **Provision all 14 phones** to their respective Zoom extensions
6. **Renumber Bus Station** from ext 718 → ext 300
7. **Create KCLC Provision Template** for BLF/speed dial key config
8. **Test all extensions** after provisioning

---

## Session Ideas / Discoveries

- pumpKIN TFTP write log location: `/Users/wwiemer/Library/CloudStorage/Dropbox/TFTP-Root/pumpKIN-Logs/Untitled`
- Phone writes logs to TFTP server after each boot: `0004f2de46b8-boot.log` and `0004f2de46b8-app.log`
- These phone-written logs are extremely useful for diagnosing provisioning failures
- Phone's part number (3111-46157-002) determines which firmware variant it requests first
- Split firmware package provides model-specific sip.ld files (e.g., `3111-46157-002.sip.ld`) — may be needed
- "Combined" firmware's 000000000000.cfg from the Polycom download is pre-configured for older SoundPoint phones, NOT VVX models — must be updated manually

---

**Checkpoint Version:** 3.0 (Inherited Context format)
**Archive Status:** Ready for next session
