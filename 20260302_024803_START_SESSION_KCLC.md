Session: KCLC-FEAT-20260302-002-SNNT-vvx400-firmware-research-retry
Model: claude-sonnet-4-6
Project: KCLC Zoom Phone Migration
Checkpoint: CHECKPOINT_20260302_0248_vvx400-firmware-upgrade-blocked.md
Priority: URGENT — Port at 11:30 AM ET (~9 hrs from checkpoint creation)

Context: VVX400 firmware upgrade via TFTP is BLOCKED. Phone connects to pumpKIN TFTP server but never downloads sip.ld. Fails at provisioning validation with "cfgProvStatusSet: Unknown status 53" and "Could not get application name". All config files are correct and being downloaded. Research Polycom's official VVX400 TFTP provisioning parameters to fix this, then retry firmware upgrade.

Immediate tasks:
1. Research: What config parameters enable VVX400 firmware update via TFTP? (see checkpoint Research Questions)
2. Research: Can 5.5.2.8571 upgrade directly to 5.9.8 or is intermediate version required?
3. Research: Does Split firmware work better than Combined for this provisioning flow?
4. Fix config files based on research and retry firmware upgrade on test phone (MAC 0004f2de46b8, IP 10.1.10.176)
5. After success: update all remaining 13 phones
6. Before 11:30 AM ET: Set Twilio forward +13522680205 → +13527820706
