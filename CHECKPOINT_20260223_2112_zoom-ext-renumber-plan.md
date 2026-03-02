# Checkpoint: KCLC Zoom Phone — Extension Renumbering Analysis

**Created:** 2026-02-23 21:12:37 EST
**Session:** KCLC-IMPL-20260223-007-CLDE-zoom-provisioning
**Model:** Claude Sonnet 4.6
**Status:** 🚧 Blocked — 3 open questions before execution
**Cutover Deadline:** Feb 28, 2026

---

## Inherited Context

### System Facts
- **Zoom app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Account-level | Activated ✅
- **Account model:** SINGLE ACCOUNT — TFH + KCLC share @thefathershouse.com
- **KCLC extension range:** 703–717 (current, needs renumbering)
- **Credentials:** Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **MCP server:** `~/.local/mcp-servers/zoom-mcp-server/`
- **ZTP URL:** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}`
- **Emergency address (all):** 2301 South St, Leesburg, FL 34748 ✅
- **DID +13522680205:** PENDING port, unassigned — needs routing decision after port completes
- **Unassigned DID:** +13527820706 — sitting in account, no owner

### Tanya Thompson Correction (NEW - THIS SESSION)
- Tanya (tanya@thefathershouse.com) is **TFH staff, NOT KCLC**
- She was incorrectly given ext 703 (a KCLC room slot)
- **Action needed:** Move Tanya to a TFH extension (open TFH slots: 231, 234, 236–238, 242–244)
- 703 should become available for Sweet Shop in the renumbering

### Kids City (kclc@thefathershouse.com) — ext 704
- This IS an KCLC admin account — may be correct
- But in the 2→7 renumbering, 704 is the target slot for Ice Cream Shop
- **Action needed:** Decision on where Kids City account extension goes (700? 699? stays at 704?)

---

## The 14 Physical Devices (sipXcom phones.csv)

| sipXcom Ext | Room Name | MAC |
|---|---|---|
| 200 | Bus Station | 0004f27baa59 |
| 201 | Toy Shop | 0004f2dd7e86 |
| 202 | Pet Shop | 0004f2887947 |
| 203 | Sweet Shop | 0004f2de4e79 |
| 204 | Ice Cream Shop | 0004f2c9dedf *(verify vs 0004f27a7b32)* |
| 205 | KCLC Administrator | 0004f2de46b8 |
| 206 | Sport Store | 0004f2ddfe05 |
| 207 | Music Store | 0004f2822926 |
| 208 | Flower Shop | 0004f2c6a847 |
| 209 | The Market | 0004f268033e |
| 210 | The Paint Shop | 64167f909af0 |
| 211 | Media/Kitchen | 64167f31a96d |
| 212 | The Clubhouse | 64167f33f295 |
| 213 | Resource Room | 0004f2de4b0e |

*Excluded: Warren's VVX500 (0004f28334a8) + duplicate ICS device (0004f27a7b32)*

---

## Proposed 2XX → 7XX Extension Renumbering

| sipXcom Ext | Room | Zoom Current Ext | Zoom Current Name | **Target 7XX** | DID | Action |
|---|---|---|---|---|---|---|
| 200 | Bus Station | NONE | — | **700** | TBD | CREATE |
| 201 | Toy Shop | 709 | Toy Shop | **701** | +13527820672 | RENUMBER |
| 202 | Pet Shop | 705 | Pet Shop | **702** | +13527820599 | RENUMBER |
| 203 | Sweet Shop | 706 | Sweet shop | **703** | +13527820654 | RENUMBER (after Tanya moves) |
| 204 | Ice Cream Shop | 708 | Ice cream shop | **704** | +13527820455 | RENUMBER (after Kids City moves) |
| 205 | KCLC Administrator | NONE | — | **705** | TBD | CREATE |
| 206 | Sport Store | 707 | Sports store | **706** | +13527820675 | RENUMBER |
| 207 | Music Store | 710 | Music Store | **707** | +13527820695 | RENUMBER |
| 208 | Flower Shop | 712 | Flower Shop | **708** | +13527820550 | RENUMBER |
| 209 | The Market | NONE | — | **709** | TBD (or +13527820706?) | CREATE |
| 210 | The Paint Shop | NONE | — | **710** | TBD | CREATE |
| 211 | Media/Kitchen | 715 | Media Center | **711** | +13527820626 | RENUMBER |
| 212 | The Clubhouse | 713 | Club house | **712** | +13527820631 | RENUMBER |
| 213 | Resource Room | 714 | The Rec Room | **713** | +13527820566 | RENUMBER |
| ? | Fire Station | 716 | Fire Station | **714** | +13527820635 | MAC NEEDED |
| ? | Police Station | 717 | Police Station | **715** | +13527820614 | MAC NEEDED |

---

## Fire Station / Police Station — KEY OPEN ISSUE

- Neither Fire Station nor Police Station appear in sipXcom phones.csv or users.md
- sipXcom users only go to ext 213 (Resource Room), then Voicemail (214) and Unused (215/216)
- Zoom already has them at 716/717 with DIDs and emergency addresses ✅
- Warren confirmed physical phones exist in those rooms
- **They are NOT part of the original 14-device sipXcom migration**
- **QUESTION:** Are these 2 additional phones beyond the 14? Or do they replace 2 of the listed rooms?
  - If additional: total becomes 16 Zoom common area phones (14 original + 2 new)
  - If replacements: 2 of the 14 sipXcom rooms WON'T need a Zoom common area phone
- **ACTION NEEDED:** Warren must physically identify the Fire Station and Police Station devices and provide MACs

---

## Zoom Current Extension Inventory (Full)

**TFH User Extensions (DO NOT TOUCH without TFH coordination):**
202, 230, 232, 233, 235, 239, 240, 241, 245, 246, 282

**KCLC User Extensions (need correction):**
- 703 — Tanya Thompson (TFH, not KCLC — MOVE to TFH range)
- 704 — Kids City / kclc@ (KCLC admin — move to accommodate Ice Cream Shop)

**KCLC Common Area Phones (current, need renumbering):**
705 Pet Shop, 706 Sweet shop, 707 Sports store, 708 Ice cream shop,
709 Toy Shop, 710 Music Store, 712 Flower Shop, 713 Club house,
714 The Rec Room, 715 Media Center, 716 Fire Station, 717 Police Station

---

## 3 Open Questions Before Any Changes

**Q1 — Fire Station / Police Station scope:**
Are they part of the 14 (replacing 2 other rooms) or additional (making it 16)? Need MACs.

**Q2 — Tanya extension:**
Which TFH extension should she move to? Open slots: 231, 234, 236, 237, 238, 242, 243, 244.

**Q3 — Kids City extension:**
Where should the kclc@ admin account live in the new numbering? Options: 700 (before rooms start), separate range, or stays at 704 with Ice Cream Shop exception.

---

## What's Ready to Execute (Once Questions Answered)

1. Move Tanya Thompson to TFH extension
2. Move Kids City to resolved extension
3. Create 4 missing common area phones (Bus Station, KCLC Administrator, The Market, The Paint Shop)
4. Renumber existing 10 common area phones (701→701, 705→702, 706→703, etc.)
5. Register all 14 MACs to their Zoom common area phone accounts
6. Test ZTP: reboot one phone, confirm auto-config
7. Route +13522680205 once port completes

---

## sipXcom Security Investigation (NEW — Separate Topic)

Warren reported:
- ~1 week ago: all phones lost registration (no inbound/outbound) — required server reboot to resolve
- Cause suspected: SIP scan/brute-force attack traffic causing system instability
- Call detail records show weekend spike in failed SIP INVITE attempts from external IPs
- Possible fail2ban or SIP intrusion detection already present — needs inspection

**NOT yet investigated** — needs SSH access to sipXcom server (no entry in ~/.ssh/config).
Need: IP address, port, auth method (root SSH or key-based).

---

## Key Files

- `Claude/phones.csv` — 14 physical devices with MACs and extensions
- `Claude/zoom-map.csv` — current sipXcom ↔ Zoom mapping
- `Claude/DISCOVERY_RESULTS_20260223.md` — full discovery output
- `docs/users.md` — sipXcom user table (201–216, note: 211=Mobile/Wireless, not room phone)
- `docs/phones.md` — sipXcom phone device table (all 16 including excluded ones)

---

**Created:** 2026-02-23 21:12:37 EST
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
