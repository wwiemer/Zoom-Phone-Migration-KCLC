# Checkpoint: KCLC Zoom Phone — Credentials Configured, App Activated

**Created:** 2026-02-23 15:26:00 EST
**Session:** KCLC-IMPL-20260223-005-CLDE-zoom-credentials-app-activated
**Model:** Claude Sonnet 4.6
**Status:** ✅ Ready — zoom-mcp-server live after Claude Code restart
**Cutover Deadline:** Feb 28, 2026

---

## Summary

Zoom S2S OAuth credentials received, all 3 MCP configs updated with live values, Strongbox entry created, WTEK-KCLC-MCP app activated on Zoom account. Scopes: Phone category selected. Ready to restart Claude Code and run 8-step Zoom discovery.

---

## Inherited Context

### System Facts

- **Zoom app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Account-level | Activated ✅
- **Credentials location:** Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **MCP server:** `~/.local/mcp-servers/zoom-mcp-server/` (mattcoatsworth/zoom-mcp-server)
- **All 3 MCP configs updated:** `~/.claude/mcp_config.json`, `~/Library/Application Support/Claude/claude_desktop_config.json`, `~/.claude.json`
- **TFH Zoom:** 200-series extensions; KCLC may already be in 700-series — confirm via discovery
- **Voicemail:** Users dial `*86` — NO pilot extension concept (ext 250 is N/A in Zoom)
- **ZTP provisioning URL:** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}`

### Active Constraints

- Cutover **Feb 28, 2026** — hard deadline
- 14 active devices confirmed scope (ext 200–213)
- Ice Cream Shop (ext 204): TWO devices in sipXcom — active = most recently registered (0004f2c9dedf or 0004f27a7b32, verify during discovery)
- MAC format for ext 210/211/212: verify 64:16:7F format (may show as 6416f7 in some systems)
- Sub-accounts CANNOT do extension-to-extension calling — if KCLC needs to reach TFH by extension, must be Single Account + Sites or Linked Org
- Real KCLC email addresses still needed (14 users) — flagged TBD in zoom-map.csv
- **NEVER** use `git add .` or `git add -A` in WarrenTEK-Operations main repo

### Key Decisions

- zoom-mcp-server installed at `~/.local/mcp-servers/` (not Dropbox) per Smart Sync policy
- All files `--assume-unchanged` in main WarrenTEK-Operations repo — use `git update-index --no-assume-unchanged <file>` before staging specific files

---

## Completed Work — DO NOT REDO

- [x] Architecture research — sub-accounts, voicemail, ZTP, API, MCP options
- [x] Scope confirmed — 14 devices ext 200–213 (VVX500, ext 500/555/250 excluded)
- [x] zoom-mcp-server installed — `~/.local/mcp-servers/zoom-mcp-server/` (0 vulns)
- [x] S2S OAuth app created — WTEK-KCLC-MCP, Account ID: VOUi3C_NSjK1JNc6a1kWpQ
- [x] All 3 MCP configs updated — live credentials, ready to use
- [x] Strongbox entry created — `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- [x] App activated — green checkmark, "Your app is activated on the account"
- [x] phones.csv updated — 14 active devices + VVX500 excluded, discrepancies flagged
- [x] zoom-map.csv updated — 14 users, ext 200–213, emails/Zoom ext TBD
- [x] SOP-Zoom-Discovery.md — 8-step discovery runbook ready

---

## Blockers & Lessons

| Item | Detail |
|------|--------|
| Real KCLC emails | Still TBD — needed for Zoom user provisioning (14 users) |
| Ice Cream Shop MAC | Verify which device is active during discovery |
| MAC format 210/211/212 | Verify 64:16:7F vs 6416f7 format during Zoom device lookup |
| Account architecture | Confirm: does KCLC need extension dialing to TFH? |

---

## Ideas & Discoveries

- Zoom granular scopes UI has 200+ scopes per category — select Phone category only for phone migration work; User category scopes not needed
- App activation is immediate — no review delay for Server-to-Server OAuth (unlike Published apps)

---

## Next Steps

**Priority order:**
1. Restart Claude Code (load zoom-mcp-server with live credentials)
2. Test connection: call `list_phone_users` or equivalent tool
3. Run full SOP-Zoom-Discovery.md (8-step runbook)
4. Fill in zoom-map.csv with live Zoom data (Zoom extensions, current config)
5. Confirm account architecture (Single account vs sub-account)
6. Get real KCLC email addresses from Warren
7. Begin user + device provisioning

---

## Git Commit Details

**Branch:** main
**Files changed:**
- `CHECKPOINT_20260223_1526_zoom-credentials-app-activated.md` — this file
- `20260223_152634_START_SESSION_KCLC.md` — session handoff for next session
- `20260223_131246_START_SESSION_KCLC.md` — previous session handoff (historical)
- `20260223_092331_START_SESSION_ZOOM.md` — early session handoff (historical)

---

**Created:** 2026-02-23
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
