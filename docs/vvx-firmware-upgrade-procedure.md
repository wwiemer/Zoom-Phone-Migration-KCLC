# VVX 400/401 Firmware Upgrade Procedure
# KCLC Zoom Phone Migration

**Created:** 2026-03-02
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC
**Status:** CONFIRMED WORKING — full path 5.5.2 → 5.7.3 → 5.9.8 validated on VVX 400 (0004f2de46b8) 2026-03-02
**Version:** 1.2.0

---

## Summary

All KCLC VVX phones shipped at firmware **5.5.2.8571** with **Updater 5.7.2.21547**.
The Updater version is the critical constraint — it only accepts the old Container format.
5.8.x and 5.9.x firmware use Container.01/Container.31 which the old Updater **rejects**.
5.7.3 uses the old Container format and **is accepted**.

**The upgrade path:**
```
5.5.2.8571 (Updater 5.7.2)
    ↓ TFTP Phase 1
5.7.3.1797 (Updater auto-upgrades to 5.9.3)
    ↓ Set admin password via WebUI (https://[phone-ip]) ← do this NOW, before Phase 2
5.9.8.5760
    ↓ TFTP Phase 2
Zoom ZTP provisioning
```

After 5.7.3 installs, the Updater automatically upgrades from 5.7.2.21547 → **5.9.3.1730**, which
can handle Container.31. The 5.9.8 install then runs cleanly on the second reboot.
After 5.9.8 installs, the Updater upgrades again to **5.9.8.4127**.

**Confirmed version table:**

| Stage | UC Software | Updater |
|-------|-------------|---------|
| Factory / as-shipped | 5.5.2.8571 | 5.7.2.21547 |
| After Phase 1 (TFTP 5.7.3) | 5.7.3.1797 | 5.9.3.1730 |
| After Phase 2 (TFTP 5.9.8) | **5.9.8.5760** | **5.9.8.4127** |

---

## TFTP Server Setup (macOS)

### Requirements
- Use **wired Ethernet** (en6) — gets IP in 10.1.x.x range, same routing domain as phones
- **Do NOT use WiFi** on a different SSID (e.g. 192.168.1.x) — TFTP ephemeral port handoff
  is blocked by the inter-subnet router even when ping (ICMP) succeeds
- **Do NOT use pumpKIN** — crashes on large TFTP file transfers (~42–46 MB)
- Use **macOS built-in TFTP daemon** only

### TFTP Root
```
/private/tftpboot/
```
All file operations in this directory require `sudo`.

### Start / Restart TFTP Daemon
```bash
sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist
```

### Verify TFTP is Serving
```bash
tftp 127.0.0.1 <<'EOF'
get sip.ver /tmp/sip_ver_test.txt
quit
EOF
cat /tmp/sip_ver_test.txt
```

### Verify Wired IP
```bash
ipconfig getifaddr en6
# Should return 10.1.36.71 (DHCP — may vary)
```

---

## Firmware Source Files

All firmware is stored in Dropbox:

| Version | Folder |
|---------|--------|
| 5.7.3.1797 | `~/Library/CloudStorage/Dropbox/Polycom/VVX/polycom-uc-software-5-7-3-rts26-d-release-sig-split/` |
| 5.9.8.5760 | `~/Library/CloudStorage/Dropbox/Polycom/VVX/Poly_UC_Software_5_9_8_release_sig_split/` |

### Model-to-File Mapping

| Model | Part Number | Firmware File |
|-------|-------------|---------------|
| VVX 400 | 3111-46157-002 | `3111-46157-002.sip.ld` |
| VVX 401 | 3111-48400-001 | `3111-48400-001.sip.ld` (also used as generic `sip.ld`) |

---

## TFTP Root File Structure

```
/private/tftpboot/
├── 000000000000.cfg          # Master config — APP_FILE_PATH="sip.ld"
├── sip.ver                   # Version manifest (e.g. "5.7.3.1797")
├── sip.ld                    # VVX 401 firmware (generic fallback)
├── 3111-46157-002.sip.ld     # VVX 400 model-specific firmware
├── 3111-48400-001.sip.ld     # VVX 401 model-specific firmware
├── [MAC].cfg                 # Per-device config — sets model-specific APP_FILE_PATH
└── ...
```

### 000000000000.cfg (master config)
```xml
<?xml version="1.0" standalone="yes"?>
<APPLICATION APP_FILE_PATH="sip.ld" CONFIG_FILES="[PHONE_MAC_ADDRESS].cfg"
  MISC_FILES="" LOG_FILE_DIRECTORY="" OVERRIDES_DIRECTORY=""
  CONTACTS_DIRECTORY="" LICENSE_DIRECTORY="" USER_PROFILES_DIRECTORY=""
  CALL_LISTS_DIRECTORY="" COREFILE_DIRECTORY="" CERTIFICATE_DIRECTORY="" />
```

### Per-device config for VVX 400 (e.g. 0004f2de46b8.cfg)
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<polycomConfig xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:noNamespaceSchemaLocation="polycomConfig.xsd">
  <APPLICATION APP_FILE_PATH="3111-46157-002.sip.ld" />
</polycomConfig>
```

**Critical:** The per-device config MUST override APP_FILE_PATH to the model-specific
firmware file. Without it, the phone falls back to the master config's `sip.ld`, which
is VVX 401 firmware and will not install on a VVX 400.

---

## Step-by-Step Procedure

### Phase 1 — Load 5.7.3 into TFTP

```bash
# Copy 5.7.3 VVX 400 firmware
sudo cp ~/Library/CloudStorage/Dropbox/Polycom/VVX/polycom-uc-software-5-7-3-rts26-d-release-sig-split/3111-46157-002.sip.ld \
  /private/tftpboot/3111-46157-002.sip.ld

# Copy 5.7.3 VVX 401 firmware (used as generic sip.ld)
sudo cp ~/Library/CloudStorage/Dropbox/Polycom/VVX/polycom-uc-software-5-7-3-rts26-d-release-sig-split/3111-48400-001.sip.ld \
  /private/tftpboot/sip.ld

# Set version manifest
echo "5.7.3.1797" | sudo tee /private/tftpboot/sip.ver
```

### Phase 1 — Create Per-Device Config

For each VVX 400, create `/private/tftpboot/[mac-lowercase-nocolons].cfg` with:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<polycomConfig xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:noNamespaceSchemaLocation="polycomConfig.xsd">
  <APPLICATION APP_FILE_PATH="3111-46157-002.sip.ld" />
</polycomConfig>
```

### Phase 1 — Configure Phone Boot Server

On the phone:
1. **Menu → Settings → Advanced** (password: `456`)
2. **Network Configuration → Provisioning Server**
3. Server Type: **TFTP**
4. Server Address: **[wired IP — e.g. 10.1.36.71]** (verify with `ipconfig getifaddr en6`)
5. Save → reboot

Phone downloads ~42 MB firmware and reboots. After install:
- UC Software: **5.7.3.1797** ✅
- Updater: **5.9.3.1730** ✅ (auto-upgraded — critical for Phase 2)

### Between Phase 1 and Phase 2 — Set Admin Password via WebUI

After 5.7.3 installs, the phone runs older firmware that does **not** enforce a mandatory
password change. Access the WebUI now to set the password before upgrading to 5.9.8.

5.9.8 enforces a password change on first WebUI login and the default `456` cannot be
re-used. Setting it here carries the password forward through the 5.9.8 upgrade.

1. Open browser → `https://[phone-ip]` (accept self-signed cert warning)
2. Login: Username `Polycom`, Password `456`
3. Navigate to **Settings → Change Password**
4. Current password: `456`
5. New password: *(see Strongbox → KCLC → "VVX Phone Admin")*
6. Save

**Notes:**
- 5.9.8 WebUI is HTTPS only (HTTP returns nothing) — always use `https://[phone-ip]`
- 5.9.8 enforces password change on first login — default `456` must be changed
- Password complexity: 4-digit numeric is accepted (e.g. `2307`) — full complexity not required
- WebUI branding changes from "Polycom" to "poly" after 5.9.8 — cosmetic only
- **Admin credentials stored in:** Strongbox → KCLC → "VVX Phone Admin"

### Phase 2 — Swap TFTP to 5.9.8

```bash
# Copy 5.9.8 VVX 400 firmware
sudo cp ~/Library/CloudStorage/Dropbox/Polycom/VVX/Poly_UC_Software_5_9_8_release_sig_split/3111-46157-002.sip.ld \
  /private/tftpboot/3111-46157-002.sip.ld

# Copy 5.9.8 VVX 401 firmware
sudo cp ~/Library/CloudStorage/Dropbox/Polycom/VVX/Poly_UC_Software_5_9_8_release_sig_split/3111-48400-001.sip.ld \
  /private/tftpboot/sip.ld

# Update version manifest
sudo cp ~/Library/CloudStorage/Dropbox/Polycom/VVX/Poly_UC_Software_5_9_8_release_sig_split/sip.ver \
  /private/tftpboot/sip.ver
```

### Phase 2 — Reboot Phone

Reboot the phone (it still has Boot Server pointed at TFTP from Phase 1).
Phone downloads ~41 MB firmware and reboots to **5.9.8.5760** ✅.

### Phase 3 — Provision to Zoom Phone

After 5.9.8 is confirmed:
1. Register MAC in Zoom Phone admin portal
2. On phone: Menu → Settings → Advanced (456) → Network → Provisioning Server
3. Server Type: **HTTPS**
4. Server Address: `provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400`
5. Save → reboot → phone provisions from Zoom ZTP

---

## Notes on Phones with sipXcom Config in Flash

All KCLC phones were previously provisioned by sipXcom. Many have the sipXcom provisioning
URL and credentials baked into flash. This causes TFTP provisioning to fail silently — the
phone downloads nothing and falls back to its old sipXcom config.

### How to Diagnose — Log Indicators

Check the phone's boot log. If you see either of these lines, the phone has sipXcom in flash
and needs a factory reset before TFTP will work:

```
sip|*|00|Sip Register Usr:2xx Dsp:[Room Name] Auth:'2xx/[mac]'
res|4|00|Failed to download file WTEK-Polycom-VVX-Bkgrd_320x240_v2.jpg
```

The WTEK background image reference is the clearest indicator — sipXcom served a
custom branded wallpaper. If the phone is looking for it, sipXcom config is in flash.

### Confirmed Affected Phones

| Phone | Evidence |
|-------|---------|
| VVX 401 — Media/Kitchen (64167f31a96d) | SSL errors → status 53. Factory reset done 2026-03-02. |
| VVX 410 — Fire Station (64167f909af0) | Registered as ext 210 "The Paint Shop KCLC", WTEK background. Factory reset needed. |

### Not Affected

| Phone | Evidence |
|-------|---------|
| VVX 400 — KCLC Admin (0004f2de46b8) | TFTP provisioning worked without factory reset. |

### Rule of Thumb

**Assume all phones need factory reset.** Check logs first — if you see the WTEK background
image line or sipXcom SIP registration, factory reset is required. If TFTP provisioning
starts cleanly without those log entries, factory reset is not needed.

### Factory Reset Procedure (VVX 400/401/410)

1. Unplug and replug power
2. During boot → press **Cancel**
3. Enter **Setup** (password: `456`)
4. Navigate to **Factory Reset** → confirm
5. After reboot, configure Boot Server:
   - Menu → Settings → Advanced (456) → Network → Provisioning Server
   - Type: **TFTP**, Address: **10.1.36.71**
6. Save → reboot → phone picks up TFTP config cleanly

---

## Phone Inventory — KCLC (VVX 400/401)

| MAC | Description | Part # | Status |
|-----|-------------|--------|--------|
| 0004f2de46b8 | KCLC Administrator (was x204) | 3111-46157-002 | ✅ 5.9.8.5760 confirmed 2026-03-02 |
| 64167f31a96d | Media/Kitchen (was x211, VVX 401) | 3111-48400-001 | 🔄 Factory reset done, 5.7.3 pending |
| 0004f27baa59 | KCLC Bus Station | 3111-46157-002 | ⏳ Pending |
| 0004f2822926 | Music Store | 3111-46157-002 | ⏳ Pending |
| 0004f2c6a847 | Flower Shop | 3111-46157-002 | ⏳ Pending |
| 0004f2dd7e86 | Toy Shop | 3111-46157-002 | ⏳ Pending |
| 0004f2ddfe05 | Sport Store | 3111-46157-002 | ⏳ Pending |
| 0004f2de4e79 | Sweet Shop | 3111-46157-002 | ⏳ Pending |
| 0004f268033e | The Market | 3111-46157-002 | ⏳ Pending |
| 0004f2de4b0e | Resource Room | 3111-46157-002 | ⏳ Pending |
| 64167f33f295 | The Clubhouse (VVX 401) | 3111-48400-001 | ⏳ Pending |

**Not in scope / separate procedure:**
- VVX 410 models (3111-46161/46162) — different part number, need separate firmware files
- VVX 500 (0004f28334a8) — Warren's office phone

---

**Created:** 2026-03-02
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC
**Status:** PRODUCTION
