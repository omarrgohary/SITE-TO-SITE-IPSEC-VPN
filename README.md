Site-to-Site IPSec VPN Configuration using Cisco Packet Tracer
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Project Overview :
This project demonstrates a Site-to-Site IPSec VPN configuration between two networks (HQ and Branch) using Cisco Packet Tracer. The configuration consists of 8 VLANs and involves multiple switches, routers, ASA 5506 firewalls, and dynamic routing via OSPF. The VPN ensures secure communication between the HQ and Branch, connecting remote VLANs through encrypted tunnels.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Network Topology :

  HQ Network:
    VLANs: 10, 50, 20, 60
    Switch 1: VLAN 10, VLAN 50 connected
    Switch 2: VLAN 20, VLAN 60 connected
    Router 1: Connected to Switch 1 and Switch 2
    ASA Firewall 1: Protects the HQ network, connected to Router 1
    
  Branch Network:
    VLANs: 30,40,70,80
    Switch 3: VLAN 30, VLAN 70 connected
    Switch 4: VLAN 40, VLAN 80 connected
    Router 2: Connected to Switch 3 and Switch 4
    ASA Firewall 2: Protects the Branch network, connected to Router 2
    
  Internet Connection:
    Both firewalls are connected to a router representing the Internet.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Key Features :
  VLAN Configuration: 8 VLANs spanning 4 switches, with trunk ports configured between routers and switches.
  OSPF Routing: Dynamic routing configured using OSPF on all routers and firewalls.
  VPN Configuration: Site-to-Site IPSec VPN configured on both ASA firewalls using IKEv1 and IPsec for secure data transmission.
  Access Control: Access Control Lists (ACLs) used for traffic filtering and securing the VPN connections between sites.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Technologies Used :
  Cisco Packet Tracer: Network simulation tool used to design and implement the network topology.
  ASA 5506 Firewalls: Used for securing the network and establishing the IPSec VPN.
  OSPF: Dynamic routing protocol configured across routers and firewalls for network discovery and communication.
  VLANs: Configured to segment the network and provide isolation between different parts of the infrastructure.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Configuration Details :
  Router Configuration
    Subinterfaces: Each router interface is configured as a subinterface for each VLAN (e.g., 192.168.10.0/30 for VLAN 10, 192.168.20.0/30 for VLAN 20, etc.).
    OSPF: Enabled on all routers to dynamically share routing information across the network.
  Firewall Configuration
    Firewall 1 (HQ):
      Configured with crypto IKEv1 policies and transform sets.
      Created access control lists (ACLs) to define the VPN traffic rules.
      Established the Site-to-Site VPN connection using IPsec between both firewalls.
      Configurations used :
          crypto ikev1 policy 10
          hash sha
          authentication pre-share
          group 2
          lifetime 80000
          encryption aes
          ex
          tunnel-group 100.50.10.6 type IPsec-l2l
          tunnel-group 100.50.10.6 IPsec-Attributes
          ikev1 pre-shared-key cisco
          ex
          crypto IPsec ikev1 transform-set TEST esp-aes esp-sha-hmac
          access-list VPN-ACL permit ip 192.168.10.0 255.255.255.252 192.168.30.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.10.0 255.255.255.252 192.168.40.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.10.0 255.255.255.252 192.168.70.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.10.0 255.255.255.252 192.168.80.0 255.255.255.252
          
          access-list VPN-ACL permit ip 192.168.20.0 255.255.255.252 192.168.30.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.20.0 255.255.255.252 192.168.40.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.20.0 255.255.255.252 192.168.70.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.20.0 255.255.255.252 192.168.80.0 255.255.255.252
          
          access-list VPN-ACL permit ip 192.168.50.0 255.255.255.252 192.168.30.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.50.0 255.255.255.252 192.168.40.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.50.0 255.255.255.252 192.168.70.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.50.0 255.255.255.252 192.168.80.0 255.255.255.252
          
          access-list VPN-ACL permit ip 192.168.60.0 255.255.255.252 192.168.30.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.60.0 255.255.255.252 192.168.40.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.60.0 255.255.255.252 192.168.70.0 255.255.255.252
          access-list VPN-ACL permit ip 192.168.60.0 255.255.255.252 192.168.80.0 255.255.255.252
          
          crypto map CMAP 10 set peer  100.50.10.6
          crypto map CMAP 10 set ikev1 transform-set TEST 
          crypto map CMAP 10 match address VPN-ACL
          crypto map CMAP int OUTSIDE
          crypto ikev1 enable OUTSIDE
          wr mem


    Firewall 2 (Branch):
      Similar configuration as Firewall 1, with adjustments for the Branch network.
      VPN policies and ACLs are set to allow communication with HQ.
      Configurations used :
        crypto ikev1 policy 10
        hash sha
        authentication pre-share
        group 2
        lifetime 80000
        encryption aes
        ex
        tunnel-group 100.50.10.2 type IPsec-l2l
        tunnel-group 100.50.10.2 IPsec-Attributes
        ikev1 pre-shared-key cisco
        ex
        crypto IPsec ikev1 transform-set TEST esp-aes esp-sha-hmac
        access-list VPN-ACL permit ip 192.168.30.0 255.255.255.252 192.168.10.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.30.0 255.255.255.252 192.168.20.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.30.0 255.255.255.252 192.168.50.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.30.0 255.255.255.252 192.168.60.0 255.255.255.252

        access-list VPN-ACL permit ip 192.168.40.0 255.255.255.252 192.168.10.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.40.0 255.255.255.252 192.168.20.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.40.0 255.255.255.252 192.168.50.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.40.0 255.255.255.252 192.168.60.0 255.255.255.252

        access-list VPN-ACL permit ip 192.168.70.0 255.255.255.252 192.168.10.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.70.0 255.255.255.252 192.168.20.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.70.0 255.255.255.252 192.168.50.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.70.0 255.255.255.252 192.168.60.0 255.255.255.252
        
        access-list VPN-ACL permit ip 192.168.80.0 255.255.255.252 192.168.10.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.80.0 255.255.255.252 192.168.20.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.80.0 255.255.255.252 192.168.50.0 255.255.255.252
        access-list VPN-ACL permit ip 192.168.80.0 255.255.255.252 192.168.60.0 255.255.255.252
        
        crypto map CMAP 10 set peer  100.50.10.2
        crypto map CMAP 10 set ikev1 transform-set TEST 
        crypto map CMAP 10 match address VPN-ACL
        crypto map CMAP int OUTSIDE
        crypto ikev1 enable OUTSIDE
        wr mem

  Key Commands Used
      IKEv1 Policy: Defines encryption methods, authentication methods, and key exchange settings.
      IPSec VPN: Set up the tunnel between the two firewalls for encrypted communication.
      ACLs: Filters traffic to ensure only authorized networks can communicate over the VPN.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Setup Instructions :
  Download Cisco Packet Tracer and open the provided project file.
  Inspect Network Topology: All devices are pre-configured; verify connections between switches, routers, and firewalls.
  Review Configuration: Examine the router, firewall, and VLAN configurations within the device settings.
  Test the VPN: Ensure successful communication between the HQ and Branch networks by sending pings and checking routing tables.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Conclusion :
  This project successfully demonstrates how to configure a Site-to-Site IPSec VPN using Cisco Packet Tracer, including the integration of VLANs, OSPF routing, and ASA Firewalls. The setup ensures secure         
  communication between two geographically separated networks.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
