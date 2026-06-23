# Test Results — Enterprise Network Security Lab
## Phase 1 VLAN & DHCP Tests
### Test 01 — DHCP VLAN10
```text
VPCS> ip dhcp
DDORA IP 192.168.10.10/24 GW 192.168.10.1
VPCS> sh ip
NAME        : VPCS[1]
IP/MASK     : 192.168.10.10/24
GATEWAY     : 192.168.10.1
DNS         : 96.45.45.45  96.45.46.46
DHCP SERVER : 192.168.10.1
DHCP LEASE  : 604796, 604800/302400/529200
MAC         : 00:50:79:66:68:0e
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500
```
### Test 02 — DHCP VLAN30 
```test
VPCS> ip dhcp
DDORA IP 192.168.30.10/24 GW 192.168.30.1
VPCS> sh ip
NAME        : VPCS[1]
IP/MASK     : 192.168.30.10/24
GATEWAY     : 192.168.30.1
DNS         : 96.45.45.45  96.45.46.46
DHCP SERVER : 192.168.30.1
DHCP LEASE  : 604795, 604800/302400/529200
MAC         : 00:50:79:66:68:09
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500
```
### Test 03 — DHCP VLAN50 
```test
VPCS> ip dhcp
DDORA IP 192.168.50.100/24 GW 192.168.50.1
VPCS> sh ip
NAME        : VPCS[1]
IP/MASK     : 192.168.50.100/24
GATEWAY     : 192.168.50.1
DNS         : 96.45.45.45  96.45.46.46
DHCP SERVER : 192.168.50.1
DHCP LEASE  : 604796, 604800/302400/529200
MAC         : 00:50:79:66:68:08
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500
```
### Test 04 — VLan 10 Ping to Gateway
```test
VPCS> ping 192.168.10.1
84 bytes from 192.168.10.1 icmp_seq=1 ttl=255 time=0.831 ms
84 bytes from 192.168.10.1 icmp_seq=2 ttl=255 time=1.321 ms
84 bytes from 192.168.10.1 icmp_seq=3 ttl=255 time=1.085 ms
84 bytes from 192.168.10.1 icmp_seq=4 ttl=255 time=12.677 ms
84 bytes from 192.168.10.1 icmp_seq=5 ttl=255 time=1.055 ms
```
## Phase 2 — VLAN Isolation Tests
### Test 05 — Vlan 10 ping to Vlan 30 (VLAN Isolation)
```text
VPCS> ping 192.168.30.10
192.168.30.10 icmp_seq=1 timeout
192.168.30.10 icmp_seq=2 timeout
192.168.30.10 icmp_seq=3 timeout
192.168.30.10 icmp_seq=4 timeout
192.168.30.10 icmp_seq=5 timeout
```
**Expected:** BLOCKED by firewall policy
    **Result:** ✅ PASS — Inter-VLAN traffic blocked (Users ↛ Guest)
### Test 06 — Vlan 10 to Vlan 50 DMZ (Allowed)
```text
VPCS> ping 192.168.50.100
84 bytes from 192.168.50.100 icmp_seq=1 ttl=63 time=9.349 ms
84 bytes from 192.168.50.100 icmp_seq=2 ttl=63 time=4.946 ms
84 bytes from 192.168.50.100 icmp_seq=3 ttl=63 time=9.740 ms
84 bytes from 192.168.50.100 icmp_seq=4 ttl=63 time=29.893 ms
84 bytes from 192.168.50.100 icmp_seq=5 ttl=63 time=10.119 ms
```
**Expected:** ALLOWED (HTTP/HTTPS/PING only)
    **Result:** ✅ PASS — Users can reach DMZ via policy 
### Test 07 — DMZ to Vlan 10 (DMZ to LAN Block)
```text
VPCS> ping 192.168.10.10
192.168.10.10 icmp_seq=1 timeout
192.168.10.10 icmp_seq=2 timeout
192.168.10.10 icmp_seq=3 timeout
192.168.10.10 icmp_seq=4 timeout
192.168.10.10 icmp_seq=5 timeout
```
**Expected:** BLOCKED by DMZ-block-LAN policy
    **Result:** ✅ PASS — DMZ cannot access LAN 
## Phase 3 — IPsec VPN Tests
### Test 08 —  IPsec Tunnel Site A & B
```test
FGT1-Master # get vpn ipsec tunnel summary
'VNP Site B' 10.2.0.2:0  selectors(total,up): 1/1  rx(pkt,err): 395/0  tx(pkt,err): 395/2
'VPN to Site A' 10.1.0.2:0  selectors(total,up): 1/1  rx(pkt,err): 416/0  tx(pkt,err): 395/3
```
<img width="1612" height="257" alt="image" src="https://github.com/user-attachments/assets/72394ab8-48ea-4dfd-9f57-b6fa69acdc86" />

### Test 09 — Vlan 10 to Site A via VPN
**Expected:**  Users (Vlan 10) can reach site A and Guest (Vlan 30) is BLOCK 
```text
VPCS> ping 172.16.10.2
84 bytes from 172.16.10.2 icmp_seq=1 ttl=62 time=28.647 ms
84 bytes from 172.16.10.2 icmp_seq=2 ttl=62 time=3.100 ms
84 bytes from 172.16.10.2 icmp_seq=3 ttl=62 time=2.962 ms
84 bytes from 172.16.10.2 icmp_seq=4 ttl=62 time=2.569 ms
84 bytes from 172.16.10.2 icmp_seq=5 ttl=62 time=2.143 ms

VPCS> ping 172.16.10.2
172.16.10.2 icmp_seq=1 timeout
172.16.10.2 icmp_seq=2 timeout
172.16.10.2 icmp_seq=3 timeout
172.16.10.2 icmp_seq=4 timeout
172.16.10.2 icmp_seq=5 timeout
```
### Test 10 — Vlan 10 to Site B via VPN
```text
VPCS> ping 172.16.20.10
84 bytes from 172.16.20.10 icmp_seq=1 ttl=62 time=15.042 ms
84 bytes from 172.16.20.10 icmp_seq=2 ttl=62 time=2.228 ms
84 bytes from 172.16.20.10 icmp_seq=3 ttl=62 time=2.423 ms
84 bytes from 172.16.20.10 icmp_seq=4 ttl=62 time=2.598 ms
84 bytes from 172.16.20.10 icmp_seq=5 ttl=62 time=2.567 ms
```
### Text 11 — OSPF via VPN
```test
FGT1-Master # get router info ospf neighbor
OSPF process 0, VRF 0:
Neighbor ID     Pri   State           Dead Time   Address         Interface
4.4.4.4           1   Full/ -         00:00:37    10.3.0.2        VPN to Site A(tun-id:10.1.0.2)
5.5.5.5           1   Full/ -         00:00:31    10.4.0.2        VNP Site B(tun-id:10.2.0.2)

FGT1-Master # get router info routing-table ospf
Routing table for VRF=0
O       10.4.0.2/32 [110/100] via VNP Site B tunnel 10.2.0.2, 01:17:01
O       172.16.10.0/24 [110/110] via VPN to Site A tunnel 10.1.0.2, 01:17:01
O       172.16.20.0/24 [110/101] via VNP Site B tunnel 10.2.0.2, 01:17:01
```

### Test 15 — SSL VPN Portal Access
SSH Tunnel: ssh -L 8443:192.168.56.200:10443 root@192.168.56.110
Browser: https://localhost:8443
Login: vpnuser / VPNuser123!
Portal: SSL-VPN Portal loaded successfully
