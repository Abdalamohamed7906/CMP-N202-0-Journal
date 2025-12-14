Perfect — below is the **fully combined, cleaned, and submission-ready document**.
You can **copy–paste this once** directly into GitHub, a report, or a LMS submission.

---

# Week 2: Security Planning and Testing Methodology (Ubuntu)

---

## 1. Performance Testing Plan

This section describes the performance testing plan, remote monitoring methodology, and testing approach. All test evidence is attached as screenshots in the indicated sections.

### 1.1 Objective

The objective of this plan is to test and monitor Ubuntu server performance remotely using command-line tools. Baseline, stress, and recovery performance are observed and recorded.

---

### 1.2 Testing Steps and Screenshot Evidence

#### 1.2.1 Baseline Monitoring

The following commands were executed to establish baseline system performance.

**Command 1: `htop`**
*Screenshot: Baseline CPU and memory usage* <br>
<img width="432" height="270" alt="Baseline monitoring output from htop" src="https://github.com/user-attachments/assets/1348d095-7e5c-467c-a1b1-8be7b254080d" />

**Command 2: `vmstat 5`**
*Screenshot: Baseline memory and process statistics* <br>
<img width="432" height="270" alt="Baseline monitoring output from vmstat 5" src="https://github.com/user-attachments/assets/36f98a8c-98ca-4df7-ad2b-679391d724b6" />

**Command 3: `iostat -xz 5`**
*Screenshot: Baseline disk I/O performance* <br>
<img width="432" height="270" alt="Baseline monitoring output from iostat -xz 5" src="https://github.com/user-attachments/assets/5472bb74-23e6-466e-8c16-eef48a14c63b" />

**Command 4: `sar -u 5 5`**
*Screenshot: CPU usage over time* <br>
<img width="432" height="270" alt="Baseline monitoring output from sar -u 5 5" src="https://github.com/user-attachments/assets/7d75c493-bbe0-4bdd-b904-10da678b8d40" />

---

#### 1.2.2 CPU Stress Test

The following command was used to generate CPU load while monitoring performance with `htop`:

```bash
stress-ng --cpu 4 --timeout 120s
```
<img width="432" height="270" alt="image" src="https://github.com/user-attachments/assets/d0ef8f50-ea18-42a7-9d7a-69ac4eca2f8c" />


---

#### 1.2.3 Memory Stress Test

Memory pressure was generated using the following command:

```bash
stress-ng --vm 2 --vm-bytes 512M --timeout 120s
```

<img width="432" height="270" alt="image" src="https://github.com/user-attachments/assets/ee6786b6-8d39-45f7-b9f5-9ae33dfd5402" />

---

#### 1.2.4 Disk Stress Test

Disk I/O stress testing was performed using:

```bash
stress-ng --hdd 1 --hdd-bytes 1G --timeout 120s --temp-path /tmp
```

<img width="432" height="270" alt="image" src="https://github.com/user-attachments/assets/9f993ded-ac6f-4b7a-bb2b-84dbba0e593b" />


---

## 2. Security Configuration Checklist

This section documents all security configurations applied to the Ubuntu system, with validation screenshots.

---

### 2.1 SSH Hardening and Firewall Configuration (UFW)

SSH access was hardened and firewall rules were configured to improve system security.

**Security measures implemented:**

* Changed SSH port from **22** to **2222**
* Disabled **root login**
* Disabled **password-based authentication**
* Enforced **key-based authentication**
* Restricted firewall access to the SSH port only

---

#### SSH Configuration

A new SSH override configuration file was created:

```bash
sudo nano /etc/ssh/sshd_config.d/override.conf
```

Configuration applied:

```text
Port 2222
PermitRootLogin no
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
```

---

#### Firewall Configuration

```bash
sudo ufw allow 2222/tcp
sudo ufw reload
```

---

#### Validation and Restart

```bash
sudo sshd -t
sudo systemctl restart ssh
sudo sshd -T
```

**Security Benefits:**

* Reduces automated scanning
* Prevents direct root access
* Enforces strong authentication

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/1c084740-b0f2-44b8-8da3-3bbf784d4557" />
<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/f7f816ed-6718-4592-b006-37f07aafd808" />
<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/64c8f26c-3941-40f9-a6c0-627892808d7d" />


---

### 2.2 Mandatory Access Control (AppArmor)

AppArmor was configured to provide Mandatory Access Control at the application level.

```bash
sudo apparmor_status
sudo systemctl status apparmor
```

Results confirmed:

* AppArmor kernel module is loaded
* Over 150 profiles are active
* Profiles are enforced automatically at boot

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/3dfcfb96-6515-494b-894f-30254567c597" />
<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/a1d06966-89a6-4e92-98e4-852edc9eb070" />

---

### 2.3 Automatic Updates

Automatic security updates were enabled using **unattended-upgrades**.

```bash
sudo nano /etc/apt/apt.conf.d/20auto-upgrades
```

The configuration confirmed:

* Automatic update checks are enabled
* Security updates are installed automatically

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/7a814310-328d-4fd0-bd86-73a16c3bd354" />


---

### 2.4 User Privilege Management

User privileges were reviewed following the **principle of least privilege**.

```bash
groups
```

Group membership and `/etc/group` were reviewed to ensure only trusted users have administrative access.

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/64d09549-c94c-431a-994a-dae54f0cd5bd" />


* `groups` command output
* Relevant `/etc/group` entries

---

### 2.5 Network Security (UFW)

Firewall rules were applied using UFW to minimize exposed services.

```bash
sudo ufw allow 2222/tcp
sudo ufw enable
sudo ufw status
```

Only SSH traffic on port **2222/tcp** is permitted.

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/32e0b32c-1e54-4a7b-a580-1e10f0e787b9" />


* `sudo ufw status`

---

### 2.6 Brute-Force Protection (Fail2Ban)

Fail2Ban was installed to protect against brute-force login attempts.

```bash
sudo systemctl status fail2ban
```

Fail2Ban automatically blocks IPs after repeated failed authentication attempts.

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/a0a1d373-621a-4d5e-8d1e-3d5318a6afbb" />
<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/c25a6a98-70cc-4778-81f4-9de5e7059cf7" />

* `sudo systemctl status fail2ban`

---

## 3. Threat Model and Mitigation Strategies

This section outlines key threats and the mitigation strategies applied.

---

### 3.1 SSH Brute-Force Attacks

**Threat:** Automated attempts to gain SSH access.

**Mitigations:**

* SSH port changed to **2222**
* Root login disabled
* Password authentication disabled
* UFW restricted access
* Fail2Ban enabled

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/3eb034c1-a838-453c-88f4-ed3d1267204a" />


---

### 3.2 Exploitation of Unpatched Software

**Threat:** Vulnerabilities in outdated packages.

**Mitigations:**

* Enabled unattended security updates
* Verified update configuration

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/49824667-45a3-486e-a93e-4f3894d869a0" />


---

### 3.3 Privilege Escalation by Local Users

**Threat:** Unauthorized elevation of privileges.

**Mitigations:**

* Verified sudo group membership
* Restricted administrative access

<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/be63b4f9-3d22-4e43-b7f2-64b3b1e125b5" />


---

### 3.4 Network Service Exploits

**Threat:** Attacks against exposed services.

**Mitigations:**

* Closed all ports except SSH
* Verified listening services:

```bash
sudo ss -tulpn
```

* Hardened SSH authentication

---
<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/2133d4ff-064b-4d2f-b691-adc9cb4c6084" />
<img width="432" height="273" alt="image" src="https://github.com/user-attachments/assets/89bbe35a-d8c9-40b7-a5f6-6a19e12ead38" />
