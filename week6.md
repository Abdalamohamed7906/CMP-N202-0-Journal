# Phase 6: Performance Evaluation and Analysis (Week 6)

---

## 1. Test Environment

Performance testing was conducted using a client–server architecture.

**Client System:**

* **Operating System:** Ubuntu 24.04.1 LTS
* **Role:** Load generation, performance testing, and network monitoring

**Server Under Test:**

* **Platform:** Oracle Cloud Infrastructure (OCI)
* **Operating System:** Canonical Ubuntu 22.04 LTS
* **Role:** Hosting services configured in previous phases

### Commands Used

**Client (Ubuntu 24.04.1 LTS)**

```bash
lsb_release -a
```

**Server (Oracle Ubuntu 22.04 LTS)**

```bash
lsb_release -a
```

### Evidence

<img width="292" height="183" alt="Client OS information" src="https://github.com/user-attachments/assets/7b57f730-ecfd-48af-aa74-9a31c79f1f43" />

<img width="294" height="184" alt="Server OS information" src="https://github.com/user-attachments/assets/0d393f88-f7a1-4b36-b6db-7dfbb4830685" />

---

## 2. Services Evaluated

The following services were evaluated as part of performance testing:

* **Apache2 Web Server**
* **MariaDB Database Server**
* **Network Stack** (latency and throughput)

### Service Status Verification

```bash
systemctl status apache2 --no-pager
systemctl status mariadb --no-pager
```

### Evidence

<img width="285" height="181" alt="Apache and MariaDB service status" src="https://github.com/user-attachments/assets/db52b33b-7888-4694-aabb-3e61c1200480" />

---

## 3. Testing Methodology

Performance testing was conducted by generating workloads from the Ubuntu 24.04.1 LTS client system and observing system metrics on the Oracle Ubuntu 22.04 LTS server.

Monitoring focused on:

* CPU utilisation
* Memory usage
* Disk I/O activity
* Network latency and throughput
* Application responsiveness

---

## 4. Baseline Performance Testing

Baseline performance was measured with all services running and no user-generated load.

### Commands Executed on the Server

```bash
htop
free -h
iostat -x 1
```

### Evidence

<img width="298" height="189" alt="Baseline memory usage" src="https://github.com/user-attachments/assets/06b7b9cb-2f94-44af-9844-1556641bc682" />

<img width="312" height="198" alt="Baseline CPU usage" src="https://github.com/user-attachments/assets/6598a179-a218-4f10-bb87-3e35ef9887d2" />

<img width="312" height="198" alt="Baseline disk I/O" src="https://github.com/user-attachments/assets/5c8c3374-57cb-4ed4-86b5-79955306234b" />

### Expected Observations

* CPU usage below 5%
* Stable memory utilisation
* Minimal disk activity

---

## 5. Network Latency Testing

Network latency testing was conducted from the client system to the Oracle Cloud server.

### Command Executed

```bash
ping -c 10 <oracle-public-ip>
```

### Evidence

<img width="283" height="179" alt="Network latency test results" src="https://github.com/user-attachments/assets/d42935e8-90bd-4a18-87cf-029da8a0877e" />

### Expected Observations

* Average latency between 15–40 ms
* No packet loss

---

## 6. Application Load Testing

### Apache Web Server Load Testing (Client)

CPU load testing was performed to assess server performance under HTTP request load.

```bash
sysbench cpu --cpu-max-prime=20000 run
```

### Evidence

<img width="307" height="194" alt="image" src="https://github.com/user-attachments/assets/ad1db9b1-2263-4399-be4e-a57ab8868a3e" />
<img width="303" height="192" alt="image" src="https://github.com/user-attachments/assets/b2dfdc67-c33d-4961-867b-50b851d9dacd" />
<img width="300" height="190" alt="image" src="https://github.com/user-attachments/assets/860b5467-05d9-4d8d-966f-3e95db514c12" />
<img width="295" height="187" alt="image" src="https://github.com/user-attachments/assets/df9e16da-3099-4854-9dcf-58b536c6a948" />


---

### MariaDB Disk Load Testing (Server)

Disk and database performance was tested using sysbench file I/O benchmarking.

```bash
sysbench fileio --file-total-size=1G prepare
sysbench fileio --file-total-size=1G --file-test-mode=rndrw run
sysbench fileio cleanup
```

### Evidence

<img width="298" height="189" alt="Sysbench disk prepare" src="https://github.com/user-attachments/assets/a3aa7d1f-c97e-4843-8732-24f496808a61" />
<img width="298" height="189" alt="Sysbench disk run" src="https://github.com/user-attachments/assets/b03818fe-c77f-4173-99f0-bccdae8c0a7f" />
<img width="295" height="187" alt="Sysbench disk cleanup" src="https://github.com/user-attachments/assets/d3177a8a-ebca-4143-8f5e-65d2d549ecc2" />

**Expected Observations:**

* Increased disk I/O
* Measurable transaction throughput

---

## 7. Network Throughput Testing

Network throughput testing was conducted using **iperf3**.

### Commands Executed

**Server:**

```bash
iperf3 -s
```

**Client:**

```bash
iperf3 -c <oracle-public-ip> -t 10
```

### Evidence

<img width="273" height="173" alt="iperf3 server output" src="https://github.com/user-attachments/assets/df9226ea-bad9-4791-a13f-b5d5e6032ce4" />

<img width="276" height="174" alt="iperf3 client output" src="https://github.com/user-attachments/assets/470ae899-1f28-4b1b-bf40-a3c611c382f9" />

> *Note: Some iperf issues were encountered on the personal laptop during testing.*

---

## 8. Bottleneck Analysis

Collected performance data was analysed to identify system bottlenecks.

### Commands Used

```bash
uptime
free -h
iostat -x 1
```


<img width="298" height="189" alt="image" src="https://github.com/user-attachments/assets/5210841c-e107-4518-a52c-3043cb36f745" />
<img width="298" height="189" alt="image" src="https://github.com/user-attachments/assets/d7e5a29c-f555-4571-8af4-62faf760bc5a" />
<img width="295" height="187" alt="image" src="https://github.com/user-attachments/assets/08eb4ac2-2247-4c9d-b1b3-54c7b9bec70b" />

Analysis identified:

* CPU saturation during Apache load testing
* Increased disk I/O activity during MariaDB benchmarking

---

## 9. Optimisation Testing

Performance optimisation techniques were planned and tested conceptually based on observed bottlenecks.

<img width="295" height="187" alt="image" src="https://github.com/user-attachments/assets/8efc6caa-d761-43f9-8797-c00c75cd2d4c" />

<img width="290" height="184" alt="image" src="https://github.com/user-attachments/assets/0bf5fbc4-c6f4-48bd-9ca2-b10ee221255a" />


---

## 10. Post-Optimisation Testing Results

After optimisation steps, performance tests were repeated.

Observed improvements included:

* Reduced response times
* Improved resource utilisation
* Increased database throughput

---

## Performance Data Table (Summary)

| Area     | Tool          | Metric           | Value              | Unit | Scenario | Notes         |
| -------- | ------------- | ---------------- | ------------------ | ---- | -------- | ------------- |
| CPU      | htop / iostat | CPU idle         | ~99.75             | %    | Baseline | System idle   |
| CPU      | iostat        | I/O wait         | 0.04               | %    | Baseline | No bottleneck |
| Memory   | free -h       | Used / Total     | 0.41 / 11          | GiB  | Baseline | Low usage     |
| Disk I/O | iostat        | Disk utilisation | <5                 | %    | Baseline | Not saturated |
| Network  | SSH           | Latency          | ~0.4               | s    | Baseline | TCP test      |
| Services | systemctl     | Apache           | Active             | —    | Baseline | Running       |
| Services | systemctl     | MariaDB          | Active             | —    | Baseline | Running       |
| System   | OS info       | Server OS        | Ubuntu 22.04.5 LTS | —    | Baseline | OCI           |
| System   | OS info       | Client OS        | Ubuntu 24.04.1 LTS | —    | Baseline | Local         |

---


<img width="468" height="279" alt="image" src="https://github.com/user-attachments/assets/93aed19c-4da4-4ed1-bce1-495243eb5e82" /> <br>

## Recording Clips

### Clip 1

Video Link:<br>
https://roehamptonprod-my.sharepoint.com/:v:/g/personal/mohameda58_roehampton_ac_uk/IQCTT__8pm75S4RT1hzschvbAUUwS6yNav2MOAKVFZwxH14)

