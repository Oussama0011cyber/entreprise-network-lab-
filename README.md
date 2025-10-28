Enterprise Network Lab

A complete **Enterprise Network Simulation** built using **Cisco Packet Tracer**.  
This project demonstrates real-world enterprise network administration skills including **router configuration, ASA firewall setup, DHCP deployment, printer integration, and LAN connectivity**.

---

 Project Overview

This lab simulates an enterprise environment where all network devices — including routers, switches, firewalls, printers, and end-user PCs — are interconnected securely.  
The design focuses on **network services, firewall security rules, and end-user connectivity** across the local area network.

---

 Topology Summary

[Router ISR4331] ⇄ [Switch] ⇄ [ASA 5506-X Firewall] ⇄ [End Devices]
⇅
[Printer + Hub + PCs]



| Device | Interface | Connected To | Purpose |
|--------|------------|---------------|----------|
| Router (ISR4331) | Gig0/0 | Switch (Fa0/1) | DHCP, Gateway |
| ASA 5506-X | Gig1/1 | Switch (Fa0/2) | Firewall Security |
| Switch | Fa0/16 | Printer | Print Server |
| Switch | Fa0/3 | Hub | End User PCs |
| Server | IP: 192.168.1.2 | Router Network | Web/DHCP/DNS Services |

---

 Configuration Summary

 Router (ISR4331)
hostname R1
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
!
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool ENTERPRISE_POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 192.168.1.2
 lease 2


 ASA 5506-X Firewall
 hostname ASA5506
interface GigabitEthernet1/1
 nameif inside
 security-level 100
 ip address 192.168.1.254 255.255.255.0
 no shutdown
!
access-list INSIDE_TO_PRINTER permit tcp 192.168.1.0 255.255.255.0 host 192.168.1.100 eq 515
access-list INSIDE_TO_PRINTER permit tcp 192.168.1.0 255.255.255.0 host 192.168.1.100 eq 631
access-group INSIDE_TO_PRINTER in interface inside
!
dhcpd enable inside
dhcpd address 192.168.1.10-192.168.1.200 inside
dhcpd dns 192.168.1.2


Switch
hostname SW1
interface FastEthernet0/1
 description Link_to_Router
 switchport mode access
!
interface FastEthernet0/2
 description Link_to_ASA
 switchport mode access
!
interface FastEthernet0/3
 description Link_to_Hub
 switchport mode access
!
interface FastEthernet0/16
 description Link_to_Printer
 switchport mode access
!

Printer Configuration
IP Address: 192.168.1.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1

Hub and End Users
All end-user PCs connected to the hub should automatically receive IP addresses from the router’s DHCP pool and access network resources through the ASA firewall.


Troubleshooting DHCP
If end-user PCs receive APIPA (169.x.x.x) addresses:

Verify the DHCP service on the router is running.

Ensure ASA allows DHCP broadcasts and replies between the router and clients.

Use debug dhcp detail and show ip dhcp binding on the router.

Ping between the router and firewall interfaces.


Tools Used
Cisco Packet Tracer (v8.x or higher)

Cisco ISR4331 Router

Cisco ASA 5506-X Firewall

Cisco Catalyst Switch

Generic Hub

Printer and End-user PCs









