# Next Session Handoff: KCLC SIPXCOM → Zoom Phone Migration

**Prepared:** 2026-02-23 09:23 EST
**Next Session Type:** KCLC Zoom Phone Migration Planning & Investigation
**Recommended Model:** Claude Haiku 4.5 (cost optimization for data gathering)
**Timeline:** Cutover end of week (Feb 28), go live next week (Mar 3+)

---

## Session Objectives

Prepare and execute migration of Kids City Learning Center (KCLC) phone system from SIPXCOM (self-hosted) to Zoom Phone (cloud-based, integrated with Fathers House Church ministry account).

### Phase 1: Investigation & Stabilization (This Session)
- [ ] Connect to SIPXCOM server via SSH (credentials to be provided)
- [ ] Investigate stability issue discovered this morning
- [ ] Document current SIPXCOM configuration & phone numbers
- [ ] Assess scope of migration

### Phase 2: Planning & Preparation (This/Next Session)
- [ ] Map SIPXCOM users/extensions/groups to Zoom Phone structure
- [ ] Design Zoom Phone configuration to replicate SIPXCOM setup
- [ ] Document phone number allocation and routing rules
- [ ] Create migration checklist and rollback plan

### Phase 3: Implementation (Next Week)
- [ ] Provision Zoom Phone users and extensions
- [ ] Configure call routing and IVR
- [ ] Test cutover scenario
- [ ] Schedule cutover for end of week
- [ ] Go live beginning of next week

---

## Context: KCLC Ministry Background

**Organization:** Kids City Learning Center (KCLC)
- Ministry of Fathers House Church (FHC)
- Currently using self-hosted SIPXCOM VoIP
- Warren donating phone services to KCLC

**Why Migrate:**
- Consolidate phone systems: FHC uses Zoom Phone
- Reduce maintenance overhead (cloud-managed)
- Unified billing/administration
- Better integration with FHC infrastructure

**Service:** Phone system for Kids City Learning Center (KCLC phone numbers and extensions)

---

## SIPXCOM Server Details (To Be Provided)

**Credentials to request from Warren:**
- [ ] SSH hostname/IP address
- [ ] SSH username & password (or key)
- [ ] SIPXCOM admin credentials
- [ ] SIPXCOM version number
- [ ] Description of stability issue discovered this morning

**Store credentials in:** Strongbox under `/VoIP` or similar category

---

## Current SIPXCOM Setup (To Be Documented)

Gather and document:
- [ ] SIPXCOM version (exact build number)
- [ ] Active users/extensions
- [ ] Phone numbers (DID, main, extensions)
- [ ] Call routing rules
- [ ] Voicemail configuration
- [ ] IVR/auto-attendant setup
- [ ] Stability issue details + root cause
- [ ] Current uptime/reliability metrics

---

## Zoom Phone Integration Context

**Parent Account:** Fathers House Church (FHC) Zoom Phone account
- Admin credentials available (stored in Strongbox)
- KCLC will be sub-account or tenant within FHC account

**Zoom Phone Features to Replicate:**
- User extensions (from SIPXCOM users)
- Call groups (if used in SIPXCOM)
- IVR menus
- Voicemail-to-email
- Call recording (if enabled in SIPXCOM)

---

## Project Reference

**Existing Project:** `~/projects/Zoom-Phone-Migration-KCLC`
- Check for existing checkpoints/documentation
- Check for any prior investigation or notes

---

## Key Dates (Hard Deadlines)

| Milestone | Date | Status |
|-----------|------|--------|
| Investigation complete | 2026-02-24 | In progress |
| Zoom Phone design finalized | 2026-02-25 | Planning |
| User provisioning complete | 2026-02-27 | Pending |
| Cutover testing | 2026-02-27 | Pending |
| **CUTOVER** | 2026-02-28 (end of week) | **HARD DATE** |
| Go live on Zoom Phone | 2026-03-03+ | **GO LIVE DATE** |

---

## Session Handoff Notes

1. **Stability Issue:** Warren discovered a stability problem this morning on SIPXCOM — investigate as priority before migration work
2. **SSH Access:** Get connection details + test SSH access immediately
3. **Documentation:** Every detail from SIPXCOM should be captured (screenshots, exports if possible)
4. **Risk:** This is a ministry service — downtime not acceptable; plan rollback carefully
5. **Timeline:** Tight schedule (1 week); prioritize investigation + design phases

---

## Links & References

- **Checkpoint:** [Previous: OpenClaw Nginx Fix](../clawdbot-vps/CHECKPOINT_20260223_0923_Hostinger-Nginx-Gateway-Port-Fix.md)
- **Project Folder:** `~/projects/Zoom-Phone-Migration-KCLC/`
- **FHC Zoom Phone Creds:** Strongbox → `/VoIP` (check with Warren if not found)
- **KCLC Contact:** Warren Wiemer (wwiemer@warrentek.com)

---

**Ready to Start:** Yes ✅
**Blocking Issues:** None (awaiting SIPXCOM credentials from Warren)
**Recommended First Action:** Request SSH & SIPXCOM version from Warren, then connect and document stability issue
