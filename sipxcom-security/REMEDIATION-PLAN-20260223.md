# sipXcom Security Remediation Plan

**Server:** ucs05.warrentek.net (CentOS 6.10, sipXcom 19.04)
**Created:** 2026-02-23 21:29:46 EST
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
**Status:** APPROVED — ready to execute in next session
**Prerequisite:** ALL backups must complete before any Step 1–4 changes

---

## Overview

Four changes, applied in order with validation between each. Each change is independently reversible. No change requires a reboot (though a final optional reboot confirms startup persistence).

| Step | Change | Risk | Reversible? | Reboot Required? |
|------|--------|------|-------------|-----------------|
| 0 | **Backups** | None | N/A | No |
| 1 | **Switch fail2ban to ipset** | Low-Medium | Yes (30 sec) | No |
| 2 | **Add swap file (4GB)** | Very Low | Yes | No |
| 3 | **SYN flood rate limiting (port 443)** | Low | Yes (1 cmd) | No |
| 4 | **Monitoring script** | None | N/A | No |

**Estimated total time:** 45–60 minutes (mostly waiting for fail2ban to reload)

---

## Step 0: Backups (MANDATORY — do before anything else)

### 0a. Backup current iptables rules
```bash
# Save complete iptables ruleset to file
iptables-save > /root/backup-iptables-$(date +%Y%m%d_%H%M%S).rules
# Verify
wc -l /root/backup-iptables-*.rules
ls -lh /root/backup-iptables-*.rules
```
**Revert command (if needed):** `iptables-restore < /root/backup-iptables-YYYYMMDD_HHMMSS.rules`

### 0b. Backup fail2ban configuration
```bash
# Backup entire fail2ban config directory
tar -czf /root/backup-fail2ban-$(date +%Y%m%d_%H%M%S).tar.gz /etc/fail2ban/
ls -lh /root/backup-fail2ban-*.tar.gz
```
**Revert command:** `tar -xzf /root/backup-fail2ban-YYYYMMDD_HHMMSS.tar.gz -C /`

### 0c. Backup sipXcom configuration
```bash
# sipXcom stores its config in /etc/sipxpbx and /var/sipxdata
tar -czf /root/backup-sipxcom-config-$(date +%Y%m%d_%H%M%S).tar.gz \
    /etc/sipxpbx/ \
    /var/sipxdata/configserver/ \
    /etc/fail2ban/ \
    2>/dev/null
ls -lh /root/backup-sipxcom-config-*.tar.gz
```

### 0d. Export current fail2ban banned IP list
```bash
# Save the current 5,926 banned IPs so they can be re-imported if needed
fail2ban-client status sip-dos | grep "Banned IP" | tr ' ' '\n' | \
    grep -E '^[0-9]' > /root/backup-sip-dos-banned-$(date +%Y%m%d_%H%M%S).txt
wc -l /root/backup-sip-dos-banned-*.txt
```

### 0e. Copy backups to safe location (off-server)
```bash
# From your Mac — pull all backups off the server
scp root@ucs05.warrentek.net:/root/backup-*.* \
    ~/Library/CloudStorage/Dropbox/Development/WarrenTEK-Operations/projects/Zoom-Phone-Migration-KCLC/sipxcom-security/backups/
```

### Validation: Backups complete
- [ ] `backup-iptables-*.rules` exists and has ~7,700+ lines
- [ ] `backup-fail2ban-*.tar.gz` exists, >0 bytes
- [ ] `backup-sipxcom-config-*.tar.gz` exists, >0 bytes
- [ ] `backup-sip-dos-banned-*.txt` has ~5,900+ lines
- [ ] All files copied to Dropbox

---

## Step 1: Switch fail2ban from iptables to ipset

**Why first:** This is the highest-impact fix. Eliminates the root cause of registration loss. Must be done before Step 3 (rate limiting adds more iptables rules, counterproductive without ipset).

### What this does
Replaces the `block-allports` fail2ban action (which adds 1 iptables rule per banned IP) with an ipset-based action (which adds all banned IPs to a hash set referenced by a single iptables rule). Result: 5,926 bans = 1 iptables lookup instead of 5,926 sequential checks.

### 1a. Create ipset-based fail2ban action
```bash
cat > /etc/fail2ban/action.d/block-allports-ipset.conf << 'EOF'
[Definition]

actionstart = ipset create fail2ban-<name> hash:ip timeout <bantime> -exist
              iptables -I INPUT 1 -m set --match-set fail2ban-<name> src -j DROP

actionstop  = iptables -D INPUT -m set --match-set fail2ban-<name> src -j DROP
              ipset flush fail2ban-<name>
              ipset destroy fail2ban-<name>

actioncheck = iptables -n -L INPUT | grep -q 'fail2ban-<name>'

actionban   = ipset add fail2ban-<name> <ip> timeout <bantime> -exist

actionunban = ipset del fail2ban-<name> <ip> -exist

[Init]
name = default
bantime = 86400
EOF
```

### 1b. Update jail.local to use new action for sip-dos
```bash
# Edit /etc/fail2ban/jail.local
# Change sip-dos action from:   action = block-allports
# Change sip-dos action to:     action = block-allports-ipset[name=sip-dos, bantime=2592000]
# (2592000 seconds = 30 days — long enough to keep bad actors out, short enough to auto-expire)
#
# Change ALL other SIP jails from: action = block-allports
# Change to:                        action = block-allports-ipset[name=<jailname>, bantime=600]
```

**Note:** The timeout in the ipset action IS the ban time. When a ban expires, ipset automatically removes the IP from the set. No manual cleanup needed, rule count stays bounded.

### 1c. Reload fail2ban
```bash
fail2ban-client reload
sleep 5
fail2ban-client status
```

### Validation after Step 1
```bash
# Check ipset tables were created
ipset list | grep -E 'Name|Number'

# Check that iptables now has a single ipset match rule instead of thousands
iptables -L INPUT -n | grep 'match-set'

# Check total iptables rule count (should drop dramatically — from 7,713 to ~100-200)
iptables -L -n | grep -c '^[A-Z APBD]'
iptables-save | wc -l

# Verify fail2ban is still banning (test with a known attack IP)
fail2ban-client status sip-dos
```

**Expected result:** iptables rule count drops from ~7,713 to ~200. ipset shows `fail2ban-sip-dos` with entries.

**If something breaks:** Restore backup:
```bash
fail2ban-client stop
iptables-restore < /root/backup-iptables-YYYYMMDD_HHMMSS.rules
tar -xzf /root/backup-fail2ban-YYYYMMDD_HHMMSS.tar.gz -C /
fail2ban-client start
```

### Test between Step 1 and Step 2
- Confirm phones are still registered (call a KCLC extension)
- Wait 10 minutes, check fail2ban is still banning new IPs: `tail -f /var/log/fail2ban.log`
- Confirm sip-dos jail is actively catching events: `fail2ban-client status sip-dos`

---

## Step 2: Add Swap File (4GB)

**Why second:** Simple, very low risk. Protects against OOM events during attack spikes. No interaction with network/firewall rules.

```bash
# Create 4GB swap file
dd if=/dev/zero of=/swapfile bs=1M count=4096
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile

# Verify swap is active
free -m

# Make permanent across reboots
echo '/swapfile none swap sw 0 0' >> /etc/fstab
# Verify fstab entry
grep swap /etc/fstab
```

### Validation after Step 2
```bash
free -m
# Should show Swap: 4096 total, 0 used
swapon -s
```

**Revert command:**
```bash
swapoff /swapfile
rm /swapfile
# Remove the fstab line (last line added)
sed -i '/\/swapfile/d' /etc/fstab
```

### Test between Step 2 and Step 3
- Run `free -m` — confirm 4096 swap
- Confirm sipXcom services still running: `service sipxpbx status`

---

## Step 3: Rate Limit SYN Flood on Port 443

**Why third:** Protects the HTTPS management interface and web services. Lower priority than Steps 1-2 (SYN flood doesn't directly cause registration loss, but it competes for CPU). Done after Step 1 so the iptables ruleset is already clean.

This uses the iptables `recent` module to limit new TCP connections to port 443 to 25 per second per source IP. Legitimate browsers never hit this limit; flood tools do.

```bash
# Allow established connections through first (no rate limiting for existing sessions)
iptables -I INPUT -p tcp --dport 443 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Rate limit NEW connections: allow 25 new SYN packets per 1 second per source IP
# Track source IPs with the 'recent' module
iptables -I INPUT -p tcp --dport 443 --syn \
    -m recent --name synflood443 --update --seconds 1 --hitcount 25 -j DROP
iptables -I INPUT -p tcp --dport 443 --syn \
    -m recent --name synflood443 --set -j ACCEPT

# Verify rules inserted
iptables -L INPUT -n | grep 443
```

### Validation after Step 3
```bash
# Verify sipXcom web admin still accessible: https://ucs05.warrentek.net/sipxconfig/app
# Verify rules are in place
iptables -L INPUT -n -v | grep 443
# Watch for the SYN flood kernel messages to stop appearing
tail -f /var/log/messages | grep -i 'flood\|syn'
```

**Revert command (2 commands):**
```bash
iptables -D INPUT -p tcp --dport 443 --syn -m recent --name synflood443 --update --seconds 1 --hitcount 25 -j DROP
iptables -D INPUT -p tcp --dport 443 --syn -m recent --name synflood443 --set -j ACCEPT
```

### Test between Step 3 and Step 4
- Confirm https://ucs05.warrentek.net loads normally
- Confirm phones still registered
- `tail -20 /var/log/messages` — SYN flood alerts should stop or drastically reduce

---

## Step 4: Save iptables Rules Permanently

After Steps 1-3 are validated, make the iptables rules survive reboots:

```bash
# Save current (working) iptables state
service iptables save
# Verify saved
cat /etc/sysconfig/iptables | head -20
```

**Note:** fail2ban and APIBAN add their rules dynamically at startup, so they do NOT need to be in the static save. The static save should capture the manual blocks, rate limiting rules, and chain definitions.

---

## Step 5: Monitoring Script

Deploy a lightweight daily check script:

```bash
cat > /usr/local/bin/sip-security-check.sh << 'EOF'
#!/bin/bash
echo "=== sipXcom Security Check $(date) ==="
echo ""
echo "-- System Load --"
uptime
free -m | grep -E 'Mem|Swap'
echo ""
echo "-- fail2ban Status --"
fail2ban-client status | grep -E 'Jail|Number'
echo ""
echo "-- sip-dos ban count --"
fail2ban-client status sip-dos | grep -E 'Currently banned|Total banned'
echo ""
echo "-- APIBAN rule count --"
iptables -L APIBAN -n 2>/dev/null | wc -l
echo ""
echo "-- Total iptables rules --"
iptables-save | wc -l
echo ""
echo "-- Recent SYN flood alerts (last hour) --"
grep "$(date '+%b %e')" /var/log/messages | grep -i 'flood' | tail -5
echo ""
echo "-- APIBAN last update --"
tail -3 /var/log/apiban-client.log
EOF

chmod +x /usr/local/bin/sip-security-check.sh
```

**Add to cron for twice-daily email report (optional):**
```bash
# Add to crontab (adjust email address)
echo "0 8,20 * * * /usr/local/bin/sip-security-check.sh | mail -s 'sipXcom Security Check $(hostname)' wwiemer@warrentek.com" >> /var/spool/cron/root
```

**Run manually:**
```bash
/usr/local/bin/sip-security-check.sh
```

---

## Full Revert (Nuclear Option)

If everything goes wrong and the server needs to be returned to exactly the pre-change state:

```bash
# 1. Stop fail2ban
fail2ban-client stop

# 2. Restore original iptables rules
iptables-restore < /root/backup-iptables-YYYYMMDD_HHMMSS.rules

# 3. Destroy any ipset tables created
ipset destroy fail2ban-sip-dos 2>/dev/null
ipset destroy fail2ban-sip-register 2>/dev/null
ipset destroy fail2ban-sip-invite 2>/dev/null
ipset destroy fail2ban-sip-options 2>/dev/null
ipset destroy fail2ban-sip-subscribe 2>/dev/null
ipset destroy fail2ban-sip-ack 2>/dev/null

# 4. Restore original fail2ban config
tar -xzf /root/backup-fail2ban-YYYYMMDD_HHMMSS.tar.gz -C /

# 5. Restart fail2ban
fail2ban-client start

# 6. Remove swap (if added)
swapoff /swapfile 2>/dev/null
rm -f /swapfile
sed -i '/\/swapfile/d' /etc/fstab

# Verify
iptables-save | wc -l   # should be back to ~7,713
free -m                  # swap should be gone
fail2ban-client status
```

---

## Monitoring After Changes

### Short-term (first 72 hours)
- Check twice daily: `ssh root@ucs05.warrentek.net '/usr/local/bin/sip-security-check.sh'`
- Watch iptables rule count — should stay flat (ipset handles bans without growing the count)
- Watch uptime — server should not need reboots

### Success Criteria
- [ ] Server runs >7 days without registration loss
- [ ] iptables rule count stays flat regardless of new bans
- [ ] fail2ban sip-dos ban count continues growing (still banning attackers) but without degrading performance
- [ ] No more "possible SYN flooding" kernel log messages

### Key Metrics to Track
```bash
# Run this daily — save output for comparison
date
uptime
iptables-save | wc -l
fail2ban-client status sip-dos | grep 'Currently banned'
ipset list fail2ban-sip-dos 2>/dev/null | grep 'Number of entries'
free -m | grep Swap
```

---

**Created:** 2026-02-23 21:29:46 EST
**Next session:** KCLC-IMPL-20260224-001-CLDE-sipxcom-security-remediation (or similar)
**Author:** Warren Wiemer / WarrenTEK Enterprises, LLC (with Claude Sonnet 4.6)
