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

## Vlan configuration 📃 on internal switches
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

## Vlan configuration 📃 on external switches
-     config t
      vlan 70
      name WORKERS
      vlan 80
      name TECHNICIANS
      vlan 999
      name Black_Hole
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
  
## Inter-vlan Routing configuration 📃 on external switches
-     interface vlan 70
      ip address 10.0.70.1 255.255.255.192
      no shutdown
      exit
      interface vlan 80
      ip address 10.0.80.1 255.255.255.224
      no shutdown
      exit

## Now Devices on the internal network should communicate to other Vlans!💯

![](https://github.com/Temijr2014/DMZ-and-Network-Hardening-Packet-Tracer-/blob/71fe5376843662f107c6f7dee0354449b2476085/Technician%2012_15_2025%2012_10_49%20PM.png)

## Port Security 🔐
Lets start out by assigning end hosts to the appropriate vlan and securing physical interfaces.
The vlan design is relatively simple with worker, technician, and separate vlans for various servers.
This helps create segmentation as well as access control when ACLs are applied.

## Internal Switch configuration 📃
-     conf t
      int ran f0/4-19, g0/1-2
      switchport access vlan 999
      shut
      int ran f0/1-23, g0/1-2
      switchport mode access
      switchport port-security
      switchport port-security mac-address sticky
      int f0/1
      switchport access vlan 20
      int ran f0/2-3
      switchport access vlan 10
      int f0/20
      switchport access vlan 30
      int f0/21
      switchport access vlan 40
      int ran f0/22-23
      switchport access vlan 50
      end
      wr

## External Switch configuration 📃
-     conf t
      int ran f0/4-23, g0/1-2
      switchport access vlan 999
      shut
      int ran f0/1-23, g0/1-2
      switchport mode access
      switchport port-security
      switchport port-security mac-address sticky
      int f0/1
      switchport access vlan 80
      int ran f0/2-3
      switchport access vlan 70
      end
      wr

## DHCP 📃 configuration on external switch
-     conf t
      ip dhcp excluded-address 10.0.70.1
      ip dhcp excluded-address 10.0.80.1
      ip dhcp pool WORKERS
      network 10.0.70.0 255.255.255.192
      default-router 10.0.70.1
      ip dhcp pool TECHNICIAN
      network 10.0.80.0 255.255.255.224
      default-router 10.0.80.1
      end
      wr
  
## DMZ Switch configuration 📃
-     conf t
      int ran f0/4-23, g0/1-2
      switchport access vlan 999
      shut
      int ran f0/1-23, g0/1-2
      switchport mode access
      switchport port-security
      switchport port-security mac-address sticky
      int ran f0/1-3, f0/24
      switchport access vlan 50
      end
      wr

  ## Verify configs:
-     show run
      show ip interface brief
      show vlan brief
      show port-security

![](https://github.com/Temijr2014/DMZ-and-Network-Hardening-Packet-Tracer-/blob/e4ad1d6a34139e4af425f673dbfa9e4c8c44fef1/Internal%2012_15_2025%2012_46_55%20PM.png)
    
![](https://github.com/Temijr2014/DMZ-and-Network-Hardening-Packet-Tracer-/blob/e4ad1d6a34139e4af425f673dbfa9e4c8c44fef1/Internal%2012_15_2025%2012_47_28%20PM.png)

![](https://github.com/Temijr2014/DMZ-and-Network-Hardening-Packet-Tracer-/blob/e4ad1d6a34139e4af425f673dbfa9e4c8c44fef1/Internal%2012_15_2025%2012_50_02%20PM.png)

![](https://github.com/Temijr2014/DMZ-and-Network-Hardening-Packet-Tracer-/blob/e4ad1d6a34139e4af425f673dbfa9e4c8c44fef1/Internal%2012_15_2025%2012_48_13%20PM.png)

## Internal server setup 💻
To configure servers in Packet Tracer, simply open the server and click on the services tab.
Take notice that all unused services are turned off.
Before configuring any of these services make sure they are toggled on.

## DHCP Server Configuration 💯
| Pool Name | Default Gateway  | Start IP | Subnet Mask | Max User |
| :---: | :--- | :--- | :--- | :--- |
| serverPool | 0.0.0.0 | 192.168.30.0 | 255.255.255.252 | 0 |
| Worker Pool | 192.168.10.1 | 192.168.10.2 | 255.255.255.248 | 2 |
| Technician Pool | 192.168.20.1 | 192.168.20.2 | 255.255.255.252 | 1 |

When DHCP is configured through a server, the interface vlans should be assigned a helper IP to the DHCP server.

## On Internal:
-     conf t
      int vlan 10
      ip helper-address 192.168.30.2
      int vlan 20
      ip helper-address 192.168.30.2
      end
      wr

## FTP Server 
First open the FTP server, delete the default username, and use the following layout:
| Username | Password | Permission |
| :---: | :--- | :--- | 
| admin | cisco | RWDNL |
| user | cisco | RL |

Obviously, in an actual scenario, significantly stronger passwords should be used.
For the time being let’s ignore password security and just focus on the protocols.

## Verify configs✅
Open command prompt on an end host in the Internal zone
Run command ftp 192.168.40.2
Supply user & password (admin, cisco)
Run dir to see what’s available
Run get [name of file] to retrieve a file

## NTP & Syslog
NTP & Syslog will be configured on Internal, which will provide logging auditability.

NTP will synchronize system clocks of the network devices in the private network.

Syslog will utilize NTP to facilitate log messages to a server for all devices in the private network.

NTP should be configured first considering that Syslog would be rendered useless without it.

On NTP server:
Enable Authentication

Key: 1
Password: cisco
On Syslog server:
Confirm service is activated

## On Internal:
-     service timestamps log datetime msec
      service timestamps debug datetime msec

-     conf t
      ntp server 192.168.50.2
      ntp authenticate
      ntp authentication-key 1 md5 cisco
      ntp trusted-key 1
      ntp update-calendar
      logging 192.168.50.3
      logging on
      logging userinfo
      logging trap
      end
      wr   

## Verify configs:
-     show clock
      show ntp associations
      show ntp status
      show logging
