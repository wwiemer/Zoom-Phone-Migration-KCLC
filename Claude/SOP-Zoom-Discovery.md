# Zoom Phone Discovery SOP
## KCLC Migration — Run When MCP is Live

**Created:** 2026-02-23
**Status:** READY TO RUN (waiting on OAuth credentials)
**Prerequisites:** zoom-mcp-server connected with valid credentials

---

## Pre-Discovery Checklist

- [ ] zoom-mcp-server credentials updated in all 3 MCP configs
- [ ] Claude Code restarted (MCP server loaded)
- [ ] Zoom MCP tools visible (`list_phone_users`, `list_phone_numbers`, etc.)

---

## Step 1: Account Structure Discovery

**Goal:** Determine if KCLC is already set up, and what account model is in use.

**Query 1 — List all phone users:**
```
Use list_phone_users to get all users. Page size: 100. Note total count.
```
**What to look for:**
- Are there users with 700-series extensions? → KCLC partial setup
- Are there users with 200-series extensions? → TFH users
- How many total phone users exist?
- What email domains are used? (warrentek.com? thefathershouse.com? kclc.org?)

**Query 2 — Check for KCLC-specific users:**
```
Filter list_phone_users or search for users with KCLC-related email or 700-series extensions
```
**Document:** List of all KCLC users found (name, email, current Zoom extension)

---

## Step 2: Extension Namespace Discovery

**Goal:** Determine the extension numbering scheme in Zoom vs. sipXcom.

**What to document:**
- TFH extension range: _____ (e.g., 100-199, 200-299?)
- KCLC extension range already assigned: _____ (e.g., 700-799?)
- Overlap risk with KCLC sipXcom 200-213 range?
- Extension digits: 3-digit or 4-digit?

**If KCLC is in 700-series:**
- Build the mapping table: sipXcom ext → Zoom ext
  - 200 (Bus Station) → 7XX?
  - 201 (Front Desk) → 7XX?
  - etc.

---

## Step 3: DID / Phone Number Discovery

**Goal:** Verify DID +1-352-268-0205 is imported and assigned correctly.

**Query:**
```
Use list_phone_numbers to list all phone numbers. Look for +13522680205.
```
**What to document:**
- Is +13522680205 present in Zoom? (Y/N)
- If yes: what is it currently assigned to? (user? auto-receptionist? unassigned?)
- Are there other KCLC DIDs present?

---

## Step 4: Sites / Account Model Discovery

**Goal:** Confirm account structure (single account + sites vs. other).

**What to check in Zoom Admin Portal:**
- Admin → Phone System Management → Company Info → Sites
- Is there a "KCLC" site defined?
- Is there a "TFH" site defined?
- Are site codes configured?

**If single account + two sites:**
- Site code for TFH: _____
- Site code for KCLC: _____
- Can KCLC users reach TFH via [site code]+[extension]?

---

## Step 5: Hunt Group / Call Queue Discovery

**Goal:** Check if any hunt groups are already configured for KCLC.

**What to look for in Admin Portal:**
- Phone System Management → Call Queues
- Phone System Management → Auto Receptionists
- Any existing KCLC routing?
- Where does +13522680205 currently ring?

---

## Step 6: Device Inventory Discovery

**Goal:** Check if any Polycom devices are already registered in Zoom.

**What to check:**
- Admin → Phone System Management → Phones & Devices
- Filter by KCLC or look for the known MACs
- Are any of the 14 devices already provisioned?

---

## Step 7: License Audit

**Goal:** Confirm sufficient Zoom Phone licenses for 14 KCLC users.

**What to check in Admin Portal:**
- Admin → Dashboard or Billing → Licenses
- How many Zoom Phone (Basic/Pro) licenses are available/unused?
- Are KCLC users already consuming licenses?
- Do we need to purchase more?

---

## Step 8: Voicemail Configuration Review

**Goal:** Understand current voicemail setup and plan for KCLC.

**What to check:**
- Do any existing KCLC users have voicemail enabled?
- Is voicemail-to-email configured? What email address?
- Note: No pilot number (ext 250) needed — users dial *86

---

## Discovery Output Document

After completing all steps, fill in this summary:

```
ZOOM ACCOUNT DISCOVERY RESULTS
Date: ___________
Performed by: Warren Wiemer

Account Model:        [ ] Single acct + sites  [ ] Sub-account  [ ] Linked org
KCLC already set up:  [ ] Yes (partial)  [ ] No
KCLC extension range: _________ (in Zoom)
TFH extension range:  _________
DID +13522680205:     [ ] Present  [ ] Missing — needs import
DID assigned to:      _________
Hunt group exists:    [ ] Yes  [ ] No
Devices registered:   _____ of 14 already in Zoom
Licenses available:   _____
Site codes:           TFH=___  KCLC=___

KCLC Users Already in Zoom:
| Zoom Ext | Name/Room | Email | SIP Ext Equivalent |
|----------|-----------|-------|--------------------|
|          |           |       |                    |

Outstanding items before provisioning can begin:
1. ___________
2. ___________
3. ___________
```

---

## Post-Discovery Next Steps

Based on discovery results:

**If KCLC already partially set up (700-series):**
- Reconcile existing users against 14-device list
- Fill gaps (missing users, unassigned devices)
- Confirm extension mapping (700-series ↔ sipXcom 200-series)

**If KCLC not yet set up:**
- Create 14 Zoom Phone users (need real email addresses from KCLC)
- Assign extensions (confirm range with Warren)
- Import DID if missing
- Register 14 device MACs (verify MACs for ext 210/211/212 first)

**Always before provisioning:**
- Confirm Ice Cream Shop active MAC (0004f2c9dedf vs 0004f27a7b32)
- Verify MAC format for ext 210/211/212 (64167f vs 6416f7)
- Obtain real email addresses for all 14 users
