# Clash of Teams 101 - Breach & Defend: Complete Adversarial Simulation Report 

This project demonstrates a full adversarial simulation involving both offensive (**Red Team**) and defensive (**Blue Team**) operations within a controlled virtual lab environment. The objective was to compromise a vulnerable system, then detect, analyze, and remediate the attack using professional monitoring techniques.

---

## Environment Setup
The lab was created using **VMware** with two machines configured on the same internal network.

| Role | Operating System | IP Address |
| :--- | :--- | :--- |
| **Attacker** | Kali Linux | 192.168.40.128  |
| **Target** | Metasploitable 2 | [cite_start]192.168.40.129  |

Connectivity was verified via a ping test to ensure both machines were reachable.

---

## Phase 1: Red Team (Offensive Execution)

### 1. Reconnaissance
The simulation began with information gathering using **Nmap**.
* **Command:** `nmap 192.168.40.129` 
* **Key Finding:** Port 139 (SMB service) was active.
* **Vulnerability:** Samba on Metasploitable 2 is known to contain exploitable vulnerabilities.

### 2. Exploitation
Exploitation was conducted using the **Metasploit Framework**.
* **Exploit Module:** `exploit/multi/samba/usermap_script` 
* **Payload:** Reverse TCP shell 
* **Result:** A command shell session was successfully opened. 
* **Access Level:** The `whoami` command confirmed **root** access.

---

##  Phase 2: Blue Team (Detection & Analysis)

### 1. Network Monitoring
Network traffic was captured using **Wireshark** before launching the exploit to observe the complete attack lifecycle.

### 2. Attack Timeline & Indicators of Compromise (IoC)


| No | Time (IST) | Protocol | Observation |
| :--- | :--- | :--- | :--- |
| 1-3 | 03:01:08 | TCP | Three-way handshake initiated on port 139. |
| 6 | 03:01:08 | SMB | Session Setup containing the malicious command. |
| 7 | 03:01:14 | TCP | Reverse connection established on port 4444. |
| 8 | 03:01:14 | TCP | Shell activity detected. |

---

## Phase 3: Remediation (Containment & Defense)

### 1. Firewall Mitigation
To block the attacker, an **iptables** rule was applied to the target system:
```bash
sudo iptables -A INPUT -s 192.168.40.128 -j DROP
```
This rule denies all incoming traffic from the attacker's IP address.

## 2. Verification of Fix

The exploit was attempted again, resulting in a ConnectionTimeout. The attack was effectively blocked, and no session was established.

# Purple Team Correlation
Every offensive action left a detectable trace at the network layer:

    Red Team Action: Nmap port scan → Blue Team Evidence: TCP connection attempts.
    Red Team Action: Malicious command delivered → Blue Team Evidence: SMB Session Setup packet.
    Red Team Action: Netcat callback → Blue Team Evidence: Reverse TCP connection on port 4444.

# Lessons Learned

    The project highlighted the importance of continuous network monitoring and proper firewall configuration.
    Timely patch management is essential to prevent exploitation of known vulnerabilities like Samba.
    The Purple Team approach strengthens security posture through collaboration between offensive and defensive strategies.
