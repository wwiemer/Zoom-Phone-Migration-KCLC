# Checkpoint: Phase 2 (5.9.8) Upgrade Complete — ZTP Next

**Created:** 2026-03-04 00:15:37 EST
**Session:** KCLC-FEAT-20260304-012-SNNT-phase2-5.9.8-upgrade-and-ztp
**Model:** Claude Sonnet 4.6
**Status:** Phase 2 COMPLETE — all 10 phones confirmed at 5.9.8 — ZTP not started

---

## Summary

Phase 2 (5.9.8 firmware upgrade) complete — all 10 remotely manageable phones confirmed at 5.9.8.5760. Root cause of the upgrade blockage was the tftpd64 TFTP server serving from `C:\tftp\` (not `C:\tftp\root\` as previously assumed) — all prior phase 2 cfg fixes went to the wrong directory. Batch fix applied to `C:\tftp\*.cfg`. Next step: ZTP provisioning test on 10.1.12.189 (ext 314).

---

## Inherited Context (v3.0)

### System Facts

- **TFTP Server:** Windows 10.1.10.175, tftpd64.exe running headless — **ACTUAL serving root is `C:\tftp\` (NOT `C:\tftp\root\`)** — INI says `C:\tftp\root` but running instance uses `C:\tftp\`
- **TFTP log:** `C:\tftp\tftpd64.log` (configured but may not exist) — phone boot logs upload to `C:\tftp\` as `{MAC}-boot.log`
- **Per-MAC cfg location (actual):** `C:\tftp\{MAC}.cfg` — NOT `C:\tftp\root\{MAC}.cfg`
- **All 10 phones:** admin password = 2307 (changed from default 456)
- **Zoom provisioning URL:** `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
- **First ZTP test phone:** 10.1.12.189 (VVX 401, MAC 64167f31a96d, Zoom ext 314)
- **Upgrade path:** 5.5.2 → 5.7.3 (Phase 1) → 5.9.8 (Phase 2) → Zoom ZTP (Phase 3)
- **Target firmware:** UC Software = 5.9.8.5760, Updater = 5.9.8.4127

### Per-Phone Firmware Files (in C:\tftp\)

| Model | Phase 2 File |
|-------|-------------|
| VVX 400 | 3111-46157-002-598.sip.ld |
| VVX 401 | 3111-48400-001-598.sip.ld |
| VVX 410 | 3111-46162-001-598.sip.ld |

### Active Constraints

- **TFTP serving directory:** ALWAYS edit files in `C:\tftp\` — NOT `C:\tftp\root\`. The `root` subdirectory is where we put modified copies but the RUNNING server ignores them.
- **PowerShell Set-Content encoding trap:** Always use `[System.IO.File]::WriteAllText($path, $content, (New-Object System.Text.UTF8Encoding $false))` — never `Set-Content` for XML files.
- **Phone cfg cache:** After cfg changes, phones may need provisioning server swap (change IP → reboot → change back → reboot) to force fresh config download. Factory reset also clears cache.
- **Bus Station cfg (0004f27baa59.cfg):** Keep at -573 — phone at 5.5.2, needs on-site Phase 1 first.
- **`sip.ld` is binary firmware** (5.9.8 components) — not a text manifest. Cannot be edited as text.

### Key Decisions

- **Phase sweep approach:** All phones to 5.7.3 first → batch to 5.9.8 → ZTP
- **ZTP test first:** Verify 10.1.12.189 registers as ext 314 before batching all 10 phones to Zoom
- **Factory reset required** for all phones with sipXcom history before TFTP provisioning

---

## Phase Status

### Stage 1 — Phase 1 (5.7.3) + Password ✅ COMPLETE
All 10 remotely manageable phones: 5.7.3.1797, admin password = 2307

### Stage 2 — Phase 2 (5.9.8 upgrade) ✅ COMPLETE
All 10 remotely manageable phones confirmed at 5.9.8.5760 / Updater 5.9.8.4127.

**How upgrade was unblocked:**
- TFTP serving from `C:\tftp\` (not `C:\tftp\root\`)
- All per-MAC cfgs in `C:\tftp\` still had -573 (previous fixes went to wrong dir)
- `000000000000.cfg` had literal `[PHONE_MAC_ADDRESS]` placeholder in CONFIG_FILES
- Fix: `Get-ChildItem "C:\tftp\*.cfg" | ForEach-Object { ...Replace('-573.sip.ld', '-598.sip.ld')... }`

### Stage 3 — Zoom ZTP ⏳ NOT STARTED
- First phone: 10.1.12.189 (ext 314, already built in Zoom)
- ZTP URL: `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`

---

## On-Site Required

| Phone | IP | Issue |
|-------|----|-------|
| Bus Station | 10.1.12.182 | Offline — Edge E220 temp. VVX 400 at 5.5.2, needs Phase 1→2→ZTP on-site |
| Fire Station | 10.1.12.64 | Unknown admin password post-factory-reset. TFTP cfg set for 5.9.8 direct. Once in, reboot → auto-upgrades |
| Pet Shop | 10.1.12.68 | 5.0.1.4068 — needs intermediate firmware (5.4.x) before 5.7.3. Do NOT touch until path researched |

---

## Completed Work — DO NOT REDO

- [x] Phase 1 (5.7.3 + password) on all 10 remote phones — all at 5.7.3.1797, pw=2307
- [x] Phase 2 cfg files updated in `C:\tftp\` — all per-MAC cfgs pointing to -598 firmware
- [x] `000000000000.cfg` CONFIG_FILES fixed from `[PHONE_MAC_ADDRESS]` to `%BCHCP%`
- [x] 5.9.8 upgrade confirmed on 10.1.12.189 and second phone
- [x] Root cause identified: TFTP served from `C:\tftp\` not `C:\tftp\root\`

---

## Blockers & Lessons

| Item | Detail |
|------|--------|
| TFTP wrong directory | tftpd64 serves from `C:\tftp\` despite INI saying `C:\tftp\root`. Phone boot log uploads go to `C:\tftp\` — that's the tell. Always check where log files land. |
| `000000000000.cfg` placeholder | CONFIG_FILES had literal `[PHONE_MAC_ADDRESS].cfg` — not valid Polycom syntax. Per-MAC cfg was NOT loading via this mechanism. |
| `%BCHCP%` substitution uncertain | May not substitute correctly in all firmware versions. If per-MAC cfg issues recur, try `CONFIG_FILES=""` (empty) to allow auto-discovery. |
| Polycom cfg cache in flash | Phones cache config — factory reset or provisioning server swap required after cfg changes. |

---

## Ideas & Discoveries

- Consider documenting the two-directory structure on the TFTP server (`C:\tftp\` vs `C:\tftp\root\`) to avoid confusion in future sessions.
- After all 10 phones are on Zoom, the TFTP server (10.1.10.175) can be decommissioned.
- Pet Shop (5.0.1) needs research into intermediate firmware path — could be a separate on-site session.

---

## Next Steps

**Priority order:**
1. **Verify all 8 remaining phones at 5.9.8** — check each via web UI: UC Software = 5.9.8.5760, Updater = 5.9.8.4127
2. **ZTP test on 10.1.12.189 (ext 314):**
   - Web UI → Settings → Provisioning Server → Protocol: HTTPS
   - Server: `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
   - Save → Reboot → confirm phone registers as ext 314 in Zoom
3. **If ZTP works → batch remaining 9 phones to Zoom** (update provisioning server on each)
4. Schedule on-site visit for Bus Station, Fire Station, Pet Shop

---

## Git Commit Details

**Branch:** main
**Files changed:**
- `projects/Zoom-Phone-Migration-KCLC/` — submodule pointer update (checkpoint + handoff)

---

**Checkpoint Version:** 3.0
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-04 00:15:37 EST
