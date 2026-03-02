# Checkpoint: VVX400 Firmware Upgrade — HTTP Server Active, Phone Not Loading Master Config

**Created:** 2026-03-02 04:21:00 EST
**Session:** KCLC-FEAT-20260302-002-SNNT-vvx400-firmware-http-upgrade
**Model:** Claude Sonnet 4.6
**Status:** BLOCKED — Phone not downloading 000000000000.cfg; firmware upgrade not triggering

---

## Summary

Major progress this session: fixed `000000000000.cfg` format (was 4.x flat attribute, now correct 5.x child element), converted per-device cfg files from INI to XML, factory reset the phone, and confirmed after reset that the phone DOES download `000000000000.cfg` and initiates `sip.ld` download. However pumpKIN crashed during the 397 MB TFTP transfer, so we switched to a Python HTTP server. In HTTP mode the phone downloads per-device config files but NOT `000000000000.cfg` (stored file list only has per-device files). The web UI "Custom Server" upgrade path expects an XML version manifest at `GET /` — Python serves HTML → error. Deadline (11:30 AM ET port) is approaching.

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
- **TFTP root:** `/Users/wwiemer/Library/CloudStorage/Dropbox/TFTP-Root/`
- **HTTP server:** `cd TFTP-Root && python3 -m http.server 8080 > /tmp/http-server.log 2>&1 &`
- **MacBook Air IP:** 10.1.10.156
- **Test phone (VVX400):** MAC 0004f2de46b8 | IP 10.1.10.176 | firmware 5.5.2.8571

### Phone Inventory (14 total)

| Model | Count | Part # | Notes |
|-------|-------|--------|-------|
| VVX 400 | ~12 | 3111-46157-002 (and others) | Most common model |
| VVX 401 | ~1 | 3111-46161-001 | Used for ext 314 (test) |
| VVX 410 | ~2 | unknown | |

### Active Constraints

- **DO NOT TOUCH TFH extensions:** 202 (Stephen O'Neal), 703 (Tanya), 230, 232, 233, 235, 239, 240, 241, 245, 246, 282
- **311 is reserved** — N11 service code, cannot be used
- **phone:write:device:admin scope not available** — MAC registration must be done manually via admin.zoom.us
- **Poly VVX401 is "End of Life"** in Zoom — will still provision and work
- **KCLC Site NOT yet created** in Zoom
- **Twilio forward pending:** +13522680205 → +13527820706 (before 11:30 AM ET port)
- **pumpKIN crashes** on large TFTP transfers (397 MB sip.ld) — use HTTP for firmware
- **Python HTTP server does NOT support PUT** — phone log uploads return 501 (harmless)
- **Web UI "Custom Server" upgrade path** expects XML manifest at `GET /` — Python returns HTML → error — do not use this path

### Key Decisions

- **Final extension mapping:** sipXcom 2XX → Zoom 3XX (+100), skip 311 (N11 reserved)
- **214→314 correction:** Media/Kitchen was at sipXcom ext 211 → Zoom ext 314 (not 311)
- **Bus Station (ext 300)** needs renumber from 718
- **Firmware upgrade method:** TFTP for config files (small, pumpKIN handles fine), HTTP for sip.ld (large — Python server)

---

## Completed Work — DO NOT REDO

- [x] `000000000000.cfg` — Rewritten to UC Software 5.x format: `<Software APP_FILE_PATH="sip.ld" />`
- [x] `0004f2de46b8.cfg` — Converted from INI to valid `<polycomConfig>` XML (empty — device params not settable via config)
- [x] `0004f2de46b8-phone.cfg` — Same fix as above
- [x] `0004f2de46b8-web.cfg` — Updated to clear `upgrade.custom.server.url=""` (was causing bad URL path construction)
- [x] `000000000000-directory.xml` — Copied from `000000000000-directory~.xml` (tilde was wrong filename)
- [x] Factory reset test phone — cleared sipXcom stored config file list from flash
- [x] Confirmed after factory reset: phone downloads `000000000000.cfg` → initiates `sip.ld` download
- [x] Switched to Python HTTP server (pumpKIN unreliable for 397 MB transfers)
- [x] HTTP server logging confirmed: phone downloads per-device files (0004f2de46b8.cfg/.phone.cfg/.web.cfg) via HTTP

---

## Blockers & Lessons

| Item | Detail |
|------|--------|
| Phone skips `000000000000.cfg` in HTTP mode | Stored file list (from previous sessions) only has per-device MAC files — master config never downloaded unless factory reset |
| pumpKIN crashes on sip.ld (397 MB) | TFTP unreliable for large files. Use HTTP for firmware. pumpKIN is fine for small config files |
| Web UI Custom Server upgrade | Phone makes `GET /` expecting XML version manifest. Python returns HTML → "XML file cannot be read" error. DO NOT USE this path |
| `upgrade.custom.server.url` bad values | Even with web.cfg cleared, phone uses cached flash value (`10.1.10.156`, `10.1.10.156:8080`) as filenames on next boot |
| Boot log line `Server said 'X' is not present` | The X value is the `upgrade.custom.server.url` being used as a filename — indicates the cached setting persists even after web.cfg clears it |
| "Could not get application name" / "Unknown status 53" | Phone can't find firmware path because it never got `000000000000.cfg` with `APP_FILE_PATH="sip.ld"` |

---

## Ideas & Discoveries

- **Key insight:** Set `APP_FILE_PATH` to full HTTP URL in `000000000000.cfg`. Use pumpKIN for TFTP config provisioning (which it handles fine), but point the firmware URL to `http://10.1.10.156:8080/sip.ld`. Phone downloads small configs via TFTP → reads master config → downloads firmware via HTTP. Hybrid approach avoids both pumpKIN large-file crash AND the stored-file-list problem.
  - In `000000000000.cfg`: `<Software APP_FILE_PATH="http://10.1.10.156:8080/sip.ld" />`
  - Keep provisioning server as TFTP for config phase
  - Firmware download goes to HTTP automatically
- Consider `prov.cfgFile.1=000000000000.cfg` in per-device config to force master config download in HTTP mode (if supported in 5.x)
- Phone log timestamps use local phone time (08:xx) while Mac shows 04:xx — different time zones, don't confuse boot cycles when cross-referencing HTTP log vs boot log

---

## Next Steps — RECOMMENDED

**Priority order:**
1. **[BEST OPTION] Hybrid TFTP/HTTP upgrade:**
   - Update `000000000000.cfg`: set `APP_FILE_PATH="http://10.1.10.156:8080/sip.ld"` (full HTTP URL)
   - Start pumpKIN (config files only — fine for small files)
   - Start Python HTTP server on 8080
   - Set phone provisioning to TFTP / 10.1.10.156
   - Reboot phone → downloads `000000000000.cfg` via TFTP → downloads `sip.ld` via HTTP
   - This worked before (TFTP config + HTTP firmware) — pumpKIN never had to serve sip.ld

2. **Before 11:30 AM ET (URGENT):** Set Twilio forward +13522680205 → +13527820706 regardless of phone status

3. **After firmware upgrade on test phone:**
   - Set ZTP URL in phone web UI
   - Create KCLC Site in Zoom
   - Provision remaining 13 phones

---

## Git Commit Details

**Branch:** main
**Files changed:**
- `TFTP-Root/000000000000.cfg` — Rewrote to UC Software 5.x format
- `TFTP-Root/0004f2de46b8.cfg` — INI → XML conversion
- `TFTP-Root/0004f2de46b8-phone.cfg` — INI → XML conversion
- `TFTP-Root/0004f2de46b8-web.cfg` — Cleared upgrade.custom.server.url
- `TFTP-Root/000000000000-directory.xml` — Copied from tilde filename

---

**Checkpoint Version:** 3.0 (Inherited Context format)
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-02 04:21:00 EST
