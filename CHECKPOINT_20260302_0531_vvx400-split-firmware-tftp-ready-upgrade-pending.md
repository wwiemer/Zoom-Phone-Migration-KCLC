# Checkpoint: VVX400 Split Firmware TFTP Ready — Upgrade Pending

**Created:** 2026-03-02 05:31:48 EST
**Session:** KCLC-FEAT-20260302-003-SNNT-vvx400-firmware-hybrid-tftp-http
**Model:** Claude Sonnet 4.6
**Status:** PAUSED — TFTP infrastructure confirmed ready, firmware upgrade NOT yet confirmed on test phone

---

## Summary

Major breakthroughs this session: discovered combined `sip.ld` (397MB) was always the root cause of TFTP failures. Switched to the split firmware package — `3111-46157-002.sip.ld` is only 41MB. macOS built-in TFTP daemon (`/usr/libexec/tftpd`, root at `/private/tftpboot/`) confirmed serving the file successfully (43MB, 13 seconds, 1 rollover). `000000000000.cfg` updated to `APP_FILE_PATH="3111-46157-002.sip.ld"` (relative filename only — HTTP URLs like `http://10.1.10.156:8080/sip.ld` cause `Unknown status 53` error). Phone was rebooted but came back in ~32 seconds — too fast for a firmware flash. Firmware version NOT yet verified. Session ended due to context limit and facility opening time pressure.

---

## Inherited Context

### System Facts

- **sipXcom server:** ucs05.warrentek.net | CentOS 6.10 | sipXcom 19.04
- **sipXcom SSH:** `sshpass -p 'wbC7PmX5YckaKa9N' ssh root@ucs05.warrentek.net`
- **KCLC public IP (fail2ban whitelisted):** 72.166.63.114
- **Zoom account:** Single account — TFH + KCLC share @thefathershouse.com
- **Zoom MCP app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Credentials in Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **Zoom OAuth token:** `curl -X POST "https://zoom.us/oauth/token?grant_type=account_credentials&account_id=VOUi3C_NSjK1JNc6a1kWpQ" -H "Authorization: Basic $(echo -n 'WwXeCJPMQWSudkZqNaLsrA:91rlSEZCC1VIPyrY4Y4dPrbUZFquqGdY' | base64)"`
- **ZTP provisioning URL (VVX401):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401`
- **KCLC E911 address:** 2307 South St, Leesburg, FL 34748 (Impact Center — NOT 2301 which is TFH)
- **Port completed:** 2026-03-02 at 11:30 AM ET — number +13522680205 (may have already ported — verify status)
- **Test voicemail email (ext 314):** wwiemer@warrentek.net
- **TFTP root (macOS):** `/private/tftpboot/` — symlinked from `/Users/wwiemer/Library/CloudStorage/Dropbox/TFTP-Root/`
- **TFTP daemon:** macOS built-in `/usr/libexec/tftpd` — start: `sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist`
- **HTTP server:** `cd ~/Library/CloudStorage/Dropbox/TFTP-Root && python3 -m http.server 8080 > /tmp/http-server.log 2>&1 &`
- **MacBook Air IP:** 10.1.10.156
- **Test phone (VVX400):** MAC 0004f2de46b8 | IP 10.1.10.176 | firmware 5.5.2.8571 (NOT YET UPGRADED)
- **Split firmware source:** `/Users/wwiemer/Library/CloudStorage/Dropbox/Polycom/VVX/Poly_UC_Software_5_9_8_release_sig_split/3111-46157-002.sip.ld`

### Phone Inventory (14 total)

| Model | Count | Part # | Firmware File |
|-------|-------|--------|---------------|
| VVX 400 | ~12 | 3111-46157-002 (and others) | 3111-46157-002.sip.ld (41MB) |
| VVX 401 | ~1 | 3111-46161-001 | 3111-46161-001.sip.ld |
| VVX 410 | ~2 | unknown | TBD |

### Active Constraints

- **DO NOT TOUCH TFH extensions:** 202 (Stephen O'Neal), 703 (Tanya), 230, 232, 233, 235, 239, 240, 241, 245, 246, 282
- **311 is reserved** — N11 service code
- **phone:write:device:admin scope not available** — MAC registration done manually via admin.zoom.us
- **Poly VVX401 is "End of Life"** in Zoom — will still provision and work
- **pumpKIN crashes** on large TFTP transfers — DO NOT USE pumpKIN
- **Python HTTP server does NOT support PUT** — phone log uploads return 501 (harmless)
- **Web UI "Custom Server" upgrade path** — phone makes `GET /` expecting XML manifest → DO NOT USE this path
- **APP_FILE_PATH must be relative filename** — full HTTP URL causes `Unknown status 53` error
- **DHCP VLAN 12 serves ucs05.warrentek.net as TFTP server** — web UI provisioning override must be explicit and saved

### Key Decisions

- **Final extension mapping:** sipXcom 2XX → Zoom 3XX (+100), skip 311 (N11 reserved)
- **Firmware upgrade method:** macOS built-in TFTP daemon (NOT pumpKIN) + split firmware files
- **Split firmware confirmed:** 3111-46157-002.sip.ld = 41MB — transfers in ~13 seconds via TFTP
- **000000000000.cfg format:** UC Software 5.x child element `<Software APP_FILE_PATH="3111-46157-002.sip.ld" />`
- **APP_FILE_PATH:** Relative filename only (not full HTTP URL)

---

## Completed Work — DO NOT REDO

- [x] pumpKIN replaced with macOS built-in TFTP daemon (`/private/tftpboot/`)
- [x] Split firmware package identified: `/Dropbox/Polycom/VVX/Poly_UC_Software_5_9_8_release_sig_split/`
- [x] `3111-46157-002.sip.ld` (41MB) copied to `/private/tftpboot/` and `~/Dropbox/TFTP-Root/`
- [x] TFTP transfer confirmed: `Received 43297448 bytes in 84566 blocks with 1 rollover` (13 seconds)
- [x] `000000000000.cfg` updated: `APP_FILE_PATH="3111-46157-002.sip.ld"` in both TFTP locations
- [x] HTTP URL in APP_FILE_PATH ruled out — causes `Unknown status 53` error
- [x] Combined sip.ld (397MB) ruled out — exceeds TFTP block counter limit

---

## Blockers & Lessons

| Item | Detail |
|------|--------|
| Combined sip.ld (397MB) fails via TFTP | Block counter overflow at ~65535. Use split firmware only |
| APP_FILE_PATH must be relative | `http://10.1.10.156:8080/sip.ld` causes `Unknown status 53`. Use `3111-46157-002.sip.ld` |
| DHCP VLAN 12 overrides provisioning | Boot log shows DHCP serving `tftp-server-name ucs05.warrentek.net` on VLAN 12. Web UI setting must explicitly override this |
| pumpKIN crashes on large files | TFTP unreliable for files >10MB. macOS tftpd is reliable |
| Python HTTP server doesn't support PUT | Boot log upload fails (501) — harmless, no action needed |
| Web UI Custom Server upgrade path | Requires XML manifest at `GET /` — Python returns HTML. DO NOT USE |
| Phone web UI access | Firmware version check: Menu → Settings → Status → Platform → Application |
| 32-second reboot = no firmware flash | Firmware flash takes 3-5 minutes total. 32s means phone booted normally without flashing |

---

## Ideas & Discoveries

- **Multi-model fleet strategy:** `000000000000.cfg` can only have one `APP_FILE_PATH` — for mixed VVX400/401/410 fleet, either use per-device MACs to override firmware file OR create model-specific master configs (000000000000-vvx400.cfg) — need to test which approach works in 5.x
- **DHCP investigation:** If web UI provisioning override doesn't stick, check if sipXcom DHCP option 66 needs to be changed to point to MacBook (10.1.10.156) instead of ucs05.warrentek.net during upgrade window
- **VVX410 part number:** Still unknown — need to identify before ordering firmware file
- **KCLC Site in Zoom:** Not yet created — must create before provisioning any phones

---

## Next Steps — RECOMMENDED

**Priority order:**

1. **FIRST BOOT of session: Verify firmware version** on test phone:
   - Phone keypad: Menu → Settings → Status → Platform → Application → check for 5.9.8
   - OR web UI: https://10.1.10.176 → Status → Platform → Application
   - If shows 5.9.8 → upgrade succeeded, proceed to ZTP step
   - If shows 5.5.2 → upgrade did NOT happen, see step 2

2. **If firmware still 5.5.2 — troubleshoot provisioning:**
   - Confirm macOS TFTP is running: `sudo launchctl list | grep tftp`
   - Confirm phone provisioning server in web UI is set to: Type=TFTP, Server=10.1.10.156
   - Confirm `000000000000.cfg` in `/private/tftpboot/` has `APP_FILE_PATH="3111-46157-002.sip.ld"`
   - Reboot phone — watch for TFTP activity in macOS syslog: `sudo log show --predicate 'process == "tftpd"' --last 5m`
   - If DHCP is overriding: temporarily change sipXcom DHCP option 66 to 10.1.10.156 OR manually set phone provisioning server and save

3. **After firmware 5.9.8 confirmed:**
   - Set ZTP URL in phone web UI: `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401` (adjust for VVX400)
   - Create KCLC Site in Zoom (admin.zoom.us)
   - Register MAC in Zoom admin
   - Provision remaining 13 phones

4. **Port/Twilio status:** +13522680205 was porting 2026-03-02 at 11:30 AM ET — verify if port completed and if Twilio forward is needed

---

## Git Commit Details

**Branch:** main (submodule: projects/Zoom-Phone-Migration-KCLC)
**Files changed this session:**
- `TFTP-Root/000000000000.cfg` — Updated `APP_FILE_PATH` to `3111-46157-002.sip.ld`
- `TFTP-Root/3111-46157-002.sip.ld` — Added split firmware file (41MB) — may be in .gitignore due to size
- `private/tftpboot/` files synced

---

**Checkpoint Version:** 3.0 (Inherited Context format)
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-02 05:31:48 EST
