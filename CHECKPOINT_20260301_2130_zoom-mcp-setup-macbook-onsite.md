# Checkpoint: Zoom MCP Setup on MacBook Air (On-Site)

**Created:** 2026-03-01
**Session:** KCLC-IMPL-20260301-003-CLDE-onsite-provisioning-and-full-buildout
**Model:** Sonnet 4.6
**Status:** In Progress

---

## Summary

Configured Zoom MCP server on MacBook Air with S2S OAuth credentials from Strongbox. All three MCP config files updated and verified. Currently on-site troubleshooting VVX401 phone provisioning — phone still registered to sipXcom (extension 211) despite factory reset and Zoom provisioning URL configuration. Ready to use Zoom MCP tools to diagnose and resolve once Claude Code restarts.

---

## Inherited Context

**Critical facts accumulated across sessions. Copy forward to every new checkpoint.**

### System Facts

- **11 phones provisioned, registered to Zoom:** Ext 200-210, 212, 313
- **1 phone problematic (current focus):** VVX401 at 10.1.12.189 (was ext 211, should be ext 314)
- **Final extension mapping:** 200-210→300-310, 211→314, 212→312, 213→313
- **311 is reserved N11** — cannot use for phone
- **All 14 phones are Poly VVX401 (End of Life in Zoom — still functional)**
- **KCLC E911 address:** 2307 South St (NOT 2301 — that is TFH church)
- **Port event:** +13522680205 ports tomorrow 2026-03-02 at 11:30 AM ET
- **MAC registration:** Requires admin.zoom.us portal (phone:write:device:admin scope unavailable via S2S OAuth)
- **Zoom common area IDs:** All 13 remaining extensions documented in previous checkpoint

### Active Constraints

- **Phone still on sipXcom:** Shows ext 211 + old speed dials (KCLC front desk, Father's House, Poison Control, Lisa mobile, Dorothy mobile, administrator office)
- **Provisioning not taking:** Factory reset completed, DHCP set to static with Zoom provisioning URL — phone still boots to sipXcom
- **Potential causes:** DHCP still pointing to sipXcom | Cached sipXcom config | Provisioning cert issue
- **Zoom MCP now available:** All credentials stored in Strongbox, configured on MacBook Air
- **Twilio forward deadline:** Must be set TONIGHT after phones verified working (before 11:30 AM ET port tomorrow)

### Key Decisions

- **Using Zoom MCP for API operations:** Renumbers, device registration, site/queue creation
- **On-site provisioning workflow:** Factory reset → Zoom config → verify registration → move to next phone
- **Separate concerns:** sipXcom deprovisioning vs Zoom provisioning (need to understand which is blocking)

---

## Completed Work — DO NOT REDO

- [x] **Ext 314 configured in Zoom** — MAC address registered, provisioning URL saved on phone
- [x] **Zoom MCP server credentials created** — S2S OAuth app in Zoom Marketplace (WTEK-KCLC-MCP)
- [x] **Zoom MCP configured on all three MacBook Air configs:** `~/.claude/mcp_config.json`, `~/Library/Application Support/Claude/claude_desktop_config.json`, `~/.claude.json`
- [x] **Credentials verified in Strongbox** — Ready to use after Claude Code restart

---

## Blockers & Lessons

| Item | Detail |
|------|--------|
| **Phone registering to sipXcom, not Zoom** | Factory reset + provisioning URL not enough to override sipXcom. Need to investigate: (1) is DHCP still pointing sipXcom provisioning server? (2) does phone need to explicitly unregister from sipXcom first? (3) is provisioning cert the issue? Use Zoom MCP to check what Zoom thinks phone state should be. |
| **sipXcom deprovisioning unknown** | Don't know if sipXcom still has this phone configured. May need to unregister from sipXcom dashboard before Zoom will accept it. Check with on-site staff or sipXcom access. |
| **MAC registration requires portal** | S2S OAuth scope `phone:write:device:admin` not available — must use admin.zoom.us portal for MAC registration. Already done for ext 314. |

---

## Ideas & Discoveries

- Consider: Does sipXcom provisioning server still exist on the network? Can we block it or redirect to Zoom?
- Observation: The phone's speed dials suggest sipXcom provisioning was so aggressive it overrode the factory reset. This could mean sipXcom has a persistent config store that survives factory reset (EEPROM or network provision file).
- Possible workflow: Unregister phone from sipXcom first → factory reset → then provision to Zoom?

---

## Next Steps

**Priority order:**
1. **Restart Claude Code** on MacBook Air to load Zoom MCP server
2. **Use Zoom MCP** to query phone registration status and check what Zoom expects vs what phone reports
3. **Diagnose sipXcom conflict:** Check if phone is trying to reach sipXcom provisioning server (network packet capture or phone logs)
4. **If diagnostic reveals sipXcom is blocking:** Unregister from sipXcom, factory reset again, then provision to Zoom
5. **Execute 12 remaining Zoom renumbers** via Zoom MCP API (ext 200-210, 212, 213)
6. **Register all phone MACs** via admin.zoom.us portal
7. **Create KCLC Zoom Site** (2307 South St, E911 address)
8. **Create call queue** (300-304 simultaneous capacity)
9. **Set Twilio forward** +13522680205 → KCLC Zoom (BEFORE 11:30 AM ET tomorrow)

---

## Git Commit Details

**Branch:** main
**Status:** Not yet committed (MCP configs are local to MacBook Air, not synced to git)
**Next commit:** After successful phone provisioning, checkpoint updates, and session conclusion

---

**Created:** 2026-03-01 21:30:00 EST
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Haiku 4.5)
