

# Design and Implementation of a Hospital System Network

## Project #7 Case Study and Requirements

    Melbourne Health Services is a well-established health provider in Australia, which offers health solutions and services to its clients. The institution operates in two locations within the same city, having the hospital headquarters 20km away from the branch hospital. Therefore, it has the following departments within its main headquarters Medical Lead Operation & Consultancy Services (MLOCS), Medical Emergency and Reporting (MER), Medical Records Management (MRM), Information Technology (IT), and Customer Service (CS). The branch hospital was designed to share the workloads with the headquarters hence it contains the following departments; Nurses & Surgery Operations (NSO), Hospital Labs (HL), Human resource (HR), Marketing (MK), and Finance (FIN). Each location is also expected to have a Guest/Waiting area (GWA) for patients or visitors.

    So far the network was using third-party services to maintain its IT services. The senior management has decided to own their network infrastructure including Local Area Network (LAN), Wide Area Network (WAN), and a Server-Side site that is expected to be located separately at the headquarters and is connected to the HQ Router with an access switch. The server-side site will host the DHCP server, DNS Server, Web Server, and Email Server. The network is expected to be cost-effective and observes the information security rule of the CIA (Confidentiality, Integrity, and Availability).
    The network is expected to have a hierarchical model with two already purchased Core routers (one at HQ and one Branch) each connecting to two subscribed ISPs. Due to security requirements, it has been decided that all the departments will be on a separate network segment within the same local area network.

    You have been hired as a network security engineer to design the network according to the requirements set by the senior management. You will consult an appropriate robust network design model to meet the design requirements. You will also implement Access Control Lists and Virtual Private Network (VPN) to enable secure communication considering security and network performance factors paramount to safeguarding Confidentiality, Integrity, and Availability of data and communication. The network security policy will comprehensively dictate the user's access to each site using Access Control List (ACL).

        - Use Cisco Packet Tracer to design and implement the network solution.
        - Use a hierarchical model providing redundancy in the network.
        - Both HQ and Branch routers are expected to be connected using a serial connection.
        - As mentioned earlier, for network cost-effectiveness, each site is expected to have one core router, two multilayer switches, and several access switches connecting each department.
        - Each department is required to have a wireless network for the users.
        - Every department in HQ is estimated to have around 60 users while in Branch is estimated to be 30 users.
        - Each department should be in a different VLAN and a different subnetwork.
        - Provided a base network of 192.168.100.0, and carry out subnetting to allocate the correct number of IP addresses to each department.
        - The company network is connected to the static, public IP addresses (Internet Protocol) 195.136.17.0/30, 195.136.17.4/30, 195.136.17.8/30, and 195.136.17.12/30 connected to the two Internet providers.
        - Configure basic device settings such as hostnames, console password, enable password, banner messages, and disable IP domain lookup.
        - Devices in all the departments are required to communicate with each other with the respective multilayer switch configured for inter-VLAN routing.
        - The Multilayer switches are expected to carry out both routing and switching functionalities and thus will be assigned IP addresses.
        - All devices in the network are expected to obtain an IP address dynamically from the dedicated DHCP servers located in the server room.
        - Devices in the server room are to be allocated IP addresses statically.
        - Use OSPF as the routing protocol to advertise routes both on the routers and multilayer switches.
        - Configure default static routing to enable routers and multilayer switches to forward any traffic that does not match routing table entries. Use next-hop IP addresses.
        - Configure SSH in all the routers and layer three switches for remote login.
        - Configure port-security for the server site department switch to allow only one device to connect to a switch port, use sticky method to obtain mac-address and violation mode shutdown.
        - Configure the extended ACL rule together with site-to-site VPN (IPSec VPN) to create a tunnel and encrypt communication between HQ and the Branch network.
        - Configure PAT to use the respective outbound router interface IPv4 address, and implement the necessary ACL rule.
        - Test Communication, ensure everything configured is working as expected.

### Technologies Implemented
    1. Creating a network topology using Cisco Packet Tracer.
    2. Hierarchical Network Design.
    3. Connecting Networking devices with Correct cabling.
    4. Configuring Basic device settings.
    5. Creating VLANs and assigning ports VLAN numbers.
    6. Subnetting and IP Addressing.
    7. Configuring Inter-VLAN Routing on the Multilayer switches (Switch Virtual Interface).
    8. Configuring Dedicated DHCP Server device to provide dynamic IP allocation.
    9. Configuring SSH for secure Remote access.
    10. Configuring OSPF as the routing protocol.
    11. Configuring NAT Overload(Port Address Translation PAT).
    12. Configuring Site-to-Site IPsec VPN.
    13. Configuring standard and extended Access Control Lists ACL.
    14. Configuring switchport security or Port-Security on the switches.
    15. Configuring WLAN or wireless network (Cisco Access Point).
    16. Host Device Configurations.
    17. Configuring ISP routers.
    18. Test and Verifying Network Communication.


#### CONFIG STEPS
    1. Configure Basic Settings to all devices plus ssh on the routers and L3 Switches

    en
    conf t
    hostname SERVER-SW
    enable password cisco
    banner motd #No Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    service password-encryption
    do wr

    // For Routers and L3 Switches
    en
    conf t
    hostname BR-Router
    enable password cisco
    banner motd #No Unauthorised Access!!!#
    no ip domain lookup
    line console 0
    password cisco
    login
    exit
    service password-encryption

    ip domain name cisco.net
    username admin password cisco
    crypto key generate rsa
    1024
    line vty 0 15
    login local
    transport input ssh
    exit
    do wr

    2. VLANs assignment plus all access and trunk ports on L2 and L3 switches.

    // For L2 Switches
    vlan 130 
    name BR_GWA
    exit

    int range fa0/1-2 
    switchport mode trunk
    exit

    int range fa0/3-24
    switchport mode access
    switchport access vlan 130
    exit

    do wr

    // For L3 Switches
    vlan 80
    vlan 90
    vlan 100
    vlan 110
    vlan 120
    vlan 130
    exit

    int range gig1/0/2-7
    switchport mode trunk
    exit

    do wr

    3. Switchport security to server-side site department

    int range fa0/2-24
    switchport port-security
    switchport port-security maximum 1
    switchport port-security mac-address sticky
    switchport port-security violation shutdown

    do sh port-security

    4. Configure OSPF on routers and L3 switches.

    // For L3 Switches
    ip routing
    router ospf 10
    network 192.168.101.128 0.0.0.31 area 0
    network 192.168.101.160 0.0.0.31 area 0
    network 192.168.101.192 0.0.0.31 area 0
    network 192.168.101.224 0.0.0.31 area 0
    network 192.168.102.0 0.0.0.31 area 0
    network 192.168.102.32 0.0.0.31 area 0
    network 192.168.102.92 0.0.0.3 area 0
    exit

    // Default Static Route / Next-Hop Routing
    ip route 0.0.0.0 0.0.0.0 192.168.102.94

    // For Core-Routers
    router ospf 10
    network 192.168.102.80 0.0.0.3 area 0
    network 192.168.102.84 0.0.0.3 area 0
    network 192.168.102.96 0.0.0.3 area 0
    network 195.136.17.0 0.0.0.3 area 0
    network 195.136.17.4 0.0.0.3 area 0
    network 192.168.102.64 0.0.0.15 area 0
    exit

    // Default Static Route
    ip route 0.0.0.0 0.0.0.0 195.136.17.2
    ip route 0.0.0.0 0.0.0.0 195.136.17.6 70
    do wr

    5. Inter-VLAN Routing on L3 Switches plus ip dhcp helper address
    
    // In case of Server-Department, L2 switch is directly connected to the core router, so to do inter-vlan routing we should create a sub-interface for the server department of vlan 70 and make it the default gateway on the core router interface using encapsulation.
    int gig0/2
    no ip address
    exit
    int gig0/2.70
    encapsulation dot1Q 70
    ip address 192.168.102.65 255.255.255.240
    ex
    do wr

    // For L3 Switches
    int vlan 80
    ip address 192.168.101.129 255.255.255.224
    ip helper-address 192.168.102.67
    exit

    int vlan 90
    ip address 192.168.101.161 255.255.255.224
    ip helper-address 192.168.102.67
    exit

    int vlan 100
    ip address 192.168.101.193 255.255.255.224
    ip helper-address 192.168.102.67
    exit

    int vlan 110
    ip address 192.168.101.225 255.255.255.224
    ip helper-address 192.168.102.67
    exit

    int vlan 120
    ip address 192.168.102.1 255.255.255.224
    ip helper-address 192.168.102.67
    exit

    int vlan 130
    ip address 192.168.102.33 255.255.255.224
    ip helper-address 192.168.102.67
    exit

    do wr

    6. PAT + Access Control List

    int se0/2/0
    ip nat outside
    exit
    int se0/2/1 
    ip nat outside
    exit

    int range gig0/0-2 
    ip nat inside
    exit
    do wr

    ip nat inside source list 1 interface se0/2/0 overload
    ip nat inside source list 1 interface se0/2/1 overload
    access-list 1 permit 192.168.100.0 0.0.0.63
    access-list 1 permit 192.168.100.64 0.0.0.63
    access-list 1 permit 192.168.100.128 0.0.0.63
    access-list 1 permit 192.168.100.192 0.0.0.63
    access-list 1 permit 192.168.101.0 0.0.0.63
    access-list 1 permit 192.168.101.64 0.0.0.63

    7. IPSEC VPN Tunnel
    license boot module c2900 technology-package securityk9

    ////////// HQ Route Aggregation //////////

    (
        192.168.100.0/26
        192.168.100.64/26
        192.168.100.128/26
        192.168.100.192/26
    )					— Summarised as 192.168.100.0/24

    (
        192.168.101.0/26
        192.168.101.64/26
    )					— Summarised as 192.168.101.0/25

    ///////// BR Route Aggregation /////////

    (
        192.168.101.128/27
        192.168.101.160/27
        192.168.101.192/27
        192.168.101.224/27
        192.168.102.0/27
        192.168.102.32/27
    )					— Summarised as 192.168.101.128/24

    // For HQ Router
    access-list 110 permit ip 192.168.100.0 0.0.0.255 192.168.101.128 0.0.0.255
    access-list 110 permit ip 192.168.101.0 0.0.0.127 192.168.101.128 0.0.0.255

    // For BR Router
    access-list 110 permit ip 192.168.101.128 0.0.0.255 192.168.100.0 0.0.0.255
    access-list 110 permit ip 192.168.101.128 0.0.0.255 192.168.101.0 0.0.0.127

    // Key exchange on both the routers using (ISAKMP) Internet Security Association Key Management Protocol
    // For HQ-Router
    crypto isakmp policy 10
    encryption aes 256
    authentication pre-share
    group 5
    exit

    crypto isakmp key SECRET123 address 192.168.102.98
    do wr

    crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
    crypto map VPN-MAP 10 ipsec-isakmp
    description This VPN connects to Branch-Network.
    set peer 192.168.102.98
    set transform-set VPN-SET
    match address 110
    exit

    int se0/3/0
    crypto map VPN-MAP
    exit

    do wr

    do sh crypto ipsec sa

    // For BR-Router
    crypto isakmp policy 10
    encryption aes 256
    authentication pre-share
    group 5
    exit

    crypto isakmp key SECRET123 address 192.168.102.97
    do wr

    crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
    crypto map VPN-MAP 10 ipsec-isakmp
    description This VPN connects to HeadQuarter-Network.
    set peer 192.168.102.97
    set transform-set VPN-SET
    match address 110
    exit

    int se0/3/0
    crypto map VPN-MAP
    exit

    do wr

    do sh crypto ipsec sa
