# Multi-Branch Enterprise Network with Attack & Defense Simulation
**Final Year Project — Bachelor of Computer Science (Cybersecurity), Lincoln University College**

---

## Project Overview

This project designs, implements, and secures a multi-branch enterprise network simulated across two environments:

- **Cisco Packet Tracer** — Full enterprise network design with production-grade configurations
- **GNS3** — Attack simulation environment to test the network's defenses under real-world threat scenarios

The network spans two branches connected over a WAN, with centralized security enforced through a Cisco ASA Firewall, redundancy through HSRP and LACP, and wireless management through CAPWAP.

---

## Network Architecture

### Branch 1 (Headquarters)
- Core multilayer switch with inter-VLAN routing
- Cisco ASA Firewall for perimeter security
- VLANs: Management (10), LAN (20), WLAN (50)
- DHCP, DNS, and Web servers in server room
- Wireless access points managed via CAPWAP

### Branch 2 (Remote Branch)
- Distribution switch with access layer
- VLANs: BLAN (60), BWLAN (90), BlackHole (99)
- Connected to HQ via IPSec VPN tunnel over WAN

### WAN / Internet
- Cisco ASA Firewall with dual ISP interfaces
- NAT/PAT for internal subnets to reach the internet
- OSPF for dynamic routing between branches

---

## Protocols & Technologies Used

| Category | Technologies |
|---|---|
| Routing | OSPF, Static Default Route |
| Redundancy | HSRP (gateway failover), LACP / EtherChannel |
| Security | Cisco ASA Firewall, ACLs, IPSec VPN, SSH (v2), Port Security |
| Switching | VLANs, Trunking, STP PortFast, BPDU Guard |
| NAT | NAT/PAT (dynamic NAT for all internal subnets) |
| Wireless | CAPWAP (WLC-based AP management) |
| Addressing | IP Subnetting, DHCP with helper-address |
| Management | SSH access restricted by ACL, password encryption, banner MOTD |

---

## Attack & Defense Simulation (GNS3)

The GNS3 environment replicates the network topology and was used to perform 9 penetration testing scenarios using tools including nmap, hping3, hydra, ike-scan, and nikto.

### Penetration Testing Results

| # | Attack | Tool | Result |
|---|---|---|---|
| 1 | Host Discovery | nmap -sn | Exposed |
| 2 | Port & Service Scan | nmap -sV | Partially Exposed |
| 3 | OS Detection | nmap -O | OS Identified |
| 4 | Firewall Evasion | nmap -f | Blocked |
| 5 | VPN Detection | ike-scan | Protected |
| 6 | DoS SYN Flood | hping3 --flood | Partially Affected |
| 7 | Internal Network Scan | nmap -sn | Blocked |
| 8 | SSH Brute Force | hydra | Blocked |
| 9 | Vulnerability Scan | nmap --script vuln / nikto | Blocked |

### Defenses Applied & Tested
- ACLs and firewall policies blocking unauthorized traffic and internal scans
- IPSec VPN protecting site-to-site tunnel (resistant to VPN detection)
- SSH access restricted to management VLAN only — brute force blocked
- BlackHole VLAN (99) isolating unused ports
- BPDU Guard preventing rogue switch injection
- Firewall permitting only required services (HTTP, DNS, DHCP, FTP, SMTP, CAPWAP)

### Tools Used
- **nmap** — Host discovery, port scanning, OS detection, firewall evasion, vulnerability scanning
- **hping3** — DoS SYN flood simulation
- **hydra** — SSH brute force testing
- **ike-scan** — VPN/IKE detection
- **nikto** — Web vulnerability scanning
- **Wireshark** — Packet capture and analysis to verify attack traffic and confirm defense effectiveness

---

## Repository Contents

This repository contains the README documentation and key configuration snippets. Project files are available upon request.

---

## Key Configuration Highlights

### Firewall ACL (permitting required services only)
```
access-list res-access extended permit icmp any any
access-list res-access extended permit udp any any eq 53   ! DNS
access-list res-access extended permit tcp any any eq 80   ! HTTP
access-list res-access extended permit tcp any any eq 25   ! SMTP
access-list res-access extended permit tcp any any eq 21   ! FTP
access-list res-access extended permit udp any any eq 5246 ! CAPWAP
access-list res-access extended permit udp any any eq 5247 ! CAPWAP
```

### HSRP (Gateway Redundancy)
```
interface vlan 20
 standby 20 ip 172.16.0.1
```

### SSH Restricted to Management VLAN
```
access-list 2 permit 192.168.10.0 255.255.255.0
access-list 2 deny any
line vty 0 4
 access-class 2 in
 transport input ssh
```

---

## Skills Demonstrated

- Enterprise network design and segmentation
- Perimeter security with Cisco ASA firewall policies
- Network redundancy and high availability (HSRP, LACP)
- Attack simulation and packet-level analysis with Wireshark
- Secure remote access configuration (SSH v2 with ACL restrictions)
- Wireless network management (CAPWAP / WLC)

---

## ⚠️ Ethical Disclaimer

All attack simulations and penetration testing techniques demonstrated in this project were conducted exclusively in an isolated virtual lab environment using GNS3. No real networks, systems, or devices were targeted at any point.

This project was developed strictly for academic and educational purposes as part of a Final Year Project in Cybersecurity. The tools and commands referenced are documented to demonstrate understanding of offensive and defensive security concepts under controlled conditions.

The author does not condone or encourage the use of these techniques against any real network or system without explicit written authorization. Unauthorized use of these methods may be illegal and unethical.

---

## Author

**MHD Wael Aljlilati**
LinkedIn: [linkedin.com/in/aljlilati-wael](https://linkedin.com/in/aljlilati-wael)
