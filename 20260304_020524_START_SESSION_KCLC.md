KCLC-FEAT-20260304-013-SNNT-ztp-provisioning-and-zoom-registration

Zoom KCLC site fully configured — all 13 extensions on 300-series, KidCity Learning Center site. Ext 314 ZTP confirmed (6.4.7.4710). Ext 304 fixed as common area phone. MCP server updated with 4 new device tools — restart required before using them.

Checkpoint: [CHECKPOINT_20260304_0202_zoom-config-complete-ztp-batch-ready.md](CHECKPOINT_20260304_0202_zoom-config-complete-ztp-batch-ready.md)
Model: sonnet — multi-step workflow with known patterns

Priorities:
1. Restart Claude/MCP server — then list_phone_devices to get Polycom type int from ext 314, then add_phone_device for ext 304 (MAC 00:04:F2:DE:46:B8, VVX 400)
2. ZTP ext 304 (10.1.10.176) and batch ZTP 9 remaining phones (301,303,305-309,312,313)
3. Post-ZTP build-out: Auto Receptionist for +13522680205, Paging Group, Intercom Group

Critical facts:
- KCLC network 10.1.10.x / 10.1.12.x is LOCAL — never use ORL2 VPN for KCLC
- File transfer: Mac (10.1.10.202) HTTP server → Windows (10.1.10.175) pulls via Invoke-WebRequest
- TFTP serving dir is C:\tftp\ (NOT C:\tftp\root\) — always edit files there
- Ext 311 = reserved N11 — never assign; Tanya 703 = TFH staff Main Site — do NOT touch
- Kids City (ext 799, kclc@thefathershouse.com) is zombie — delete or repurpose after ZTP
