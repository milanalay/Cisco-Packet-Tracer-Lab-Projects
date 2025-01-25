![Screenshot](https://github.com/milanalay/Cisco-Packet-Tracer-Lab-Projects/blob/main/Project_6_Company%3ABusiness%20System%20Network/Screenshot%202025-01-19%20at%201.01.32%20am.png)

# Design and Implementation of a Company/Business System Network

## Project #6 Case Study and Requirements

    A trading floor Support centre employs 600 staff. They have recently expanded and as a result, need to move to a new building. A building has been identified but has no network. This means that before they can make to move out, new network service needs to be designed and implemented in the new building. Existing Network comprises of the following elements: The new building is expected to have three floors with two departments in each for example;
        1. First floor- (Sales and Marketing Department-120 users expected, Human Resource and Logistics Department-120 users expected).
        2. Second floor- (Finance and Accounts Department-120 users expected, Administrator and Public Relations Department-120 users expected).
        3. Third floor- (ICT-120 users expected, Server Room-12 devices expected).

    Therefore, as a key member of the Networks Team, you have been tasked to design a network for the new building. At this stage, logical design is required, which shows the measures that you would put in place to ensure that the new network meets the current business need and is future-proofed:
        - Use Cisco Packet Tracer to design and implement the network solution.
        - Use hieratical model providing redundancy at every layer i.e. two routers and two multilayer switches are expected to be used to provide redundancy.
        - The network is also expected to connect to at least two ISPs to provide redundancy and each router to the connected to the two ISPs.
        - Each department is required to have a wireless network for the users.
        - Each department should be in a different VLAN and in different subnetwork.
        - Provided a base network of 172.16.1.0, carry out subnetting to allocate the correct number of IP addresses to each department.
        - The company network is connected to the static, public IP addresses (Internet Protocol) 195.136.17.0/30, 195.136.17.4/30, 195.136.17.8/30 and 195.136.17.12/30 connected to the two Internet providers.
        - Configure basic device settings such as hostnames, console password, enable password, banner messages, disable IP domain lookup.
        - Devices in all the departments are required to communicate with each other with the respective multilayer switch configured for inter-VLAN routing.
        - The Multilayer switches are expected to carry out both routing and switching functionalities thus will be assigned IP addresses.
        - All devices in the network are expected to obtain an IP address dynamically from the dedicated DHCP servers located at the server room.
        - Devices in the server room are to be allocated IP address statically.
        - Use OSPF as the routing protocol to advertise routes both on the routers and multilayer switches.
        - Configure SSH in all the routers and layer three switches for remote login.
        - Configure port-security for the Finance and Accounts department to allow only one device to connect to a switchport, use sticky method to obtain mac-address and violation mode shutdown.
        - Configure PAT to use the respective outbound router interface IPv4 address, implement the necessary ACL rule.
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
    12. Configuring standard and extended Access Control Lists ACL.
    13. Configuring switchport security or Port-Security on the switches.
    14. Configuring WLAN or wireless network (Cisco Access Point).
    15. Host Device Configurations.
    16. Configuring ISP routers.
    17. Test and Verifying Network Communication.


#### Config Steps
    1. Basic settings to all devices plus ssh on the routers and L3 switches

        hostname Core-R2
        line console 0
        password cisco
        login
        exit

        enable password cisco
        no ip domain-lookup
        banner motd #No Unauthorised Access!!!#
        service password-encryption

        do wr

        ip domain name cisco.net
        username admin password cisco
        crypto key generate rsa
        1024
        line vty 0 15
        login local
        transport input ssh
        exit

        do wr

        ip ssh version 2
        do wr


    2. VLANs assignment plus all access and trunk ports on L2 and L3 switches.

    2.1. changing switchports to trunk and access:

        int range fa0/1-2
        switchport mode trunk
        exit

        vlan 60
        name ServerRoom
        exit
        vlan 99
        name BlackHole
        exit

        int range fa0/3-24
        switchport mode access
        switchport access vlan 60
        exit

        int range gig0/1-2
        switchport mode access
        switchport access vlan 99
        exit

        do wr

    2.2. Now, for Layer 3 Switch

        int range gig1/0/3-8
        switchport mode trunk

        vlan 10 
        name Sales
        vlan 20 
        name HR
        vlan 30
        name Finance
        vlan 40
        name Admin
        vlan 50
        name ICT
        vlan 60 
        name ServerRoom

        exit

        do wr

    3. Switchport security to Finance department

        int range fa0/3-24 
        switchport port-security
        switchport port-security maximum 1
        switchport port-security mac-address sticky
        switchport port-security violation shutdown
        exit
        do wr


    4. Assign OSPF routing protocol to the routers and L3 switches to advertise all the networks.

    4.1. For L3 Switches
        ip routing
        router ospf 10
        router-id 1.1.1.1
        network 172.16.1.0 0.0.0.127 area 0
        network 172.16.1.128 0.0.0.127 area 0
        network 172.16.2.0 0.0.0.127 area 0
        network 172.16.2.128 0.0.0.127 area 0
        network 172.16.3.0 0.0.0.127 area 0
        network 172.16.3.128 0.0.0.15 area 0
        network 172.16.3.152 0.0.0.3 area 0
        network 172.16.3.156 0.0.0.3 area 0

    4.2. For Core Routers
        router ospf 10
        router-id 4.4.4.4
        network 172.16.3.148 0.0.0.3 area 0
        network 172.16.3.156 0.0.0.3 area 0
        network 195.136.17.8 0.0.0.3 area 0
        network 195.136.17.12 0.0.0.3 area 0

        do wr
        exit
        do wr


    5. Inter vlan routing in L3 switches plus ip dhcp helper addresses
        int vlan 10
        no sh
        ip address 172.16.1.1 255.255.255.128
        ip helper-address 172.16.3.130
        ex

        int vlan 20
        no sh
        ip address 172.16.1.129 255.255.255.128
        ip helper-address 172.16.3.130
        ex
        int vlan 30
        no sh
        ip address 172.16.2.1 255.255.255.128
        ip helper-address 172.16.3.130
        ex

        int vlan 40
        no sh
        ip address 172.16.2.129 255.255.255.128
        ip helper-address 172.16.3.130
        ex

        int vlan 50
        no sh
        ip address 172.16.3.1 255.255.255.128
        ip helper-address 172.16.3.130
        ex

        int vlan 60
        no sh
        ip address 172.16.3.129 255.255.255.240
        ip helper-address 172.16.3.130
        ex

    6. Configure PAT in core routers to change the public IP address to private IP address

        ip nat inside source list 1 int se0/3/0 overload
        ip nat inside source list 1 int se0/3/1 overload
        access-list 1 permit 172.16.1.0 0.0.0.127
        access-list 1 permit 172.16.1.128 0.0.0.127
        access-list 1 permit 172.16.2.0 0.0.0.127
        access-list 1 permit 172.16.2.128 0.0.0.127
        access-list 1 permit 172.16.3.0 0.0.0.127
        access-list 1 permit 172.16.3.128 0.0.0.15

        int range gig0/0-1
        ip nat inside
        exit

        int range se0/3/0
        ip nat outside 
        ex
        int range se0/3/1
        ip nat outside
        ex

        do wr


    7. Default Static Route
        ip route 0.0.0.0 0.0.0.0 se0/3/0
        ip route 0.0.0.0 0.0.0.0 se0/3/1 70

        ip route 0.0.0.0 0.0.0.0 gig1/01
        ip route 0.0.0.0 0.0.0.0 gig1/0/2 70
