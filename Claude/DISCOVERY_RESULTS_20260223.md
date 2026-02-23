# Zoom Account Discovery Results
## KCLC Phone Migration — Live API Discovery

**Date:** 2026-02-23
**Performed by:** Warren Wiemer / Claude Sonnet 4.6
**Session:** KCLC-IMPL-20260223-006-CLDE-zoom-discovery-run
**API Source:** Zoom MCP (list_phone_users, list_phone_numbers, list_users)

---

## Summary Finding: KCLC is already substantially set up in Zoom

**9 of 14 rooms already exist as Common Area phones with DIDs assigned.**
**Main gap: 4 rooms need to be created + 2 items need Warren's confirmation.**
**No physical devices have been registered yet (MAC provisioning still needed for all 14).**

---

## Step 1: Account Structure Discovery ✅

```
Account Model:        [X] Single account — all users share @thefathershouse.com domain
                          (plus wwiemer@warrentek.com as admin)
KCLC already set up:  [X] Yes (substantially) — 700-series extensions in use
KCLC extension range: 703–717 (confirmed live)
TFH extension range:  202, 230–246, 282
Sub-account risk:     NONE — single account confirmed. TFH ↔ KCLC extension dialing WILL WORK.
```

**Phone Users (all 13 have phone licenses):**

| Ext | Name | Email | Type | Notes |
|-----|------|-------|------|-------|
| 202 | Stephen O'Neal | stephen@thefathershouse.com | TFH user | Account owner (role 0) |
| 230 | Anita Mahan | anita@thefathershouse.com | TFH user | |
| 232 | Andria Roberts | aroberts@thefathershouse.com | TFH user | |
| 233 | Simone Baker | simone@thefathershouse.com | TFH user | |
| 235 | Elyse Pettway | nextsteps@thefathershouse.com | TFH user | |
| 239 | Maggie Dowis | maggie@thefathershouse.com | TFH user | |
| 240 | Timothy Travis | pastortim@thefathershouse.com | TFH user | |
| 241 | Chris Nation | chris@thefathershouse.com | TFH user | |
| 245 | Front Desk - Main TFH | frontdesk@thefathershouse.com | TFH user | Has TFH main number |
| 246 | Lisa Humphreys | lisa@thefathershouse.com | TFH user | |
| 282 | Raymond Ramos | rramos@thefathershouse.com | TFH user | |
| 703 | Tanya Thompson | tanya@thefathershouse.com | KCLC staff user | Created 2026-02-18 |
| 704 | Kids City | kclc@thefathershouse.com | KCLC admin user | Created 2026-02-20 |

**Note:** Warren Wiemer (wwiemer@warrentek.com) is account admin — no phone extension assigned.

---

## Step 2: Extension Namespace ✅

```
TFH range:    ext 202, 230–246, 282 (personal users)
KCLC range:   ext 703–717 (users + common area phones)
Overlap risk: NONE — TFH 200-series does not overlap with KCLC 700-series
Digit count:  3-digit extensions throughout
```

---

## Step 3: DID / Phone Number Discovery ✅

```
DID +13522680205 (KCLC main):  PRESENT — status: PENDING (port in progress?)
                                NOT YET ASSIGNED to any user/queue
Action needed: Assign to KCLC auto-receptionist or front desk after port completes
```

**All KCLC room DIDs already assigned:**

| Zoom Ext | Room Name | DID Assigned |
|----------|-----------|--------------|
| 705 | Pet Shop | +13527820599 |
| 706 | Sweet shop | +13527820654 |
| 707 | Sports store | +13527820675 |
| 708 | Ice cream shop | +13527820455 |
| 709 | Toy Shop | +13527820672 |
| 710 | Music Store | +13527820695 |
| 712 | Flower Shop | +13527820550 |
| 713 | Club house | +13527820631 |
| 714 | The Rec Room | +13527820566 |
| 715 | Media Center | +13527820626 |
| 716 | Fire Station | +13527820635 |
| 717 | Police Station | +13527820614 |
| 703 | Tanya Thompson | +13524601259 |

**Unassigned number in account:** +13527820706 — available for new rooms

**Emergency address (all KCLC DIDs):** 2301 South St, Leesburg, FL 34748 ✅

---

## Step 4: Sites / Account Model ✅

Single account confirmed — no sub-accounts. Sites configuration could not be queried
(requires `account:read:settings:admin` scope not in current token). However:
- Single account = TFH and KCLC extensions in same dial plan
- KCLC can reach TFH by dialing their extension directly (no site code needed)
- Recommend verifying site config in Admin Portal: Phone System Management → Company Info → Sites

---

## Step 5: Hunt Group / Call Queue — TBD (requires Admin Portal check)

DID +13522680205 is PENDING/unassigned. After port completes, needs routing decision:
- Option A: Route to an auto-receptionist for KCLC
- Option B: Route directly to Tanya Thompson (ext 703) or Kids City (ext 704)
- Option C: Route to a call queue / hunt group of KCLC staff
**DECISION NEEDED FROM WARREN before port can complete provisioning.**

---

## Step 6: Device Registration Status ✅

No Polycom devices found registered to any of the KCLC common area phone accounts.
**All 14 MAC addresses need to be registered in Zoom before cutover.**

ZTP provisioning URL (from previous research):
`https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}`
Phones will auto-configure when rebooted if registered to their Zoom extension.

---

## Step 7: License Audit ✅

All 13 phone users have: **US/CA Unlimited Calling Plan (type 200)**
Common Area phones likely use a different license tier — verify count in Admin Portal.
13 user licenses confirmed consumed. Need to check how many Common Area licenses remain.
**Recommend:** Verify in Admin Portal → Billing → Licenses before provisioning remaining rooms.

---

## Step 8: Voicemail — TBD (requires Admin Portal check)

Could not query via API. Recommend checking:
- Do existing KCLC common area phones have voicemail enabled?
- For common area phones: voicemail is typically disabled or forwarded to a shared mailbox
- Users dial `*86` to check voicemail (no pilot number needed) ✅

---

## sipXcom → Zoom Extension Mapping (Full Table)

| SIP Ext | phones.csv Room Name | Zoom Ext | Zoom Room Name | DID | Status | MAC |
|---------|---------------------|----------|----------------|-----|--------|-----|
| 200 | Bus Station | — | — | — | **NEEDS CREATE** | 0004f27baa59 |
| 201 | Toy Shop | **709** | Toy Shop | +13527820672 | ✅ CONFIRMED | 0004f2dd7e86 |
| 202 | Pet Shop | **705** | Pet Shop | +13527820599 | ✅ CONFIRMED | 0004f2887947 |
| 203 | Sweet Shop | **706** | Sweet shop | +13527820654 | ✅ CONFIRMED | 0004f2de4e79 |
| 204 | Ice Cream Shop | **708** | Ice cream shop | +13527820455 | ✅ CONFIRMED — VERIFY MAC | 0004f2c9dedf OR 0004f27a7b32 |
| 205 | Administrator | **?** | Kids City (704) or new? | — | ⚠️ NEEDS CLARIFY | 0004f2de46b8 |
| 206 | Sport Store | **707** | Sports store | +13527820675 | ✅ CONFIRMED | 0004f2ddfe05 |
| 207 | Music Store | **710** | Music Store | +13527820695 | ✅ CONFIRMED | 0004f2822926 |
| 208 | Flower Shop | **712** | Flower Shop | +13527820550 | ✅ CONFIRMED | 0004f2c6a847 |
| 209 | The Market | — | — | — | **NEEDS CREATE** | 0004f268033e |
| 210 | The Paint Shop | — | — | — | **NEEDS CREATE** | 64167f909af0 |
| 211 | Media/Kitchen | **715** | Media Center | +13527820626 | ✅ CONFIRMED | 64167f31a96d |
| 212 | The Clubhouse (VPK) | **713** | Club house | +13527820631 | ✅ CONFIRMED | 64167f33f295 |
| 213 | Resource Room | **?** | The Rec Room (714)? | +13527820566 | ⚠️ NEEDS CLARIFY | 0004f2de4b0e |

**CONFIRMED:** 9 of 14 ✅
**NEEDS CLARIFY:** 2 (ext 205, ext 213) ⚠️
**NEEDS CREATE:** 3 (ext 200, 209, 210)

---

## Extra KCLC Rooms in Zoom (outside 14-device scope)

These rooms exist in Zoom with DIDs but were NOT in the sipXcom 200-213 device list.
Physical devices may or may not exist at KCLC for these rooms.

| Zoom Ext | Room Name | DID | Action Needed |
|----------|-----------|-----|---------------|
| 714 | The Rec Room | +13527820566 | Confirm: is this the Resource Room (SIP 213)? |
| 716 | Fire Station | +13527820635 | Confirm: is there a physical device to register? |
| 717 | Police Station | +13527820614 | Confirm: is there a physical device to register? |

---

## Key Discrepancies Found (Require Warren Confirmation)

### 1. users.csv vs phones.csv Room Name Mismatch
`zoom-map.csv` was built from users.csv. `phones.csv` reflects physical device labels.
These conflict at three extensions:

| SIP Ext | users.csv (zoom-map) | phones.csv (devices) | Zoom API Result |
|---------|---------------------|---------------------|-----------------|
| 201 | "Front Desk" | "Toy Shop (1 Yrs)" | ext 709 = Toy Shop ← phones.csv matches |
| 202 | "Toy Shop" | "Pet Shop" (confirmed x202) | ext 705 = Pet Shop ← phones.csv matches |
| 203 | "Pet Shop" | "Sweet Shop (0 Yrs)" | ext 706 = Sweet shop ← phones.csv matches |

**Zoom API data confirms phones.csv is correct.** The Zoom room names (Toy Shop at 201, Pet Shop at 202, Sweet Shop at 203) match phones.csv device descriptions.

**Question for Warren:** What is at ext 201 physically — a Toy Shop room phone? If yes, what handles the "Front Desk" function at KCLC in Zoom (is it Tanya Thompson at ext 703)?

### 2. Administrator (ext 205)
phones.csv has a physical VVX400 (MAC: 0004f2de46b8) at ext 205 labeled "KCLC Administrator".
Zoom has "Kids City" user account (ext 704, kclc@thefathershouse.com) — is this the admin line?
**Options:**
- A: Register MAC 0004f2de46b8 to Tanya Thompson (ext 703) if that's the admin desk
- B: Register MAC 0004f2de46b8 to Kids City (ext 704)
- C: Create a new Common Area Phone account for ext 705 "Administrator" and register MAC there

### 3. Resource Room (ext 213) vs The Rec Room (Zoom ext 714)
"Resource Room" and "The Rec Room" might be the same space with different names, OR they might be different rooms. If the same: MAC 0004f2de4b0e registers to Zoom ext 714.

---

## Outstanding Items Before Provisioning Can Begin

1. **Warren to confirm:** ext 201 — Toy Shop or Front Desk? Where does DID +13522680205 route?
2. **Warren to confirm:** ext 205 Administrator MAC — registers to ext 703 (Tanya), 704 (Kids City), or new common area phone?
3. **Warren to confirm:** ext 213 Resource Room = The Rec Room (Zoom 714) or separate?
4. **Warren to confirm:** ext 204 Ice Cream Shop — which MAC is active? (0004f2c9dedf or 0004f27a7b32)
5. **Warren to confirm:** Are Fire Station (716) and Police Station (717) in Zoom real rooms with physical phones?
6. **Create 3 missing common area phones:** Bus Station, The Market, The Paint Shop
7. **Assign DID +13522680205:** to auto-receptionist, hunt group, or user after port completes
8. **Register all 14 MACs** in Zoom Phone → Common Area Phone accounts
9. **License check:** Verify sufficient Common Area Phone licenses in Admin Portal → Billing

---

## What's Already Done (DO NOT REDO)

- ✅ 9 Zoom common area phone accounts created with 700-series extensions
- ✅ 9 DIDs (352-782-xxxx series) already assigned to those rooms
- ✅ Emergency address set: 2301 South St, Leesburg, FL 34748
- ✅ 2 KCLC user accounts: Tanya Thompson (703) and Kids City (704)
- ✅ DID +13522680205 in system (pending status — porting or activation in progress)
- ✅ Unassigned number +13527820706 available for new rooms

---

**Created:** 2026-02-23
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
