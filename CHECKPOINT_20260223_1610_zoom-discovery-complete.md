# Checkpoint: KCLC Zoom Phone — Discovery Complete

**Created:** 2026-02-23 16:10:40 EST
**Session:** KCLC-IMPL-20260223-006-CLDE-zoom-discovery-run
**Model:** Claude Sonnet 4.6
**Status:** ✅ Discovery Done — Warren must answer 5 questions before provisioning
**Cutover Deadline:** Feb 28, 2026

---

## Summary

Full Zoom API discovery run via MCP. KCLC is already substantially set up in Zoom — 9 of 14 rooms confirmed with extensions and DIDs. 3 rooms need to be created. 2 items need Warren's decision. All 14 physical devices still need MAC registration.

---

## Inherited Context

### System Facts

- **Zoom app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Account-level | Activated ✅
- **Account model:** SINGLE ACCOUNT (confirmed) — TFH + KCLC share one account, @thefathershouse.com domain
- **Extension-to-extension calling:** WORKS — TFH can call KCLC and vice versa by extension
- **KCLC extension range:** 703–717 (users + common area phones)
- **TFH extension range:** 202, 230–246, 282 (personal users)
- **Credentials:** Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **MCP server:** `~/.local/mcp-servers/zoom-mcp-server/`
- **Account ID:** VOUi3C_NSjK1JNc6a1kWpQ
- **ZTP provisioning URL:** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}`
- **Emergency address (all):** 2301 South St, Leesburg, FL 34748 ✅

### Key KCLC Users in Zoom
- **Tanya Thompson** — tanya@thefathershouse.com — ext 703 — DID +13524601259
- **Kids City** — kclc@thefathershouse.com — ext 704 — no DID

### sipXcom → Zoom Mapping (confirmed)
| SIP Ext | Room | Zoom Ext | DID | MAC |
|---------|------|----------|-----|-----|
| 201 | Toy Shop | 709 | +13527820672 | 0004f2dd7e86 |
| 202 | Pet Shop | 705 | +13527820599 | 0004f2887947 |
| 203 | Sweet Shop | 706 | +13527820654 | 0004f2de4e79 |
| 204 | Ice Cream Shop | 708 | +13527820455 | VERIFY: 0004f2c9dedf vs 0004f27a7b32 |
| 206 | Sport Store | 707 | +13527820675 | 0004f2ddfe05 |
| 207 | Music Store | 710 | +13527820695 | 0004f2822926 |
| 208 | Flower Shop | 712 | +13527820550 | 0004f2c6a847 |
| 211 | Media/Kitchen | 715 | +13527820626 | 64167f31a96d |
| 212 | The Clubhouse | 713 | +13527820631 | 64167f33f295 |

### DID +13522680205 (KCLC main)
Status: **PENDING** in Zoom — present but unassigned. After port completes, assign to KCLC auto-receptionist or hunt group. Decision needed from Warren on routing.

### Active Constraints
- **Cutover Feb 28, 2026** — hard deadline (5 days)
- **NEVER** use `git add .` or `git add -A` in WarrenTEK-Operations main repo
- git status is slow in this repo due to assume-unchanged workaround

---

## Completed Work — DO NOT REDO

- [x] Zoom MCP connected — all tools working ✅
- [x] Full list_phone_users pulled (13 users, all @thefathershouse.com = single account confirmed)
- [x] Full list_phone_numbers pulled (26 numbers, 9 KCLC room DIDs already assigned)
- [x] list_users pulled (14 total Zoom users — Warren is admin, Stephen O'Neal is owner)
- [x] sipXcom phones.csv cross-referenced with Zoom API data
- [x] 9 of 14 rooms confirmed matched to Zoom common area phones
- [x] 3 rooms identified as needing creation: Bus Station, The Market, The Paint Shop
- [x] 2 items flagged for Warren's clarification: ext 205, ext 213
- [x] zoom-map.csv updated with all confirmed mappings and MACs
- [x] DISCOVERY_RESULTS_20260223.md created (full SOP output)

---

## 5 Questions for Warren (ALL NEEDED BEFORE PROVISIONING)

**Q1 — ext 201 identity:**
phones.csv says ext 201 = "Toy Shop" physical phone. Zoom ext 709 = "Toy Shop" (confirmed match). But zoom-map (from users.csv) said ext 201 = "Front Desk". Is the front desk function handled by Tanya Thompson (ext 703)? Where should DID +13522680205 route?

**Q2 — ext 205 Administrator:**
Physical VVX400 (MAC 0004f2de46b8) is at ext 205 "KCLC Administrator". Should this device register to:
- A: Tanya Thompson (ext 703 — her personal user account)
- B: Kids City (ext 704 — the kclc@thefathershouse.com account)
- C: New common area phone account for an "Admin" room

**Q3 — ext 213 Resource Room vs Zoom "The Rec Room" (ext 714):**
Are these the same room? If yes → register MAC 0004f2de4b0e to Zoom ext 714.

**Q4 — ext 204 Ice Cream Shop active MAC:**
Two physical devices share ext 204. Which is the ACTIVE one?
- 0004f2c9dedf (VVX410)
- 0004f27a7b32 (VVX410)

**Q5 — Rooms Fire Station (716) + Police Station (717):**
These exist in Zoom with DIDs but have no sipXcom counterpart in the 14-device list.
Do physical VVX phones exist at KCLC for these rooms? If yes, they should be provisioned too (beyond the original 14-device scope). If no, they stay as Zoom-only softphone rooms.

---

## Next Steps (After Warren Answers Questions)

1. Create 3 missing common area phones: Bus Station, The Market, The Paint Shop
2. Register all 14 MACs in Zoom Phone → Phones & Devices
3. Assign DID +13522680205 to KCLC auto-receptionist or hunt group
4. Verify device registration by rebooting one test phone (ZTP should auto-configure)
5. Test ext-to-ext calls: TFH → KCLC and KCLC → TFH
6. Test DID +13522680205 inbound routing
7. Plan cutover: coordinate with KCLC staff (brief window to reboot all phones)

---

## Blockers

| Item | Owner | Urgency |
|------|-------|---------|
| Q1–Q5 above | Warren to answer | HIGH — cutover Feb 28 |
| DID +13522680205 port completion | Zoom/carrier | Must complete before cutover |
| License count for common area phones | Warren to check Admin Portal | HIGH |

---

## Key Files

- `Claude/DISCOVERY_RESULTS_20260223.md` — full discovery SOP output
- `Claude/zoom-map.csv` — updated with all confirmed Zoom extensions and MACs
- `Claude/phones.csv` — 14 physical devices with MACs
- `Claude/SOP-Zoom-Discovery.md` — discovery runbook (completed)

---

**Created:** 2026-02-23
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
