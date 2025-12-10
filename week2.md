Phase 2: Security Planning and Testing Methodology (Ubuntu) 

1. Performance Testing Plan 

This section describes the performance testing plan, remote monitoring methodology, and testing approach. All test evidence should be attached as screenshots in the indicated sections below. 

Objective 

The goal of this plan is to test and monitor Ubuntu server performance remotely using command-line tools. Baseline, stress, and recovery performance will be observed and recorded through screenshots. 

 
 

Testing Steps and Screenshot Placeholders 

    **Baseline Monitoring:** Run the following commands and take screenshots of each output: 

    htop 

A screenshot of a computer

Description automatically generated 
   - vmstat 5A screenshot of a computer

Description automatically generated 
   - iostat -xz 5A screenshot of a computer

Description automatically generated 
   - sar -u 5 5A screenshot of a computer

Description automatically generated 
 
 

2. **CPU Stress Test:** Use the following command to generate load and monitor with htop: 

   - stress-ng --cpu 4 --timeout 120s 

A screenshot of a computer

Description automatically generated 
 

3. **Memory Stress Test:** 

   - stress-ng --vm 2 --vm-bytes 512M --timeout 120s 

A screenshot of a computer

Description automatically generated 
 

4. **Disk Stress Test:** 

   - stress-ng --hdd 1 --hdd-bytes 1G --timeout 120s --temp-path /tmp 

A screenshot of a computer

Description automatically generated 
 

 
 

2. Security Configuration Checklist 

Document all security configurations applied to the Ubuntu system. Attach screenshots verifying configuration commands and results. 

SSH Hardening & Firewall Configuration (UFW) 

I made my Ubuntu system more secure by changing the default SSH port to 2222, turning off root login, and using key-based authentication instead of passwords. This helps stop unwanted access and keeps the system safer from automated attacks. I made a new configuration file at /etc/ssh/sshd_config.d/override.conf and added these lines: Port 2222, PermitRootLogin no, PasswordAuthentication no, KbdInteractiveAuthentication no, and PubkeyAuthentication yes. This ensures only users with proper SSH keys can connect and prevents password-based attacks. I updated the firewall with sudo ufw allow 2222/tcp and reloaded it. I checked the settings for errors using sudo sshd -t, restarted the SSH service with sudo systemctl restart ssh, and confirmed the changes with sudo sshd -T. These steps greatly improved security by stopping brute-force attacks, blocking direct root access, and requiring strong encryption for login. 

 

I set up the Uncomplicated Firewall (UFW) on my Ubuntu computer to manage incoming network traffic and let only the necessary services through. First, I turned the firewall on using the command sudo ufw enable, which sets up default rules to block any unwanted connections. Since SSH was set up to use a different port, 2222, I created a rule to allow secure access on that port with sudo ufw allow 2222/tcp. Then I restarted the firewall with sudo ufw reload. I checked the current rules by running sudo ufw status, and it showed that only port 2222 was open, while all other ports were still blocked. This setup makes my system more secure by limiting the possible entry points for attackers and ensuring only approved traffic can reach the server. 

 

 
 

 
 

Mandatory Access Control (AppArmor) 

I set up Mandatory Access Control with AppArmor to improve security at the application level on my system. I made sure AppArmor was installed, turned on, and working by running the command sudo apparmor_status. The result showed more than 150 profiles were loaded, with many in enforce mode, and the AppArmor kernel module was running. I also checked the AppArmor service using sudo systemctl status apparmor to confirm it was active and set to start automatically when the system boots. This setup ensures that security rules are applied automatically to all applications, limiting what they can do and reducing the risk of damage if an application is hacked. 

 

 

 

 

 

 

 

Automatic Updates 

I set up automatic updates by making sure the Ubuntu unattended-upgrades service was installed and running. I checked the settings in the configuration file at /etc/apt/apt.conf.d/20auto-upgrades, which showed that automatic update checks and security upgrades were both turned on. This helps my system get important security fixes without me having to do anything, so it stays up to date and safe from vulnerabilities. 
 

 

User Privilege Management 

I handled user permissions by checking which users are part of administrative groups and making sure only approved accounts have sudo access. I used the groups command to see which groups my user is in and confirmed that my account is part of the sudo group, which gives me admin rights when needed. This setup helps follow the principle of least privilege by giving elevated permissions only to those who really need them.] 
 

 

Network Security 

I set up network security mainly using UFW to limit incoming traffic and make the system less vulnerable. After moving the SSH service to port 2222, I opened that port by running sudo ufw allow 2222/tcp. Then I turned on the firewall with sudo ufw enable. I checked the settings using sudo ufw status, which confirmed that only port 2222 was allowed. This setup blocks unwanted or unauthorized connections, making the system more secure.] 
 

 

Brute-Force Protection (Fail2Ban) 

I set up protection against brute-force attacks by installing Fail2Ban, which watches for login attempts and automatically blocks IP addresses that keep trying to log in unsuccessfully. After I installed it, I checked if the service was working by running the command sudo systemctl status fail2ban, and it showed that it was active and keeping the system safe. This setup helps stop automated attacks on SSH and other services. 

 

3. Threat Model and Mitigation Strategies 

Identify at least three security threats to your Ubuntu system and describe corresponding mitigation strategies. Add screenshots or configuration evidence where relevant. 

SSH Brute-Force Attacks 

Description: Unauthorized SSH login attempts using password guessing. 

Mitigation Strategies I did: 

    Changed the SSH port from 22 to 2222 to reduce automated scanning. 

    Disabled root login to prevent direct attacks against the root account. 

    Disabled password authentication, forcing key-based authentication only. 

    Enabled the firewall (UFW) and allowed port 2222 for SSH access. 

    Ensured Fail2Ban is running to automatically block IPs with repeated failed login attempts. 

 

 
 

Exploitation of Unpatched Software 

Description: Attackers leveraging outdated packages to gain system access. 

Mitigation Strategies I did: 

    Verified that automatic security updates are enabled using Ubuntu’s unattended-upgrades service. 

    Checked the configuration in /etc/apt/apt.conf.d/20auto-upgrades to ensure security updates are applied automatically. 

    Ensured the package update mechanism runs regularly to install new patches without manual intervention. 

 
 

 
 

 

Privilege Escalation by Local Users 

Description: Users exploiting sudo or weak permissions to gain root privileges. 

Mitigation Strategies I did: 

    Verified user group membership using groups to ensure only trusted users have sudo rights. 

    Ensured the sudo group contains only authorized administrators by checking /etc/group. 

    Minimized unnecessary sudo access to reduce the chance of privilege escalation. 
     

 
 

 

 

Network Service Exploits 

Description: Unnecessary or open ports being used to access system services. 

Mitigation Strategies I did: 

    -Configured UFW to block all ports except SSH on port 2222, reducing the number of exposed services. 

    Verified open ports using sudo ss -tulpn to ensure no unnecessary services were listening. 

    Hardened SSH configuration so only key-based authentication is allowed over the permitted SSH port. 

 
 

 

  

 
 

 
 
