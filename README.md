# Bare-Metal Cybersecurity Home Lab

<p align="center">
  <a href="https://securityonionsolutions.com/"><img src="https://img.shields.io/badge/Security-Onion-blue"></a>
  <a href="https://www.splunk.com/"><img src="https://img.shields.io/badge/Splunk-Log%20Analysis-orange"></a>
  <a href="https://www.cisco.com/"><img src="https://img.shields.io/badge/Cisco-Catalyst%203550-green"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey"></a>
</p>

---

## Table of Contents
- [Overview](#overview)
- [Architecture / Topology](#architecture--topology)
- [Interconnections](#interconnections)
- [Environment & Tools](#environment--tools)
- [Lab Setup & Configuration](#lab-setup--configuration)
  - [Planned Detailed Guides](#planned-detailed-guides)
- [Use Cases & Skills Learned](#use-cases--skills-learned)
- [Future Improvements](#future-improvements)
- [References](#references)

---

## Overview
This project documents my **Bare-Metal Cybersecurity Home Lab**, designed to replicate a small enterprise network using physical devices.  

Unlike my virtual lab, this setup leverages **dedicated hardware** including a Cisco Catalyst switch, Security Onion running bare-metal, and a Splunk server for centralized log aggregation. The lab enables hands-on training in:

- Active Directory administration (Windows Server)  
- Security monitoring and IDS/IPS (Security Onion)  
- Log aggregation and correlation (Splunk)  
- Penetration testing (Kali Linux)  
- Network traffic mirroring via Cisco SPAN  

---

## Architecture / Topology

![Lab Topology](docs/topology-diagram.png)  
*Figure 1 – Bare-metal home cybersecurity lab topology with Splunk for log management.*

### Devices and Roles

- **Cisco Catalyst 3550** – Managed switch with SPAN configured to mirror traffic.  
- **Security Onion (dual NICs)**  
  - Mgmt NIC: Admin access 
  - Sniff NIC: Monitors mirrored traffic (Zeek & Suricata).  
- **Splunk Server** – Central log aggregator, ingesting logs from:  
  - Windows Server (AD/DNS/DHCP)  
  - Windows 11 management workstation  
  - Security Onion (HEC/syslog)  
  - Cisco Catalyst (syslog)  
- **Windows Server 2019 (AD)** – Domain Controller with DNS/DHCP.  
- **Windows 11 Management Workstation** – Domain-joined client for testing.  
- **Kali Linux** – Offensive penetration testing and red team exercises.  
- **Home Router** – Gateway for updates and external connectivity.  

---

## Interconnections

- **Router → Catalyst** – Primary uplink.  
- **Catalyst → Security Onion (Mgmt + Sniff)** – SPAN monitoring + admin access.  
- **Catalyst → Splunk Server** – Mgmt/log forwarding.  
- **Catalyst → Windows Server / Workstation / Kali** – Endpoints + AD environment.  
- **Security Onion → Splunk Server** – Forward IDS/NSM logs.  
- **Windows Devices → Splunk Server** – Forward Windows event logs.  

---

## Environment & Tools

### Hardware
- **Switch**: Cisco Catalyst 3550  
- **Security Onion**: Bare-metal install (MacBook)  
- **Splunk Server**: Rocky Linux 9 (dedicated machine)  
- **Windows Server 2019**: AD, DNS, DHCP  
- **Windows 11 Pro**: Management workstation  
- **Kali Linux**: Attack box  

### Defensive Tools
- **Active Directory** – Authentication, DNS/DHCP.  
- **Security Onion** – Zeek, Suricata, Elastic Stack, syslog forwarding.  
- **Splunk** – Log aggregation, dashboards, and SIEM capabilities.  

### Offensive Tools
- **Kali Linux Toolset** – Includes Hydra, Evil-WinRM, SecLists, NetExec, and other post-exploitation frameworks.  

---

## Lab Setup & Configuration
This section provides a **summary of the setup**. Detailed build notes will follow in `/docs`.

### Key Configurations
- **Cisco Catalyst 3550**  
  - Configured SPAN: mirrors uplink + access ports to Security Onion sniff NIC.  
- **Security Onion**  
  - Dual NIC configuration (Mgmt + Sniff).  
  - Zeek and Suricata monitoring.  
  - Logs forwarded to Splunk.  
- **Splunk Server**  
  - Receives logs via:  
    - Port 9997 (Universal Forwarders).  
    - Port 8088 (HEC).  
    - Port 514 (Syslog).  
  - Dashboards for DNS, AD events, Zeek/Suricata alerts.  
- **Windows Server 2019**  
  - Domain Controller with DNS/DHCP.  
  - Security events forwarded to Splunk.  

---

### Planned Detailed Guides
*(Coming Soon — placeholders for expansion)*  
* **Cisco Catalyst SPAN Configuration**  
* **Security Onion (Bare-Metal) Installation**  
* **Splunk Forwarder Configuration**  
* **Windows Server AD Setup**  
* **Log Integration Dashboards**  

---

## Use Cases & Skills Learned

This lab provides a platform for real-world scenarios across Blue, Red, and Purple team practices.  

<details>
<summary>Blue Team Skills</summary>

- Active Directory administration and hardening  
- IDS/IPS deployment (Zeek, Suricata on Security Onion)  
- Log forwarding and aggregation (Splunk)  
- Dashboard creation and event correlation  

</details>

<details>
<summary>Red Team Skills</summary>

- Brute-force authentication testing (Hydra)  
- Post-exploitation with Evil-WinRM  
- Lateral movement via NetExec  
- Reconnaissance and exploitation with SecLists and RDP  

</details>

<details>
<summary>Purple Team Skills</summary>

- Map red team techniques to blue detections (MITRE ATT&CK)  
- Validate SIEM alerts against simulated attacks  
- Improve logging pipelines based on findings  
- Develop iterative detection playbooks  

</details>

**Example Scenario:**  
> Simulate brute-force login attempts from Kali → Detect failed logins in Security Onion → Correlate with Splunk → Tune alert rules.  

---

## Future Improvements (Roadmap)

**Near Term**
- Add pfSense firewall as perimeter defense.  
- Expand Splunk dashboards with custom detection rules.  

**Mid Term**
- Deploy honeypots for threat emulation.  
- Add Linux endpoints for mixed-environment testing.  

**Long Term**
- Automate builds with Ansible/Terraform.  
- Expand to cloud integration (AWS/Azure testbed).  

---

## References
- [Security Onion Documentation](https://securityonion.net/docs/)  
- [Splunk Docs](https://docs.splunk.com/)  
- [Cisco Catalyst 3550 Config Guide](https://www.cisco.com/)  
- [Windows Server AD Overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)  
- [Kali Linux Tools](https://www.kali.org/tools/)  
