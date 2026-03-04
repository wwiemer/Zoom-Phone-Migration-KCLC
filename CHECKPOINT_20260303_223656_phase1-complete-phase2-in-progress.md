# Checkpoint: Phase 1 Complete — Phase 2 (5.9.8) In Progress

**Created:** 2026-03-03 22:36:56 EST
**Session:** KCLC-FEAT-20260303-011-SNNT-tftp-firmware-transfer-verify
**Model:** Claude Sonnet 4.6
**Status:** ACTIVE — 10 phones at 5.7.3 + password done; Phase 2 cfg deployed; phone cache issue being resolved

---

## Summary

Phase 1 complete for all 10 remotely manageable phones — all at 5.7.3.1797 with admin password changed from 456 to 2307. Phase 2 cfg files deployed (all 10 pointing to -598.sip.ld). Encountered two issues: (1) PowerShell Set-Content wrote UTF-16 encoding — fixed with [System.IO.File]::WriteAllText using explicit UTF-8 no-BOM encoding; (2) phones using flash-cached cfg and not downloading updated files — fix is provisioning server swap trick (change IP, reboot, change back, reboot). TFTP infrastructure confirmed solid — 121 files in C:\tftp\root\, server running at 10.1.10.175 port 69.

---

## Inherited Context (v3.0)

### System Facts

- **TFTP Server:** Windows 10.1.10.175 `C:\tftp\root\` (121 files) — command: `tftpd64.exe -i C:\tftp\root -l C:\tftp\tftpd64.log`
- **Python HTTP server:** Running on MacBook Air at `/private/tmp/` port 8080 (PID 8174) — Tailscale 100.108.108.84 — NOTE: slow (~2.3 Mbps over DERP relay)
- **Windows hostname:** DESKTOP-2K90BBM
- **Upgrade path:** 5.5.2 → 5.7.3 (Phase 1) → 5.9.8 (Phase 2)
- **Admin password (all upgraded phones):** 2307 (changed from default 456)
- **Zoom provisioning URL:** `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
- **First ZTP test phone:** 10.1.12.189 (VVX 401, MAC 64167f31a96d, Zoom ext 314)

### Per-Phone Firmware Files (flat in C:\tftp\root\)

| Model | Phase 1 | Phase 2 |
|-------|---------|---------|
| VVX 400 | 3111-46157-002-573.sip.ld | 3111-46157-002-598.sip.ld |
| VVX 401 | 3111-48400-001-573.sip.ld | 3111-48400-001-598.sip.ld |
| VVX 410 | 3111-46162-001-573.sip.ld | 3111-46162-001-598.sip.ld |

### Active Constraints

- **PowerShell Set-Content encoding trap:** Always use `[System.IO.File]::WriteAllText($path, $content, (New-Object System.Text.UTF8Encoding $false))` — never `Set-Content` for XML files. Set-Content defaults to ANSI/Unicode which breaks Polycom XML parsing.
- **Phone cfg cache:** Phones cache cfg in flash. After cfg changes, must do provisioning server swap (change IP → reboot → change back → reboot) to force fresh download.
- **Bus Station cfg (0004f27baa59.cfg):** Keep at -573 — phone still at 5.5.2, needs Phase 1 first on-site.
- **Tailscale slow:** KCLC Windows → MacBook Air Tailscale goes through DERP relay at ~2.3 Mbps. For future large transfers use Cloudflare Quick Tunnel instead.

### Key Decisions

- **Factory reset required** for all phones with sipXcom history before TFTP provisioning
- **Phase sweep approach:** All phones to 5.7.3 first, then batch to 5.9.8, then ZTP
- **Fire Station cfg (64167f909af0.cfg):** Already points to 5.9.8 directly (was at 5.8.0, skip Phase 1)

---

## Phase Status

### Stage 1 — Phase 1 (5.7.3) + Password
| Room | IP | Status |
|------|----|--------|
| Media/Kitchen | 10.1.12.189 | ✅ 5.7.3 + pw 2307 |
| Toy Shop | 10.1.12.185 | ✅ 5.7.3 + pw 2307 |
| Sweet Shop | 10.1.12.180 | ✅ 5.7.3 + pw 2307 |
| Sport Store | 10.1.12.183 | ✅ 5.7.3 + pw 2307 |
| Ice Cream Shop | 10.1.12.65 | ✅ 5.7.3 + pw 2307 |
| Music Store | 10.1.12.186 | ✅ 5.7.3 + pw 2307 |
| Flower Shop | 10.1.12.190 | ✅ 5.7.3 + pw 2307 |
| Police Station | 10.1.12.191 | ✅ 5.7.3 + pw 2307 |
| The Clubhouse | 10.1.12.187 | ✅ 5.7.3 + pw 2307 |
| Rec Room | 10.1.12.181 | ✅ 5.7.3 + pw 2307 |

### Stage 2 — Phase 2 (5.9.8) cfg deployed, upgrades pending
- All 10 cfg files updated to -598.sip.ld (UTF-8 no-BOM, verified)
- **Current blocker:** Phones reading cached cfg with -573 from flash
- **Fix in progress:** Provisioning server swap on 10.1.12.189 to force cache clear

### Stage 3 — Zoom ZTP (not started)
- First phone: 10.1.12.189 (ext 314, already built out in Zoom)
- After first ZTP confirmed working → batch remaining phones

---

## On-Site Required

| Phone | IP | Issue |
|-------|----|-------|
| Bus Station | 10.1.12.182 | Offline — Edge E220 temporary. VVX 400 at 5.5.2 needs Phase 1→2→ZTP |
| Fire Station | 10.1.12.64 | Unknown admin password post-factory-reset. TFTP cfg already set for 5.9.8 direct. Once in, just reboot → upgrades automatically |
| Pet Shop | 10.1.12.68 | 5.0.1.4068 pre-Updater era. Needs intermediate firmware (5.4.x) before 5.7.3. Do not touch until path researched |

---

## Next Steps — REQUIRED

1. **Complete Phase 2 cache-bust on 10.1.12.189:**
   - Web UI → Settings → Provisioning Server → change to `10.1.10.176` → Save → Reboot
   - After boot: change back to `10.1.10.175` → Save → Reboot
   - Phone should read updated cfg, download 5.9.8, install, reboot to 5.9.8.5760

2. **Confirm 5.9.8 on 10.1.12.189:**
   - Web UI: UC Software = 5.9.8.5760, Updater = 5.9.8.4127

3. **Repeat cache-bust + 5.9.8 upgrade on remaining 9 phones** (same provisioning swap trick)

4. **Stage 3 — ZTP test on 10.1.12.189:**
   - Settings → Provisioning Server → HTTPS → `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
   - Phone registers to Zoom as ext 314
   - If successful → batch remaining phones

5. **After all 10 at 5.9.8 + registered to Zoom:**
   - Schedule on-site visit for Bus Station, Fire Station, Pet Shop

---

## Key Discoveries This Session

- **PowerShell Set-Content UTF-16 trap:** Set-Content writes Unicode (UTF-16) by default for XML. Polycom phones fail silently and use flash cache. Fix: `[System.IO.File]::WriteAllText` with explicit `UTF8Encoding($false)`.
- **Polycom cfg flash cache:** Phones don't re-download cfg unless provisioning server address changes. Must do server swap trick to force fresh download.
- **Tailscale DERP relay:** ~2.3 Mbps regardless of gigabit on both sides. Cloudflare Quick Tunnel is the better alternative for large file transfers.
- **Regex replacement in replacement string:** PowerShell `-replace` uses the replacement string literally for `\` but in our case the `\.` in replacement was passed through. Always use `.Replace()` for literal string replacements.

---

**Checkpoint Version:** 3.0
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-03 22:36:56 EST
