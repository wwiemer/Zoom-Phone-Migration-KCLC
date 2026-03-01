# Checkpoint: Zoom Discovery Complete — Ready to Execute

**Created:** 2026-03-01
**Session:** KCLC-IMPL-20260301-001-CLDE-zoom-discovery-and-planning
**Model:** Claude Sonnet 4.6
**Status:** In Progress — Awaiting Warren go-ahead to execute

---

## Summary

Complete read-only discovery of all 14 KCLC extensions across sipXcom and Zoom. All MACs verified from live sipXcom registration page (14/14 active). Full execution plan locked in MASTER-EXTENSION-REFERENCE-20260301.md. Zoom pre-migration backup written. Port of 352-268-0205 is scheduled for 2026-03-02 at 11:30 AM ET — Twilio forward to Zoom must be set TONIGHT before port completes.

---

## Inherited Context

### System Facts

- **sipXcom server:** ucs05.warrentek.net | CentOS 6.10 | sipXcom 19.04
- **sipXcom SSH:** `sshpass -p 'wbC7PmX5YckaKa9N' ssh root@ucs05.warrentek.net`
- **sipXcom DB:** `psql -U postgres SIPXCONFIG` (uppercase — "sipxconfig" fails)
- **KCLC public IP (fail2ban whitelisted):** 72.166.63.114
- **Zoom account:** Single account — TFH + KCLC share @thefathershouse.com
- **Zoom MCP app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Credentials in Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **ZTP provisioning URL:** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}`
- **Emergency address (all devices):** 2301 South St, Leesburg, FL 34748
- **Port completing:** 2026-03-02 at 11:30 AM ET — number +13522680205

### Active Constraints

- **NO CHANGES to Zoom or sipXcom until Warren gives explicit go-ahead**
- **DO NOT TOUCH TFH extensions:** 202 (Stephen O'Neal), 703 (Tanya), 230, 232, 233, 235, 239, 240, 241, 245, 246, 282
- **3XX range is completely clean** in Zoom — no conflicts. All KCLC moves go 2XX → 3XX.
- **KCLC Administrator = kclc@thefathershouse.com** — this is a USER account (ext 704 → 304), NOT a common area phone
- **ext 204 IP 10.1.10.176** is correct (different VLAN from 10.1.12.x — confirmed acceptable by Warren)
- **Ignore MAC 0004F2C9DEDF** — Ice Cream Shop duplicate, did NOT register in live check; active MAC is 0004F27A7B32

### Key Decisions

- **Extension numbering:** sipXcom 2XX → Zoom 3XX (+100 offset). Simple, clean, no conflicts.
- **Bus Station (ext 300) is the main inbound number target.** Twilio forward tonight → +13527820706 (Bus Station's current Zoom DID). After port: +13522680205 assigned directly to ext 300.
- **Inbound call queue replication:** Create Zoom Call Queue with members 300, 301, 302, 303, 304 (simultaneous ring, 24s), voicemail fallback to ext 304 (kclc@thefathershouse.com)
- **Pre-migration backup completed:** ZOOM-BACKUP-PRE-MIGRATION-20260301.json

---

## Completed Work — DO NOT REDO

- [x] Read all prior checkpoints and source files (phones.csv, zoom-map.csv, users.md)
- [x] Pulled live Zoom data via MCP: 13 phone users, 26 phone numbers, 12 common area phones
- [x] Corrected extension assignment errors from prior docs (204=KCLC Admin, 205=Sport Store, 206=Ice Cream Shop)
- [x] Verified active MACs from live sipXcom registration screenshot (14/14 active, 2026-03-01 17:34 ET)
- [x] Identified correct Ice Cream Shop active MAC: 0004F27A7B32 (not 0004F2C9DEDF)
- [x] Confirmed ext 209 = Police Station (MAC 0004F268033E), ext 210 = Fire Station (MAC 64167F909AF0)
- [x] Identified Kids City (kclc@thefathershouse.com, ext 704) = KCLC Administrator
- [x] Queried sipXcom live DB for inbound call routing: hunt group KCLC-MAIN-HG-01, ext 902, rings 200/201/202/203/204 simultaneously (24s), voicemail → ext 250 → kclc@thefathershouse.com
- [x] Created MASTER-EXTENSION-REFERENCE-20260301.md — all 14 extensions locked and verified
- [x] Created ZOOM-BACKUP-PRE-MIGRATION-20260301.json — complete Zoom state snapshot

---

## The 14-Extension Migration Table (Quick Reference)

| # | sipXcom | Room | → Zoom | Active MAC | Action |
|---|---|---|---|---|---|
| 1 | 200 | Bus Station | **300** | 0004F27BAA59 | CREATE + DID +13527820706 |
| 2 | 201 | Toy Shop | **301** | 0004F2DD7E86 | Renumber 709→301 |
| 3 | 202 | Pet Shop | **302** | 0004F2887947 | Renumber 705→302 |
| 4 | 203 | Sweet Shop | **303** | 0004F2DE4E79 | Renumber 706→303 + fix name |
| 5 | 204 | KCLC Administrator | **304** | 0004F2DE46B8 | Renumber USER 704→304, rename |
| 6 | 205 | Sport Store | **305** | 0004F2DDFE05 | Renumber 707→305 + fix name |
| 7 | 206 | Ice Cream Shop | **306** | 0004F27A7B32 | Renumber 708→306 + fix name |
| 8 | 207 | Music Store | **307** | 0004F2822926 | Renumber 710→307 |
| 9 | 208 | Flower Shop | **308** | 0004F2C6A847 | Renumber 712→308 |
| 10 | 209 | Police Station | **309** | 0004F268033E | Renumber 717→309 |
| 11 | 210 | Fire Station | **310** | 64167F909AF0 | Renumber 716→310 |
| 12 | 211 | Media/Kitchen | **311** | 64167F31A96D | Renumber 715→311 + fix name |
| 13 | 212 | The Clubhouse | **312** | 64167F33F295 | Renumber 713→312 + fix name |
| 14 | 213 | Resource Room | **313** | 0004F2DE4B0E | Renumber 714→313 + fix name |

---

## Blockers & Lessons

| Item | Detail |
|------|--------|
| sipXcom DB name case | Must use uppercase "SIPXCONFIG" — lowercase fails with "database does not exist" |
| Ice Cream Shop dual MAC | Two MACs registered to ext 206 in sipXcom. Live registration confirms 0004F27A7B32 active; 0004F2C9DEDF inactive (ignore) |
| sipXcom room name errors | ext 209 labeled "The Market", ext 210 "The Paint Shop" in sipXcom — both wrong. Warren confirmed actual room names. |
| 7XX range avoided | Stephen had temp configs in 700-series. Switched to 3XX — completely clean range. |

---

## Ideas & Discoveries

- After migration, sipXcom decommission can be planned — ext 215 (Lisa Humphreys) also listed but is NOT part of this migration scope
- sipXcom security remediation (Steps 1-4) remains pending — deprioritized until Zoom migration complete
- Zoom Call Queue should replicate the sipXcom simultaneous ring behavior exactly (not sequential)

---

## Next Steps — Execution Order

**AWAIT Warren's go-ahead, then execute in this order:**

1. **TONIGHT (before port window):**
   - Set Twilio forward: +13522680205 → +13527820706
   - Create ext 300 (Bus Station) common area phone, assign DID +13527820706, register MAC 0004F27BAA59
   - Renumber 12 existing common area phones 7XX → 3XX (with name fixes)
   - Renumber Kids City user 704 → 304, rename to "KCLC Administrator", register MAC 0004F2DE46B8
   - Register all 14 MACs to respective Zoom 3XX extensions via ZTP
   - Create Zoom Call Queue: members 300/301/302/303/304, simultaneous 24s, voicemail → 304

2. **2026-03-02 after 11:30 AM ET (post-port):**
   - Assign +13522680205 as DID to ext 300 in Zoom
   - Remove Twilio forward

3. **Later (separate session):**
   - sipXcom security remediation (Steps 1-4 from REMEDIATION-PLAN-20260223.md)

---

## Key Files

| File | Purpose |
|------|---------|
| `MASTER-EXTENSION-REFERENCE-20260301.md` | **LOCKED** execution reference — all 14 extensions |
| `ZOOM-BACKUP-PRE-MIGRATION-20260301.json` | Pre-migration Zoom state snapshot |
| `sipxcom-security/REMEDIATION-PLAN-20260223.md` | sipXcom security fix (deferred) |
| `Claude/phones.csv` | Original 14 sipXcom devices with MACs |

---

## Git Commit Details

**Branch:** main
**Files committed this session:**
- `MASTER-EXTENSION-REFERENCE-20260301.md` — locked 14-extension migration table
- `ZOOM-BACKUP-PRE-MIGRATION-20260301.json` — pre-migration Zoom state backup
- `CHECKPOINT_20260301_1832_zoom-discovery-complete-ready-to-execute.md` — this file

---

**Created:** 2026-03-01
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
