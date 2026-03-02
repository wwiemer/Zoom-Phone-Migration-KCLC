# Checkpoint: Ext 314 Configured in Zoom — On-Site Provisioning Pending

**Created:** 2026-03-01 20:44 EST
**Session:** KCLC-IMPL-20260301-002-CLDE-zoom-3xx-buildout-execution
**Model:** Claude Sonnet 4.6
**Status:** In Progress — Warren driving to site to physically provision phones

---

## Summary

Extension numbering plan finalized (311 is reserved N11 code — Media/Kitchen moved to 314). Zoom changes executed for ext 314: renumbered 715→314, renamed, voicemail set to wwiemer@warrentek.net, MAC registered via portal. Provisioning URL entered in Polycom web UI at 10.1.12.189 but remote reboot failed — Warren going on-site to physically factory reset and provision phones. Port of 352-268-0205 is tomorrow at 11:30 AM ET.

---

## Inherited Context

### System Facts

- **sipXcom server:** ucs05.warrentek.net | CentOS 6.10 | sipXcom 19.04
- **sipXcom SSH:** `sshpass -p 'wbC7PmX5YckaKa9N' ssh root@ucs05.warrentek.net`
- **sipXcom DB:** `psql -U postgres SIPXCONFIG` (uppercase — "sipxconfig" fails)
- **KCLC public IP (fail2ban whitelisted):** 72.166.63.114
- **Zoom account:** Single account — TFH + KCLC share @thefathershouse.com
- **Zoom MCP app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Credentials in Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **Zoom API base URL:** https://api.zoom.us/v2
- **Zoom OAuth token:** Refresh via: `curl -X POST "https://zoom.us/oauth/token?grant_type=account_credentials&account_id=VOUi3C_NSjK1JNc6a1kWpQ" -H "Authorization: Basic $(echo -n 'WwXeCJPMQWSudkZqNaLsrA:91rlSEZCC1VIPyrY4Y4dPrbUZFquqGdY' | base64)"`
- **ZTP provisioning URL (VVX401):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401`
- **KCLC E911 address:** 2307 South St, Leesburg, FL 34748 (Impact Center — NOT 2301 which is TFH church)
- **TFH E911 address:** 2301 South St, Leesburg, FL 34748
- **Port completing:** 2026-03-02 at 11:30 AM ET — number +13522680205
- **Test voicemail email (ext 314):** wwiemer@warrentek.net

### Active Constraints

- **DO NOT TOUCH TFH extensions:** 202 (Stephen O'Neal), 703 (Tanya), 230, 232, 233, 235, 239, 240, 241, 245, 246, 282
- **311 is reserved** — N11 service code, cannot be used as extension in Zoom
- **phone:write:device:admin scope not available** via S2S OAuth app — MAC registration must be done manually via admin.zoom.us → Phones & Devices → Add
- **Remote reboot of Polycom VVX401 at 10.1.12.189 does not work** — factory reset requires physical access
- **check_voicemails_over_phone is DISABLED** account-wide — discuss with Stephen before enabling
- **Poly vvx401 is "End of Life"** in Zoom — will still provision and work, no firmware updates
- **2307 South St emergency address NOT yet created** in Zoom — only 2301 exists currently
- **KCLC Site NOT yet created** in Zoom — all KCLC extensions still on default TFH site

### Key Decisions

- **Final extension mapping:** sipXcom 2XX → Zoom 3XX (+100), except 211→314 (311 reserved), 212→312, 213→313
- **311 permanently skipped** — same logic Stephen used for 711 in the 700 series
- **KCLC needs separate Zoom Site** with 2307 South St as default E911 — do AFTER on-site provisioning
- **BLF/speed dial config** (poison control, etc.) needs to be pulled from sipXcom and recreated as Zoom Line Keys via Provision Template — separate task
- **Twilio forward TONIGHT** (after all phones provisioned): +13522680205 → +13527820706
- **Bus Station (ext 300)** already exists in Zoom at ext 718 (ID: 7xxzuF9fSmCwccpgEFeraA) — just needs renumbering, not creation

---

## Completed Work — DO NOT REDO

- [x] Confirmed 311 is reserved N11 — corrected plan, Media/Kitchen → ext 314
- [x] Updated MASTER-EXTENSION-REFERENCE: 212→312, 213→313, 211→314, ext 313 named "Rec Room"
- [x] Updated CHECKPOINT_20260301_1832 with corrected mapping
- [x] Executed Zoom: renumbered ext 715 → 314, renamed "Media Center" → "Media/Kitchen"
- [x] Set voicemail notification email on ext 314 → wwiemer@warrentek.net
- [x] Registered MAC 64167F31A96D (Poly vvx401) to ext 314 via admin.zoom.us portal
- [x] Entered ZTP provisioning URL in Polycom web UI at 10.1.12.189 (single URL field: `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401`)
- [x] Identified correct KCLC E911 address: 2307 South St (not 2301)
- [x] Updated MASTER-EXTENSION-REFERENCE E911 address to 2307 South St
- [x] Confirmed cross-site dialing works — Stephen (TFH ext 202) can dial KCLC ext 300 directly
- [x] Confirmed Bus Station exists at Zoom ext 718 — needs renumber to 300, not a new creation
- [x] Pushed updated files to GitHub

---

## Blockers & Lessons

| Item | Detail |
|------|--------|
| 311 reserved (N11) | ALL Nxx ranges have Nx11 reserved (211, 311, 411, 511, 611, 711, 811, 911). No 3-digit century avoids this. Solution: skip 311, use 314 for Media/Kitchen. |
| phone:write:device:admin missing | Zoom S2S OAuth app does not expose this scope. MAC registration must be done manually via admin.zoom.us portal for all 14 phones. |
| Remote Polycom reboot blocked | VVX401 web UI would not execute factory reset or reboot remotely. Physical button press required. Provisioning URL IS saved in the phone already. |
| 2307 South St not in Zoom | KCLC emergency address needs to be created in Zoom. Currently only 2301 (TFH) exists. Create when setting up KCLC site. |
| Poly vvx401 End of Life | Zoom flags VVX401 (and likely all VVX series) as EOL. Functional for now, no firmware updates. All 14 KCLC phones are VVX series. |
| 703 conflict in 700 range | TFH's Tanya is at ext 703 — reason we cannot use 700 series for KCLC sequential mapping |

---

## Ideas & Discoveries

- **KCLC Zoom Site** would solve E911, grouping, and policy separation cleanly — create as priority item tonight
- **BLF/speed dial config** currently on sipXcom phone groups — pull before decommission, recreate as Zoom Provision Template (poison control numbers, speed dials, etc.)
- **Provision Template** for all KCLC devices — once BLF config is pulled, create one template and bind to all 14 devices at once
- **check_voicemails_over_phone** is disabled account-wide — Stephen should decide whether to enable for Zoom users

---

## Next Steps — ON-SITE EXECUTION

**1. Factory reset / provision ext 314 (test phone — Media/Kitchen, 10.1.12.189):**
- Physically press factory reset on VVX401 (usually Admin Settings → Factory Reset on the phone, or hold OK for 10 sec during boot)
- Phone reboots, contacts Zoom ZTP at saved URL, downloads ext 314 config
- Verify: status goes Online in Zoom portal, ext 314 shows on phone display
- Test: call +1 (352) 782-0626 — phone should ring
- Verify: voicemail notification arrives at wwiemer@warrentek.net

**2. If test passes — build out all remaining Zoom changes (API, via Claude):**
- Renumber 12 remaining KCLC common areas: 705→301, 706→303, 707→305, 708→306, 709→301, 710→307, 712→308, 713→312, 714→313, 716→310, 717→309, 718→300
- Register MACs for remaining 13 phones via admin.zoom.us (manual, one by one)
- Create KCLC Zoom Site with 2307 South St E911 address
- Move all 14 KCLC extensions to KCLC site
- Create Zoom Call Queue: members 300/301/302/303/304, simultaneous ring 24s, voicemail → 304

**3. Tonight (after all phones confirmed working):**
- Set Twilio forward: +13522680205 → +13527820706

**4. Tomorrow 2026-03-02 after 11:30 AM ET (post-port):**
- Assign +13522680205 as DID to ext 300 in Zoom
- Remove Twilio forward

**5. Separate session (after migration complete):**
- Pull BLF/speed dial config from sipXcom
- Create Zoom Provision Template with line keys
- Bind template to all 14 KCLC devices
- sipXcom security remediation (Steps 1-4 from REMEDIATION-PLAN-20260223.md)

---

## Key Files

| File | Purpose |
|------|---------|
| `MASTER-EXTENSION-REFERENCE-20260301.md` | **LOCKED** execution reference — all 14 extensions with corrected mapping |
| `ZOOM-BACKUP-PRE-MIGRATION-20260301.json` | Pre-migration Zoom state snapshot |
| `CHECKPOINT_20260301_1832_zoom-discovery-complete-ready-to-execute.md` | Previous session checkpoint |

---

## Zoom Common Area IDs (Live — for API calls)

| Ext (current) | Room | Zoom ID |
|---|---|---|
| 718 | Bus Station | 7xxzuF9fSmCwccpgEFeraA |
| 709 | Toy Shop | PXKs8k8KQNOlXxniWe-nRg |
| 705 | Pet Shop | hwrsUXo8ThiOSVeMJqeXHw |
| 706 | Sweet Shop | g1kapE7iSDKy8m0gfYgqXQ |
| 707 | Sport Store | BSqFwmSETLCIwKHmLBEQEw |
| 708 | Ice Cream Shop | 5OhV6Py-R7KZBTihOi6ciQ |
| 710 | Music Store | XzA8eGZyRYiqhes2xyIxBA |
| 712 | Flower Shop | Rok-gCQiTpygUdw_oY_7iA |
| 713 | The Clubhouse | PUjT9QjDQte2iIIyLtE_yA |
| 714 | Rec Room | xJ4meCDRSVOFeVc0PvJBWg |
| 716 | Fire Station | SRfjyS2JQIuWp_Y8tqddgg |
| 717 | Police Station | 1erXNiTNS--opt5UHdK3LA |
| 314 | Media/Kitchen ✅ DONE | 6Q076YEDRmSmo4qp1G3Bug |

---

**Created:** 2026-03-01
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
