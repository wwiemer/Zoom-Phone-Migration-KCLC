# Checkpoint: Zoom Config Complete — ZTP Batch Ready

**Created:** 2026-03-04 02:02:25 EST
**Session:** KCLC-FEAT-20260304-013-SNNT-ztp-provisioning-and-zoom-registration
**Model:** Claude Sonnet 4.6
**Status:** Zoom KCLC site fully configured — ZTP batch ready — MCP device tools added — awaiting MCP restart

---

## Summary

All 13 KCLC common area phones are now correctly numbered (300-series) and assigned to the KidCity Learning Center site in Zoom. Ext 304 was fixed: Kids City user (kclc@) moved to 799 to free the number, Warren created ext 304 as a proper common area phone in portal. Zoom MCP server updated with 4 new phone device management tools (list/get/add device, assign device to user). MCP needs restart before new tools are active. ZTP batch is ready to run for 9 remote phones once MCP is restarted and ext 304 device is created.

---

## Inherited Context (v3.0)

### System Facts

- **TFTP Server:** Windows 10.1.10.175, serving from `C:\tftp\` (NOT `C:\tftp\root\`) — always edit files there
- **Mac IP on KCLC network:** 10.1.10.202 (en0) — same subnet as TFTP server
- **HTTP server on Mac:** Used to transfer files to Windows via PowerShell `Invoke-WebRequest` — Mac is the server, Windows pulls
- **All 10 remote phones:** admin password = 2307, firmware 5.9.8.5760
- **ZTP URL (VVX 400/410):** `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
- **ZTP URL (VVX 401):** `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401`
- **Zoom auto-upgrades firmware to 6.4.7.4710 during ZTP** (confirmed from ext 314)
- **KCLC site ID:** `dUxbXwstSW6rvziP3tF1xA` (name: "KidCity Learning Center")
- **Kids City user** (kclc@thefathershouse.com) moved to ext 799 — no longer occupying 304
- **Pet Shop 5.4.7 firmware:** Downloaded to `/Users/warrenwiemer/Library/CloudStorage/Dropbox/Polycom/VVX/Polycom-UC-Software-5-4-7-rts27-release-sig-split/` — file needed: `3111-46162-001.sip.ld` → rename to `3111-46162-001-547.sip.ld` on TFTP server

### Zoom KCLC Site — Current State (Verified Live)

| Ext | Room | DID | Type | Device | Status |
|-----|------|-----|------|--------|--------|
| 300 | KCLC - Bus Station | +13522680205 | commonArea | Poly edge-e220 | ✅ Online (temp) |
| 301 | Toy Shop | +13527820672 | commonArea | — | ⏳ ZTP ready |
| 302 | Pet Shop | +13527820599 | commonArea | — | ⏳ On-site (5.0.1) |
| 303 | Sweet shop | +13527820654 | commonArea | — | ⏳ ZTP ready |
| 304 | KCLC Admin | *(none)* | commonArea | — | ⏳ Device being created |
| 305 | Sports store | +13527820675 | commonArea | — | ⏳ ZTP ready |
| 306 | Ice cream shop | +13527820455 | commonArea | — | ⏳ ZTP ready |
| 307 | Music Store | +13527820695 | commonArea | — | ⏳ ZTP ready |
| 308 | Flower Shop | +13527820550 | commonArea | — | ⏳ ZTP ready |
| 309 | Police Station | +13527820614 | commonArea | — | ⏳ ZTP ready |
| 310 | Fire Station | +13527820635 | commonArea | — | ⏳ On-site (unknown pw) |
| 312 | Club house | +13527820631 | commonArea | — | ⏳ ZTP ready |
| 313 | The Rec Room | +13527820566 | commonArea | — | ⏳ ZTP ready |
| 314 | Media/Kitchen | +13527820626 | commonArea | Poly vvx401 | ✅ Online (ZTP done, 6.4.7) |
| 703 | Tanya | +13524601259 | user | — | ✅ Main Site (TFH — do not touch) |
| 799 | Kids City | *(none)* | user | — | ⚠️ Placeholder — delete or reassign |

### MAC Address Reference (All 14 Phones)

| Ext | Room | MAC | Model | IP | FW | ZTP Status |
|-----|------|-----|-------|----|----|-----------|
| 300 | Bus Station | 0004f27baa59 | VVX 400 | 10.1.12.182 | 5.5.2 | On-site |
| 301 | Toy Shop | 0004f2dd7e86 | VVX 400 | 10.1.12.185 | 5.9.8 | Ready |
| 302 | Pet Shop | 0004f2887947 | VVX 410 | 10.1.12.68 | 5.0.1 | On-site |
| 303 | Sweet Shop | 0004f2de4e79 | VVX 400 | 10.1.12.180 | 5.9.8 | Ready |
| 304 | KCLC Admin | **0004f2de46b8** | VVX 400 | 10.1.10.176 | 5.9.8 | Ready |
| 305 | Sport Store | 0004f2ddfe05 | VVX 400 | 10.1.12.183 | 5.9.8 | Ready |
| 306 | Ice Cream Shop | 0004f27a7b32 | VVX 410 | 10.1.12.65 | 5.9.8 | Ready |
| 307 | Music Store | 0004f2822926 | VVX 400 | 10.1.12.186 | 5.9.8 | Ready |
| 308 | Flower Shop | 0004f2c6a847 | VVX 400 | 10.1.12.190 | 5.9.8 | Ready |
| 309 | Police Station | 0004f268033e | VVX 400 | 10.1.12.191 | 5.9.8 | Ready |
| 310 | Fire Station | 64167f909af0 | VVX 410 | 10.1.12.64 | Unknown | On-site |
| 312 | Club house | 64167f33f295 | VVX 401 | 10.1.12.187 | 5.9.8 | Ready |
| 313 | Rec Room | 0004f2de4b0e | VVX 400 | 10.1.12.181 | 5.9.8 | Ready |
| 314 | Media/Kitchen | 64167f31a96d | VVX 401 | 10.1.12.189 | 6.4.7 | ✅ Done |

### Active Constraints

- **Never touch ORL2 VPN for KCLC work** — 10.1.10.x and 10.1.12.x are local KCLC network, not ARC
- **HTTP file transfer:** Mac serves files at 10.1.10.202 → Windows (10.1.10.175) pulls via `Invoke-WebRequest`
- **Kids City (ext 799):** Still exists as a user account — needs to be deleted from Zoom portal or repurposed
- **Tanya (ext 703):** TFH staff, Main Site — do NOT move to KCLC, do NOT change
- **Ext 311:** Reserved N11 service code — never assign
- **TFTP serving directory:** Always `C:\tftp\` — never `C:\tftp\root\`

### Key Discoveries This Session

- `update_phone_user` MCP tool works on common area phones (not just user accounts) — pass common area ID as user_id
- Common area phones have `shared_office_*@thefathershouse.com` auto-generated email (vs real email for users)
- Zoom ZTP auto-upgrades firmware: 5.9.8.5760 → 6.4.7.4710 during provisioning
- Zoom MCP server is a custom Node.js server in `projects/mcp-servers/zoom-mcp-server/` — can add tools directly

---

## Phase Status

### ✅ COMPLETE
- Phase 1 (5.7.3): All 10 remote phones
- Phase 2 (5.9.8): All 10 remote phones + KCLC Admin (304)
- ZTP test: Ext 314 Media/Kitchen — online, 6.4.7.4710
- Zoom KCLC site: Created, all 13 extensions renumbered to 300-series, correct site
- Kids City ext 704 → 304 (now a proper common area phone)
- Zoom MCP server: 4 new device management tools added to phone.js

### ⏳ IMMEDIATE NEXT (this session's stopping point)

1. **Restart Claude / MCP server** to load new device tools
2. **Create device for ext 304** via `add_phone_device`:
   - First run `list_phone_devices` to get Polycom type integer from ext 314's device
   - Then `add_phone_device` for ext 304: MAC `00:04:F2:DE:46:B8`, model "VVX 400", assigned_to = ext 304 common area ID
3. **ZTP ext 304** (10.1.10.176) — provisioning server already set, phone trying to register
4. **ZTP batch** — 9 remaining phones (301-309, 312-313)

### ⏳ ZTP BATCH ORDER (9 phones)

| Ext | Room | IP | Model | ZTP URL suffix |
|-----|------|----|-------|---------------|
| 304 | KCLC Admin | 10.1.10.176 | VVX 400 | VVX400 |
| 301 | Toy Shop | 10.1.12.185 | VVX 400 | VVX400 |
| 303 | Sweet Shop | 10.1.12.180 | VVX 400 | VVX400 |
| 305 | Sport Store | 10.1.12.183 | VVX 400 | VVX400 |
| 306 | Ice Cream Shop | 10.1.12.65 | VVX 410 | VVX400 |
| 307 | Music Store | 10.1.12.186 | VVX 400 | VVX400 |
| 308 | Flower Shop | 10.1.12.190 | VVX 400 | VVX400 |
| 309 | Police Station | 10.1.12.191 | VVX 400 | VVX400 |
| 312 | Club house | 10.1.12.187 | VVX 401 | **VVX401** |
| 313 | Rec Room | 10.1.12.181 | VVX 400 | VVX400 |

### ⏳ ON-SITE REQUIRED

| Ext | Room | IP | Issue |
|-----|------|----|-------|
| 300 | Bus Station | 10.1.12.182 | Offline, VVX at 5.5.2, Edge E220 temp |
| 302 | Pet Shop | 10.1.12.68 | Firmware 5.0.1 → needs 5.4.7 → 5.7.3 → ZTP |
| 310 | Fire Station | 10.1.12.64 | Unknown admin password post-factory-reset |

### ⏳ POST-ZTP ZOOM BUILD-OUT

| Feature | Zoom Equivalent | Notes |
|---------|----------------|-------|
| Main inbound +13522680205 | Auto Receptionist | Decide: direct to 300 or IVR menu? |
| Paging (11 members, "KCLC Classrooms") | Zoom Paging Group | Members likely 301-310, 312-314 |
| Intercom (*76 + ext, 5s auto-answer) | Zoom Intercom Group | All 14 phones |
| After-hours routing | Auto Receptionist schedule | Need hours + menu script |

---

## Ideas & Discoveries

- Zoom MCP server is fully extensible — missing API endpoints can be added directly to `src/tools/phone.js`
- Pattern for new tools: add to `phoneTools` array, no server.js changes needed
- Consider adding auto-receptionist, call queue, and paging group tools to the MCP server for the post-ZTP build-out phase
- Kids City user (ext 799) should be deleted or converted — it's a zombie account now that 304 is a proper common area phone

---

**Checkpoint Version:** 3.0
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-04 02:02:25 EST
