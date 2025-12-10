# Week 3: Performance Testing Journal  

<br>

---

## 1. Applications  

The applications below were selected to represent different workload types required for performance evaluation on the Ubuntu system.  

<br>

### Workload Table  

| **Workload Type**     | **Application**             | **Justification**                                                |
|-----------------------|-----------------------------|------------------------------------------------------------------|
| **CPU-Intensive**     | stress-ng (CPU mode)        | Generates repeatable and controlled CPU load.                    |
| **RAM-Intensive**     | stress-ng (VM/memory mode)  | Allows configurable memory allocation to test RAM pressure.      |
| **I/O-Intensive**     | fio                         | Industry-standard disk I/O benchmarking tool.                    |
| **Network-Intensive** | iperf3                      | Measures network performance such as throughput and bandwidth.   |
| **Server Application**| apache2                     | Real-world HTTP server to test backend server performance.       |

<br>
<br>

---

## 2. Installation Documentation (SSH-Based Installation)  

Below are the exact commands used to install each performance-testing application:  

<br>

### Install stress-ng, fio, iperf3, apache2  

```bash
sudo apt update
sudo apt install stress-ng fio iperf3 apache2 -y
````

<br>

### Verify installations

```bash
stress-ng --version
fio --version
iperf3 --version
apache2 -v
```

<br>
<br>

---

## 3. Expected Resource Profiles

<br>

### **CPU-Intensive (stress-ng CPU)**

High CPU usage (80–100%), low RAM, no disk or network activity.

<br>

### **RAM-Intensive (stress-ng VM)**

High RAM usage depending on allocation size, moderate CPU usage.

<br>

### **I/O-Intensive (fio)**

High disk read/write activity, low CPU and RAM usage.

<br>

### **Network-Intensive (iperf3)**

High network bandwidth usage, low CPU, minimal RAM.

<br>

### **Server Application (apache2)**

Low CPU when idle, spikes under load; low-to-moderate RAM and network usage.

<br>
<br>

---

## 4. Monitoring Strategy

This section details the measurement approach for each application using monitoring tools: **htop**, **iotop**, and **iftop**.

<br>

---

### **CPU Test – stress-ng**

Commands used:

```bash
htop
stress-ng --cpu 4 --timeout 60s
```

The CPU -intenstive tess was run with stress-ng to make 4 CPU cores work for 60 seconds. At the same time, htop was used to check the CPU usage, the overall load, and what processes were running.
<br>

---

### **RAM Test – stress-ng VM**

Commands used:

```bash
htop
stress-ng --vm 2 --vm-bytes 1G --timeout 60s
```

The RAM-intensive test was run using stress-ng, with two memory workers each using 1GB of RAM. At the same time, htop was used to check how much RAM was being used and what processes were running. As expected, htop showed a big rise in memory use during the test, showing that the system was using a lot of memory. 

<br>

---

### **Disk Test – fio**

Commands used:

```bash
sudo iotop
fio --name=seqwrite --filename=fiotestfile --size=1G --bs=1M --rw=write --direct=1
rm fiotestfile
```

For the storage test, I used fio to write a 1GB file and kept an eye on iotop to check the disk activity. Fio showed that the system was writing data quickly, which meant the storage drive was working hard. Iotop also showed higher write activity at the same time. This confirmed that the test was properly stressing the disk.

<br>

---

### **Network Test – iperf3**

Commands used:

```bash
iperf3 -s
iperf3 -c 192.168.1.84 -t 10
```
I used iPerf3 to check how well my system's network is performing. I started an iPerf3 server and then connected a client to it. The test lasted for 10 seconds and showed a steady bitrate of about 123 Gbytes per second. Both the server and client reports had the same results, which means the network can handle high data transfer speeds. The screenshots show the speed and bitrate numbers from the test.
<br>

---

### **Server Application Test – apache2**

Commands used:

```bash
sudo systemctl start apache2
sudo systemctl status apache2
ab -n 500 -c 50 http://127.0.0.1/
htop
```

AFor the server-intensive test, I used ApacheBench to send 1,000 requests to my Apache web server, with 50 requests running at the same time. While the test was happening, I used htop to check how much CPU and memory Apache was using. The ApacheBench results showed that all 1,000 requests were completed without any problems, and the server handled them quickly. The test also showed how many requests the server could handle each second and how long each request took. The htop display showed that Apache used more CPU during the test, which shows the workload was stressing the web server. 

<br>
<br>

---

## References

* Colin King, *stress-ng User Manual*, Ubuntu Manpages – [https://manpages.ubuntu.com/manpages/stable/man1/stress-ng.1.html](https://manpages.ubuntu.com/manpages/stable/man1/stress-ng.1.html)
* Jens Axboe, *Flexible I/O Tester (fio) Documentation* – [https://fio.readthedocs.io/en/latest/](https://fio.readthedocs.io/en/latest/)
* ESnet, *iperf3 Tool Documentation* – [https://iperf.fr/iperf-doc.php](https://iperf.fr/iperf-doc.php)
* Apache Software Foundation, *Apache HTTP Server Docs* – [https://httpd.apache.org/docs/](https://httpd.apache.org/docs/)

---

