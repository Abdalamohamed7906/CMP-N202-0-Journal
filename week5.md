# Week 5: Advanced Security and Monitoring

<br>

---

## 1. Access Control using AppArmor

AppArmor is used to enforce Mandatory Access Control (MAC) on the server. It restricts how applications interact with system resources even if a service is compromised. The `aa-status` command confirms AppArmor is loaded and profiles are enforced, while kernel logs provide tracking and reporting evidence.

### Commands Used
```bash
sudo aa-status
sudo journalctl -k | grep -i apparmor | tail
````

### Evidence

<img width="1000" src="evidence/screenshots/01_apparmor_status.png" />
<img width="1000" src="evidence/screenshots/02_apparmor_logs.png" />

<br><br>

---

## 2. Automatic Security Updates (unattended-upgrades)

Automatic security updates were configured using unattended-upgrades to ensure the system receives security patches without manual intervention. The configuration file confirms automatic updates are enabled, and the service status verifies that it is running.

### Commands Used

```bash
cat /etc/apt/apt.conf.d/20auto-upgrades
systemctl status unattended-upgrades --no-pager
```

### Evidence

<img width="1000" src="evidence/screenshots/03_auto_upgrades_config.png" />
<img width="1000" src="evidence/screenshots/04_unattended_status.png" />

<br><br>

---

## 3. Fail2Ban Intrusion Detection and Prevention

Fail2Ban was installed and configured to protect the SSH service by monitoring authentication logs and banning IP addresses after repeated failed login attempts. The sshd jail configuration is verified, and the service status confirms Fail2Ban is actively running.

### Commands Used

```bash
sudo systemctl restart fail2ban
sudo systemctl status fail2ban --no-pager
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

### Evidence

<img width="1000" src="evidence/screenshots/05_fail2ban_config.png" />
<img width="1000" src="evidence/screenshots/06_fail2ban_running.png" />
<img width="1000" src="evidence/screenshots/07_fail2ban_sshd_status.png" />

<br><br>

---

## 4. Security Baseline Verification Script

A security baseline verification script (`security-baseline.sh`) was created and executed on the server. The script checks the status of AppArmor, unattended upgrades, Fail2Ban, and SSH to ensure all security controls from Phase 4 and Phase 5 are active.

### Commands Used

```bash
sudo nano /usr/local/bin/security-baseline.sh
sudo chmod +x /usr/local/bin/security-baseline.sh
sudo /usr/local/bin/security-baseline.sh
```

### Evidence

<img width="1000" src="evidence/screenshots/08_baseline_script.png" />
<img width="1000" src="evidence/screenshots/09_baseline_run.png" />

<br><br>

---

## 5. Remote Monitoring Script

A remote monitoring script (`monitor-server.sh`) was created to run on the workstation and connect to the server via SSH. The script collects system metrics including hostname, uptime, memory usage, and disk usage.

### Commands Used

```bash
nano scripts/monitor-server.sh
chmod +x scripts/monitor-server.sh
./scripts/monitor-server.sh
```

### Evidence

<img width="1000" src="evidence/screenshots/10_monitor_script.png" />
<img width="1000" src="evidence/screenshots/11_monitor_output.png" />

<br><br>

---

## Included Scripts

* `scripts/fail2ban-setup.sh`
* `scripts/security-baseline.sh`
* `scripts/monitor-server.sh`

``

------
### Clip 1

Video Link:  
[Paste GitHub / OneDrive / Google Drive link here]

---

### Clip 2

Video Link:  
[Paste GitHub / OneDrive / Google Drive link here]

---

### Clip 3

Video Link:  
[Paste GitHub / OneDrive / Google Drive link here]





