# KCLC Zoom Pre-ZTP Configuration Plan

**Created:** 2026-03-04 01:02:49 EST
**Session:** KCLC-FEAT-20260304-013-SNNT-ztp-provisioning-and-zoom-registration
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Status:** ACTIVE — IN PROGRESS

---

## Overview

Phase 2 (5.9.8 firmware) is complete on all 10 remotely accessible phones + the KCLC Admin phone (11 total). ZTP was validated on ext 314 (Media-Kitchen) — it provisioned and came online at firmware 6.4.7.4710. However, the phone landed on **Main Site** instead of the KCLC site.

**Blocker:** All KCLC common area phones are currently numbered in the 700 range (705–717) and assigned to Main Site. They must be renumbered to 300-series and assigned to the KCLC site BEFORE the remaining 9 phones are ZTP provisioned — otherwise every phone will land on the wrong site with the wrong extension number.

---

## Current State vs Target State

### Phone Users (Zoom User Accounts)

| Name | Email | Current Ext | Target Ext | Site | Action |
|------|-------|-------------|------------|------|--------|
| Kids City | kclc@thefathershouse.com | **704** | **304** | Main Site → KCLC | Renumber + move site |
| Tanya | tanya@thefathershouse.com | 703 | 703 | Main Site → KCLC | Move site only |
| Stephen O'Neal | stephen@thefathershouse.com | 202 | 202 | Main Site | No change (TFH) |
| Anita Mahan | anita@thefathershouse.com | 230 | 230 | Main Site | No change (TFH) |
| Andria Roberts | aroberts@thefathershouse.com | 232 | 232 | Main Site | No change (TFH) |
| Simone Baker | simone@thefathershouse.com | 233 | 233 | Main Site | No change (TFH) |
| Elyse Pettway | nextsteps@thefathershouse.com | 235 | 235 | Main Site | No change (TFH) |
| Maggie Dowis | MAGGIE@THEFATHERSHOUSE.COM | 239 | 239 | Main Site | No change (TFH) |
| Timothy Travis | pastortim@thefathershouse.com | 240 | 240 | Main Site | No change (TFH) |
| Chris Nation | chris@thefathershouse.com | 241 | 241 | Main Site | No change (TFH) |
| Front Desk - Main TFH | frontdesk@thefathershouse.com | 245 | 245 | Main Site | No change (TFH) |
| Lisa Humphreys | lisa@thefathershouse.com | 246 | 246 | Main Site | No change (TFH) |
| Raymond Ramos | rramos@thefathershouse.com | 282 | 282 | Main Site | No change (TFH) |

### Common Area Phones (KCLC Rooms)

| Room | Current Ext | Target Ext | DID | MAC | Site | Action |
|------|-------------|------------|-----|-----|------|--------|
| Bus Station | *(none)* | **300** | +13522680205 / +13527820706 | 0004f27baa59 | — | **CREATE** + assign DIDs |
| Toy Shop | 709 | **301** | +13527820672 | 0004f2dd7e86 | Main Site → KCLC | Renumber + move + MAC |
| Pet Shop | 705 | **302** | +13527820599 | 0004f2887947 | Main Site → KCLC | Renumber + move + MAC |
| Sweet Shop | 706 | **303** | +13527820654 | 0004f2de4e79 | Main Site → KCLC | Renumber + move + MAC |
| KCLC Admin | *(user 704)* | **304** | *(none — internal)* | 0004f2de46b8 | Main Site → KCLC | Renumber user 704→304 + move |
| Sport Store | 707 | **305** | +13527820675 | 0004f2ddfe05 | Main Site → KCLC | Renumber + move + MAC |
| Ice Cream Shop | 708 | **306** | +13527820455 | 0004f27a7b32 | Main Site → KCLC | Renumber + move + MAC |
| Music Store | 710 | **307** | +13527820695 | 0004f2822926 | Main Site → KCLC | Renumber + move + MAC |
| Flower Shop | 712 | **308** | +13527820550 | 0004f2c6a847 | Main Site → KCLC | Renumber + move + MAC |
| Police Station | 717 | **309** | +13527820614 | 0004f268033e | Main Site → KCLC | Renumber + move + MAC |
| Fire Station | 716 | **310** | +13527820635 | 64167f909af0 | Main Site → KCLC | Renumber + move + MAC |
| *(RESERVED)* | — | **311** | — | — | — | Do not assign (N11 service code) |
| The Clubhouse | 713 | **312** | +13527820631 | 64167f33f295 | Main Site → KCLC | Renumber + move + MAC |
| Rec Room | 714 | **313** | +13527820566 | 0004f2de4b0e | Main Site → KCLC | Renumber + move + MAC |
| Media/Kitchen | 715→**314** ✅ | **314** ✅ | +13527820626 | 64167f31a96d | Main Site → KCLC | ✅ ZTP done — move site only |

---

## Pre-ZTP Checklist (Zoom Admin — Do In This Order)

### STEP 1 — Create KCLC Site

> ⚠️ Must be done BEFORE any renumbering or phone assignments.

**Where:** Zoom Admin Portal → Phone System → Sites → Add Site

| Field | Value |
|-------|-------|
| Site Name | Kid City Learning Center |
| Short Name | KCLC |
| Emergency Address | 2301 South St, Leesburg, FL 34748 |
| Extension Range | 300–399 |
| Country | United States |
| Time Zone | America/New_York |

**After creating site:** Note the Site ID (needed for MCP API calls).

---

### STEP 2 — Move Tanya (703) to KCLC Site

> Tanya Thompson is KCLC staff — should be on KCLC site.

**Where:** Zoom Admin → Phone System → Users → Tanya → Edit → Site → Kid City Learning Center

No extension number change needed (703 stays).

---

### STEP 3 — Renumber Kids City (704 → 304) + Move to KCLC Site

**Where:** Zoom Admin → Phone System → Users → Kids City → Edit

| Field | Change |
|-------|--------|
| Extension | 704 → **304** |
| Site | Main Site → **Kid City Learning Center** |

---

### STEP 4 — Create Common Area Phone: Bus Station (Ext 300)

> ⚠️ This is a NEW common area phone — does not exist yet.

**Where:** Zoom Admin → Phone System → Common Area Phones → Add

| Field | Value |
|-------|-------|
| Display Name | Bus Station |
| Extension | 300 |
| Site | Kid City Learning Center |
| Emergency Address | 2301 South St, Leesburg, FL 34748 |

**After creating:** Assign both DIDs:
- +13522680205 (main inbound line — currently PENDING port)
- +13527820706 (secondary)

**MAC Registration:** Will be completed via ZTP when on-site (10.1.12.182 is currently offline).

---

### STEP 5 — Renumber + Move All Existing Common Area Phones (700 → 300 Series)

> Do these 12 changes in Zoom Admin → Phone System → Common Area Phones.
> For each: Edit → change Extension → change Site to Kid City Learning Center → Save.

| # | Room | Old Ext | New Ext | DID | Notes |
|---|------|---------|---------|-----|-------|
| 1 | Toy Shop | 709 | **301** | +13527820672 | |
| 2 | Pet Shop | 705 | **302** | +13527820599 | |
| 3 | Sweet Shop | 706 | **303** | +13527820654 | |
| 4 | Sport Store | 707 | **305** | +13527820675 | |
| 5 | Ice Cream Shop | 708 | **306** | +13527820455 | Confirm MAC = 0004f27a7b32 |
| 6 | Music Store | 710 | **307** | +13527820695 | |
| 7 | Flower Shop | 712 | **308** | +13527820550 | |
| 8 | Police Station | 717 | **309** | +13527820614 | Was labeled "The Market" in sipXcom |
| 9 | Fire Station | 716 | **310** | +13527820635 | |
| 10 | The Clubhouse | 713 | **312** | +13527820631 | |
| 11 | Rec Room | 714 | **313** | +13527820566 | |
| 12 | Media/Kitchen | **314** ✅ | **314** ✅ | +13527820626 | ZTP done — site change only |

> ⚠️ Ext 311 is intentionally skipped — N11 service code, cannot be assigned.

---

### STEP 6 — Move Media/Kitchen (314) from Main Site to KCLC Site

> This phone is already ZTP'd and online. Only the site assignment is wrong.

**Where:** Zoom Admin → Phone System → Common Area Phones → Media-Kitchen → Edit → Site → Kid City Learning Center

---

### STEP 7 — Verify KCLC Admin Phone (Ext 304)

Confirm the KCLC Admin phone (10.1.10.176, MAC 0004f2de46b8, ext 304) is ready for ZTP:
- Firmware: 5.9.8.5760 ✅ (already confirmed)
- Extension 304 exists in Zoom ✅ (after Step 3)
- Site: KCLC ✅ (after Step 3)

---

## ZTP Batch Process (After All Zoom Steps Above Complete)

### ZTP Order of Operations

> Phones go live as soon as provisioning completes (~5–10 min each).
> Can be done sequentially: set provisioning URL → reboot → move to next phone while previous configures.

| # | Room | IP | Model | Zoom Ext | ZTP URL Suffix | Priority |
|---|------|----|----|---------|---------------|----------|
| 1 | Media/Kitchen | ✅ ZTP done | VVX 401 | 314 | VVX401 | ✅ COMPLETE |
| 2 | KCLC Admin | 10.1.10.176 | VVX 400 | 304 | VVX400 | Do first (different VLAN) |
| 3 | Toy Shop | 10.1.12.185 | VVX 400 | 301 | VVX400 | |
| 4 | Sweet Shop | 10.1.12.180 | VVX 400 | 303 | VVX400 | |
| 5 | Sport Store | 10.1.12.183 | VVX 400 | 305 | VVX400 | |
| 6 | Ice Cream Shop | 10.1.12.65 | VVX 410 | 306 | VVX400 | |
| 7 | Music Store | 10.1.12.186 | VVX 400 | 307 | VVX400 | |
| 8 | Flower Shop | 10.1.12.190 | VVX 400 | 308 | VVX400 | |
| 9 | Police Station | 10.1.12.191 | VVX 400 | 309 | VVX400 | |
| 10 | The Clubhouse | 10.1.12.187 | VVX 401 | 312 | VVX401 | |
| 11 | Rec Room | 10.1.12.181 | VVX 400 | 313 | VVX400 | |

**ZTP Provisioning URL by model:**

| Model | URL |
|-------|-----|
| VVX 400 / VVX 410 | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| VVX 401 | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401` |

**ZTP Steps (per phone):**
1. Web UI → Settings → Advanced (password: **2307**)
2. Network Configuration → Provisioning Server
3. Server Type: **HTTPS**
4. Server Address: `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` (no https://)
5. Save → Reboot
6. Wait ~5–10 min → verify Online in Zoom dashboard
7. Zoom will auto-upgrade firmware to 6.4.7.4710 during provisioning

**What to expect after ZTP:**
- Web UI access locked out (Zoom takes control) ✅ normal
- Phone unreachable by ping briefly during factory reset ✅ normal
- Firmware upgrades from 5.9.8.5760 → 6.4.7.4710 ✅ confirmed from ext 314

---

## On-Site Required Phones

These three phones cannot be completed remotely:

| Room | IP | Model | Zoom Ext | Issue | Action |
|------|----|-------|----------|-------|--------|
| Bus Station | 10.1.12.182 | VVX 400 | 300 | Offline — Edge E220 temp in use | ZTP on-site: 5.5.2 → Phase 1 → Phase 2 → ZTP |
| Fire Station | 10.1.12.64 | VVX 410 | 310 | Unknown admin password post-factory-reset | Physical recovery → TFTP cfg already set for 5.9.8 direct → ZTP |
| Pet Shop | 10.1.12.68 | VVX 410 | 302 | Firmware 5.0.1 — needs intermediate 5.4.x step | 5.0.1 → 5.4.x → 5.7.3 → ZTP (researching 5.4.x firmware) |

**Pet Shop upgrade path detail:**
```
5.0.1.4068 → 5.4.x (need split sip.ld) → 5.7.3 (already on TFTP) → ZTP (Zoom pushes 6.4.7)
```
- MAC: `0004f2887947` → cfg file: `C:\tftp\0004f2887947.cfg`
- 5.4.x firmware file needed: `3111-46162-001-5.4.x.sip.ld` (VVX 410, split)
- Once at 5.7.3: update cfg to point to 5.9.8, reboot, then ZTP

---

## Complete Phone Inventory (All 14)

| # | Room | IP | Model | MAC (TFTP) | sipXcom Ext | Zoom Ext | DID | FW Status | ZTP Status |
|---|------|----|-------|-----------|-------------|----------|-----|-----------|-----------|
| 1 | Bus Station | 10.1.12.182 | VVX 400 | 0004f27baa59 | 200 | 300 | +13522680205 / +13527820706 | 5.5.2 | ⏳ On-site |
| 2 | Toy Shop | 10.1.12.185 | VVX 400 | 0004f2dd7e86 | 201 | 301 | +13527820672 | ✅ 5.9.8 | ⏳ Ready |
| 3 | Pet Shop | 10.1.12.68 | VVX 410 | 0004f2887947 | 202 | 302 | +13527820599 | ❌ 5.0.1 | ⏳ On-site |
| 4 | Sweet Shop | 10.1.12.180 | VVX 400 | 0004f2de4e79 | 203 | 303 | +13527820654 | ✅ 5.9.8 | ⏳ Ready |
| 5 | KCLC Admin | 10.1.10.176 | VVX 400 | 0004f2de46b8 | 204 | 304 | *(internal)* | ✅ 5.9.8 | ⏳ Ready |
| 6 | Sport Store | 10.1.12.183 | VVX 400 | 0004f2ddfe05 | 205 | 305 | +13527820675 | ✅ 5.9.8 | ⏳ Ready |
| 7 | Ice Cream Shop | 10.1.12.65 | VVX 410 | 0004f27a7b32 | 206 | 306 | +13527820455 | ✅ 5.9.8 | ⏳ Ready |
| 8 | Music Store | 10.1.12.186 | VVX 400 | 0004f2822926 | 207 | 307 | +13527820695 | ✅ 5.9.8 | ⏳ Ready |
| 9 | Flower Shop | 10.1.12.190 | VVX 400 | 0004f2c6a847 | 208 | 308 | +13527820550 | ✅ 5.9.8 | ⏳ Ready |
| 10 | Police Station | 10.1.12.191 | VVX 400 | 0004f268033e | 209 | 309 | +13527820614 | ✅ 5.9.8 | ⏳ Ready |
| 11 | Fire Station | 10.1.12.64 | VVX 410 | 64167f909af0 | 210 | 310 | +13527820635 | ❓ Unknown | ⏳ On-site |
| 12 | The Clubhouse | 10.1.12.187 | VVX 401 | 64167f33f295 | 212 | 312 | +13527820631 | ✅ 5.9.8 | ⏳ Ready |
| 13 | Rec Room | 10.1.12.181 | VVX 400 | 0004f2de4b0e | 213 | 313 | +13527820566 | ✅ 5.9.8 | ⏳ Ready |
| 14 | Media/Kitchen | 10.1.12.189 | VVX 401 | 64167f31a96d | 211 | 314 | +13527820626 | ✅ 6.4.7 | ✅ ZTP done (wrong site) |

---

## Summary: What Needs to Happen Before ZTP Batch

```
[ ] Step 1 — Create KCLC site in Zoom Admin
[ ] Step 2 — Move Tanya (703) to KCLC site
[ ] Step 3 — Renumber Kids City 704 → 304 + move to KCLC site
[ ] Step 4 — Create Bus Station (ext 300) common area phone on KCLC site
[ ] Step 5 — Renumber all 12 common area phones from 700-series → 300-series + move to KCLC site
[ ] Step 6 — Move Media/Kitchen (314) from Main Site → KCLC site
[ ] Step 7 — Verify KCLC Admin (304) ready for ZTP
[ ] ZTP — Run through 10 remote phones in batch
[ ] On-site — Bus Station, Fire Station, Pet Shop
```

---

## Open Questions / Decisions Needed

1. **Ext 300 (Bus Station) — two DIDs:** Does +13522680205 (main) route to ext 300 as the primary inbound? Or is it a hunt group / auto-attendant? This affects whether +13522680205 should be assigned directly to the phone or to a call queue.

2. **Tanya (703) — stays as phone user or becomes common area?** Currently a user account (703). Should stay as user account (has a personal DID +13524601259). Just needs site changed.

3. **KCLC Admin (304) — Kids City user account or common area phone?** Currently "Kids City" user account (kclc@thefathershouse.com). This is a shared login/admin account, not a personal user. Leaving as phone user is fine — just renumber to 304.

4. **Site name** — Confirm: "Kid City Learning Center" (matches KCLC branding) or "KCLC"?

---

**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-04 01:02:49 EST
**Status:** ACTIVE
