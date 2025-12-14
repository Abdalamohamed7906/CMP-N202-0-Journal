# Phase 7: Security Audit and System Evaluation (Week 7)

<br>

---

## 1. Environment and Audit Scope

This audit checks how secure a production server is after it has gone through several steps to make it more protected, like improving SSH settings, using Fail2Ban, enforcing AppArmor, and setting up automatic updates. The goal here is to make sure these security measures are working well and to find out if there are still any security issues left.

**Server Under Audit**

* **Platform:** Oracle Cloud Infrastructure (OCI)
* **Operating System:** Canonical Ubuntu 22.04 LTS
* **Role:** Production server configured in previous phases

### Evidence

<img width="295" height="185" alt="image" src="https://github.com/user-attachments/assets/52f9dc7f-d174-400d-a1c0-af540a554219" />

<br><br>

---

## 2. Audit Methodology

The security audit was conducted using automated scanning tools and manual configuration review. The following tools and techniques were used:

* **Lynis** for system security auditing
* **Nmap** for network security assessment
* **Manual inspection** of SSH, firewall, access controls, and services


<br><br>

---

## 3. Initial Lynis Security Audit (Baseline)

An initial Lynis scan was performed to establish a baseline security posture.

**Command executed**

```bash
lynis audit system
```

Key outputs included the hardening index, warnings, and improvement suggestions.

The baseline scan showed that several security measures put in place earlier were still working, like AppArmor and Fail2Ban. It also found some areas where more protection could be added, and some recommendations were just general advice instead of urgent fixes.

### Evidence

<img width="288" height="180" alt="image" src="https://github.com/user-attachments/assets/ea7e7eb5-d4c4-4c29-b296-33a39391b714" />
<img width="283" height="177" alt="image" src="https://github.com/user-attachments/assets/17f03252-3f3f-4d10-ac01-4539d3641167" />

<br><br>

---

## 4. Network Security Assessment (Nmap)

Network exposure was evaluated using Nmap to identify open ports and running services.

**Commands executed**

```bash
nmap -sV <server-ip>
nmap -O <server-ip>
```

This shows that the server’s outside exposure to attacks is kept very small and carefully managed.

### Evidence

<img width="301" height="188" alt="image" src="https://github.com/user-attachments/assets/0dbd1fc8-895f-4c24-9f90-40256aa8a019" />


<br><br>

---

## 5. SSH Security Configuration Review

The SSH configuration was reviewed to ensure secure remote access.

**Verified settings included:**

* Root login disabled, reducing the risk of brute-force privilege escalation
* Password authentication disabled, enforcing key-based authentication only
* Protocol 2 enforced, preventing the use of deprecated and insecure SSH protocols

The SSH-related recommendations from Lynis, like lowering the number of login attempts and turning off forwarding features, were checked. Some stayed the same because they were needed for proper function, but the SSH settings are still secure and strong overall.

### Evidence
<img width="278" height="174" alt="image" src="https://github.com/user-attachments/assets/5ebd8f98-cc2d-42ad-88b7-4c6a24e8b480" />

<br><br>

---

## 6. Firewall Configuration Review

The system firewall was reviewed to ensure only required ports were exposed.

**Command executed**

```bash
ufw status verbose
```

Even though UFW was checked, it was decided that host-based firewall rules were not really needed in this setup.

The server is running on Oracle Cloud Infrastructure, which already has strong network-level firewalls (security lists or NSGs). Only specific ports that are allowed are open to the outside world. Extra firewall layers were not added to keep things simpler and reduce the risk of configuration errors. This follows the idea of defense-in-depth, where the main network protection comes from cloud-level firewalls.

### Evidence

<img width="306" height="191" alt="image" src="https://github.com/user-attachments/assets/9d1bacf9-0397-4f8b-bca4-363beb5c956e" />


<br><br>

---

## 7. Access Control Verification

User accounts and privilege assignments were reviewed to ensure least-privilege access.

**Checks performed:**

* Review of user accounts
* Verification of sudo privileges
* Inspection of critical file permissions

No unauthorized users or extra permissions were found. File permission checks reported by Lynis were reviewed, and notes were made where permissions could be made stricter without affecting system functionality.

### Evidence
<img width="288" height="180" alt="image" src="https://github.com/user-attachments/assets/0d92de3d-ba79-4347-a9c8-eca99ab680be" /><img width="291" height="182" alt="image" src="https://github.com/user-attachments/assets/754ffc24-8f70-41a9-b54b-b8c15ede01c7" />
<img width="287" height="180" alt="image" src="https://github.com/user-attachments/assets/fdd56acf-c095-4dc6-8dcc-858864771eb0" />


<br><br>

---

## 8. Service Audit and Justification

All enabled and running services were reviewed to ensure they were required for system operation.

Services found by Lynis and Nmap were checked against system requirements. Services such as SSH, system logging, and time synchronisation were necessary for managing and running the server.

### Evidence

<img width="305" height="191" alt="image" src="https://github.com/user-attachments/assets/4349d1b1-20ac-49f0-9dc8-1920738bb6dd" />
<img width="306" height="192" alt="image" src="https://github.com/user-attachments/assets/e1d0b67a-2e7a-4fff-b17c-2d3c7e4700cb" />

<br><br>

---

## 9. System Configuration Review

Kernel and system security settings were reviewed, including address space layout randomisation (ASLR) and automatic security updates.

Lynis identified some kernel settings that were different from its default profile. These differences were reviewed and found to be either:

* Safely handled by system defaults
* Intentionally left unchanged to avoid system instability

Automatic security updates were confirmed as enabled, ensuring vulnerabilities are patched quickly.

### Evidence
<img width="305" height="191" alt="image" src="https://github.com/user-attachments/assets/7198c731-bff9-489a-9ab6-19bb9411d1c7" />
<br><br>

---

## 10. Remediation Actions

Based on Lynis recommendations, remediation actions were applied, including system updates, service hardening, and configuration improvements.

Some Lynis suggestions, such as installing additional malware scanners or integrity tools, were documented but not implemented. This decision was made because the system already uses multiple security layers, and these tools were not necessary for the server’s intended role.

### Evidence

<img width="303" height="190" alt="image" src="https://github.com/user-attachments/assets/75e99bdd-2c03-4789-ae93-0c5d0c7e5b4f" />


<br><br>

---

## 11. Post-Remediation Lynis Audit

A second Lynis scan was conducted to validate the effectiveness of remediation actions.

**Command executed**

```bash
lynis audit system
```

### Evidence

<img width="311" height="195" alt="image" src="https://github.com/user-attachments/assets/74c4deae-0283-44f2-8a7c-bb0a5e63d48d" />
<img width="316" height="198" alt="image" src="https://github.com/user-attachments/assets/3c2eb5d1-d54f-45b5-9cc0-6753ef0add06" />


<br><br>

---

## 12. Remaining Risk Assessment

Remaining risks were identified and documented based on residual findings and system exposure.

The remaining open ports are only for required services and are protected by:

* Secure configurations
* Key-based authentication
* Cloud-level firewall rules
* Tools that prevent malicious activity

Future improvements may include:

* Adding a file integrity monitoring tool
* Further SSH hardening based on usage needs
* Regular security audits to ensure continued compliance

Overall, the system shows strong security suitable for a real-world production environment.

