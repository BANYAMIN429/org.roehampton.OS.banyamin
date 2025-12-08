
# Week 3: Application Selection for Performance Testing

## 1. Introduction
This week involves selecting and installing the specific applications that will be used to generate workloads on the server. The goal is to choose tools that isolate specific system resources (CPU, RAM, Disk I/O, Network) to enable precise performance analysis in Week 6.

## 2. Application Selection Matrix
I have selected the following utilities to represent the required workload types.

| Workload Type | Selected Application | Justification |
| :--- | :--- | :--- |
| **CPU-Intensive** | **Sysbench** (CPU Mode) | A standard industry benchmark tool capable of generating high CPU loads through complex prime number calculations. |
| **RAM-Intensive** | **Stress-ng** | A versatile stress-testing tool that can spawn specific workers to consume exact amounts of memory, triggering paging and swap usage. |
| **I/O-Intensive** | **Sysbench** (FileIO Mode) | Capable of generating random read/write operations to stress the virtual hard disk and file system. |
| **Network-Intensive** | **iPerf3** | A pure network bandwidth testing tool that measures maximum throughput between the workstation and server. |
| **Server Application** | **Apache2** (Web Server) | A widely used HTTP server. It allows for "Real-world" load testing by simulating multiple concurrent web requests. |

## 3. Installation Documentation
The following commands were executed via SSH to install the selected tools package.

### Update Package Lists
```bash
sudo apt update
````

### Install Benchmarking Tools

```bash
sudo apt install -y sysbench stress-ng iperf3 apache2
```

### Verification

Each installation was verified by checking the version number to ensure successful deployment.

  * `sysbench --version`
  * `stress-ng --version`
  * `iperf3 --version`
  * `apache2 -v`

## 4\. Expected Resource Profiles

This table predicts how the system is expected to behave during the testing phase (Week 6).

| Application | Expected Behavior | Primary Metric to Watch |
| :--- | :--- | :--- |
| **Sysbench (CPU)** | All allocated CPU cores should hit 100% utilization. System load average will rise significantly. | `User %` and `Load Average` |
| **Stress-ng (RAM)** | Available memory will drop rapidly. If it exceeds physical RAM, the system will start using "Swap" space, slowing down performance. | `Free Memory` and `Swap Usage` |
| **Sysbench (I/O)** | High volume of reads/writes. The CPU "Wait" time (wa) should increase as the processor waits for the disk to catch up. | `iowait` and `MB/s` |
| **iPerf3** | Network throughput should saturate the virtual 1Gbps link. CPU usage may rise slightly due to packet processing. | `Bitrate (Mbits/sec)` |
| **Apache2** | CPU and RAM usage will scale linearly with the number of concurrent connections (simulated users). | `Requests per second` |

## 5\. Monitoring Strategy

To capture this data, I will use the following monitoring approach:

1.  **Real-time:** Using `htop` for visual confirmation of load.
2.  **Logging:** Developing a bash script (`monitor-server.sh`) in Week 5 that runs `vmstat` and `mpstat` in the background to log precise numbers to a CSV file for analysis.

## 6\. Installation Evidence

The screenshot below demonstrates the successful installation of all selected tools on the Ubuntu Server.


