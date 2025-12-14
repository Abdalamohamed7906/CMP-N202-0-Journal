# Week 4: Initial System Configuration & Security Implementation

<br>

---

## 1. Configure SSH with Key-Based Authentication  

I set up a SSH key-based authentication to access the server safely without using passwords. I generated a pair of keys, and the private key was kept securely on the work computer with tight access controls. Logging in with the SSH key shows that the connection is encrypted and doesn't need a password, which helps protect against attacks that try many passwords. 

### Commands Used
```bash
ssh -i ssh-key-2025-12-12.key ubuntu@140.238.77.45
whoami
uname -r
````

<img width="2560" height="1600" alt="clip1ss1" src="https://github.com/user-attachments/assets/08127359-26f5-4edc-b3ec-b51acf1ff762" />
<img width="2560" height="1600" alt="clip1ss2" src="https://github.com/user-attachments/assets/9b3424d8-995e-4311-b3d5-e80954e590d3" />
<img width="2560" height="1600" alt="clip1ss3" src="https://github.com/user-attachments/assets/137899bc-4527-48d4-af7e-415f61423ad7" />

<br><br>

---

## 2. Configure Firewall Permitting SSH from One Workstation Only

I set up a Uncomplicated Firewall (UFW) to let in SSH connections only from one trusted computer IP address. Any other attempts to connect via SSH from different IPs were blocked. This helped protect the system by restricting remote access to just one known source.

### Commands Used

```bash
sudo ufw status numbered
sudo ufw allow from 192.168.1.84 to any port 22
sudo ufw deny 22/tcp
sudo ufw enable
sudo ufw status numbered
```

<img width="2560" height="1600" alt="clip2ss1" src="https://github.com/user-attachments/assets/05cb3394-cb5e-4c72-a8d1-5516e8fa6dcc" />
<img width="2560" height="1600" alt="clip2ss2" src="https://github.com/user-attachments/assets/b1806fcf-c5b2-4ef4-8786-cf9ce56f371d" />
<img width="2560" height="1600" alt="clip2ss3" src="https://github.com/user-attachments/assets/9a84ceed-3ad7-4a2e-bcd3-d15c4611f77d" />
<img width="2560" height="1600" alt="clip2ss5" src="https://github.com/user-attachments/assets/2c88a4bd-6888-49f1-a54f-7fcc2b17f242" />

<br><br>

---

## 3. Manage Users and Implement Privilege Management

A non-root admin user called 'adminuser' was made to follow the principle of least privilege. This user was given sudo access to do admin work without using the root account directly. SSH key access was set up for this user, and the permissions were checked to make sure the login was secure.

### Commands Used

```bash
sudo adduser adminuser
sudo usermod -aG sudo adminuser

sudo mkdir /home/adminuser/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/adminuser/.ssh/
sudo chown -R adminuser:adminuser /home/adminuser/.ssh
sudo chmod 700 /home/adminuser/.ssh
sudo chmod 600 /home/adminuser/.ssh/authorized_keys

ssh -i ssh-key-2025-12-12.key adminuser@140.238.77.45
whoami
sudo apt update
```

<img width="2560" height="1600" alt="clip3ss1" src="https://github.com/user-attachments/assets/b9a22b4e-9bdb-46fc-b0fc-0d8f77279cb8" />
<img width="2560" height="1600" alt="clip3ss2" src="https://github.com/user-attachments/assets/ce74f6d4-2370-4e8e-abdc-185a1ca8fbc2" />
<img width="2560" height="1600" alt="clip3ss3" src="https://github.com/user-attachments/assets/248bfd6e-ec6c-4486-aac8-5cec2b130578" />
<img width="2560" height="1600" alt="clip3ss4" src="https://github.com/user-attachments/assets/aedbd434-0a55-41b4-a418-64fbd9f5ca82" />
<img width="2560" height="1600" alt="clip3ss5" src="https://github.com/user-attachments/assets/d4ba3ea9-859a-46ce-b9a8-db675cc54a51" />
<img width="2560" height="1600" alt="clip3ss6" src="https://github.com/user-attachments/assets/7856b1a6-a56d-4f04-ba23-7a4c0aae627b" />
<img width="2560" height="1600" alt="clip3ss7" src="https://github.com/user-attachments/assets/1d7094fe-3d41-4c3b-b9da-03b2e9d6fd79" />
<img width="2560" height="1600" alt="clip3ss8" src="https://github.com/user-attachments/assets/d0826d80-e064-4608-83c2-dc3c4035cacf" />
<img width="2560" height="1600" alt="clip3ss9" src="https://github.com/user-attachments/assets/6d566962-02a1-45e6-97d4-74b856f4edaa" />
<img width="2560" height="1600" alt="clip3ss10" src="https://github.com/user-attachments/assets/7c778db3-6268-4ca6-b428-b152c24d7026" />

<br><br>

---

## 4. Configuration Files

A non-root admin user called 'adminuser' was made to follow the principle of least privilege. This user was given sudo access to do admin work without using the root account directly. SSH key access was set up for this user, and the permissions were checked to make sure the login was secure.

### Commands Used

```bash
sudo nano /etc/ssh/sshd_config
sudo systemctl restart ssh
sudo systemctl status ssh
```

<img width="2560" height="1600" alt="clip4ss 1" src="https://github.com/user-attachments/assets/d4254f1f-c4f3-4948-b713-43231d244384" />
<img width="2560" height="1600" alt="clip4ss 2" src="https://github.com/user-attachments/assets/8ad4073a-bb3d-4686-9306-2075d94ffe0e" />
<img width="2560" height="1600" alt="clip4ss 3" src="https://github.com/user-attachments/assets/4b287df2-9856-4479-9811-163c491cab76" />

<br><br>

---

## 5. Remote Administration Evidence

Remote administration was shown by running system management commands through SSH. Commands like updating packages and checking service status prove that the administrative user can safely manage the server from a distance.

### Commands Used

```bash
sudo apt update
sudo systemctl status ssh
hostnamectl
```

<img width="2560" height="1600" alt="clip5ss1" src="https://github.com/user-attachments/assets/1c4f5889-cf08-477b-8099-93d925511cd3" />
<img width="2560" height="1600" alt="Screenshot from 2025-12-12 05-41-51" src="https://github.com/user-attachments/assets/e904c06f-a85d-4fec-be93-b7d0c5f27837" />

<br><br>

---

## 6. **Video / Clip Uploads**


* Clip 1 – <br>
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQDhaTIAYQfFT6jzAwmCjhgVAX6RzGjr4ib_gpJomRjQlEY <br>

* Clip 2 – <br>
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQCYx6IpnVUaSIuFMoKLu4SQAUGujEBtcXaCOWwi8Tj4GYY) <br>

* Clip 3 – <br>
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQCY02Hs7IaiTIKufQXVwVNYAan2diNcyjDkA6MWaqWUW5o <br>

* Clip 4 – <br>
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQDn1ODmTgL5QaQBTjfqai56AYZRDXRIitA0JSQn1H7SZ8I) <br>

* Clip 5 –
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQCtRJ0Q8acrQ5d8huxR2xMhAcBGKDVVZOQwlq4HRhtLl3k
<br><br>

---

