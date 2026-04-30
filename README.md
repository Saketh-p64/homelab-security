# Cybersecurity Home Lab

A personal home lab built for hands-on cybersecurity practice, 
detection engineering, and security research.

## Lab Architecture

- **Hypervisor:** Proxmox VE 9.1 running on Beelink Mini S12 Pro
- **Attacker VM:** Kali Linux 2026.1
- **Target VM:** Metasploitable 2
- **Network:** All VMs isolated on internal bridge (vmbr0)

## Projects

### 1. Network Reconnaissance — Nmap Service Scan

**Objective:** Map the attack surface of a target machine

**Tool:** Nmap 7.99

**Command:**
nmap -sV 192.168.1.156

**Findings:**
- 21/tcp — vsftpd 2.3.4 (backdoor CVE-2011-2523)
- 22/tcp — OpenSSH 4.7p1
- 23/tcp — Telnet (unencrypted)
- 80/tcp — Apache 2.2.8
- 3306/tcp — MySQL 5.0.51a (exposed database)
- 5900/tcp — VNC (unauthenticated remote access)
- 8180/tcp — Apache Tomcat

**Takeaway:** Target exposed 20+ services running 
outdated vulnerable software representing a large attack surface.

---

### 2. Exploitation — vsftpd 2.3.4 Backdoor (CVE-2011-2523)

**Objective:** Exploit a known backdoor vulnerability to gain root access

**Tool:** Metasploit Framework

**Steps:**
1. Launched msfconsole
2. Loaded module: exploit/unix/ftp/vsftpd_234_backdoor
3. Set RHOSTS to target IP
4. Set LHOST to attacker IP
5. Executed exploit

**Result:** Successfully obtained root shell on target machine. 
Confirmed root access by reading /etc/shadow — 
containing hashed credentials for all system users.

**Remediation:**
- Update vsftpd to a non-backdoored version
- Block FTP (port 21) at firewall if not needed
- Use SFTP instead of FTP for secure file transfers
- Implement network segmentation to limit exposure

**CVE:** CVE-2011-2523
