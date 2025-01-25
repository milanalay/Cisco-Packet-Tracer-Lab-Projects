

# Design and Implementation of a VOIP- IP Telephony System Network

## Project #8 Case Study and Requirements

    Turtle Consultancy Limited specialised in delivering IT infrastructure solutions to mediumsized organizations worldwide. With the expansion of the company, a newly acquired branch needs a network. Your manager is faced with the demands of business and a plethora of technology challenges. You have been recently hired as a Network Engineer and assigned the task of designing and implementing a VoIP network that is based on the requirements and specifications outlined by your manager.

    All desktops have an associated telephone set (each PC is connecting directly to a Phone, not a switch). The network consists of four servers (DHCP, EMAIL, DNS,HTTP) located at the server side site and is fully configured for the operations, and all servers are shared between all users. Each group has been assigned the task of designing, and implementing a network infrastructure for Turtle Consultancy Limited by internetworking three departments which are as follows:;
        1. Finance: 20 Phones + 20 PCs & 1 printer
        2. Sales: 20 Phones + 20 PCs & 1 printer
        3. HR: 20 Phones + 20 PCs & 1 printer
        4. ICT: 20 Phones + 20 PCs & 1 printer

    The IT Manager emphasized scalability and availability, and hence you are required to provide a complete network infrastructure design and implementation. Turtle Consultancy Limited will be using the following IP address: 192.168.100.0/24 for Data, 172.16.100.0/24 for Voice, and 10.10.10.0/24 between the routers.
        1. Design a networked system to meet the given specifications. Use packet tracer software to design your network.
        2. Routers- Each department is to have VoIP enabled router with server-side LAN attached to the ICT department router. Note: use Cisco 2811 router.
        3. Switches- Each department has an access layer switch. Note: use Cisco 2960 switch.
        4. Connections- Use serial connections between a router and a router, then a straightthrough cable between the router to switch, switch to hosts, phones to PCs.
        5. Subnets- Each department will be accessing two subnetworks, for example, data and voice subnets. Note: carry out appropriate subnetting.
        6. Basic settings- Configure basic device settings such as hostnames, console passwords, enable passwords, banner messages, encrypt all passwords, and disable IP domain lookup.
        7. DHCP Server- For voice (VoIP), use the respective router as the DHCP server while for Data use the DHCP server device at the server-side site.
        8. VLANs- Each department will be in two VLANS. One for data and another for voice. Note: All IP phones in the network should be in VLAN 100.
        9. Inter-VLAN Routing- Use router-on-a-stick to enable inter-VLAN routing on the network. Note: create subinterfaces for both data and voice VLANs.
        10. IP Addressing- All devices in the network are expected to obtain an IP address dynamically from the respective DHCP servers while the devices in the server room are to be allocated IP addresses statically.
        11. Routing protocol- Use OSPF as the routing protocol to advertise routes on the routers.
        12. Remote Access- Configure SSH in all the routers for remote login.
        13. Telephony service- Configure VoIP on the routers and allocate dial numbers in this format for the departments, Finance(1..), HR (2..), Sales (3..), and ICT (4..), (where 1.. can be 101 to 199) and so on.
        14. Routing for VoIP- Configure dial-peering on the routers to allow IP phones from different routers to communicate.
        15. Finalize- Test Communication, ensure everything configured is working as expected.

### Technologies Implemented
    1. Creating a network topology using Cisco Packet Tracer.
    2. Hierarchical Network Design.
    3. Connecting Networking devices with Correct cabling.
    4. Configuring Basic device settings.
    5. Creating VLANs and assigning ports VLAN numbers.
    6. Creating both data and voice VLANs and assigning ports VLAN numbers.
    7. Subnetting and IP Addressing.
    8. Configuring Inter-VLAN Routing on the Routers (router-on-a-stick).
    9. Configuring Dedicated DHCP Server device for Data to provide dynamic IP allocation.
    10. Configuring Routers as DHCP server for Voice to provide IP Phones dynamic IP allocation.
    11. Configuring SSH for secure Remote access.
    12. Configuring OSPF as the routing protocol.
    13. Configuring VoIP or Telephony service configuration in all routers.
    14. Configuring Routing for VoIP or Dial peering configuration in all routers.
    15. Host Device Configurations.
    16. Test and Verifying Network Communication.


#### CONFIG STEPS

    1. Configure Basic Settings to all devices plus ssh on the routers
        // For L2 Switch
        en
        conf t
        hostname ServerRoom-SW
        enable password cisco
        banner motd #No Unauthorised Access!!!#
        no ip domain lookup
        line console 0
        password cisco
        login
        exit
        service password-encryption
        do wr

        // For Routers
        en
        conf t
        hostname ICT-Router
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
        crypto key generate rsa general-keys modulus 1024
        ip ssh version 2
        line vty 0 15
        login local
        transport input ssh
        exit
        do wr


    2. VLANs assignment plus all access and trunk ports on the switches.

        // For L2 Switches
        vlan 40
        name DATA
        exit
        vlan 100
        name VOICE
        exit

        int  fa0/1
        switchport mode trunk
        exit

        int range fa0/2-24
        switchport mode access
        switchport access vlan 40
        switchport voice vlan 100
        exit

        do wr


    3. Configure DHCP for Voice on Routers

        service dhcp
        ip dhcp excluded-address 172.16.100.1	(default gateway for voice network)
        ip dhcp pool FinanceVoice
        network 172.16.100.0 255.255.255.224
        default-router 172.16.100.1
        option 150 ip 172.16.100.1
        exit

        do wr


    4. Configure OSPF on routers

        // For Core-Routers
        router ospf 10
        network 10.10.10.8 0.0.0.3 area 0
        network 10.10.10.12 0.0.0.3 area 0
        network 192.168.100.64 0.0.0.31 area 0
        network 172.16.100.64 0.0.0.31 area 0
        exit

        do wr


    5. Inter-VLAN Routing on Routers plus ip dhcp helper address

        // In case of L2 switch directly connected to the core router, to do inter-vlan routing we should create a sub-interface and make it the default gateway on the core router interface using encapsulation.
        // For DATA in vlan 10
        int fa0/0.40
        encapsulation dot1Q 40
        ip address 192.168.100.97 255.255.255.224
        ip helper-address 192.168.100.130
        exit

        int fa0/0.100
        encapsulation dot1Q 100
        ip address 172.16.100.97 255.255.255.224
        ex

        int fa0/1.50
        encapsulation dot1Q 50
        ip address 192.168.100.129 255.255.255.248
        exit

        do wr


    6. Configure VoIP configuration in all routers 

        telephony-service
        max-dn 20
        max-ephones 20
        ip source-address 172.16.100.97 port 2000
        auto assign 1 to 20
        exit 

        ephone-dn 1
        number 401
        ephone-dn 2 
        number 402
        ephone-dn 3
        number 403
        ephone-dn 4
        number 404
        ephone-dn 5 
        number 405
        ephone-dn 6
        number 406
        ephone-dn 7
        number 407
        ephone-dn 8 
        number 408
        ephone-dn 9
        number 409
        ephone-dn 10
        number 410

        do wr


    7. Dial Peering configuration in all routers
    // For first router (Finance-Router) 
        // Peering with HR-Router
        dial-peer voice 1 voip			// 1 is the group number
        destination-pattern 2..			// 2.. is the pattern such as line numbers of destination (201 - 299)
        session target ipv4:10.10.10.2	// ip address could be any link within the routers (in this case serial ports connecting all the routers)
        exit

        // Peering with ICT-Router
        dial-peer voice 2 voip
        destination-pattern 4..
        session target ipv4:10.10.10.6
        exit

        // Peering with Sales-Router
        dial-peer voice 3 voip
        destination-pattern 3..
        session target ipv4:10.10.10.10
        exit

        do wr

    // For HR-Router
        // Peering with Finance-Router
        dial-peer voice 1 voip
        destination-pattern 1..
        session target ipv4:10.10.10.1
        exit

        // Peering with Sales-Router
        dial-peer voice 4 voip
        destination-pattern 3..
        session target ipv4:10.10.10.10
        exit

        // Peering with ICT-Router
        dial-peer voice 5 voip
        destination-pattern 4..
        session target ipv4:10.10.10.14
        exit

        do wr


    // For Sales Router
        // Peering with Finance-Router
        dial-peer voice 3 voip
        destination-pattern 1..
        session target ipv4:10.10.10.1
        exit

        // Peering with HR-Router
        dial-peer voice 4 voip
        destination-pattern 2..
        session target ipv4:10.10.10.9
        exit

        // Peering with ICT-Router
        dial-peer voice 6 voip
        destination-pattern 4..
        session target ipv4:10.10.10.14
        exit

        do wr


    // For ICT Router
        // Peering with Finance-Router
        dial-peer voice 2 voip
        destination-pattern 1..
        session target ipv4:10.10.10.5
        exit

        // Peering with HR-Router
        dial-peer voice 5 voip
        destination-pattern 2..
        session target ipv4:10.10.10.9
        exit

        // Peering with Sales-Router
        dial-peer voice 6 voip
        destination-pattern 3..
        session target ipv4:10.10.10.13
        exit

        do wr
