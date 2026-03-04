# Checkpoint: ZTP Reference Table Complete — Manual Portal Workflow

**Created:** 2026-03-04 04:38:46 EST
**Session:** KCLC-FEAT-20260304-013-SNNT-ztp-provisioning-and-zoom-registration
**Model:** Claude Sonnet 4.6
**Status:** ZTP reference table complete — manual device creation in Zoom portal — ZTP batch ready to execute

---

## Summary

MCP server restarted with 4 new device tools active. Attempted to use `add_phone_device` MCP tool for ext 304 but the Zoom API requires a numeric `type` integer for device brand — the `list_phone_devices` response only returns string names ("Polycom vvx401"), not the integer. API documentation was not publicly accessible to resolve this. Warren will create devices manually in Zoom Phone portal (Phones & Devices → Add Device). Full ZTP batch reference table produced with MACs, models, IPs, and provisioning URLs for all 10 remaining phones.

---

## Inherited Context (v3.0)

### System Facts

- **TFTP Server:** Windows 10.1.10.175, serving from `C:\tftp\` (NOT `C:\tftp\root\`) — always edit files there
- **Mac IP on KCLC network:** 10.1.10.202 (en0) — same subnet as TFTP server
- **HTTP server on Mac:** Used to transfer files to Windows via PowerShell `Invoke-WebRequest` — Mac is the server, Windows pulls
- **All 10 remote phones:** admin password = 2307, firmware 5.9.8.5760
- **ZTP URL (VVX 400/410):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
- **ZTP URL (VVX 401):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401`
- **Zoom auto-upgrades firmware to 6.4.7.4710 during ZTP** (confirmed from ext 314)
- **KCLC site ID:** `dUxbXwstSW6rvziP3tF1xA` (name: "KidCity Learning Center")
- **Kids City user** (kclc@thefathershouse.com) moved to ext 799 — no longer occupying 304
- **Pet Shop 5.4.7 firmware:** Downloaded to `/Users/warrenwiemer/Library/CloudStorage/Dropbox/Polycom/VVX/Polycom-UC-Software-5-4-7-rts27-release-sig-split/` — file needed: `3111-46162-001.sip.ld` → rename to `3111-46162-001-547.sip.ld` on TFTP server
- **zoom-mcp-server submodule:** Points to mattcoatsworth/zoom-mcp-server — no push access; local commits ahead of remote but cannot sync

### Zoom KCLC Site — Current State (Verified Live)

| Ext | Room | DID | Type | Device | Status |
|-----|------|-----|------|--------|--------|
| 300 | KCLC - Bus Station | +13522680205 | commonArea | Poly edge-e220 | ✅ Online (temp) |
| 301 | Toy Shop | +13527820672 | commonArea | — | ⏳ ZTP ready |
| 302 | Pet Shop | +13527820599 | commonArea | — | ⏳ On-site (5.0.1) |
| 303 | Sweet shop | +13527820654 | commonArea | — | ⏳ ZTP ready |
| 304 | KCLC Admin | *(none)* | commonArea | — | ⏳ Device not yet created in portal |
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

### ZTP Batch Reference — All 10 Phones

| Pri | Ext | Room | IP | Model | MAC Address | Provisioning URL |
|-----|-----|------|----|-------|-------------|-----------------|
| 1 | 304 | KCLC Admin | 10.1.10.176 | VVX 400 | `00:04:F2:DE:46:B8` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| 2 | 301 | Toy Shop | 10.1.12.185 | VVX 400 | `00:04:F2:DD:7E:86` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| 3 | 303 | Sweet Shop | 10.1.12.180 | VVX 400 | `00:04:F2:DE:4E:79` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| 4 | 305 | Sports Store | 10.1.12.183 | VVX 400 | `00:04:F2:DD:FE:05` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| 5 | 306 | Ice Cream Shop | 10.1.12.65 | VVX 410 | `00:04:F2:7A:7B:32` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| 6 | 307 | Music Store | 10.1.12.186 | VVX 400 | `00:04:F2:82:29:26` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| 7 | 308 | Flower Shop | 10.1.12.190 | VVX 400 | `00:04:F2:C6:A8:47` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| 8 | 309 | Police Station | 10.1.12.191 | VVX 400 | `00:04:F2:68:03:3E` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |
| 9 | 312 | Club House | 10.1.12.187 | VVX 401 | `64:16:7F:33:F2:95` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401` |
| 10 | 313 | Rec Room | 10.1.12.181 | VVX 400 | `00:04:F2:DE:4B:0E` | `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400` |

### Active Constraints

- **Never touch ORL2 VPN for KCLC work** — 10.1.10.x and 10.1.12.x are local KCLC network, not ARC
- **HTTP file transfer:** Mac serves files at 10.1.10.202 → Windows (10.1.10.175) pulls via `Invoke-WebRequest`
- **Kids City (ext 799):** Still exists as a user account — needs to be deleted from Zoom portal or repurposed
- **Tanya (ext 703):** TFH staff, Main Site — do NOT move to KCLC, do NOT change
- **Ext 311:** Reserved N11 service code — never assign
- **TFTP serving directory:** Always `C:\tftp\` — never `C:\tftp\root\`

### Key Discoveries This Session

- Zoom Phone API `POST /phone/devices` requires numeric `type` integer for device brand — not documented publicly and not returned by `list_phone_devices` or `get_phone_device`
- MCP `add_phone_device` tool as designed cannot auto-discover the type integer — needs Zoom API docs or trial/error to find Polycom value
- Manual device creation in Zoom portal is faster than resolving the API integer issue
- zoom-mcp-server remote (mattcoatsworth/zoom-mcp-server) blocks push from wwiemer — local commits are ahead, cannot sync without forking or different auth

---

## Phase Status

### ✅ COMPLETE
- Phase 1 (5.7.3): All 10 remote phones
- Phase 2 (5.9.8): All 10 remote phones + KCLC Admin (304)
- ZTP test: Ext 314 Media/Kitchen — online, 6.4.7.4710
- Zoom KCLC site: Created, all 13 extensions renumbered to 300-series, correct site
- Kids City ext 704 → 304 (now a proper common area phone)
- Zoom MCP server: 4 new device management tools added (MCP restart complete)
- ZTP batch reference table: All 10 phones with MAC, model, IP, provisioning URL

### ⏳ IMMEDIATE NEXT

1. **Create device for ext 304 in Zoom portal** (manually):
   - Zoom Admin → Phone → Phones & Devices → Add Device
   - MAC: `00:04:F2:DE:46:B8`, Model: VVX 400, assign to ext 304 common area
2. **ZTP batch** — 10 phones (ext 304 first, then 301, 303, 305-309, 312, 313)
   - Phone web UI: `http://<IP>` → Settings → Provisioning Server → enter URL
   - Admin pw: `2307`
3. **On-site phones** (300, 302, 310) — require physical access

### ⏳ POST-ZTP ZOOM BUILD-OUT

| Feature | Zoom Equivalent | Notes |
|---------|----------------|-------|
| Main inbound +13522680205 | Auto Receptionist | Decide: direct to 300 or IVR menu? |
| Paging (11 members, "KCLC Classrooms") | Zoom Paging Group | Members likely 301-310, 312-314 |
| Intercom (*76 + ext, 5s auto-answer) | Zoom Intercom Group | All 14 phones |
| After-hours routing | Auto Receptionist schedule | Need hours + menu script |

---

## Ideas & Discoveries

- Zoom MCP `add_phone_device` needs a fix: should accept brand string ("Polycom") and map to integer internally, OR query Zoom's `/phone/device_types` endpoint if it exists
- Consider forking zoom-mcp-server to WarrenTEK GitHub org to gain push access and maintain our customizations
- Kids City user (ext 799) should be deleted — zombie account now that 304 is proper common area phone

---

**Checkpoint Version:** 3.0
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-04 04:38:46 EST
