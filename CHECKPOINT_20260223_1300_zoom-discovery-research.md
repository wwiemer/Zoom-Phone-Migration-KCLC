# Checkpoint: KCLC Zoom Phone Migration — Phase 2 Discovery Research

**Created:** 2026-02-23 13:00:00 EST
**Session:** KCLC-RSCH-20260224-004-CLDE-zoom-phone-phase2-implementation
**Status:** 🚧 In Progress — Waiting on Zoom OAuth credentials
**Cutover Deadline:** Feb 28, 2026

---

## Inherited Context

- Phase 1 (sipXcom data extraction) complete as of 2026-02-23
- Phase 2 planning complete: provisioning plan + device implementation guide written
- Today: Architecture research + zoom-mcp-server installed; blocked on OAuth credentials

---

## What Was Accomplished This Session

### Scope Corrections Applied
- ❌ VVX500 (Warren's office device) — excluded from migration
- ❌ Extension 500 (WarrenTEK Helpdesk) — not needed
- ❌ Extension 555 (Test User) — not needed
- ❌ Extension 250 (voicemail pilot) — does NOT exist in Zoom Phone (users dial *86)
- ✅ **Confirmed scope: 14 devices, extensions 200–213**

### Architecture Research Completed
**Zoom Phone account models (3 options):**
1. Single account + Sites → full extension-to-extension dialing, unified billing
2. Sub-accounts → NO cross-account extension dialing (hard Zoom limitation)
3. Linked Organization → separate accounts WITH extension dialing

**Critical finding:** Sub-accounts CANNOT do extension-to-extension calling. If KCLC needs to call TFH by extension, must use Single Account + Sites or Linked Org.

**Warren's info:** TFH users are in 200-series in Zoom. KCLC may already be partially set up in 700-series. Must do Zoom discovery before finalizing any plan.

**Voicemail architecture:**
- No pilot number concept in Zoom Phone
- Users dial *86 for own voicemail
- Shared mailbox access via Zoom client/web/provisioned phone only
- Voicemail-to-email transcription is automatic and included

**Device provisioning:**
- All 14 devices are Polycom/Poly VVX series — all Zoom Phone certified
- 3 devices (ext 210/211/212) have newer Poly OUI (64:16:7F) — MAC accuracy needs verification
- Zero Touch Provisioning (ZTP) preferred: add MAC to admin portal, phone auto-configures
- Firmware must be v4.0.0+

### Zoom MCP Server Installed
- **Repo:** mattcoatsworth/zoom-mcp-server
- **Install path:** `~/.local/mcp-servers/zoom-mcp-server/`
- **Status:** Installed, 0 vulnerabilities, all 3 MCP configs updated with PLACEHOLDER credentials
- **Blocked on:** Zoom S2S OAuth credentials (Account ID, Client ID, Client Secret)

### Credentials Blocker
- Warren has admin role in TFH Zoom account (not account owner)
- Admin role lacks App Marketplace Edit permission
- Email sent 2026-02-23 to TFH account owner requesting permission update
- Once granted: Warren creates S2S OAuth app → gets 3 values → I update all 3 configs → live

### Data Files Updated
- `Claude/phones.csv` — 15 active devices (14 + 1 excluded VVX500), Ice Cream Shop duplicate flagged, MAC discrepancy noted
- `Claude/zoom-map.csv` — Cleaned to 14 users (ext 200–213), placeholder emails flagged as TBD, Zoom extensions as TBD pending discovery
- `Claude/SOP-Zoom-Discovery.md` — 8-step discovery runbook ready to execute

---

## Confirmed Scope: 14 Devices

| Ext | Room/Station | MAC | Model | Notes |
|-----|-------------|-----|-------|-------|
| 200 | Bus Station | 0004f27baa59 | VVX400 | Not in original users.csv |
| 201 | Front Desk/Toy Shop | 0004f2dd7e86 | VVX400 | Name discrepancy - verify |
| 202 | Pet Shop | 0004f2887947 | VVX410 | Confirmed as x202 |
| 203 | Sweet Shop | 0004f2de4e79 | VVX400 | Verify ext during discovery |
| 204 | Ice Cream Shop | VERIFY | VVX410 | Two devices: 0004f2c9dedf OR 0004f27a7b32 — active = most recently registered |
| 205 | Administrator | 0004f2de46b8 | VVX400 | Verify ext during discovery |
| 206 | Sport Store | 0004f2ddfe05 | VVX400 | Verify ext during discovery |
| 207 | Music Store/Ice Cream | 0004f2822926 | VVX400 | Name discrepancy - verify |
| 208 | Flower Shop | 0004f2c6a847 | VVX400 | |
| 209 | The Market | 0004f268033e | VVX400 | |
| 210 | The Paint Shop | 64167f909af0 | VVX410 | Verify MAC format |
| 211 | Media/Kitchen | 64167f31a96d | VVX401 | Verify MAC format |
| 212 | The Clubhouse (VPK) | 64167f33f295 | VVX401 | Verify MAC format |
| 213 | Resource Room | 0004f2de4b0e | VVX400 | |

---

## Blocking Items (In Priority Order)

1. **Zoom credentials** — TFH account owner grants permission → Warren creates S2S OAuth app
2. **Real email addresses** — Need actual KCLC staff emails for Zoom user provisioning (14 users)
3. **Ice Cream Shop MAC** — Verify which device (0004f2c9dedf vs 0004f27a7b32) is active
4. **MAC format verification** — Ext 210/211/212 (64:16:7F devices) — verify exact format
5. **Account architecture decision** — Does KCLC need extension dialing to TFH users?

---

## Next Session: Zoom Discovery + Provisioning

**As soon as credentials arrive:**

1. Update all 3 MCP configs with real credentials (replace PLACEHOLDER values)
2. Restart Claude Code
3. Run SOP-Zoom-Discovery.md (8-step discovery)
4. Document results
5. Build final provisioning plan based on actual Zoom account state
6. Begin user + device provisioning

**Files ready for next session:**
- `Claude/SOP-Zoom-Discovery.md` — Discovery runbook
- `Claude/phones.csv` — Updated device inventory
- `Claude/zoom-map.csv` — Extension mapping (TBD fields to fill in)

---

## Key References

- [SOP-Zoom-Discovery.md](Claude/SOP-Zoom-Discovery.md) — Discovery runbook
- [phones.csv](Claude/phones.csv) — Device inventory
- [zoom-map.csv](Claude/zoom-map.csv) — Extension mapping
- [MCP Registry](../../projects/mcp-servers/MCP-REGISTRY.md) — zoom server entry
- Zoom provisioning URL: `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/{model}`
- Voicemail access: `*86` (no pilot number)
- ZTP preferred over DHCP Option 66 for device provisioning
