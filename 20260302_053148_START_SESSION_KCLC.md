Session: KCLC-FEAT-20260302-004-SNNT-vvx400-firmware-verify-and-ztp
Project: Zoom Phone Migration — KCLC
Model: claude-sonnet-4-6 (Sonnet recommended — standard provisioning work)
Checkpoint: CHECKPOINT_20260302_0531_vvx400-split-firmware-tftp-ready-upgrade-pending.md

CONTEXT: Split firmware (3111-46157-002.sip.ld, 41MB) is in /private/tftpboot/ and TFTP daemon is confirmed working. 000000000000.cfg has APP_FILE_PATH="3111-46157-002.sip.ld". Phone was rebooted but firmware upgrade NOT yet confirmed — 32-second reboot was too fast for a firmware flash (should take 3-5 min).

FIRST TASK: Verify firmware version on test phone (MAC 0004f2de46b8, IP 10.1.10.176).
- Phone keypad: Menu → Settings → Status → Platform → Application → looking for 5.9.8
- If still 5.5.2: ensure TFTP provisioning server is set to 10.1.10.156 in web UI, reboot
- Key gotcha: DHCP on VLAN 12 serves ucs05.warrentek.net as TFTP server — web UI must explicitly override

AFTER FIRMWARE CONFIRMED:
1. ZTP → Zoom provisioning on test phone
2. Create KCLC Site in Zoom (admin.zoom.us)
3. Provision remaining 13 phones
4. Check port status: +13522680205 was scheduled to port 11:30 AM ET today

CRITICAL RULES:
- APP_FILE_PATH = relative filename ONLY (not full HTTP URL)
- DO NOT use pumpKIN — use macOS built-in TFTP daemon
- DO NOT use web UI Custom Server upgrade path
- DO NOT touch TFH extensions: 202, 703, 230, 232, 233, 235, 239, 240, 241, 245, 246, 282
