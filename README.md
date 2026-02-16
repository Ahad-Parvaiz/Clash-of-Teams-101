# [cite_start]Clash of Teams 101 - Breach & Defend: Complete Adversarial Simulation Report [cite: 2, 3]

[cite_start]This project demonstrates a full adversarial simulation involving both offensive (**Red Team**) and defensive (**Blue Team**) operations within a controlled virtual lab environment[cite: 7]. [cite_start]The objective was to compromise a vulnerable system, then detect, analyze, and remediate the attack using professional monitoring techniques[cite: 8].

---

## 🏗️ Environment Setup
[cite_start]The lab was created using **VMware** with two machines configured on the same internal network[cite: 12].

| Role | Operating System | IP Address |
| :--- | :--- | :--- |
| **Attacker** | Kali Linux | [cite_start]192.168.40.128 [cite: 13] |
| **Target** | Metasploitable 2 | [cite_start]192.168.40.129 [cite: 13] |



[cite_start]Connectivity was verified via a ping test to ensure both machines were reachable[cite: 64, 68].

---

## 🔴 Phase 1: Red Team (Offensive Execution)

### 1. Reconnaissance
[cite_start]The simulation began with information gathering using **Nmap**[cite: 77]. 
* [cite_start]**Command:** `nmap 192.168.40.129` [cite: 79]
* [cite_start]**Key Finding:** Port 139 (SMB service) was active[cite: 140].
* [cite_start]**Vulnerability:** Samba on Metasploitable 2 is known to contain exploitable vulnerabilities[cite: 141].

### 2. Exploitation
[cite_start]Exploitation was conducted using the **Metasploit Framework**[cite: 144].
* [cite_start]**Exploit Module:** `exploit/multi/samba/usermap_script` [cite: 148]
* [cite_start]**Payload:** Reverse TCP shell [cite: 157]
* [cite_start]**Result:** A command shell session was successfully opened[cite: 154]. 
* [cite_start]**Access Level:** The `whoami` command confirmed **root** access[cite: 160, 161].

---

## 🔵 Phase 2: Blue Team (Detection & Analysis)

### 1. Network Monitoring
[cite_start]Network traffic was captured using **Wireshark** before launching the exploit to observe the complete attack lifecycle[cite: 178, 179].

### 2. Attack Timeline & Indicators of Compromise (IoC)


| No | Time (IST) | Protocol | Observation |
| :--- | :--- | :--- | :--- |
| 1-3 | 03:01:08 | TCP | [cite_start]Three-way handshake initiated on port 139[cite: 190]. |
| 6 | 03:01:08 | SMB | [cite_start]Session Setup containing the malicious command[cite: 191]. |
| 7 | 03:01:14 | TCP | [cite_start]Reverse connection established on port 4444[cite: 191]. |
| 8 | 03:01:14 | TCP | [cite_start]Shell activity detected[cite: 191]. |

---

## 🛡️ Phase 3: Remediation (Containment & Defense)

### 1. Firewall Mitigation
[cite_start]To block the attacker, an **iptables** rule was applied to the target system[cite: 197, 198]:
```bash
sudo iptables -A INPUT -s 192.168.40.128 -j DROP
```
This rule denies all incoming traffic from the attacker's IP address.

## 2. Verification of Fix

The exploit was attempted again, resulting in a ConnectionTimeout. The attack was effectively blocked, and no session was established.

# ⚖️ Purple Team Correlation
Every offensive action left a detectable trace at the network layer:
    Red Team Action: Nmap port scan → Blue Team Evidence: TCP connection attempts.
    Red Team Action: Malicious command delivered → Blue Team Evidence: SMB Session Setup packet.
    Red Team Action: Netcat callback → Blue Team Evidence: Reverse TCP connection on port 4444.

# 🎓 Lessons Learned
    The project highlighted the importance of continuous network monitoring and proper firewall configuration.
    Timely patch management is essential to prevent exploitation of known vulnerabilities like Samba.
    The Purple Team approach strengthens security posture through collaboration between offensive and defensive strategies.
