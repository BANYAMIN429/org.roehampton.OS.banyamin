# Week 2: Security Planning and Testing Methodology 🛡️

**Phase:** 2 (Design)
**Focus:** Security Baseline Design & Performance Testing Strategy

---

## 1. Performance Testing Plan 📊

To critically evaluate the Ubuntu Server's behavior under load (as required in Week 6), I have designed the following testing methodology. This ensures consistent measurement across different workloads.

### **A. Key Metrics & Tools**
I will monitor four core system resources using standard Linux command-line utilities.

| Metric | Description | Primary Tool | Alternative/Secondary |
| :--- | :--- | :--- | :--- |
| **CPU Usage** | Processor load and queue depth | `top` / `htop` | `vmstat` |
| **Memory (RAM)** | Used vs. Available memory and swap usage | `free -h` | `vmstat` |
| **Disk I/O** | Read/Write speeds and wait times | `iostat` | `iotop` |
| **Network** | Latency and packet loss | `ping` | `ip -s link` |

### **B. Testing Methodology**
For each application deployed in Week 3, I will execute a "Before" and "After" test:
1.  **Baseline State:** Record metrics while the system is idle (only SSH running).
2.  **Load State:** Record metrics while the application is actively processing tasks (e.g., simulating 50 user requests).
3.  **Data Collection:** Screenshots of `htop` and `iostat` will be captured during peak load to evidence resource bottlenecks.

---

## 2. Security Configuration Checklist 🔒

I have established a "Security Baseline" that must be implemented during the Deployment phase (Week 4 & 5). This checklist enforces the principle of least privilege and defense-in-depth.

### **✅ System Access & Authentication**
- [ ] **SSH Key-Based Authentication:** Generate RSA/ED25519 keys on Windows 10; transfer public key to server.
- [ ] **Disable Password Authentication:** Modify `/etc/ssh/sshd_config` to set `PasswordAuthentication no`.
- [ ] **Disable Root Login:** Ensure `PermitRootLogin no` is set to prevent direct root access.

### **✅ Network Security (Firewall)**
- [ ] **Install UFW:** Ensure Uncomplicated Firewall is active.
- [ ] **Default Policies:** Deny all incoming traffic; allow all outgoing traffic.
- [ ] **Allow SSH:** Open port 22/tcp specifically for the Windows 10 Host IP (`192.168.1.78`) to limit exposure.

### **✅ System Hardening**
- [ ] **Automatic Updates:** Configure `unattended-upgrades` to auto-patch security vulnerabilities.
- [ ] **Privilege Management:** Create a dedicated administrative user (`banyamin_admin`) and minimize `sudo` usage.
- [ ] **Intrusion Detection:** Plan to install **Fail2Ban** (Week 5) to ban IPs after repeated failed login attempts.

---

## 3. Threat Model 🕵️‍♂️

I have identified three specific threats relevant to a headless Linux server exposed to a network.

### **Threat 1: Brute-Force SSH Attacks**
* **Description:** Automated bots attempting thousands of password combinations to guess the root password.
* **Impact:** Unauthorized full system access and potential data theft or malware installation.
* **Mitigation Strategy:** Disable password authentication entirely (use SSH keys only) and implement **Fail2Ban** to lock out offenders.

### **Threat 2: Unpatched Software Vulnerabilities**
* **Description:** Attackers exploiting known bugs in outdated software (e.g., an old version of the Linux Kernel or SSH daemon).
* **Impact:** Remote Code Execution (RCE) or service denial.
* **Mitigation Strategy:** Enable **Automatic Security Updates** to ensure patches are applied daily without manual intervention.

### **Threat 3: Internal Privilege Escalation**
* **Description:** A compromised low-level user account trying to gain root access to destroy system files.
* **Impact:** Total loss of system integrity.
* **Mitigation Strategy:** Strict **Sudoer configuration** (only specific users allowed) and file permission auditing (755/644 rules).

---

## 4. Learning Reflection 🧠

This week focused on the *design* of a secure infrastructure rather than just installation.

* **Key Learning:** I learned that security is not a single tool but a layered approach ("Defense in Depth"). Simply having a password is insufficient; we need firewalls, keys, and automated monitoring working together.
* **CLI Practice:** I have started practicing file manipulation commands (`cp`, `mv`, `nano`) on the server to prepare for the configuration file editing required next week.
