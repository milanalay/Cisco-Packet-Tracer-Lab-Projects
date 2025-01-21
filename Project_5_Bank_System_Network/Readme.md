![Screenshot](https://github.com/milanalay/Cisco-Packet-Tracer-Lab-Projects/blob/main/Project_5_Bank_System_Network/Screenshot%202025-01-15%20at%203.27.57%20pm.png)

# Design and Implementation of a Bank System Network

## Project #5 Case Study and Requirements

    Radeon Company Ltd. is a US-owned company that deals with Banking and Insurance. The company is intending to expand its services across the African continent having the first branch to be located in Nairobi, Kenya. The company has secured a four-story building to operate within the Kenyan capital city. Therefore, the company would like to allow sourcing the knowledge from a group of final-year students from the local university to design and implement their company network. Assume you are among the students to take over this role, carefully read down the requirements then model the design and implement the network based on the company's needs. Each floor has departments as provided below:

        First Floor
        1. Department(Management), No. of PC(20), No. of Printers(4)
        2. Department(Research), No. of PC(20), No. of Printers(4)
        3. Department(Human Resource), No. of PC(20), No. of Printers(4)

        Second Floor
        1. Department(Marketing), No. of PC(20), No. of Printers(4)
        2. Department(Accounting), No. of PC(20), No. of Printers(4)
        3. Department(Finance), No. of PC(20), No. of Printers(4)

        Third Floor
        1. Department(Logistics and Store), No. of PC(20), No. of Printers(4)
        2. Department(Customer Care), No. of PC(20), No. of Printers(4)
        3. Department(Guest Area), No. of PC(40), No. of Printers(2)

        Fourth Floor
        1. Department(Administration), No. of PC(20), No. of Printers(2)
        2. Department(ICT), No. of PC(20), No. of Printers(2)
        3. Server Room, 2 Admin PCs, 3 (DHCP, HTTP and Email)



    - Use a software modeling tool to visualize the network topology (Use Hierarchical Network Design)
        - Software Modelling Tools: MS Visio, Visual Paradigm, or Draw.io for modeling network design.
    - Use any of the following network simulation software to implement the above topology.
        - Simulation software: Cisco Packet tracer or GNS3 for design and implementation.
    - Use OSPF as the routing protocol to advertise routes.
    - Each department is required to have a wireless network for the users.
    - Each department except the server room will be anticipated to have around 60 users both wired and wireless users.
    - Host devices in the network are required to obtain IPv4 addresses automatically.
    - Devices in all the departments are required to communicate with each other.
    - Create HTTP, and E-mail servers.
    - All devices in the network are expected to obtain an IP address dynamically from the dedicated DHCP servers located at the server room.
    - Configure SSH in all the routers for remote login.
    - Configure the basic configuration of the devices: Hostnames, Line Console and Enable passwords, Banner messages Disable domain IP lookup, encrypt all configured passwords.
    - Each department should be in a different VLAN and subnetwork; VLANs you will use in your case, e.g. 10, 20, 30… etc..
    - Planning of IP Addresses: You have been given 192.168.10.0 as the base address for this network. Do subnetting based on the number of hosts in every department as provided above. Identify subnet mask, useable IP address range, and broadcast address for each subnet.
    - End Device Configurations: Configure all the end devices in the network with the appropriate IP address based on the calculations above.
    - Configure port-security: Use sticky command to obtain MAC Address and Violation mode of the shutdown.
    - Test and Verifying Network Communication.


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
    11. Configuring switchport security or Port-Security on the switches.
    12. Configuring WLAN or wireless network (Cisco Access Point).
    13. Host Device Configurations.
    14. Test and Verifying Network Communication.


#### CONFIG STEPS 
    1. Basic settings to all devices plus ssh on the routers and L3 switches.
        hostname CoreLayerRouter4
        banner motd #This is CoreLayerRouter4 #
         
        line console 0
        password cisco
        login
        exit
        
        line vty 0 15
        password cisco
        login 
        exit

        ip domain-name cisco.net
        username cisco password cisco

        crypto key generate rsa
        1024

        line vty 0 15
        login local
        transport input ssh
        exit
         
        no ip domain-lookup
        enable password cisco
        service password-encryption

    2. VLANs assignment plus all access and trunk ports and switchport security to all L2 switches.
        int range fa0/1-2
        switchport mode trunk 
        exit

        vlan 10
        name "Vlan_name"
        ex
        
        int range fa0/3-24
        switchport mode access
        switchport access vlan 10
        switchport port-security
        switchport port-security maximum 2
        switchport port-security mac-address sticky
        switchport port-security violation shutdown 

    3. Subnetting and IP addressing.
        // Changing to trunk ports
        int range gig1/0/3-8
        switchport mode trunk
        exit
        do wr  

        // Assign IP address to switch
        Take a interface: int gig1/0/1
        And use command: no switchport
        Assign IP address: ip address 10.10.10.1 255.255.255.192

    4. OSPF on the routers and L3 switches.
        ip routing
        router ospf 10

        network 10.10.10.48 0.0.0.3 area 0 
        network 10.10.10.52 0.0.0.3 area 0

        network 192.168.11.128 0.0.0.63 area 0
        network 192.168.11.192 0.0.0.63 area 0
        network 192.168.12.0 0.0.0.63 area 0
        network 192.168.12.64 0.0.0.63 area 0
        network 192.168.12.128 0.0.0.63 area 0
        network 192.168.12.192 0.0.0.63 area 0

    5. Inter-VLAN routing on the L3 switches plus ip dhcp helper address.
        vlan 70
        vlan 80
        vlan 90
        vlan 100
        vlan 110
        vlan 120

        int vlan 70
        no shutdown
        ip address 192.168.11.129 255.255.255.192
        ip helper-address 192.168.12.196
        exit

        int vlan 80
        no shutdown
        ip address 192.168.11.193 255.255.255.192
        ip helper-address 192.168.12.196
        exit

        int vlan 90
        no shutdown
        ip address 192.168.12.1 255.255.255.192
        ip helper-address 192.168.12.196
        exit

        int vlan 100
        no shutdown
        ip address 192.168.12.65 255.255.255.192
        ip helper-address 192.168.12.196
        exit

        int vlan 110
        no shutdown
        ip address 192.168.12.129 255.255.255.192
        ip helper-address 192.168.12.196
        exit

        int vlan 120
        no shutdown
        ip address 192.168.12.193 255.255.255.192
        exit

        do wr




    
