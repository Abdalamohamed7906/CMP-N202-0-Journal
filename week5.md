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
<img width="295" height="185" alt="image" src="https://github.com/user-attachments/assets/51e7f946-de90-41ae-8fd2-be3def86c24c" />
<img width="295" height="185" alt="image" src="https://github.com/user-attachments/assets/7899c9ce-cbf9-4fe1-a3d8-f8f8051a84b2" />
<img width="297" height="186" alt="image" src="https://github.com/user-attachments/assets/c917c5ca-854d-435c-bd73-6e490a0f8aa8" />
<img width="298" height="186" alt="image" src="https://github.com/user-attachments/assets/b239b36d-043f-43d2-a1c2-86a29a46da5e" />
<img width="295" height="185" alt="image" src="https://github.com/user-attachments/assets/627452f9-58c2-4c11-b3f0-878356473855" />
<img width="295" height="184" alt="image" src="https://github.com/user-attachments/assets/0ead3ff8-1884-4c67-8eff-8509d06dff4d" />



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
<img width="292" height="182" alt="image" src="https://github.com/user-attachments/assets/5d01b54b-2115-46d4-a6ca-dc5539457c98" />
<img width="290" height="181" alt="image" src="https://github.com/user-attachments/assets/4fc0a486-fc23-4ab8-a5bb-01456aeef7b8" />
<img width="291" height="182" alt="image" src="https://github.com/user-attachments/assets/b3c3899a-6d3d-412e-87d3-32642e93986b" />


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
<img width="290" height="182" alt="image" src="https://github.com/user-attachments/assets/dfe76c3d-7f3a-4186-b714-97d1183b08b3" />
<img width="289" height="181" alt="image" src="https://github.com/user-attachments/assets/639c0ea0-4333-4cc2-8c09-feb1d41faefb" />
<img width="288" height="180" alt="image" src="https://github.com/user-attachments/assets/370850e4-6c3c-477c-8d28-f46b973193b5" />

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

<img width="290" height="181" alt="image" src="https://github.com/user-attachments/assets/3328c096-29d3-4ab7-ada5-34a9aa91c1e7" />
<img width="293" height="183" alt="image" src="https://github.com/user-attachments/assets/691c59ff-75ee-4bd2-8a80-f4f5eee539a5" />

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


<img width="309" height="193" alt="image" src="https://github.com/user-attachments/assets/c463bf98-2c42-4222-9f8d-a069ef6fe366" />
<img width="316" height="197" alt="image" src="https://github.com/user-attachments/assets/e5e397b1-fc49-4df7-9199-2c5867b713ce" />


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
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQBTKp6QvCRhQLiwCQfQpTAuAaSxtZ1RwmO65lQUKddeah8
---

### Clip 2

Video Link:  
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQAQ_OOUdCtDQb_DBs6GGmM2AZLffUCusgbDv9S8aNUVaG4

---

### Clip 3

Video Link:  
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQCTT__8pm75S4RT1hzschvbAUtaTrL80VzmXnivwwAL32g


### Clip 4

Video Link:  
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQB1y1GJYdtnQIOPjrNZrCTrAVKraz803d9bODbeDjLTYdE




