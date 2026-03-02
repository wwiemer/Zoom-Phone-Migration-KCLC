KCLC-FEAT-20260302-003-SNNT-vvx400-firmware-hybrid-tftp-http

VVX400 firmware upgrade stalled: phone gets per-device configs via HTTP but not the master 000000000000.cfg, so firmware download never triggers. Python HTTP server running on port 8080. Port deadline is 11:30 AM ET today.

Checkpoint: [CHECKPOINT_20260302_0421_vvx400-firmware-http-upgrade-stalled.md](projects/Zoom-Phone-Migration-KCLC/CHECKPOINT_20260302_0421_vvx400-firmware-http-upgrade-stalled.md)
Model: sonnet — known problem, clear next step to try

Priorities:
1. URGENT FIRST: Set Twilio forward +13522680205 → +13527820706 before 11:30 AM ET
2. Update 000000000000.cfg APP_FILE_PATH to full HTTP URL, switch phone back to TFTP, reboot
3. After firmware upgrade: ZTP → Zoom, create KCLC Site, provision all 14 phones

Critical facts:
- MacBook Air IP: 10.1.10.156 | TFTP root: ~/Dropbox/TFTP-Root/ | HTTP: python3 -m http.server 8080
- Test phone: MAC 0004f2de46b8, IP 10.1.10.176, firmware 5.5.2 (needs 5.9.8 for TLS 1.2)
- Best fix: set APP_FILE_PATH="http://10.1.10.156:8080/sip.ld" in 000000000000.cfg, use TFTP for config phase
- pumpKIN crashes on 397 MB sip.ld — HTTP only for firmware; web UI Custom Server upgrade path doesn't work
- DO NOT touch TFH extensions: 202, 703, 230-246 range
