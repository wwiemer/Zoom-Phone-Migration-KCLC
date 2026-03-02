# Checkpoint: sipXcom Extraction & Backups Complete
**Session:** KCLC-IMPL-20260224-001-CLDE-sipxcom-security-remediation
**Date:** 2026-02-24 | 12:35 EST
**Status:** ✅ EXTRACTION & BACKUPS COMPLETE | Ready for evening remediation

---

## Completed Work (This Session)

### ✅ SIPXCOM Backups (Step 0 - COMPLETE)
- **All 4 backups executed** and verified: 2.6M total
  - iptables: 7,855 rules (419K)
  - fail2ban: Complete config (75K)
  - sipXcom: Entire config (1.4M)
  - Banned IPs: 5,926 entries (82K)
- **Location**: `/projects/Zoom-Phone-Migration-KCLC/sipxcom-security/backups/` (Dropbox synced)
- **Revert capability**: Confirmed (iptables-restore, tar extraction)

### ✅ SIPXCOM Configuration Extraction (COMPLETE)
**Operating Hours** (from attendant_working_hours table):
- Mon-Fri: 9:00 AM - 6:00 PM ET (enabled)
- Sat-Sun: Disabled (after-hours mode)
- Schedule ID: 7

**Auto Attendants**:
- ID 1: "After hours" (afterhours.wav)
- ID 2: "Operator" (autoattendant.wav)
- After-hours routing: → Extension 100 (live attendant, 20-sec ring)
- Voicemail: Extension 101

**Main DID Routing**:
- DID: 352.268.0205
- Path: Twilio SIP trunk (kclc-siptrk-01.pstn.us1.twilio.com)
- Account: kclcadmin
- Likely terminus: Extension 200 (to verify tonight)

**Documentation Created**:
- SIPXCOM-SCHEDULE-EXTRACTION-20260224.md (complete mapping)
- VERIFY-EXTENSION-200-DID.sh (verification script)

### ✅ Risk/Reversibility Assessment (COMPLETE)
| Step | Risk | Reversibility | Timing |
|------|------|---------------|--------|
| 1 (ipset switch) | Low-Medium | 30 sec | 6:30 PM ET ✅ |
| 3 (SYN limiting) | Very Low | 1 cmd | Daytime OK |

**Recommendation**: Execute Step 1 at 6:30 PM ET (user on-site, Bria testing ready)

---

## Inherited Context (Critical for Next Session)

### Phone System Details
- **Server**: ucs05.warrentek.net (CentOS 6.10, sipXcom 19.04)
- **SSH**: `sshpass -p 'wbC7PmX5YckaKa9N' ssh root@ucs05.warrentek.net`
- **Status**: Production live system (ZERO DOWNTIME CONSTRAINT)
- **Whitelist IP**: 72.166.63.114 (do not ban)

### Root Cause (Confirmed)
- **Problem**: 7,713 iptables rules from fail2ban (raw iptables, not ipset)
- **Impact**: CPU spikes → registration loss every ~3 days
- **Solution**: ipset hash tables (1 rule vs 5,926 individual rules)

### KCLC Business Context
- **Location**: Learning Center
- **Hours**: Mon-Fri 9 AM - 6 PM ET (closes 6:00 PM, business done 6:30 PM)
- **Saturday/Sunday**: Closed (after-hours mode)
- **Main DID**: 352.268.0205 → Extension 200
- **Help Desk**: Extension 500 (user's Bria app on iPhone)

### Zoom Migration Context (Feb 28 deadline)
- **Two tracks running**: SIPXCOM security (tonight) + ZOOM renumbering (parallel track)
- **ZOOM pending**: Tanya Thompson relocation, Fire Station/Police Station MACs, 4 room creates
- **Operating hours** must be replicated in Zoom: Mon-Fri 9-6 PM ET

---

## Next Steps (This Evening)

### 6:30 PM ET - On-Site at KCLC
1. **Pre-execution verification**: Confirm ext 200/DID 352.268.0205 routing
2. **Execute Step 1** (ipset switch): ~10 minutes
   - Commands in REMEDIATION-PLAN-20260223.md (Step 1a-1c)
3. **Post-execution testing**: Bria app (ext 500) call verification
4. **Validate registration**: Confirm phones still registered after ipset switch
5. **Optional**: Extract audio files (afterhours.wav, autoattendant.wav)

### Session Execution SOP (All commands ready in remediation plan)
- Step 1 (ipset): Ready ✅
- Step 2 (swap): Ready ✅
- Step 3 (SYN limit): Ready ✅
- Validation checks: Ready ✅

---

## Critical Facts for Continuity
- **Git status**: KCLC project files created (SIPXCOM-SCHEDULE-EXTRACTION-20260224.md, VERIFY-EXTENSION-200-DID.sh)
- **Backups verified**: All files in Dropbox, verified with ls -lh
- **Database status**: SIPXCONFIG primary (not SIPXCHANGE)
- **Twilio integration**: Active; sipxbridge configured and registered
- **Fail2ban currently**: 5,926 banned IPs (excessive rule count confirmed)
- **Audio prompts location**: `/var/sipxdata/mediaserver/data/prompts/`

---

**Created**: 2026-02-24 12:35 EST
**Checkpoint**: COMPLETE
**Status**: Ready for 6:30 PM ET Step 1 execution
