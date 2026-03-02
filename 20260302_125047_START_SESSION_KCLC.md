KCLC-FEAT-20260302-006-SNNT-vvx-factory-reset-5.7.3-clean-install

5.7.3 Container format accepted by 5.7.2 Updater (confirmed — "Could not get application name" gone). KCLC main +13522680205 live on Edge E220 ext 300. Remaining blocker: sipXcom HTTPS config in VVX 401 flash causing SSL errors → status 53. Fix: factory reset + WiFi TFTP.

Checkpoint: [CHECKPOINT_20260302_1233_5.7.3-accepted-ssl-blocker-factory-reset-next.md](CHECKPOINT_20260302_1233_5.7.3-accepted-ssl-blocker-factory-reset-next.md)
Model: sonnet — hands-on phone provisioning, TFTP config, multi-step upgrade

Priorities:
1. Verify KCLC WiFi IP (run `ipconfig getifaddr en0` — should be 10.1.57.125, NOT 192.168.1.x). Start TFTP. Ping 10.1.57.125 from VVX 401 (Menu → Diagnostics → Ping) to confirm VLAN 12 routing.
2. Factory reset VVX 401: interrupt boot → Cancel → Setup (456) → Factory Reset → reconfigure Boot Server: Static, 10.1.57.125. Reboot → 5.7.3 should install clean.
3. After 5.7.3 on VVX 401: swap sip.ld to 5.9.8 split, update sip.ver → 5.9.8.5760, reboot for final upgrade.

Critical facts:
- TFTP root: /private/tftpboot/ | sip.ld = 5.7.3 VVX401 | sip.ver = 5.7.3.1797 | 3111-46157-002.sip.ld still 5.9.8 (needs replacing with 5.7.3 before VVX 400 upgrade)
- WiFi TFTP IP: 10.1.57.125 (KCLC network) — NOT 192.168.1.176 (different subnet, unreachable by phones)
- macOS TFTP daemon binds 0.0.0.0 — reload: `sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist`
- DO NOT TOUCH TFH extensions: 202, 703, 230, 232, 233, 235, 239, 240, 241, 245, 246, 282 | 311 reserved
- 5.7.3 split firmware: ~/Library/CloudStorage/Dropbox/Polycom/VVX/polycom-uc-software-5-7-3-rts26-d-release-sig-split/
