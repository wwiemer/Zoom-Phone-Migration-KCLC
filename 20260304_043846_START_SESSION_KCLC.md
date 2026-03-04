KCLC-FEAT-20260304-014-SNNT-ztp-batch-execution

ZTP reference table done. Manual device creation in portal needed for ext 304 first, then batch ZTP 9 remaining phones. MCP device tools active but add_phone_device needs Polycom type integer (unknown — create manually in portal instead).

Checkpoint: [CHECKPOINT_20260304_0438_ztp-reference-table-manual-workflow.md](CHECKPOINT_20260304_0438_ztp-reference-table-manual-workflow.md)
Model: sonnet — execution workflow, known patterns

Priorities:
1. Create device for ext 304 in Zoom portal: Phones & Devices → Add Device → MAC 00:04:F2:DE:46:B8, VVX 400, assign to ext 304 common area
2. ZTP ext 304 (10.1.10.176) via phone web UI — admin pw 2307
3. ZTP batch: 301, 303, 305-309, 312, 313 — provisioning URL in checkpoint table
4. Post-ZTP: Auto Receptionist for +13522680205, Paging Group, Intercom Group

Critical facts:
- KCLC network 10.1.10.x / 10.1.12.x is LOCAL — never use ORL2 VPN for KCLC
- VVX 400/410 URL: https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX400
- VVX 401 URL: https://provpp.zoom.us/api/v2/pbx/provisioning/Polycom/VVX401
- Admin pw all 10 remote phones: 2307
- Ext 311 = reserved N11 — never assign; Tanya 703 = TFH Main Site — do NOT touch
- zoom-mcp-server remote is mattcoatsworth/— no push access from wwiemer
