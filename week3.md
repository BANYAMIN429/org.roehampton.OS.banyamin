
# Week 3: Application Selection for Performance Testing 🧪

**Phase:** 3 (Application Selection)
**Focus:** Workload Selection, Installation, and Resource Profiling

---

## 1. Application Selection Matrix 📋

To accurately evaluate the operating system's performance under various conditions, I have selected a suite of industry-standard benchmarking tools and a real-world server application.

| Workload Type | Application Selected | Justification |
| :--- | :--- | :--- |
| **CPU-Intensive** | **Sysbench (Prime)** | A modular, cross-platform and multi-threaded benchmark tool. It calculates prime numbers to generate high CPU load and test thread scheduling. |
| **RAM-Intensive** | **Stress-ng** | A tool specifically designed to stress test computer subsystems. It can spawn workers to rapidly allocate and deallocate memory, forcing the OS to manage paging and swapping. |
| **I/O-Intensive** | **Sysbench (FileIO)** | Can simulate complex file operations (random read/write) to test disk I/O throughput and latency, essential for database server simulation. |
| **Network-Intensive** | **iPerf3** | The standard tool for active measurements of the maximum achievable bandwidth on IP networks. It will test the virtualized network bridge throughput. |
| **Server Application** | **Apache HTTP Server** | A widely-used web server. I will use it to test how the OS handles concurrent connection requests and process forking under load. |

---

## 2. Installation Documentation 💾

The following commands were executed via SSH on the Ubuntu Server (`192.168.1.187`) to install the selected toolset.

### **A. System Update**
First, I ensured the package lists were up to date to avoid conflicts.
```bash
sudo apt update && sudo apt upgrade -y
````

### **B. Installing Benchmarking Tools**

I installed `sysbench`, `stress-ng`, and `iperf3` from the default Ubuntu repositories.

```bash
sudo apt install -y sysbench stress-ng iperf3
```

  * **Verification:**
      * `sysbench --version`
      * `stress-ng --version`
      * `iperf3 --version`

### **C. Installing Server Application (Apache)**

I installed the Apache2 web server to serve as the long-running background service.

```bash
sudo apt install -y apache2
```

  * **Verification:** I checked the service status to ensure it started automatically.

<!-- end list -->

```bash
sudo systemctl status apache2
```

-----

## 3\. Expected Resource Profiles 📈

Based on the nature of these applications, I anticipate the following resource usage patterns during the testing phase (Week 6).

| Application | Primary Resource Impact | Secondary Impact | Expected Behavior |
| :--- | :--- | :--- | :--- |
| **Sysbench (CPU)** | **CPU (100%)** | Power/Heat | User space CPU usage will spike. The kernel scheduler should balance threads across available cores. |
| **Stress-ng (VM)** | **Memory (RAM)** | Disk (Swap) | Rapid RAM consumption. If physical RAM fills, the OS should start "swapping" to disk, causing significant slowdowns. |
| **Sysbench (File)** | **Disk I/O** | CPU (Wait) | High "I/O Wait" times in CPU metrics. The system may become unresponsive to other commands as the disk bus saturates. |
| **iPerf3** | **Network** | CPU (Softirq) | High network throughput (approx 1Gbps). CPU usage may rise due to interrupt handling for incoming packets. |
| **Apache2** | **Mixed** | Process Count | Moderate CPU and RAM usage, increasing linearly with the number of concurrent client connections. |

-----

## 4\. Monitoring Strategy 🔍

To capture data for the "Performance Evaluation" in Week 6, I will use the following approach for each application:

1.  **Terminal 1 (Controller):** SSH into the server to execute the workload command (e.g., `sysbench cpu run`).
2.  **Terminal 2 (Monitor):** SSH into the server to run real-time monitoring tools.
      * **Tool:** `htop` (for visual confirmation of CPU/RAM usage).
      * **Tool:** `dstat` or `vmstat 1` (for logging numerical data every second).
3.  **Data Capture:** I will take screenshots of `htop` showing the resource spike and copy the text output from the benchmark tool itself into my journal.

-----

## 5\. Learning Reflection 🧠

  * **Package Management:** I deepened my understanding of `apt` (Advanced Package Tool) and the importance of updating repositories (`apt update`) before installing new software to ensure dependency compatibility.
  * **Tool Selection:** I learned that "Performance" isn't a single metric. A system can have a fast CPU but be slow due to poor Disk I/O. Therefore, selecting specific tools like `sysbench` (for isolation) vs. `apache2` (for real-world simulation) provides a more holistic view of the OS.

