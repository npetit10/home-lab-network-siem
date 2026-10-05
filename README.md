# Home Lab: Segmented Network Architecture + SIEM Pipeline

A hands-on cybersecurity portfolio project simulating an enterprise network environment with multi-zone segmentation, firewall enforcement, and a full threat detection pipeline.

---

## Project Overview

This project builds a realistic "company network" inside a home lab using virtual machines. A pfSense firewall enforces strict zone-based rules across three internal zones (DMZ, LAN, Management) and the WAN, and a Wazuh SIEM ingests logs from every machine to detect threats in real time.

The lab scenario: an attacker attempts to breach a DMZ web server and pivot into the internal LAN. The firewall blocks the lateral movement, and the SIEM captures every step (reconnaissance, brute-force attempts, and the blocked pivot), which is then written up as a formal incident report.

---

## Project Status

🚧 **In progress (started July 2026).** This README documents the design. Configs, detection rules, and attack evidence are being added to the folders below as each phase is completed.

- [ ] Network diagram and threat model
- [ ] pfSense VLAN and firewall rule exports
- [ ] Wazuh deployment and agent onboarding
- [ ] Custom Wazuh rules mapped to MITRE ATT&CK
- [ ] Attack scenarios 01 to 05 with alert evidence
- [ ] Incident report

---

## Architecture

### Zones

| Zone | VLAN | Purpose |
|---|---|---|
| DMZ | 20 | Public-facing services; isolated from internal network |
| LAN | 10 | Trusted internal assets; simulates corporate environment |
| Management | 30 | Admin-only; hosts SIEM and analyst workstation |

### Key Firewall Rules

| Source | Destination | Action | Reason |
|---|---|---|---|
| DMZ | LAN | **BLOCK** | Prevent lateral movement from compromised DMZ |
| LAN | DMZ | ALLOW (HTTP/S only) | Internal access to web services |
| Management | All zones | ALLOW | SIEM log collection and admin access |
| Internet | DMZ | ALLOW (HTTP/S only) | Public-facing web server access |
| Internet | LAN | **BLOCK** | No direct external access to internal assets |

---

## Tools & Stack

| Category | Tool |
|---|---|
| Hypervisor | Proxmox VE |
| Firewall / Router | pfSense |
| SIEM | Wazuh (all-in-one) |
| Endpoint telemetry | Sysmon (Windows) |
| Log shipping | Wazuh agents + syslog (pfSense) |
| Traffic analysis | Wireshark / tcpdump |
| Attack simulation | Nmap, Hydra, Metasploit |
| Diagramming | draw.io |

---

## Repository Structure (planned)

```
home-lab-network-siem/
├── network-design/
│   ├── network-diagram.png       # Full zone diagram
│   ├── threat-model.md           # Written threat model and design rationale
│   └── firewall-rules-export/    # pfSense ruleset exports per interface
├── wazuh-rules/
│   ├── custom_rules.xml          # All custom detection rules
│   └── mitre-mapping.md          # Rules mapped to MITRE ATT&CK techniques
├── attack-scenarios/
│   ├── 01-port-scan/             # Nmap scan + Wazuh alert evidence
│   ├── 02-brute-force-ssh/       # Hydra attack + detection logs
│   ├── 03-failed-windows-logins/ # Event ID 4625 threshold alert
│   ├── 04-privilege-escalation/  # Sudo abuse detection
│   └── 05-lateral-movement/      # DMZ → LAN pivot attempt (blocked)
├── reports/
│   └── incident-report-sample.pdf  # Formal incident report writeup
└── README.md
```

---

## Skills Demonstrated

- Network segmentation and zone-based firewall design (pfSense)
- VLAN configuration and traffic isolation
- SIEM deployment and log pipeline construction (Wazuh)
- Custom detection rule authoring mapped to MITRE ATT&CK
- Attack simulation and adversary emulation
- Formal incident reporting and technical documentation
- Packet capture and traffic analysis (Wireshark)

---

*Project started July 2026 as part of a cybersecurity portfolio. All attack simulations were conducted in an isolated lab environment against machines I own and operate.*
