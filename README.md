# home-lab-network-siem

Secure Network Architecture and SIEM Pipeline used to simulate a live enterprise network using virtual machines with zone segmentation, firewall enforcement, and a full threat detection pipeline.
An attacker attempts to breach a DMZ web server and pivot into the internal LAN. The firewall blocks the lateral movement. The SIEM catches every step: reconnaissance, brute force, and the blocked pivot, generating a real incident that is documented as a formal report.

# Zones

DMZ           VLAN 20:  Public-facing services; isolated from internal network
LAN           VLAN 10: Trusted internal assets; simulates corporate environment
Management    VLAN 30: Admin-only; hosts SIEM and analyst workstation

# Firewall Rules

Source:      Destination:      Action:             Reason:
DMZ          LAN               BLOCK               Prevent lateral movement from compromised DMZ
LAN          DMZ               ALLOW (HTTPS only)  Internal access to web services
Management   All Zones         ALLOW               SIEM log collection and admin access
WAN          DMZ               ALLOW (HTTPS only)  Public-facing web server access
WAN          LAN               BLOCK               No direct external access to internal assets

# Tools and Stack

Hypervisor            Proxmox VE
Firewall/Router       pfSense
SIEM                  Wazuh (all-in-one)
Endpoint telemetry    Sysmon
Log Shipping          Wazuh agents + syslog(pfSense)
Traffic analysis      Wireshark / tcpdump
Attack Simulation     Nmap, Hydra, Metasploit
Diagramming           draw.io
