# Enterprise Network Security Lab
<p align="center">
  <b>A complete enterprise-grade network security infrastructure simulation built on EVE-NG</b><br><br>
  <i>FortiGate HA Cluster · IPsec Site-to-Site VPN · SSL VPN · IPS/IDS · VLAN Segmentation · SNMP Monitoring</i>
</p>
<p align="center">
  <a href="#features">Features</a> · 
  <a href="#architecture">Architecture</a> · 
  <a href="#results">Results</a>
</p>

## Overview
This project simulates enterprise network infrastructure using industry-standard tools and technologies. This lab demonstrates practical network engineering and security concepts, including high firewall availability, IPsec VPN encryption tunneling combined with OSPF dynamic routing, VLAN segmentation, intrusion prevention, and infrastructure monitoring.
## Features
| Feature | Technology | Status |
| :--- | :--- | :---: |
| Network Simulation | EVE-NG Community 6.2.0-4 | ✅ |
| Next-Gen Firewall | FortiGate v7.0.12 | ✅ |
| High Availability | FortiGate Active-Passive HA | ✅ |
| WAN Routing | Cisco vIOS Router (R3) | ✅ |
| VLAN Segmentation | 5 VLANs (Users, Servers, Guest, DMZ) | ✅ |
| DHCP Services | FortiGate DHCP per VLAN | ✅ |
| IPsec VPN | Site-to-Site (2 remote sites) | ✅ |
| Routing |  OSPF over IPsec VPN | ✅ |
| SSL VPN | Web Mode + Tunnel Mode | ✅ |
| Intrusion Prevention | IPS-Lab Profile (32 signatures) | ✅ |
| Web Filtering | WebFilter-Lab Profile | ✅ |
| SNMP Monitoring | SNMPv2c Community | ✅ |
| Firewall Policies | 9 rules with NAT & IPS | ✅ |
| Attack/Defense Scenarios | Port scan, VLAN isolation tests | ✅ |
## Architecture
### Network Topology
<img width="1452" height="866" alt="Ảnh chụp màn hình 2026-06-23 094517" src="https://github.com/user-attachments/assets/120b9aa5-f5ab-4f17-804d-d3f4e636db61" />

### Physical Connections
| Device | Interface | Connected To | Interface |
| :--- | :--- | :--- | :--- |
| R3 | e0/0 | FGT1 | port1 |
| R3 | e0/0 | FGT2 | port1 |
| R3 | e0/2 | R4 | e0/0 |
| R3 | e0/3 | FGT5 | port1 |
| FGT1 | port2 | SW-Core | e0/0 (trunk) |
| FGT2 | port2 | SW-Core| e0/1 (trunk) |
| FGT1 | port4 | FGT2 | port4 (HA) |
| FGT1 | port3 | Cloud0 | pnet0 (Mgmt) |
| SW-Core | e1/0 | VPC | eth0 (VLAN10) |
| SW-Core | e0/3 | VPC7 | eth0 (VLAN10) |
| SW-Core | e1/2 | Server | eth0 (VLAN20) |
| SW-Core | e2/1 | PC9 | eth0 (VLAN30) |
| SW-Core | e2/3 | PC8 | eth0 (VLAN50) |
##  IP Addressing Plan
### WAN Links 
| Link | Network | R3 IP | FGT IP |
| :--- | :--- | :--- | :--- |
| WAN1 (FGT1) | 10.0.0.0/30 | 10.0.0.1 | 10.0.0.2 |
| WAN2 (FGT2) | 10.0.0.4/30 | 10.0.0.5 | 10.0.0.6 |
### Inter-Site Links
| Link | Network | R3 IP | FGT IP |
| :--- | :--- | :--- | :--- |
| To Site A (R4) | 10.1.0.0/30 | 10.1.0.1 | 10.1.0.2 |
| To Site B (FGT5) | 10.2.0.0/24 | 10.2.0.1 | 10.2.0.2 |
### VLANs — Headquarters
### VLANs — Headquarters
| VLAN | Name | Network | Gateway | DHCP Pool |
| :---: | :--- | :--- | :--- | :--- |
| 10 | Users | 192.168.10.0/24 | 192.168.10.1 | .10 – .254 |
| 20 | Servers | 192.168.20.0/24 | 192.168.20.1 | .100 – .200 |
| 30 | Guest | 192.168.30.0/24 | 192.168.30.1 | .10 – .200 |
| 50 | DMZ | 192.168.50.0/24 | 192.168.50.1 | .100 – .200 |
### Remote Sites LANs
| Site | Network | Gateway |
| :--- | :--- | :--- |
| Site A (R4) | 172.16.10.0/24 | 172.16.10.1 |
| Site B (R5) | 172.16.20.0/24 | 172.16.20.1 |
### VPN Pools
| Type | Range |
| :--- | :--- |
| SSL VPN Pool | 10.10.10.10 – 10.10.10.100 |
| Management | 192.168.56.200/24 |
##  Results
### Test Summary
| Test | Description | Expected | Result |
| :---: | :--- | :--- | :---: |
| 01 | DHCP VLAN10 VPC7 | 192.168.10.10 | ✅ |
| 02 | DHCP VLAN10 VPC | 192.168.10.11 | ✅ |
| 03 | DHCP VLAN30 VPC9 | 192.168.30.10 | ✅ |
| 04 | DHCP VLAN50 VPC8 | 192.168.50.100 | ✅ |
| 05 | VPC7 → Gateway ping | OK | ✅ |
| 06 | VPC7 → VPC9 VLAN isolation | BLOCKED | ✅ |
| 07 | VPC7 → VPC8 DMZ access | ALLOWED | ✅ |
| 08 | VPC8 → VPC7 DMZ→LAN block | BLOCKED | ✅ |
| 09 | FortiGate HA sync | In-Sync | ✅ |
| 10 | R3 → FGT1 ping | OK | ✅ |
| 11 | IPsec VPN Site A | Tunnel active | ✅ |
| 12 | IPsec VPN Site B | Tunnel active | ✅ |
| 13 | OSPF over IPsec VPN |  Active | ✅ |
| 14 | VPC7 → Site A (172.16.10.1) | OK via VPN | ✅ |
| 15 | VPC7 → Site B (172.16.20.1) | OK via VPN | ✅ |
| 16| SSL VPN Web Mode | Portal accessible | ✅ |
| 17 | IPS Port Scan detection | Ports filtered | ✅ |
| 18 | SNMP port 161 | Open | ✅ |
