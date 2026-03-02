KCLC-FEAT-20260302-005-SNNT-vvx-firmware-5.8x-upgrade

Root cause confirmed last session: all UC Software 5.9.8 uses SigKey=152, incompatible with 5.7.2 Updater on phones. Need 5.8.x intermediate firmware to upgrade the Updater first, then 5.9.8 installs cleanly. Phones still on 5.5.2.

Checkpoint: [CHECKPOINT_20260302_0819_sigkey152-blockers-5.8x-needed.md](CHECKPOINT_20260302_0819_sigkey152-blockers-5.8x-needed.md)
Model: sonnet — known problem, defined solution path, needs web research + TFTP ops

Priorities:
1. Find and download UC Software 5.8.x for VVX phones (HP Poly support portal or Poly community forum) — verify SigKey <150 in header
2. Deploy 5.8.x to TFTP root, reboot VVX 401 (10.1.12.189, Warren physically present) — confirm upgrade succeeds, then swap to 5.9.8
3. Verify port status: +13522680205 was scheduled to port 11:30 AM ET 2026-03-02

Critical facts:
- MacBook Air IP: 10.1.36.71 (DHCP — verify at session start) | TFTP root: /private/tftpboot/
- Two-step upgrade: 5.5.2 → 5.8.x (updates Updater) → 5.9.8 (enables TLS 1.2 for ZTP)
- sip.ld = 3111-46157-002.sip.ld (VVX 400, 41MB) currently in TFTP root pointing to 5.9.8 — replace with 5.8.x
- VVX 401 part: 3111-48400-001 | MAC: 64:16:7F:31:A9:6D | IP: 10.1.12.189 | Boot Server: Static
- DO NOT touch TFH extensions: 202, 703, 230, 232, 233, 235, 239, 240, 241, 245, 246, 282
