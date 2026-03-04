KCLC-FEAT-20260304-015-SNNT-ztp-final-and-zoom-buildout

All firmware done. 3 phones completing ZTP. Post-ZTP Zoom build-out next.

Checkpoint: [CHECKPOINT_20260304_0757_all-firmware-complete-ztp-finishing.md](CHECKPOINT_20260304_0757_all-firmware-complete-ztp-finishing.md)
Model: sonnet — execution workflow, known patterns

Priorities:
1. Confirm Pet Shop (302), Fire Station (310), Bus Station VVX (300) ZTP complete and live
2. Remove temp Edge E220 device from ext 300 after VVX ZTPs
3. Keys & Positions on all remaining phones (6 speed dials each — see checkpoint)
4. Check ext 314 Media/Kitchen — showing offline, likely just unplugged
5. Auto Receptionist for +13522680205 (main KCLC number → ext 300 Bus Station)
6. Paging Group: KCLC Classrooms (members 301–310, 312–314)
7. Intercom Group: all 14 phones

Critical facts:
- KCLC network 10.1.10.x / 10.1.12.x is LOCAL — never use ORL2 VPN
- Mac HTTP server: python3 -m http.server 8080 --directory /private/tftpboot (MUST use --directory flag)
- Mac WiFi IP at KCLC: 192.168.1.176 (not 10.1.10.x) — but can reach Windows TFTP at 10.1.10.175
- Zoom MCP add_phone_device blocked (scope error) — use portal for device creation
- Speed dials: per-phone Keys & Positions ONLY — provisioning template approach does not work
- Ext 311 = reserved N11 — never assign; Tanya 703 = TFH Main Site — do NOT touch
- Kids City (ext 799) = zombie account — delete from portal
