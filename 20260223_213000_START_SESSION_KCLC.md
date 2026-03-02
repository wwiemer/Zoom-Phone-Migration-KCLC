KCLC-IMPL-20260224-001-CLDE-sipxcom-security-remediation

Two active tracks. Pick up whichever is most urgent. Zoom cutover is Feb 28 — that track may need to go first.

Checkpoint: CHECKPOINT_20260223_2130_session-conclusion.md
Model: sonnet — SSH commands + Zoom API calls, well-defined scope

Priorities:
1. ZOOM (Feb 28 deadline): Warren answers 3 questions → execute renumbering + MAC registration
2. SIPXCOM SECURITY: Run remediation plan — backups first, then ipset switch, swap, SYN rate limiting

Critical facts:
- sipXcom SSH: sshpass -p 'wbC7PmX5YckaKa9N' ssh root@ucs05.warrentek.net
- sipXcom root cause: 7,713 iptables rules from fail2ban raw iptables (not ipset) → CPU spikes → registration loss
- FULL remediation plan (all commands ready): sipxcom-security/REMEDIATION-PLAN-20260223.md
- DO BACKUPS FIRST: iptables-save, tar fail2ban config, tar sipxcom config, export banned IP list
- Zoom: Tanya Thompson (ext 703) is TFH not KCLC — needs to move to TFH extension range
- Zoom: Fire Station (716) + Police Station (717) need MACs — not in sipXcom phones.csv
- Zoom: 4 rooms need created (Bus Station/700, KCLC Admin/705, The Market/709, Paint Shop/710)
- Zoom MCP creds: Strongbox → /Database/MCP-Servers → "Zoom - WTEK-KCLC-MCP App"
