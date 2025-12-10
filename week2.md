
# Phase 2: Security Planning and Testing Methodology (Ubuntu)  
<br>

---

## 1. Performance Testing Plan  

This section describes the performance testing plan, remote monitoring methodology, and testing approach. All test evidence should be attached as screenshots in the indicated sections below.  
<br>

### 1.1 Objective  

The goal of this plan is to test and monitor Ubuntu server performance remotely using command-line tools. Baseline, stress, and recovery performance will be observed and recorded through screenshots.  
<br>

### 1.2 Testing Steps and Screenshot Placeholders  

#### 1.2.1 Baseline Monitoring  

Run the following commands and take screenshots of each output:  

```bash
htop
vmstat 5
iostat -xz 5
sar -u 5 5
````

> **Screenshot Here:**

<br>

#### 1.2.2 CPU Stress Test

Use the following command to generate CPU load and monitor the system with `htop` during the test:

```bash
stress-ng --cpu 4 --timeout 120s
```

> **Screenshot Here:** 

<br>

#### 1.2.3 Memory Stress Test

Run the following command to generate memory pressure on the system:

```bash
stress-ng --vm 2 --vm-bytes 512M --timeout 120s
```

> **Screenshot Here:**

<br>

#### 1.2.4 Disk Stress Test

Use the following command to stress the disk subsystem:

```bash
stress-ng --hdd 1 --hdd-bytes 1G --timeout 120s --temp-path /tmp
```

> **Screenshot Here:** 

<br><br>

---

## 2. Security Configuration Checklist

Document all security configurations applied to the Ubuntu system. Attach screenshots verifying configuration commands and results. <br>

### 2.1 SSH Hardening & Firewall Configuration (UFW)

I improved the security of the Ubuntu system by hardening SSH and configuring the firewall:

* Changed the default SSH port from **22** to **2222**.
* Disabled **root login** over SSH.
* Disabled **password-based login** and enabled **key-based authentication only**.
* Updated UFW firewall rules to allow only the new SSH port.

A new SSH configuration override file was created at:

```bash
sudo nano /etc/ssh/sshd_config.d/override.conf
```

With the following settings:

```text
Port 2222
PermitRootLogin no
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
```

Firewall configuration for SSH:

```bash
sudo ufw allow 2222/tcp
sudo ufw reload
```

Validation and service restart:

```bash
sudo sshd -t
sudo systemctl restart ssh
sudo sshd -T
```

These steps strengthen security by:

* Reducing automated SSH scans on the default port.
* Preventing direct root login.
* Enforcing key-based authentication.

> **Screenshot :**
>
> * sshd_config override file
> * `sudo ufw status`
> * `sudo sshd -T`

<br>

### 2.2 Mandatory Access Control (AppArmor)

I configured **AppArmor** to provide Mandatory Access Control (MAC) at the application level.

To check AppArmor status:

```bash
sudo apparmor_status
```

This showed:

* The AppArmor kernel module is loaded.
* 150+ profiles are loaded.
* Many profiles are in **enforce** mode.

I also verified the service status:

```bash
sudo systemctl status apparmor
```

AppArmor is active and enabled at boot, ensuring application profiles are enforced automatically and limiting what compromised applications can do.

> **Screenshot Here:**
>
> * `sudo apparmor_status`
> * `sudo systemctl status apparmor`

<br>

### 2.3 Automatic Updates

I enabled automatic security updates using Ubuntu’s **unattended-upgrades** feature.

Configuration file checked:

```bash
sudo nano /etc/apt/apt.conf.d/20auto-upgrades
```

The configuration showed that:

* Automatic update checks are enabled.
* Security updates are automatically installed.

This ensures the system regularly receives important security patches without manual intervention.

> **Screenshot Here:** 

<br>

### 2.4 User Privilege Management

I reviewed and managed user privileges to follow the **principle of least privilege**:

* Checked group membership for my user:

```bash
groups
```

* Verified sudo group membership in `/etc/group` to ensure only trusted users have admin rights.

This limits the number of users with elevated permissions and reduces the risk of privilege escalation.

>**Screenshot Here:**
>
> * `groups` output
> * relevant lines from `/etc/group`

<br>

### 2.5 Network Security (UFW)

Network security was improved mainly using **UFW**:

* SSH was moved to port **2222**.
* Only the required SSH port was allowed through the firewall.

Commands used:

```bash
sudo ufw allow 2222/tcp
sudo ufw enable
sudo ufw status
```

The final configuration shows that only port **2222/tcp** is open, while other ports remain blocked, reducing the attack surface.

> **Screenshot Here:**

<br>

### 2.6 Brute-Force Protection (Fail2Ban)

To protect against brute-force attacks, I installed and enabled **Fail2Ban**, which monitors login attempts and bans IPs with repeated failures.

Service status was checked with:

```bash
sudo systemctl status fail2ban
```

Fail2Ban helps defend against automated attacks on SSH and other services by blocking malicious IPs after multiple failed login attempts.

> **Screenshot Here:** 

<br><br>

---

## 3. Threat Model and Mitigation Strategies

This section identifies key threats to the Ubuntu system and the mitigation strategies applied. <br>

### 3.1 SSH Brute-Force Attacks

**Description:**
Attackers attempt to gain unauthorized SSH access using password guessing and automated login attempts.

**Mitigation strategies applied:**

* Changed SSH port from **22** to **2222** to reduce automated scanning.
* Disabled **root login** to prevent direct attacks on the root account.
* Disabled **password authentication** and enforced **key-based authentication** only.
* Enabled **UFW** and allowed only port **2222/tcp** for SSH access.
* Ensured **Fail2Ban** is running to automatically block IPs with repeated failed login attempts.

<br>

### 3.2 Exploitation of Unpatched Software

**Description:**
Attackers exploit vulnerabilities in outdated system packages or software to gain access or execute malicious code.

**Mitigation strategies applied:**

* Enabled **automatic security updates** using Ubuntu’s `unattended-upgrades` service.

* Verified configuration in:

  ```bash
  /etc/apt/apt.conf.d/20auto-upgrades
  ```

* Ensured the update mechanism runs regularly so new patches are installed without manual action.

<br>

### 3.3 Privilege Escalation by Local Users

**Description:**
Local users attempt to gain root or higher-level privileges by abusing sudo access or misconfigured permissions.

**Mitigation strategies applied:**

* Verified user group membership with `groups` to ensure only trusted users have sudo rights.
* Checked the `sudo` group in `/etc/group` to confirm only authorized administrators are included.
* Minimized unnecessary sudo access to reduce the likelihood of successful privilege escalation.

<br>

### 3.4 Network Service Exploits

**Description:**
Attackers target unnecessary or exposed network services on open ports to gain access or disrupt the system.

**Mitigation strategies applied:**

* Configured UFW to block all ports except SSH on **port 2222**, reducing the number of externally exposed services.

* Verified open ports with:

  ```bash
  sudo ss -tulpn
  ```

* Hardened SSH so that only key-based authentication is allowed, and only over the restricted SSH port.

<br><br>

---

```
```
