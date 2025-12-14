# Week 6: Performance Evaluation and Analysis

<br>

---

## 1. Test Environment Overview

Performance testing was conducted using a client-server setup. The client system (Ubuntu 24.04.1 LTS) was used to initiate tests and network checks, while the Oracle Cloud server (Ubuntu 22.04 LTS) was monitored for CPU, memory, and disk I/O behaviour under different workloads.

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

<img width="292" height="183" alt="image" src="https://github.com/user-attachments/assets/7b57f730-ecfd-48af-aa74-9a31c79f1f43" />

<img width="294" height="184" alt="image" src="https://github.com/user-attachments/assets/0d393f88-f7a1-4b36-b6db-7dfbb4830685" />


<br><br>

---

## 2. Service Status Verification

Core services required for testing were verified as running before collecting baseline metrics. This confirms the server is operational and configured correctly.

### Commands Used

```bash
systemctl status apache2 --no-pager
systemctl status mariadb --no-pager
```

### Evidence

<img width="285" height="181" alt="image" src="https://github.com/user-attachments/assets/db52b33b-7888-4694-aabb-3e61c1200480" />


<br><br>

---

## 3. Baseline Performance Metrics (CPU + Memory)

Baseline measurements establish a reference point for later analysis. `free -h` was used to capture memory distribution, and `htop` was used for real-time CPU/process monitoring.

### Commands Used

```bash
free -h
htop
```

### Evidence
<img width="298" height="189" alt="image" src="https://github.com/user-attachments/assets/06b7b9cb-2f94-44af-9844-1556641bc682" />
<img width="312" height="198" alt="image" src="https://github.com/user-attachments/assets/6598a179-a218-4f10-bb87-3e35ef9887d2" />

<br><br>

---

## 4. Baseline Disk I/O Performance (iostat)

Disk behaviour was monitored using `iostat -x 1` to capture extended disk statistics such as read/write throughput, I/O wait, and utilisation.

### Commands Used

```bash
iostat -x 1
```

### Evidence

<img width="312" height="198" alt="image" src="https://github.com/user-attachments/assets/5c8c3374-57cb-4ed4-86b5-79955306234b" />

<br><br>

---

## 5. Network Performance Analysis (Latency)

Latency was tested from the client system to the Oracle server. This provides baseline network responsiveness and helps identify whether network conditions may impact service performance.

### Commands Used

```bash
ping -c 10 <oracle-public-ip>
```

### Evidence

<img width="1000" src="evidence/screenshots/08_latency_test.png" />

<br><br>

---

## 6. Network Performance Analysis (Throughput)

Throughput was tested using iperf3 in a client-server configuration. The server was placed in listening mode, and the client initiated the throughput test.

### Commands Used

**Server**

```bash
iperf3 -s
```

**Client**

```bash
iperf3 -c <oracle-public-ip> -t 10
```

### Evidence

<img width="1000" src="evidence/screenshots/09_iperf_server.png" />
<img width="1000" src="evidence/screenshots/10_iperf_client.png" />

<br><br>

---

## 7. CPU Load Testing (sysbench)

CPU load testing was performed using sysbench to evaluate CPU performance under stress. This supports identification of CPU-related bottlenecks and provides measurable output (events per second and execution time).

### Commands Used

```bash
sysbench cpu --cpu-max-prime=20000 run
```

### Evidence

<img width="1000" src="evidence/screenshots/11_sysbench_cpu.png" />

<br><br>

---

## 8. Disk Load Testing (sysbench fileio)

Disk load testing was performed using sysbench file I/O benchmarking. Test files were created, a random read/write benchmark was executed, and temporary files were cleaned up afterwards.

### Commands Used

```bash
sysbench fileio --file-total-size=1G prepare
sysbench fileio --file-total-size=1G --file-test-mode=rndrw run
sysbench fileio cleanup
```

### Evidence

<img width="1000" src="evidence/screenshots/12_fileio_prepare.png" />
<img width="1000" src="evidence/screenshots/13_fileio_run.png" />
<img width="1000" src="evidence/screenshots/14_fileio_cleanup.png" />

<br><br>

---

## 9. Bottleneck Analysis

Bottleneck analysis was performed by comparing system behaviour across CPU, memory, and disk metrics during testing. Load averages, memory availability, and disk utilisation were used as indicators of resource constraints.

### Commands Used

```bash
uptime
free -h
iostat -x 1
```

### Evidence

<img width="1000" src="evidence/screenshots/15_uptime_loadavg.png" />
<img width="1000" src="evidence/screenshots/16_free_postload.png" />
<img width="1000" src="evidence/screenshots/17_iostat_postload.png" />

<br><br>

---

## 10. Performance Visualisations (Charts and Graphs)

Performance visualisations were created using collected baseline metrics to provide a clear overview of CPU, memory, and disk activity. A combined baseline chart summarises resource availability and activity.

### Included Visualisations

* Baseline CPU utilisation
* Baseline memory distribution
* Baseline disk I/O throughput
* Combined baseline performance summary (single image)

### Evidence

<img width="1000" src="evidence/graphs/01_baseline_summary_graph.png" />

<br><br>

---

## 11. Performance Data Table (Structured Measurements)

A structured performance table was created to summarise baseline measurements across CPU, memory, disk I/O, network, and services. This table supports comparison and analysis across metrics.


| Area | Tool | Metric | Value | Unit | Scenario | Notes |
|-----|------|--------|-------|------|----------|-------|
| CPU | htop / iostat | CPU utilisation (idle) | ~99.75 | % | Baseline | System idle |
| CPU | iostat | CPU I/O wait | 0.04 | % | Baseline | No CPU bottleneck |
| Memory | free -h | Total memory | 11 | GiB | Baseline | Installed RAM |
| Memory | free -h | Used memory | 0.41 | GiB | Baseline | Low usage |
| Memory | free -h | Free memory | 8.2 | GiB | Baseline | High availability |
| Memory | free -h | Swap usage | 0 | GiB | Baseline | Swap disabled |
| Disk I/O | iostat | Disk read throughput | 8.83 | kB/s | Baseline | Low activity |
| Disk I/O | iostat | Disk write throughput | 68.27 | kB/s | Baseline | Background writes |
| Disk I/O | iostat | Disk utilisation | <5 | % | Baseline | Disk not saturated |
| Network | SSH | Connection latency | ~0.4 | seconds | Baseline | TCP-based test |
| Services | systemctl | Apache service status | Active | N/A | Baseline | Web server running |
| Services | systemctl | MariaDB service status | Active | N/A | Baseline | Database running |
| System | OS info | Server OS | Ubuntu 22.04.5 LTS | N/A | Baseline | Oracle Cloud |
| System | OS info | Client OS | Ubuntu 24.04.1 LTS | N/A | Baseline | Local workstation |


<br><br>

---

## Included Scripts / Tools Used

* `sysbench`
* `sysstat (iostat)`
* `iperf3`
* `htop`

<br>

---

## Recording Clips

### Clip 1

Video Link:
[Paste GitHub / OneDrive / Google Drive link here]

