# Enterprise Network Security Lab
<p align="center">
  <b>A complete enterprise-grade network security infrastructure simulation built on EVE-NG</b><br><br>
  <i>FortiGate HA Cluster · IPsec Site-to-Site VPN · SSL VPN · IPS/IDS · VLAN Segmentation · SNMP Monitoring</i>
</p>
<p align="center">
  <a href="#features">Features</a> · 
  <a href="#architecture">Architecture</a> · 
  <a href="#lab-setup">Lab Setup</a> · 
  <a href="#configuration">Configuration</a> · 
  <a href="#testing">Testing</a> · 
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
| R3 | e0/3 | FGT | port1 |
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
| WAN1 (FGT1) | e0/0 | FGT1 | port1 |
| R3 | e0/0 | FGT2 | port1 |
### IP Addressing Plan

