# DHCPv6-Configuration
Configure and verify a Stateless &amp; Stateful DHCPv6 Server
Objectives
Part 1: Build the Network and Configure Basic Device Settings
Part 2: Verify SLAAC address assignment from R1
Part 3: Configure and verify a Stateless DHCPv6 Server on R1
Part 4: Configure and verify a Stateful DHCPv6 Server on R1
Part 5: Configure and verify a DHCPv6 Relay on R2

Required Resources
 2 Routers (Cisco 4221 with Cisco IOS XE Release 16.9.4 universal image or comparable)
 2 Switches (Cisco 2960 with Cisco IOS Release 15.2(2) lanbasek9 image or comparable) - Optional
 2 PCs (Windows with a terminal emulation program, such as Tera Term)
 Console cables to configure the Cisco IOS devices via the console ports
 Ethernet cables as shown in the topology

Configure interfaces and routing for both routers.
a. Configure the G0/0/0 and G0/0/1 interfaces on R1 and R2 with the IPv6 addresses specified in the table
above.
b. Configure a default route on each router pointed to the IP address of G0/0/0 on the other router using the
following commands.
R1(config)# ipv6 route ::/0 2001:db8:acad:2::2
R2(config)# ipv6 route ::/0 2001:db8:acad:2::1
c. Verify routing is working by pinging R2’s G0/0/1 address from R1.
d. Save the running configuration to the startup configuration file.

Part 4: Configure a stateful DHCPv6 server on R1
In Part 4, you will configure R1 to respond to DHCPv6 requests from the LAN on R2.
a. Create a DHCPv6 pool on R1 for the 2001:db8:acad:3:aaaa::/80 network. This will provide addresses to
the LAN connected to interface G0/0/1 on R2. As a part of the pool, set the DNS server to
2001:db8:acad::254, and set the domain name to STATEFUL.com.
Open configuration window
R1(config)# ipv6 dhcp pool R2-STATEFUL
R1(config-dhcp)# address prefix 2001:db8:acad:3:aaa::/80
R1(config-dhcp)# dns-server 2001:db8:acad::254
R1(config-dhcp)# domain-name STATEFUL.com
b. Assign the DHCPv6 pool you just created to interface g0/0/0 on R1.
R1(config)# interface g0/0/0
R1(config-if)# ipv6 dhcp server R2-STATEFUL

Part 5: Configure and verify DHCPv6 relay on R2.
In Part 5, you will configure and verify DHCPv6 relay on R2, allowing PC-B to receive an IPv6 Address.
Step 2: Configure R2 as a DHCP relay agent for the LAN on G0/0/1.
a. Configure the ipv6 dhcp relay command on R2 interface G0/0/1, specifying the destination address of
the G0/0/0 interface on R1. Also configure the managed-config-flag command.
Open configuration window
R2(config)# interface g0/0/1
R2(config-if)# ipv6 nd managed-config-flag
R2(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
<img width="1366" height="768" alt="Screenshot (34)" src="https://github.com/user-attachments/assets/d4f7cfea-ba3b-40aa-85c5-4cff344ee225" />
