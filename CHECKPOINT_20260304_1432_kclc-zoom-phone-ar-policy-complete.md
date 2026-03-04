# CHECKPOINT — KCLC Zoom Phone Migration
**Date:** 2026-03-04 14:32:00 EST
**Session:** KCLC-FEAT-20260304-014-CLDE-zoom-phone-ar-policy-complete
**Status:** PAUSED — Switching to Mac Mini M2, resuming same session

---

## Inherited Context (v3.0)

- **All 14 KCLC phones confirmed online** at firmware 5.9.8, ZTP complete
- **KCLC Site ID:** `dUxbXwstSW6rvziP3tF1xA`
- **Main KCLC DID:** +13522680205, number ID: `U40FFnIeQX2CLASBRyRe2g`
- **DID currently assigned to:** Bus Station ext 300 (WRONG — needs to move to AR)
- **E911 confirmed correct:** 2307 South St, Leesburg FL 34748 on all phones
- **Zoom MCP server:** 18 tools now active (was 8); all new tools committed & pushed
- **OAuth scopes:** Warren selected all phone scopes via "select all" master in Marketplace
- **OAuth token refresh:** Needed — restart Claude Code on Mac Mini will handle this automatically
- **MacBook→Mac Mini path alias:** `/Users/wwiemer/` symlinks to `/Users/warrenwiemer/` — no config changes needed
- **Kids City zombie:** ext 799 at kclc@thefathershouse.com — US/CA Unlimited wasted license, delete via portal
- **Common area phones:** 14 phones (ext 300–314), all on KCLC site, no Zoom user accounts

---

## KCLC Phone Reference Table

| Ext | Role | Common Area ID | DID? |
|-----|------|----------------|------|
| 300 | Bus Station | (from list_common_area_phones) | +13522680205 → MOVE TO AR |
| 304 | KCLC Administrator | (from list_common_area_phones) | None — needs one |
| 301-303, 305-314 | Classrooms | (from list_common_area_phones) | Release DIDs if any |
| 799 | Kids City zombie (user acct) | N/A | Delete |

---

## Current Blockers & Status

### 1. Outbound Caller ID — ALL 14 Phones
- **Problem:** +13522680205 assigned to ext 300 (CAP), not the AR → phones can't use it as outbound ID
- **Solution (confirmed):** Move DID from ext 300 → KCLC Auto Receptionist
  - Site Outbound Caller ID page: "Auto Receptionists Numbers (For all users and Common Areas)" ✅ checked
  - Once DID is on AR, ALL KCLC phones can use it; batch-set via API
- **Portal action needed:** Phone System → Auto Receptionists → KCLC-AR → assign +13522680205; remove from ext 300
- **After portal action:** Run batch API update via MCP to set all 14 phones

### 2. Paging Group — "KCLC Classrooms" (ext 390)
- **Members:** ext 301–310, 312–314 (13 classroom phones)
- **Blocker:** API returns "No endpoint" — Cloud Paging feature likely not enabled
- **Portal action needed:** Zoom Admin → Phone System Management → Settings → Features → enable Cloud Paging
- **After enable:** `create_paging_group` + `add_paging_group_members` via MCP

### 3. Intercom Group — All 14 Phones
- **Same blocker as paging:** Intercom feature likely not enabled
- **Portal action needed:** Same settings page → enable Intercom
- **After enable:** `create_intercom_group` + `add_intercom_group_members` via MCP

### 4. Auto Receptionist Setup
- **KCLC-AR** exists but needs full configuration
- **Cannot activate until after 5:30 PM tonight** (live site)
- **Staging plan:** Build call queue, configure AR routing, voicemail → don't flip DID until after 5:30
- **Questions for Dorothy (call tomorrow):** See Dorothy email draft below

### 5. Business Hours
- Set under: Zoom Admin → Phone System → Sites → KidCity Learning Center → Hours
- **Unknown:** Actual hours — need from Dorothy

### 6. Voicemail → Email
- Target: kclc@thefathershouse.com
- Configured on AR/call queue after-hours routing

---

## DID Policy Decision (Warren's Call)

**Recommended architecture:**
- ext 300 (Bus Station): Keep DID — transportation coordinator needs direct line
- ext 304 (KCLC Admin): Assign a DID — director needs direct line
- ext 301–303, 305–314 (Classrooms): NO DID — present main KCLC number only; releasing DIDs back to pool
- Main KCLC number: +13522680205 lives on Auto Receptionist

**Outbound policy for classrooms:** TBD — ask Dorothy (unrestricted vs. emergency-only vs. whitelist)

---

## Tonight's Staging Plan (Before 5:30 PM)

1. Move +13522680205 from ext 300 → KCLC Auto Receptionist (portal)
2. Enable Cloud Paging + Intercom (Zoom Admin → Phone System → Settings → Features)
3. Build Call Queue "KCLC Main" — members: 300, 304, maybe 301–305 (portal)
4. Configure AR routing → call queue during hours; voicemail after hours (portal)
5. Set voicemail notification → kclc@thefathershouse.com (portal)
6. **After 5:30:** Flip DID from ext 300 to AR, go live
7. Batch-set outbound caller ID on all 14 phones via MCP API
8. Create paging group via MCP
9. Create intercom group via MCP

---

## Dorothy Email Draft

**Subject:** KidCity Learning Center Phone System — A Few Questions Before We Go Live

Hi Dorothy,

We're in the final stages of setting up your new phone system. A few quick questions:

1. **Business Hours** — Days/times you're open (open time, close time, any half-days)?
2. **After-Hours Calls** — When someone calls after hours: voicemail? Forwarded to a cell? Message with hours?
3. **Voicemail Notifications** — Email address for voicemail-to-email? Backup contact?
4. **Classroom Phone Outbound Policy** — Should classroom phones call any number, or restricted to 911/Poison Control/emergency numbers only?
5. **Main Number Routing** — During hours: ring one desk first, ring multiple simultaneously, or auto-attendant menu?
6. **Auto Attendant Greeting** — What would you like the greeting to say?

Thanks, Warren

---

## MCP Tools Status

| Tool | Status | Notes |
|------|--------|-------|
| list_common_area_phones | ✅ Working | Returns all 14 KCLC phones |
| get_common_area_phone | ✅ Working | Confirmed ext 304 has no DID |
| update_common_area_phone | ✅ Working (limited) | Can set outbound_caller_id only if number accessible |
| list_phone_numbers | ✅ Working | Confirmed +13522680205 ID |
| list_phone_devices | ✅ Working | All 14 devices confirmed |
| list_paging_groups | ❌ "No endpoint" | Need to enable Cloud Paging feature |
| create_paging_group | ❌ Untested | Same blocker |
| add_paging_group_members | ❌ Untested | Same blocker |
| list_intercom_groups | ❌ "No endpoint" | Need to enable Intercom feature |
| create_intercom_group | ❌ Untested | Same blocker |
| add_intercom_group_members | ❌ Untested | Same blocker |
| list_provision_templates | ✅ Working | Templates listed |

---

## Next Steps (Immediate — On Mac Mini)

1. Verify OAuth token has new scopes (test a call after restart)
2. Follow tonight's staging plan above
3. After Warren does portal actions, run batch outbound caller ID update via MCP
4. Create paging + intercom groups via MCP (after feature enable)
5. Send Dorothy email
6. Replace Edge E220 on ext 300 with original VVX (MAC 0004F27BAA59) — portal ZTP

---

**Checkpoint Version:** 3.0
**Next Session Model Recommendation:** Sonnet (standard config work + API calls)
