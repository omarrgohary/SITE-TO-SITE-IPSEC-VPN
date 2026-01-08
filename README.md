# Site-to-Site IPSec VPN Configuration using Cisco Packet Tracer

---

## Project Overview
This project demonstrates a **Site-to-Site IPSec VPN** configuration between two networks (HQ and Branch) using **Cisco Packet Tracer**.  
It includes 8 VLANs, multiple switches, routers, ASA 5506 firewalls, and dynamic routing via **OSPF**.  
The VPN ensures secure communication between HQ and Branch networks through encrypted tunnels.

---

## Network Topology

**HQ Network**
- VLANs: 10, 50, 20, 60  
- Switch 1: VLAN 10 & VLAN 50 connected  
- Switch 2: VLAN 20 & VLAN 60 connected  
- Router 1: Connected to both switches  
- ASA Firewall 1: Protects HQ network, connected to Router 1  

**Branch Network**
- VLANs: 30, 40, 70, 80  
- Switch 3: VLAN 30 & VLAN 70 connected  
- Switch 4: VLAN 40 & VLAN 80 connected  
- Router 2: Connected to both switches  
- ASA Firewall 2: Protects Branch network, connected to Router 2  

**Internet Connection**
- Both firewalls are connected to a router representing the Internet

---

## Key Features
- **VLAN Configuration:** 8 VLANs across 4 switches, trunk ports configured between routers and switches  
- **OSPF Routing:** Dynamic routing configured on all routers and firewalls  
- **VPN Configuration:** Site-to-Site IPSec VPN using IKEv1 and IPsec for secure data transmission  
- **Access Control:** ACLs used to filter traffic and secure VPN connections between sites  

---

## Technologies Used
- **Cisco Packet Tracer:** Network simulation and implementation  
- **ASA 5506 Firewalls:** Network security and IPSec VPN  
- **OSPF:** Dynamic routing protocol  
- **VLANs:** Network segmentation and isolation  

---

## Configuration Details

### Router Configuration
- Subinterfaces: Each router interface configured as a subinterface for each VLAN (e.g., `192.168.10.0/30` for VLAN 10)  
- OSPF: Enabled on all routers for dynamic routing  

### Firewall Configuration

**Firewall 1 (HQ)**
```text
crypto ikev1 policy 10
hash sha
authentication pre-share
group 2
lifetime 80000
encryption aes
tunnel-group 100.50.10.6 type IPsec-l2l
tunnel-group 100.50.10.6 IPsec-Attributes
ikev1 pre-shared-key cisco
crypto IPsec ikev1 transform-set TEST esp-aes esp-sha-hmac
access-list VPN-ACL permit ip 192.168.10.0 255.255.255.252 192.168.30.0 255.255.255.252
...
crypto map CMAP 10 set peer 100.50.10.6
crypto map CMAP 10 set ikev1 transform-set TEST 
crypto map CMAP 10 match address VPN-ACL
crypto map CMAP int OUTSIDE
crypto ikev1 enable OUTSIDE
wr mem
Firewall 2 (Branch)

text
Copy code
crypto ikev1 policy 10
hash sha
authentication pre-share
group 2
lifetime 80000
encryption aes
tunnel-group 100.50.10.2 type IPsec-l2l
tunnel-group 100.50.10.2 IPsec-Attributes
ikev1 pre-shared-key cisco
crypto IPsec ikev1 transform-set TEST esp-aes esp-sha-hmac
access-list VPN-ACL permit ip 192.168.30.0 255.255.255.252 192.168.10.0 255.255.255.252
...
crypto map CMAP 10 set peer 100.50.10.2
crypto map CMAP 10 set ikev1 transform-set TEST 
crypto map CMAP 10 match address VPN-ACL
crypto map CMAP int OUTSIDE
crypto ikev1 enable OUTSIDE
wr mem
Key Commands

IKEv1 Policy: Defines encryption, authentication, and key exchange

IPSec VPN: Establishes secure tunnel between firewalls

ACLs: Ensure only authorized networks communicate over VPN

Setup Instructions
Download Cisco Packet Tracer and open the provided project file

Inspect the network topology: Verify connections between switches, routers, and firewalls

Review configurations: Check router, firewall, and VLAN settings

Test the VPN: Send pings and inspect routing tables to confirm HQ-Branch communication

Conclusion
This project demonstrates the configuration of a Site-to-Site IPSec VPN using Cisco Packet Tracer, integrating VLANs, OSPF routing, and ASA Firewalls to ensure secure communication between geographically separated networks.

yaml
Copy code

---

If you want, I can **also make a version with a clean table of VLANs, IPs, and firewall mappings** for better readability—it will look very professional on GitHub.  

Do you want me to do that too?






You said:
yes


ChatGPT said:
Perfect! Here's a GitHub-ready version with a professional table for VLANs, subnets, and firewall mappings, making it much cleaner and easier to read:

markdown
Copy code
# Site-to-Site IPSec VPN Configuration using Cisco Packet Tracer

---

## Project Overview
This project demonstrates a **Site-to-Site IPSec VPN** configuration between two networks (HQ and Branch) using **Cisco Packet Tracer**.  
It includes 8 VLANs, multiple switches, routers, ASA 5506 firewalls, and dynamic routing via **OSPF**.  
The VPN ensures secure communication between HQ and Branch networks through encrypted tunnels.

---

## Network Topology

| Network | Device        | VLANs/Subnets                | Description                        |
|---------|---------------|-----------------------------|------------------------------------|
| HQ      | Switch 1      | VLAN 10, 50                 | Connects HQ subnets                |
| HQ      | Switch 2      | VLAN 20, 60                 | Connects HQ subnets                |
| HQ      | Router 1      | 192.168.10.0/30 - VLAN 10  | Router interface for VLANs         |
| HQ      | ASA Firewall1 | N/A                         | Protects HQ network, handles VPN   |
| Branch  | Switch 3      | VLAN 30, 70                 | Connects Branch subnets            |
| Branch  | Switch 4      | VLAN 40, 80                 | Connects Branch subnets            |
| Branch  | Router 2      | 192.168.30.0/30 - VLAN 30  | Router interface for VLANs         |
| Branch  | ASA Firewall2 | N/A                         | Protects Branch network, handles VPN |
| Both    | Internet      | N/A                         | Connection to external network     |

---

## Key Features
- **VLAN Configuration:** 8 VLANs across 4 switches, trunk ports configured between routers and switches  
- **OSPF Routing:** Dynamic routing configured on all routers and firewalls  
- **VPN Configuration:** Site-to-Site IPSec VPN using IKEv1 and IPsec for secure data transmission  
- **Access Control:** ACLs filter traffic to secure VPN connections between sites  

---

## Technologies Used
- **Cisco Packet Tracer:** Network simulation and implementation  
- **ASA 5506 Firewalls:** Network security and IPSec VPN  
- **OSPF:** Dynamic routing protocol  
- **VLANs:** Network segmentation and isolation  

---

## Configuration Details

### Router Configuration
- Subinterfaces: Each router interface configured as a subinterface for each VLAN  
- OSPF: Enabled on all routers for dynamic routing  

### Firewall Configuration

**Firewall 1 (HQ)**
```text
crypto ikev1 policy 10
hash sha
authentication pre-share
group 2
lifetime 80000
encryption aes
tunnel-group 100.50.10.6 type IPsec-l2l
tunnel-group 100.50.10.6 IPsec-Attributes
ikev1 pre-shared-key cisco
crypto IPsec ikev1 transform-set TEST esp-aes esp-sha-hmac
access-list VPN-ACL permit ip 192.168.10.0 255.255.255.252 192.168.30.0 255.255.255.252
...
crypto map CMAP 10 set peer 100.50.10.6
crypto map CMAP 10 set ikev1 transform-set TEST 
crypto map CMAP 10 match address VPN-ACL
crypto map CMAP int OUTSIDE
crypto ikev1 enable OUTSIDE
wr mem
Firewall 2 (Branch)

text
Copy code
crypto ikev1 policy 10
hash sha
authentication pre-share
group 2
lifetime 80000
encryption aes
tunnel-group 100.50.10.2 type IPsec-l2l
tunnel-group 100.50.10.2 IPsec-Attributes
ikev1 pre-shared-key cisco
crypto IPsec ikev1 transform-set TEST esp-aes esp-sha-hmac
access-list VPN-ACL permit ip 192.168.30.0 255.255.255.252 192.168.10.0 255.255.255.252
...
crypto map CMAP 10 set peer 100.50.10.2
crypto map CMAP 10 set ikev1 transform-set TEST 
crypto map CMAP 10 match address VPN-ACL
crypto map CMAP int OUTSIDE
crypto ikev1 enable OUTSIDE
wr mem
Key Commands

IKEv1 Policy: Defines encryption, authentication, and key exchange

IPSec VPN: Establishes secure tunnel between firewalls

ACLs: Ensures only authorized networks communicate over VPN

VLAN, Subnet & ACL Table
VLAN	HQ Subnet	Branch Subnet	ACLs Permit
10	192.168.10.0/30	192.168.30.0/30	HQ VLAN10 → Branch VLAN30
10	192.168.10.0/30	192.168.40.0/30	HQ VLAN10 → Branch VLAN40
10	192.168.10.0/30	192.168.70.0/30	HQ VLAN10 → Branch VLAN70
10	192.168.10.0/30	192.168.80.0/30	HQ VLAN10 → Branch VLAN80
20	192.168.20.0/30	192.168.30.0/30	HQ VLAN20 → Branch VLAN30
20	192.168.20.0/30	192.168.40.0/30	HQ VLAN20 → Branch VLAN40
20	192.168.20.0/30	192.168.70.0/30	HQ VLAN20 → Branch VLAN70
20	192.168.20.0/30	192.168.80.0/30	HQ VLAN20 → Branch VLAN80
50	192.168.50.0/30	192.168.30.0/30	HQ VLAN50 → Branch VLAN30
50	192.168.50.0/30	192.168.40.0/30	HQ VLAN50 → Branch VLAN40
50	192.168.50.0/30	192.168.70.0/30	HQ VLAN50 → Branch VLAN70
50	192.168.50.0/30	192.168.80.0/30	HQ VLAN50 → Branch VLAN80
60	192.168.60.0/30	192.168.30.0/30	HQ VLAN60 → Branch VLAN30
60	192.168.60.0/30	192.168.40.0/30	HQ VLAN60 → Branch VLAN40
60	192.168.60.0/30	192.168.70.0/30	HQ VLAN60 → Branch VLAN70
60	192.168.60.0/30	192.168.80.0/30	HQ VLAN60 → Branch VLAN80

Setup Instructions
Download Cisco Packet Tracer and open the provided project file

Inspect the network topology: Verify connections between switches, routers, and firewalls

Review configurations: Check router, firewall, and VLAN settings

Test the VPN: Send pings and inspect routing tables to confirm HQ-Branch communication

Conclusion
This project demonstrates the configuration of a Site-to-Site IPSec VPN using Cisco Packet Tracer, integrating VLANs, OSPF routing, and ASA Firewalls to ensure secure communication between geographically separated networks.
