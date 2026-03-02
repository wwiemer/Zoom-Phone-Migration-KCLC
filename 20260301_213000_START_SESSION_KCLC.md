KCLC-IMPL-20260301-004-CLDE-onsite-phone-provisioning-diagnosis

Zoom MCP server now configured on MacBook Air with S2S OAuth credentials. All three config files updated. Currently on-site troubleshooting VVX401 phone provisioning — phone still shows extension 211 (sipXcom) with old speed dials despite factory reset and Zoom provisioning URL. Ready to use Zoom MCP to diagnose registration conflict.

Checkpoint: [CHECKPOINT_20260301_2130_zoom-mcp-setup-macbook-onsite.md](projects/Zoom-Phone-Migration-KCLC/CHECKPOINT_20260301_2130_zoom-mcp-setup-macbook-onsite.md)
Model: sonnet — multi-step provisioning troubleshooting with Zoom MCP API calls, then 12 remaining renumbers, Zoom Site/queue creation, Twilio forward

Priorities:
1. Restart Claude Code to load Zoom MCP, use it to check phone registration status and diagnose sipXcom vs Zoom conflict
2. Execute diagnostics: Is phone reaching sipXcom provisioning server or Zoom? Does sipXcom still have phone configured?
3. If sipXcom is blocking: Unregister from sipXcom, factory reset again, provision to Zoom; then execute 12 remaining renumbers + MAC registration
4. Create KCLC Zoom Site (2307 South St E911) and call queue (300-304 simultaneous)
5. Set Twilio forward +13522680205 → KCLC Zoom TONIGHT (before 11:30 AM ET port tomorrow)

Critical facts:
- Phone IP: 10.1.12.189 | Ext should be 314, currently shows 211 (sipXcom)
- Final mapping: 200-210→300-310, 211→314, 212→312, 213→313 (11 phones done, 1 problematic, 12 remaining)
- Port event: +13522680205 tomorrow 2026-03-02 11:30 AM ET — Twilio forward must be set TONIGHT
- Zoom MCP credentials in Strongbox (WTEK-KCLC-MCP app) — all three MacBook Air MCP configs updated
- MAC registration requires admin.zoom.us portal (S2S scope unavailable)
