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
### Test 15 — SSL VPN Portal Access

```text
SSH Tunnel: ssh -L 8443:192.168.56.200:10443 root@192.168.56.110
Browser: https://localhost:8443
Login: vpnuser / VPNuser123!
Portal: SSL-VPN Portal loaded successfully
