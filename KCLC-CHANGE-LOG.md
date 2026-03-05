# KCLC Zoom Phone Migration — Change Log
**Project:** KidCity Learning Center Zoom Phone Migration
**Site:** 2307 South St, Leesburg FL 34748
**Main DID:** +13522680205
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC
**Created:** 2026-03-05
**Status:** IN PROGRESS

---

## Purpose

This log provides a complete transactional history of all changes made to the KCLC phone system during and after the Zoom Phone migration. All configuration decisions are documented with requester, source of authority, and date.

---

## Change History

---

### [2026-02-23 through 2026-03-02] Phase 1 — Discovery & Migration

**What was done:**
- Full discovery of sipXcom 19.04 system (ucs05.warrentek.net)
- Extracted: extension table, hunt group configuration, business hours, IVR/auto attendant config, paging group, intercom config
- Identified 14 active Polycom VVX phones with confirmed MAC addresses
- Extension renumbering plan: sipXcom 200–214 → Zoom 300–314 (+100 offset)
- Firmware upgrade: all 14 phones updated to 5.9.8 via TFTP
- ZTP provisioning: all 14 phones registered to Zoom via Zero Touch Provisioning
- Port of +13522680205 completed: 2026-03-02 at 11:30 AM ET (Twilio → Zoom)
- E911 address set on all devices: 2307 South St, Leesburg FL 34748

**Authority:** Warren Wiemer (WarrenTEK), system owner

---

### [2026-03-04] Phase 2 — Post-Migration Configuration

**Session:** KCLC-FEAT-20260304-014-CLDE

**Changes:**
- Confirmed all 14 phones online at firmware 5.9.8
- DID +13522680205 confirmed assigned to ext 300 (Bus Station) — pending move to Auto Receptionist
- Identified outbound caller ID blocker: DID must be on Auto Receptionist, not ext 300
- Identified Cloud Paging and Intercom features not yet enabled
- Drafted Dorothy email to confirm business hours, routing, and outbound policy

---

### [2026-03-05] Phase 3 — Dorothy Confirmation & Name Corrections

#### 3a. Business Hours Established
**Requested by:** Dorothy Matz, Executive Director, KidCity Learning Center
**Via:** Email received 2026-03-05 (approx. 3:45 PM ET)
**Decision:** Monday–Friday 6:30 AM – 5:30 PM ET | Saturday–Sunday: closed (after-hours)

*Note: sipXcom extraction showed 9:00 AM – 6:00 PM. Dorothy confirmed actual operational hours are 6:30 AM – 5:30 PM. Parent drop-off begins at 6:30 AM.*

---

#### 3b. Inbound Call Routing During Business Hours
**Requested by:** Dorothy Matz, Executive Director
**Via:** Email 2026-03-05
**Decision:** Ring **Bus Station (ext 300)** and **Admin Office (ext 304)** simultaneously

*Note: Old sipXcom hunt group KCLC-MAIN-HG-01 (ext 902) rang ext 200/201/202/203/204 simultaneously for 24 seconds. Dorothy's request reduces the ring group to 2 phones: Bus Station + Admin Office only.*

---

#### 3c. After-Hours Routing
**Requested by:** Dorothy Matz, Executive Director
**Via:** Email 2026-03-05
**Decision:** Voicemail to kclc@thefathershouse.com | Simple voicemail greeting

**After-hours greeting (approved):**
> "You have reached KidCity Learning Center. Our office is currently closed. Our business hours are Monday through Friday, 6:30 AM to 5:30 PM. Please leave a message and we will return your call the next business day. Thank you and God bless."

---

#### 3d. Auto Attendant Greeting (During Hours)
**Requested by:** Dorothy Matz, Executive Director
**Via:** Email 2026-03-05
**Decision:** Text-to-speech greeting

**During-hours greeting (approved):**
> "Thank you for calling KidCity Learning Center. Please hold while we connect your call."

---

#### 3e. Classroom Phone Outbound Policy
**Requested by:** Dorothy Matz, Executive Director
**Via:** Email 2026-03-05
**Decision:** Unrestricted outbound calling on all classroom phones

---

#### 3f. Extension 311 — Reserved (Cannot Use)
**Issue identified by:** Warren Wiemer / WarrenTEK
**Communicated to:** Dorothy Matz via email 2026-03-05
**Reason:** Extension 311 is a reserved N11 service code in Zoom Phone (same restriction category as 911, 411). Zoom does not permit 311 as a user extension number.
**Resolution:** Media Center phone assigned to **ext 314** instead.
**Dorothy notified:** Yes — email sent 2026-03-05 with corrected extension list.

---

#### 3g. Extension Display Names — Corrected Per Dorothy's List
**Requested by:** Dorothy Matz, Executive Director
**Via:** Email 2026-03-05 (attached corrected extension list)
**Applied by:** Warren Wiemer via Zoom Phone API
**Date applied:** 2026-03-05
**API used:** PATCH /phone/common_areas/{id} (new Common Area API, Common Area Optimization enabled)

| Ext | Old Name | New Name | Changed? |
|-----|----------|----------|----------|
| 300 | Bus Station | Bus Station | No change |
| 301 | Toy Shop | Toy Shop | No change |
| 302 | Pet Shop | Pet Shop | No change |
| 303 | Sweet Shop | Sweet Shop | No change |
| 304 | KCLC Administrator | **Admin Office** | ✅ Updated |
| 305 | Sports Store | Sports Store | No change (already correct) |
| 306 | Ice Cream Shop | Ice Cream Shop | No change |
| 307 | Music Store | Music Store | No change |
| 308 | Flower Shop | Flower Shop | No change |
| 309 | Police Station | Police Station | No change |
| 310 | Fire Station | Fire Station | No change |
| 311 | N/A | N/A | Reserved — cannot use |
| 312 | Club House | **The Clubhouse** | ✅ Updated |
| 313 | The Rec Room | **Rec. Room** | ✅ Updated |
| 314 | Media/Kitchen | **Media Center** | ✅ Updated |

---

#### 3h. Device Display Names — Synchronized to Match Common Area Names
**Reason:** Device records maintained a separate display_name field that did not auto-sync when common area names were updated.
**Applied by:** Warren Wiemer via Zoom Phone API
**Date applied:** 2026-03-05
**API used:** PATCH /phone/devices/{id}

| Device ID | Old Name | New Name |
|-----------|----------|----------|
| LxDYWMfPRZGxLd0iph2I8w | KCLC Administrator | Admin Office |
| pVQnKeJUQ2qs0XIgICZU4g | Club House | The Clubhouse |
| 9DBa7oNCTfKqP1tSyNkssg | Rec Room | Rec. Room |
| GncNffzEQUm3tpOUhDa89w | Media-Kitchen | Media Center |

---

## Pending Changes (Not Yet Applied)

| Item | Status | Dependency |
|------|--------|------------|
| Enable Cloud Paging feature | Pending | Warren — portal action |
| Enable Intercom feature | Pending | Warren — portal action |
| Configure KCLC Auto Receptionist (greetings, hours, routing) | Pending | Warren — portal action |
| Assign temp DID (+13527820706) to AR for testing | Pending | AR must be configured first |
| Test AR from external line | Pending | Temp DID on AR |
| Move +13522680205 from ext 300 → AR (go live) | Pending | Testing must pass |
| Batch set outbound caller ID — all 14 phones → +13522680205 | Pending | DID must be on AR first |
| Create call queue: 300 + 304 simultaneous, 24s, voicemail → kclc@thefathershouse.com | Pending | AR must exist |
| Create paging group ext 390 (classrooms 301-310, 312-314) | Pending | Cloud Paging enabled |
| Create intercom group (all 14 phones) | Pending | Intercom enabled |
| Delete Kids City zombie account (ext 799, kclc@thefathershouse.com user) | Pending | After migration complete |
| Send go-live notification to Dorothy with test instructions | Pending | After go-live |

---

## Reference

- **Dorothy Matz:** Executive Director, KidCity Learning Center — kclc@thefathershouse.com
- **KCLC Site ID (Zoom):** dUxbXwstSW6rvziP3tF1xA
- **Main DID number ID:** U40FFnIeQX2CLASBRyRe2g
- **Source of truth for extension table:** MASTER-EXTENSION-REFERENCE-20260301.md
- **sipXcom discovery:** docs/ folder and CHECKPOINT_20260224_123501_sipxcom-extraction-backups-complete.md

---

**Last Updated:** 2026-03-05
