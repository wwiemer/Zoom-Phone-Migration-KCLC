# KCLC Master Device Reference
# Zoom Phone Migration — Polycom VVX Inventory

**Created:** 2026-03-02
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC
**Status:** ACTIVE — updated as phones progress through upgrade
**Version:** 1.0.0

---

## Master Device Table

| # | Room | Model | Part # | MAC | MAC (TFTP) | LAN IP | VLAN | sipXcom Ext | Zoom Ext | DID | Firmware | TFTP Config | Zoom Status | Notes |
|---|------|-------|--------|-----|------------|--------|------|-------------|----------|-----|----------|-------------|-------------|-------|
| 1 | Bus Station | VVX 400 | 3111-46157-002 | 0004F27BAA59 | 0004f27baa59 | 10.1.12.182 | 12 | 200 | 300 | +13522680205 / +13527820706 | 5.5.2 ⏳ | Phase 1 ready | ⏳ Pending | Main KCLC number. Port complete 2026-03-02. **TEMPORARILY OFFLINE** — replaced with Poly Edge E220 while VVX upgrades in progress. Do last. |
| 2 | Toy Shop | VVX 400 | 3111-46157-002 | 0004F2DD7E86 | 0004f2dd7e86 | 10.1.12.185 | 12 | 201 | 301 | +13527820672 | 5.7.3.1797 🔄 | Phase 2 pending | ⏳ Pending | Factory reset 2026-03-03. Phase 1 confirmed. Password changed to 2307. |
| 3 | Pet Shop | VVX 410 | 3111-46162-001 | 0004F2887947 | 0004f2887947 | 10.1.12.68 | 12 | 202 | 302 | +13527820599 | **5.0.1.4068** ⚠️ | ❌ Blocked | ⏳ Pending | **ON-SITE REQUIRED** — BootROM 5.2.1.3271. Pre-Updater era. Cannot upgrade directly to 5.7.3 (uses polycom/5.0.1/ path structure). Needs intermediate firmware (5.4.x) research before upgrade attempt. |
| 4 | Sweet Shop | VVX 400 | 3111-46157-002 | 0004F2DE4E79 | 0004f2de4e79 | 10.1.12.180 | 12 | 203 | 303 | +13527820654 | 5.5.2 ⏳ | Phase 1 ready | ⏳ Pending | |
| 5 | KCLC Administrator | VVX 400 | 3111-46157-002 | 0004F2DE46B8 | 0004f2de46b8 | 10.1.10.176 | 10 | 204 | 304 | *(none)* | **5.9.8.5760** ✅ | Phase 2 complete | ⏳ ZTP pending | Different VLAN (10). First phone upgraded — full path confirmed. |
| 6 | Sport Store | VVX 400 | 3111-46157-002 | 0004F2DDFE05 | 0004f2ddfe05 | 10.1.12.183 | 12 | 205 | 305 | +13527820675 | 5.5.2 ⏳ | Phase 1 ready | ⏳ Pending | |
| 7 | Ice Cream Shop | VVX 410 | 3111-46162-001 | 0004F27A7B32 | 0004f27a7b32 | 10.1.12.65 | 12 | 206 | 306 | +13527820455 | 5.5.2.8571 ⏳ | Phase 1 ready | ⏳ Pending | Updater 5.7.2.21547. Standard two-step path. |
| 8 | Music Store | VVX 400 | 3111-46157-002 | 0004F2822926 | 0004f2822926 | 10.1.12.186 | 12 | 207 | 307 | +13527820695 | 5.5.2 ⏳ | Phase 1 ready | ⏳ Pending | |
| 9 | Flower Shop | VVX 400 | 3111-46157-002 | 0004F2C6A847 | 0004f2c6a847 | 10.1.12.190 | 12 | 208 | 308 | +13527820550 | 5.5.2 ⏳ | Phase 1 ready | ⏳ Pending | |
| 10 | Police Station | VVX 400 | 3111-46157-002 | 0004F268033E | 0004f268033e | 10.1.12.191 | 12 | 209 | 309 | +13527820614 | 5.5.2 ⏳ | Phase 1 ready | ⏳ Pending | sipXcom labeled "The Market" — actual room: Police Station |
| 11 | Fire Station | VVX 410 | 3111-46162-001 | 64167F909AF0 | 64167f909af0 | 10.1.12.64 | 12 | 210 | 310 | +13527820635 | **Unknown** ⚠️ | TFTP cfg points to 5.9.8 | ⏳ Pending | **ON-SITE REQUIRED** — Factory reset attempted remotely, unknown password blocking web UI access. Needs physical recovery. TFTP cfg already set for direct 5.9.8. |
| 12 | The Clubhouse | VVX 401 | 3111-48400-001 | 64167F33F295 | 64167f33f295 | 10.1.12.187 | 12 | 212 | 312 | +13527820631 | 5.5.2 ⏳ | Phase 1 ready | ⏳ Pending | |
| 13 | Rec Room | VVX 400 | 3111-46157-002 | 0004F2DE4B0E | 0004f2de4b0e | 10.1.12.181 | 12 | 213 | 313 | +13527820566 | 5.5.2 ⏳ | Phase 1 ready | ⏳ Pending | |
| 14 | Media/Kitchen | VVX 401 | 3111-48400-001 | 64167F31A96D | 64167f31a96d | 10.1.12.189 | 12 | 211 | 314 | +13527820626 | 5.7.3.1797 🔄 | Phase 2 pending | ⏳ Pending | Factory reset done 2026-03-02. Phase 1 confirmed 2026-03-03. Password changed to 2307. |

---

## Status Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Complete |
| ⏳ | Pending / not started |
| ⚠️ | Needs attention / investigation |
| ❌ | Blocked |

## Firmware Status Values

| Value | Meaning |
|-------|---------|
| `5.5.2 ⏳` | As-shipped, not yet upgraded |
| `5.7.3 🔄` | Phase 1 complete, Phase 2 pending |
| `5.9.8.5760 ✅` | Fully upgraded, ready for Zoom ZTP |

## Zoom Status Values

| Value | Meaning |
|-------|---------|
| `⏳ Pending` | Firmware upgrade not complete yet |
| `⏳ ZTP pending` | Firmware done, not yet pointed to Zoom |
| `🔄 ZTP in progress` | Provisioning URL set, provisioning |
| `✅ Live` | Registered and active on Zoom Phone |

---

## Firmware Upgrade Workflow (per phone)

```
Step 1 — TFTP Phase 1
  Point phone Boot Server → 10.1.36.71 (wired TFTP)
  Phone has per-device .cfg → pulls 5.7.3 model-specific firmware
  Confirm via WebUI: UC Software = 5.7.3.1797, Updater = 5.9.3.1730

Step 2 — Set Admin Password (between phases)
  https://[phone-ip] → Settings → Change Password
  New password: (see Strongbox → KCLC → "VVX Phone Admin")

Step 3 — TFTP Phase 2
  Edit /private/tftpboot/[mac].cfg → change -573 to -598 in APP_FILE_PATH
  Reboot phone → pulls 5.9.8 firmware
  Confirm via WebUI: UC Software = 5.9.8.5760, Updater = 5.9.8.4127

Step 4 — Zoom ZTP
  Register MAC in Zoom Phone admin portal
  Set extension number (300–314) and assign DID
  On phone → Provisioning Server → HTTPS → provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400
  Phone reboots → registers to Zoom
```

**To advance a phone from Phase 1 → Phase 2:** Edit one line in its `.cfg` file (change `-573` to `-598`). No file copying required.

---

## VVX 410 — Status (confirmed 2026-03-02)

All three VVX 410s confirmed as **3111-46162-001 Rev:A**.

| MAC | Room | Firmware | Updater | TFTP Config | Action |
|-----|------|----------|---------|-------------|--------|
| 0004F27A7B32 | Ice Cream Shop | 5.5.2.8571 | 5.7.2.21547 | ✅ Phase 1 ready | Standard path: 5.7.3 → 5.9.8 |
| 64167F909AF0 | Fire Station | 5.8.0.12386 | 5.9.5.12119 | ✅ Phase 2 ready | Skip Phase 1 — go straight to 5.9.8 |
| 0004F2887947 | Pet Shop | **5.0.1.4068** | BootROM 5.2.1 | ❌ Blocked | Pre-Updater era — investigate before touching |

**Pet Shop investigation needed:**
- Firmware 5.0.1 predates the Updater mechanism entirely (WebUI shows "BootROM Software Version" not "Updater Version")
- BootROM 5.2.1.3271 — unknown if it can accept 5.7.3 directly
- May require intermediate firmware step (e.g. 5.4.x) before 5.7.3
- Do NOT attempt upgrade until path is confirmed

---

## Out of Scope

| MAC | Description | Reason |
|-----|-------------|--------|
| 0004F28334A8 | Warren's VVX 500 at Office | Not a KCLC device |
| 0004F2C9DEDF | Ice Cream Shop (duplicate) | Did not register in 2026-03-01 live check — inactive |

---

## Zoom Extension Map (300 Range — All Clear)

| Ext | Room | DID |
|-----|------|-----|
| 300 | Bus Station | +13522680205 (main), +13527820706 |
| 301 | Toy Shop | +13527820672 |
| 302 | Pet Shop | +13527820599 |
| 303 | Sweet Shop | +13527820654 |
| 304 | KCLC Administrator | *(none — internal only)* |
| 305 | Sport Store | +13527820675 |
| 306 | Ice Cream Shop | +13527820455 |
| 307 | Music Store | +13527820695 |
| 308 | Flower Shop | +13527820550 |
| 309 | Police Station | +13527820614 |
| 310 | Fire Station | +13527820635 |
| 311 | *(RESERVED)* | N11 service code — do not assign |
| 312 | The Clubhouse | +13527820631 |
| 313 | Rec Room | +13527820566 |
| 314 | Media/Kitchen | +13527820626 |

---

**Created:** 2026-03-02
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC
**Version:** 1.0.0
**Status:** ACTIVE
