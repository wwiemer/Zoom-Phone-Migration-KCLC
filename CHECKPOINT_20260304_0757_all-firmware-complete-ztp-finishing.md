# Checkpoint: All Firmware Upgrades Complete — ZTP Finishing

**Created:** 2026-03-04 07:57:14 EST
**Session:** KCLC-FEAT-20260304-014-SNNT-ztp-batch-execution
**Model:** Claude Sonnet 4.6
**Status:** All 14 phones at 5.9.8 — 11 ZTP'd live — 3 ZTP in progress — post-ZTP build-out next

---

## Summary

All standard and special firmware upgrades are complete. Bus Station original VVX (ext 200→300) upgraded 5.5.2→5.7.3→5.9.8 via TFTP. Pet Shop (ext 302) completed special pre-Updater path: 5.4.7→5.7.3→5.9.8 via TFTP. Fire Station (ext 310) confirmed at 5.9.8. All batch ZTP phones (301–309, 312–313, 304) confirmed live in Zoom. Three phones still completing ZTP (300, 302, 310). Speed dial line keys confirmed via per-phone Keys & Positions; provisioning template approach does not work (Zoom overwrites). Post-ZTP build-out (Auto Receptionist, Paging, Intercom) is next.

---

## Inherited Context (v3.0)

### System Facts

- **TFTP Server:** Windows 10.1.10.175, serving from `C:\tftp\` — files transferred via Mac HTTP server
- **Mac HTTP server:** `python3 -m http.server 8080 --directory /private/tftpboot` at 192.168.1.176:8080 — **MUST use `--directory` flag** or server roots to /private/tmp
- **Mac WiFi IP at KCLC:** 192.168.1.176 (NOT 10.1.10.202 — that was a prior session) — can still ping 10.1.10.175
- **TFTP cfg files:** `/private/tftpboot/` on Mac — source of truth; pushed to Windows via Invoke-WebRequest
- **All phones:** admin password = 2307, final ZTP firmware = 6.4.7.4710
- **ZTP URL (VVX 400/410):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
- **ZTP URL (VVX 401):** `https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401`
- **KCLC site ID:** `dUxbXwstSW6rvziP3tF1xA` (name: "KidCity Learning Center")
- **Pet Shop 5.4.7 sip.ld:** `/private/tftpboot/3111-46162-001-547.sip.ld` (40MB, already on Windows C:\tftp\)
- **Zoom MCP add_phone_device:** Blocked — scope `phone:write:device:admin` not in token — use Zoom portal for all device creation
- **Speed dials:** Per-phone Keys & Positions ONLY — provisioning template approach does NOT work (Zoom overwrites)
- **Kids City (ext 799):** Zombie account — delete or reassign

### Zoom KCLC Site — Current State

| Ext | Room | MAC | Model | DID | Firmware | Zoom Status |
|-----|------|-----|-------|-----|----------|-------------|
| 300 | Bus Station | `0004F27BAA59` (VVX) / `4825676D548C` (E220 temp) | VVX 400 | +13522680205 | 5.9.8 ✅ | ⏳ ZTP in progress — remove Edge E220 after VVX ZTPs |
| 301 | Toy Shop | `0004F2DD7E86` | VVX 400 | +13527820672 | 6.4.7 | ✅ Live |
| 302 | Pet Shop | `0004F2887947` | VVX 410 | +13527820599 | 5.9.8 ✅ | ⏳ ZTP in progress |
| 303 | Sweet Shop | `0004F2DE4E79` | VVX 400 | +13527820654 | 6.4.7 | ✅ Live |
| 304 | KCLC Admin | `0004F2DE46B8` | VVX 400 | *(none)* | 6.4.7 | ✅ Live |
| 305 | Sports Store | `0004F2DDFE05` | VVX 400 | +13527820675 | 6.4.7 | ✅ Live |
| 306 | Ice Cream Shop | `0004F27A7B32` | VVX 410 | +13527820455 | 6.4.7 | ✅ Live |
| 307 | Music Store | `0004F2822926` | VVX 400 | +13527820695 | 6.4.7 | ✅ Live |
| 308 | Flower Shop | `0004F2C6A847` | VVX 400 | +13527820550 | 6.4.7 | ✅ Live |
| 309 | Police Station | `0004F268033E` | VVX 400 | +13527820614 | 6.4.7 | ✅ Live |
| 310 | Fire Station | `64167F909AF0` | VVX 410 | +13527820635 | 5.9.8 ✅ | ⏳ ZTP in progress |
| 311 | *(RESERVED)* | — | — | — | — | N11 — never assign |
| 312 | Club House | `64167F33F295` | VVX 401 | +13527820631 | 6.4.7 | ✅ Live |
| 313 | Rec Room | `0004F2DE4B0E` | VVX 400 | +13527820566 | 6.4.7 | ✅ Live |
| 314 | Media/Kitchen | `64167F31A96D` | VVX 401 | +13527820626 | 6.4.7 | ⚠️ Offline (unplugged?) |

### Active Constraints

- **Never touch ORL2 VPN for KCLC** — 10.1.10.x and 10.1.12.x are local KCLC network
- **Tanya (ext 703):** TFH Main Site staff — do NOT move or change
- **Ext 311:** Reserved N11 — never assign
- **Bus Station (ext 300):** Has TWO devices — temp Edge E220 must be removed after VVX ZTPs

### Speed Dial Config (all KCLC phones — same for all)

| Key | Type | Number | Alias |
|-----|------|--------|-------|
| 2 | Speed Dial | 3523151815 | The Father's House |
| 3 | Speed Dial | 8002221222 | Poison Control |
| 4 | Speed Dial | 3524063357 | Lisa (Mobile) |
| 5 | Speed Dial | 3529733068 | Dorothy (Mobile) |
| 6 | BLF | 304 | KCLC Admin |
| 7 | BLF | 300 | Bus Station |

Configure via: Zoom Admin → Phone System Mgmt → Users & Rooms → Common Area Phones → [phone] → Settings → Keys & Positions → Manage Keys

---

## Phase Status

### ✅ COMPLETE
- All firmware upgrades — all 14 phones at 5.9.8 (or 6.4.7 post-ZTP)
- Pet Shop special path: 5.0.1 → 5.4.7 → 5.7.3 → 5.9.8 ✅
- Bus Station VVX: 5.5.2 → 5.7.3 → 5.9.8 ✅
- Fire Station: 5.8.0 → 5.9.8 ✅
- Batch ZTP (11 phones): 301–309, 312–313, 304 — all live ✅
- MASTER-DEVICE-REFERENCE.md updated to v2.0.0
- Speed dial translation (SipXcom 200-series → Zoom 300-series)

### ⏳ IMMEDIATE NEXT

1. **Complete ZTP for 3 remaining phones:**
   - Ext 302 (Pet Shop): portal device entry + ZTP URL on phone at 10.1.12.68
   - Ext 310 (Fire Station): portal device entry (Warren handling) + ZTP URL on phone at 10.1.12.64
   - Ext 300 (Bus Station VVX): portal device entry (MAC 0004F27BAA59) + ZTP URL on phone at 10.1.12.182 → then remove Edge E220 device

2. **Keys & Positions** on all remaining phones not yet configured

3. **Ext 314 (Media/Kitchen):** Check if just unplugged — verify online once plugged in

### ⏳ POST-ZTP ZOOM BUILD-OUT

| Feature | Zoom Equivalent | Notes |
|---------|----------------|-------|
| Main inbound +13522680205 | Auto Receptionist | Assign to Bus Station (300) or IVR menu |
| Paging ("KCLC Classrooms") | Zoom Paging Group | Members: 301–310, 312–314 |
| Intercom (*76 + ext) | Zoom Intercom Group | All 14 phones |
| After-hours routing | Auto Receptionist schedule | Need hours + menu script from Warren |

---

## Ideas & Discoveries This Session

- Zoom provisioning template (Polycom XML) is overwritten by Zoom — cannot use for speed dials
- Pet Shop pre-Updater 5.0.1 path confirmed: 5.4.7 → 5.7.3 → 5.9.8 (all working)
- Mac HTTP server `--directory` flag is critical — omitting it causes server to root at /private/tmp
- MacBook Air WiFi gets 192.168.1.x at KCLC but can still reach 10.1.10.175 via KCLC router

---

**Checkpoint Version:** 3.0
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Created:** 2026-03-04 07:57:14 EST
