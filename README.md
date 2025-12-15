# DMZ and Network Hardening (Packet Tracer)🌐

## Objective✔️

The objective of this project is to design and implement a secure DMZ (Demilitarized Zone) network architecture using Cisco Packet Tracer, while applying industry-standard network hardening techniques. This project demonstrates how to properly segment internal, external, and DMZ networks, enforce access control through ACLs, secure Layer 2 switches, configure NAT on a firewall, and implement basic services such as HTTP, FTP, Email, and SSH.

### Skills Learned 💯

- Understand how DMZ networks protect internal systems from external threats.

- Configure secure VLAN segmentation for proper isolation of network zones.

- Apply ACLs to control and restrict traffic between the Internet, DMZ, and LAN.

- Implement NAT on a firewall to securely expose services to external clients.

- Harden switches and routers with port security, SSH, and login protection.

- Build a realistic SOC-oriented environment suitable for monitoring and analysis.

### Tools Used⚒️

- Packet Tracer.

## Steps✅
- firstly lets begin by setting up are Topology as you can see below
## Topology 🌐 
![Topology](https://github.com/Temijr2014/DMZ-and-Network-Hardening-Packet-Tracer-/blob/460dd49c7985a0186d6b06aee7ff47070eef084b/DMZ%20Topology.png)
## Devices 🖥️
- Firewall 5505.
- Switch 2960-24TT.
- Switch 3560-24PS (2).
- Server FTP (2).
- Server NTP.
- Server DHCP.
- Server Syslog.
- Server Email.
- Server HTTP.
- PCs (4).

## Ip Table 📃

Hostname        Interface       IP Address      Subnet Mask 
=============================================================
-     Internal        Vlan 10         192.168.10.1    255.255.255.248
-                 Vlan 20         192.168.20.1    255.255.255.252
-                 Vlan 30         192.168.30.1    255.255.255.252
-                 Vlan 40         192.168.40.1    255.255.255.252
-                 Vlan 50         192.168.50.1    255.255.255.248
-                  F0/24           192.168.1.1     255.255.255.252

Firewall 🧱🔥
-        Et0/0(Vlan 1)   192.168.1.2     255.255.255.252  <= Internal
-             Et0/1(Vlan 2)   172.16.50.1     255.255.255.248  <= DMZ
-              Et0/2(Vlan 3)   209.165.200.2   255.255.255.252  <= External

DMZ 💂‍♂️

 -            Vlan 50         172.16.50.2     255.255.255.248

External 🌐
-     Vlan 70         10.0.70.1       255.255.255.248
-     Vlan 80         10.0.80.1       255.255.255.252
-     F0/24           209.165.200.1   255.255.255.252

Servers 🖥️
=========
-     DHCP            N/A             192.168.30.2    255.255.255.252
-     Internal FTP    N/A             192.168.40.2    255.255.255.252
-     NTP             N/A             192.168.50.2    255.255.255.248
-     Syslog          N/A             192.168.50.3    255.255.255.248 

-     Email           N/A             172.16.50.3     255.255.255.248
-     HTTP            N/A             172.16.50.4     255.255.255.248
-     External FTP    N/A             172.16.50.5     255.255.255.248

Vlans
======

Internal 💻
---------
-     10  -   Workers
-     20  -   Technician
-     30  -   DHCP
-     40  -   FTP
-     50  -   NTP / Syslog
-     999 - Black_Hole

External 🌐
---------
-     70  -   Workers
-     80  -   Technician
-     999 - Black_Hole

DMZ 💂‍♂️
----
-     50 - IP
-     999 - Black_Hole

## vlan configuration 📃 on internal switches
-     config t
      Vlan 10
      name Workers    
      exit
      config t
      Vlan 20
      name Technician    
      exit
      config t
      Vlan 30
      name DHCP   
      exit
      config t
      Vlan 40
      name FTP  
      exit
      config t
      Vlan 40
      name NTP / Syslog  
      exit

## Inter-vlan Routing configuration 📃 on internal switches

-     interface vlan 10
      ip address 192.168.10.1 255.255.255.248
      no shutdown
      interface vlan 20
      ip address 192.168.20.1 255.255.255.252
      no shutdown
      interface vlan 30
      ip address 192.168.30.1 255.255.255.252
      no shutdown
      interface vlan 40
      ip address 192.168.40.1 255.255.255.252
      no shutdown
      interface vlan 50
      ip address 192.168.50.1 255.255.255.248
      no shutdown
