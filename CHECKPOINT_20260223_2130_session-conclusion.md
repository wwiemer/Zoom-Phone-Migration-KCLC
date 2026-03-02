# Checkpoint: KCLC Session Conclusion — Two Active Tracks

**Created:** 2026-02-23 21:30:00 EST
**Session:** KCLC-IMPL-20260223-007-CLDE-zoom-provisioning
**Model:** Claude Sonnet 4.6
**Status:** 🚧 Two tracks in progress — both blocked on decisions/execution
**Cutover Deadline (Zoom):** Feb 28, 2026

---

## Inherited Context

### Zoom System Facts
- **Zoom app:** WTEK-KCLC-MCP | Server-to-Server OAuth | Activated ✅
- **Account model:** SINGLE ACCOUNT — TFH + KCLC share @thefathershouse.com
- **Credentials:** Strongbox → `/Database/MCP-Servers` → "Zoom - WTEK-KCLC-MCP App"
- **MCP server:** `~/.local/mcp-servers/zoom-mcp-server/`
- **ZTP provisioning URL:** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}`
- **Emergency address (all):** 2301 South St, Leesburg, FL 34748 ✅

### sipXcom System Facts
- **Server:** ucs05.warrentek.net | CentOS 6.10 | sipXcom 19.04
- **SSH:** `sshpass -p 'wbC7PmX5YckaKa9N' ssh root@ucs05.warrentek.net`
- **Credentials:** Strongbox → `/Database/VoIP` → "KCLC-SIPXCOM-Server"
- **KCLC public IP (whitelisted in fail2ban):** 72.166.63.114

---

## TRACK 1: Zoom Phone Migration

### Status: Blocked on 3 open questions

**Proposed extension renumbering (sipXcom 2XX → Zoom 7XX):**

| sipXcom | Room | Zoom Current | Target 7XX | Action |
|---|---|---|---|---|
| 200 | Bus Station | NONE | 700 | CREATE |
| 201 | Toy Shop | 709 | 701 | RENUMBER |
| 202 | Pet Shop | 705 | 702 | RENUMBER |
| 203 | Sweet Shop | 706 | 703 | RENUMBER (after Tanya moves) |
| 204 | Ice Cream Shop | 708 | 704 | RENUMBER (after Kids City moves) |
| 205 | KCLC Administrator | NONE | 705 | CREATE |
| 206 | Sport Store | 707 | 706 | RENUMBER |
| 207 | Music Store | 710 | 707 | RENUMBER |
| 208 | Flower Shop | 712 | 708 | RENUMBER |
| 209 | The Market | NONE | 709 | CREATE |
| 210 | The Paint Shop | NONE | 710 | CREATE |
| 211 | Media/Kitchen | 715 | 711 | RENUMBER |
| 212 | The Clubhouse | 713 | 712 | RENUMBER |
| 213 | Resource Room | 714 | 713 | RENUMBER |
| ? | Fire Station | 716 | 714 | MAC NEEDED |
| ? | Police Station | 717 | 715 | MAC NEEDED |

**3 open questions (Warren must answer before execution):**

**Q1 — Fire Station + Police Station MACs:**
Neither appears in sipXcom phones.csv or users.md (users go up to 213/Resource Room). Zoom has them at 716/717 with DIDs. Warren confirmed physical phones exist. Need: what are the MAC addresses of those two phones? Or are they two of the existing 14 sipXcom devices under different names?

**Q2 — Tanya Thompson extension:**
Tanya (tanya@thefathershouse.com) is TFH staff — incorrectly placed at ext 703 (a KCLC slot). She needs to move to a TFH extension. Available TFH slots: 231, 234, 236, 237, 238, 242, 243, 244. Which should she use?

**Q3 — Kids City (kclc@) extension:**
Kids City at ext 704 needs to move to free up 704 for Ice Cream Shop. Where should the kclc@ admin account live? Options: 700 (before rooms), or another number.

**Also needed:** DID +13522680205 routing decision (still PENDING port, unassigned).

### Key Files
- `Claude/phones.csv` — 14 physical devices with MACs
- `Claude/zoom-map.csv` — current sipXcom ↔ Zoom mapping
- `sipxcom-security/FINDINGS-20260223.md` — security discovery
- `sipxcom-security/REMEDIATION-PLAN-20260223.md` — security fix plan

---

## TRACK 2: sipXcom Security Remediation

### Status: Ready to execute — awaiting dedicated session

**Root cause identified:** fail2ban uses raw iptables (not ipset). 5,926 permanently banned IPs = 5,926 iptables rules = 7,713 total rules. Linear rule traversal causes CPU spikes during attack bursts → phones lose SIP registration. Reboots clear the bans temporarily; cycle repeats every ~3 days.

**Approved remediation plan (in order):**

| Step | Action | Priority | Risk |
|---|---|---|---|
| 0 | Backup iptables, fail2ban config, sipXcom config, banned IP list | MANDATORY FIRST | None |
| 1 | Switch fail2ban to ipset | HIGH — root cause fix | Low-Medium |
| 2 | Add 4GB swap file | MEDIUM | Very Low |
| 3 | Rate limit SYN flood on port 443 | MEDIUM | Low |
| 4 | Save iptables rules permanently | LOW | None |
| 5 | Deploy monitoring script (twice-daily check) | LOW | None |

**Full plan with all commands:** `sipxcom-security/REMEDIATION-PLAN-20260223.md`

**Estimated session time:** 45–60 minutes

**Success criteria:** Server runs >7 days without registration loss. iptables rule count stays flat as bans accumulate.

---

## Next Session Priorities

**Immediate (before Feb 28):**
1. Warren answers Q1–Q3 above → execute Zoom renumbering + MAC registration
2. Route +13522680205 once port completes

**Can be scheduled separately:**
3. sipXcom security remediation (dedicated session — do when phones are NOT in use, ~30 min window)

---

**Created:** 2026-02-23 21:30:00 EST
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
