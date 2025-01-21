

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