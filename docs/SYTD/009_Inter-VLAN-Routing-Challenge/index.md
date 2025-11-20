---
title: "009_Inter-VLAN-Routing-Challenge"
author: "Felix Mackinger"
klasse: "4AHITS"
subject: "SYTD"
date: "2025-11-20"
---



## Router

````sh
version 15.1
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname R1
!
!
!
!
!
!
!
!
ip cef
ipv6 unicast-routing
!
no ipv6 cef
!
!
!
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
license udi pid CISCO1941/K9 sn FTX15240DK4
!
!
!
!
!
!
!
!
!
ip ssh version 1
ip domain-name lab.int
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface GigabitEthernet0/0
 description -- Uplink HQ --
 ip address 172.17.25.2 255.255.255.252
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 description -- Connection LAN --
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/1.10
 description Staff
 encapsulation dot1Q 10
 ip address 172.17.10.1 255.255.255.0
!
interface GigabitEthernet0/1.20
 description Students
 encapsulation dot1Q 20
 ip address 172.17.20.1 255.255.255.0
!
interface GigabitEthernet0/1.30
 description Guest
 encapsulation dot1Q 30
 ip address 172.17.30.1 255.255.255.0
!
interface GigabitEthernet0/1.88
 encapsulation dot1Q 88 native
 ip address 172.17.88.1 255.255.255.0
!
interface GigabitEthernet0/1.99
 description Mgmt
 encapsulation dot1Q 99 
 ip address 172.17.99.1 255.255.255.0
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
ip route 0.0.0.0 0.0.0.0 GigabitEthernet0/0 
!
ip flow-export version 9
!
!
!
no cdp run
!
banner motd ^C!Illegal hier zu sein!^^C
!
!
!
!
line con 0
 exec-timeout 15 0
 login local
!
line aux 0
 exec-timeout 15 0
 login local
!
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
!
!
!
end
````



## Switch

````sh
version 15.0
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
!
!
!
!
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 shutdown
!
interface FastEthernet0/2
 shutdown
!
interface FastEthernet0/3
 shutdown
!
interface FastEthernet0/4
 shutdown
!
interface FastEthernet0/5
 shutdown
!
interface FastEthernet0/6
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/7
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/8
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/9
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/10
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/11
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/12
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/13
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/14
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/15
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/16
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/17
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/18
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/19
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/20
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/21
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/22
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/23
 switchport access vlan 20
 switchport mode access
!
interface FastEthernet0/24
 switchport access vlan 20
 switchport mode access
!
interface GigabitEthernet0/1
 switchport trunk native vlan 88
 switchport mode trunk
!
interface GigabitEthernet0/2
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan99
 ip address 172.17.99.10 255.255.255.0
!
ip default-gateway 172.17.99.1
!
no cdp run
!
banner motd ^C!Illegal hier zu sein!^^C
!
!
!
!
!
line con 0
 login local
 exec-timeout 15 0
!
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
line vty 5 15
 login
!
!
!
!
end
````


### Check

````sh
S1#sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Gig0/2
10   Faculty/Staff                    active    Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17
20   Students                         active    Fa0/18, Fa0/19, Fa0/20, Fa0/21
                                                Fa0/22, Fa0/23, Fa0/24
30   Guest(Default)                   active    Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10
88   Native                           active    
99   Management                       active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
````


## Ping Check


### Router
````sh
R1#ping 172.17.10.21

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.17.10.21, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms

R1#ping 172.17.20.22

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.17.20.22, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/0 ms

R1#ping 172.17.30.23

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.17.30.23, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/0 ms

R1#ping 172.17.50.254

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.17.50.254, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
````

### PC1

````sh
C:\>ping 172.17.50.254

Pinging 172.17.50.254 with 32 bytes of data:

Request timed out.
Request timed out.
Request timed out.
Reply from 172.17.50.254: bytes=32 time<1ms TTL=126

Ping statistics for 172.17.50.254:
    Packets: Sent = 4, Received = 1, Lost = 3 (75% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
C:\>
C:\>ping 172.17.99.10

Pinging 172.17.99.10 with 32 bytes of data:

Request timed out.
Request timed out.
Reply from 172.17.99.10: bytes=32 time<1ms TTL=254
Reply from 172.17.99.10: bytes=32 time<1ms TTL=254

Ping statistics for 172.17.99.10:
    Packets: Sent = 4, Received = 2, Lost = 2 (50% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 172.17.99.1

Pinging 172.17.99.1 with 32 bytes of data:

Reply from 172.17.99.1: bytes=32 time<1ms TTL=255
Reply from 172.17.99.1: bytes=32 time<1ms TTL=255
Reply from 172.17.99.1: bytes=32 time<1ms TTL=255
Reply from 172.17.99.1: bytes=32 time<1ms TTL=255

Ping statistics for 172.17.99.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 172.17.20.22

Pinging 172.17.20.22 with 32 bytes of data:

Reply from 172.17.20.22: bytes=32 time<1ms TTL=127
Reply from 172.17.20.22: bytes=32 time<1ms TTL=127
Reply from 172.17.20.22: bytes=32 time<1ms TTL=127
Reply from 172.17.20.22: bytes=32 time<1ms TTL=127

Ping statistics for 172.17.20.22:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 172.17.30.23

Pinging 172.17.30.23 with 32 bytes of data:

Reply from 172.17.30.23: bytes=32 time<1ms TTL=127
Reply from 172.17.30.23: bytes=32 time<1ms TTL=127
Reply from 172.17.30.23: bytes=32 time<1ms TTL=127
Reply from 172.17.30.23: bytes=32 time<1ms TTL=127

Ping statistics for 172.17.30.23:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
````

