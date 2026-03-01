# KCLC Zoom Phone Migration — Master Extension Reference

**Created:** 2026-03-01 17:55 EST
**Status:** LOCKED — All 14 extensions verified. Read-only discovery complete.
**Port Date:** 2026-03-02 at 11:30 AM ET
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC

---

## Source of Truth

- **sipXcom registration screenshot:** 2026-03-01 17:34 ET — 14/14 active ✅
- **Zoom live data pull:** 2026-03-01 17:42 ET
- **MAC addresses:** Confirmed from live sipXcom registration list

---

## Complete 14-Extension Migration Table

| # | sipXcom Ext | Room Name (Correct) | → Zoom Target | Active MAC | LAN IP | Zoom Current Ext | Zoom Current Name | DID |
|---|---|---|---|---|---|---|---|---|
| 1 | 200 | Bus Station | **300** | 0004F27BAA59 | 10.1.12.182 | *(create new)* | — | +13527820706 → +13522680205 *(post-port)* |
| 2 | 201 | Toy Shop | **301** | 0004F2DD7E86 | 10.1.12.185 | 709 | Toy Shop | +13527820672 |
| 3 | 202 | Pet Shop | **302** | 0004F2887947 | 10.1.12.68 | 705 | Pet Shop | +13527820599 |
| 4 | 203 | Sweet Shop | **303** | 0004F2DE4E79 | 10.1.12.180 | 706 | Sweet shop | +13527820654 |
| 5 | 204 | KCLC Administrator | **304** | 0004F2DE46B8 | 10.1.10.176 | 704 | Kids City | *(none)* |
| 6 | 205 | Sport Store | **305** | 0004F2DDFE05 | 10.1.12.183 | 707 | Sports store | +13527820675 |
| 7 | 206 | Ice Cream Shop | **306** | 0004F27A7B32 | 10.1.12.65 | 708 | Ice cream shop | +13527820455 |
| 8 | 207 | Music Store | **307** | 0004F2822926 | 10.1.12.186 | 710 | Music Store | +13527820695 |
| 9 | 208 | Flower Shop | **308** | 0004F2C6A847 | 10.1.12.190 | 712 | Flower Shop | +13527820550 |
| 10 | 209 | Police Station | **309** | 0004F268033E | 10.1.12.191 | 717 | Police Station | +13527820614 |
| 11 | 210 | Fire Station | **310** | 64167F909AF0 | 10.1.12.64 | 716 | Fire Station | +13527820635 |
| 12 | 211 | Media/Kitchen | **311** | 64167F31A96D | 10.1.12.189 | 715 | Media Center | +13527820626 |
| 13 | 212 | The Clubhouse | **312** | 64167F33F295 | 10.1.12.187 | 713 | Club house | +13527820631 |
| 14 | 213 | Resource Room | **313** | 0004F2DE4B0E | 10.1.12.181 | 714 | The Rec Room | +13527820566 |

---

## Actions Required Per Entry (Execution Checklist — DO NOT EXECUTE YET)

| Zoom Target | Room | Action Type | Detail |
|---|---|---|---|
| **300** | Bus Station | **CREATE** common area phone | No existing Zoom entry. Assign DID +13527820706. Register MAC 0004F27BAA59. |
| **301** | Toy Shop | RENUMBER + rename | 709 → 301. Name stays "Toy Shop". |
| **302** | Pet Shop | RENUMBER + rename | 705 → 302. Name stays "Pet Shop". |
| **303** | Sweet Shop | RENUMBER + rename | 706 → 303. Fix cap: "Sweet shop" → "Sweet Shop". |
| **304** | KCLC Administrator | RENUMBER user acct | 704 → 304. kclc@thefathershouse.com. Rename "Kids City" → "KCLC Administrator". Register MAC 0004F2DE46B8. |
| **305** | Sport Store | RENUMBER + rename | 707 → 305. Fix cap: "Sports store" → "Sport Store". |
| **306** | Ice Cream Shop | RENUMBER + rename | 708 → 306. Fix cap: "Ice cream shop" → "Ice Cream Shop". |
| **307** | Music Store | RENUMBER | 710 → 307. Name stays "Music Store". |
| **308** | Flower Shop | RENUMBER | 712 → 308. Name stays "Flower Shop". |
| **309** | Police Station | RENUMBER | 717 → 309. Name stays "Police Station". |
| **310** | Fire Station | RENUMBER | 716 → 310. Name stays "Fire Station". |
| **311** | Media/Kitchen | RENUMBER + rename | 715 → 311. Fix name: "Media Center" → "Media/Kitchen". |
| **312** | The Clubhouse | RENUMBER + rename | 713 → 312. Fix name: "Club house" → "The Clubhouse". |
| **313** | Resource Room | RENUMBER + rename | 714 → 313. Fix name: "The Rec Room" → "Resource Room". |

---

## DID / Number Routing Plan

### Main KCLC Number (352-268-0205)

| Phase | Action |
|---|---|
| **Tonight (before port)** | In Twilio: forward +13522680205 → +13527820706 (Zoom ext 300) |
| **Port completes (~11:30 AM 2026-03-02)** | +13522680205 goes live in Zoom. Assign as DID to ext 300. Remove Twilio forward. |

### Unassigned DID

| Number | Assign To |
|---|---|
| +13527820706 | Ext 300 — Bus Station (temp main number until port completes, then retained as secondary) |

### Rooms Without DIDs (internal only)

| Ext | Room |
|---|---|
| 304 | KCLC Administrator |

---

## Zoom Account Context

- **Account:** Single account — TFH + KCLC share @thefathershouse.com
- **Account owner:** Stephen O'Neal (ext 202, stephen@thefathershouse.com) — TFH
- **KCLC admin user:** kclc@thefathershouse.com (ext 704 → moves to 304)
- **300 range:** Completely clear in Zoom — no conflicts
- **ZTP provisioning URL:** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}`
- **Emergency address (all devices):** 2301 South St, Leesburg, FL 34748

---

## Zoom Users NOT Part of Migration (leave untouched)

| Ext | Name | Email | Note |
|---|---|---|---|
| 202 | Stephen O'Neal | stephen@thefathershouse.com | TFH account admin |
| 703 | Tanya | tanya@thefathershouse.com | TFH staff — lower priority cleanup |

---

## sipXcom Notes (Reference Only)

- **Server:** ucs05.warrentek.net | CentOS 6.10 | sipXcom 19.04
- **Inactive MAC (Ice Cream Shop duplicate):** 0004F2C9DEDF — did NOT register in 2026-03-01 live check. Ignore.
- **ext 204 IP subnet:** 10.1.10.176 — different VLAN from rest (10.1.12.x). Confirmed correct per Warren. No action needed.
- **sipXcom label discrepancies (wrong in system, correct names above):**
  - ext 209 labeled "The Market" in sipXcom → actual room: **Police Station**
  - ext 210 labeled "The Paint Shop" in sipXcom → actual room: **Fire Station**

---

**Created:** 2026-03-01
**Status:** LOCKED — awaiting Warren go-ahead to execute
