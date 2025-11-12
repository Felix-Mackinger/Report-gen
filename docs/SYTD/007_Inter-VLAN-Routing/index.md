---
title: "007_Inter-VLAN-Routing"
author: "Felix Mackinger"
klasse: "4AHITS"
subject: "SYTD"
date: "2025-11-06"
---


# Aufgabenstellung

[Aufgabenstellung](https://felix-mackinger.github.io/Report-gen/SYTD/docs/SYTD\007_Inter-VLAN-Routing\img\Übung_07-Inter-VLAN_Routing_Basics.pdf)


## Router on A Stick

![Logical Layout](/img/logical-layout_ROS.png)


### VLAN
```sh
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/2
10   VLAN0010                         active    Fa0/11
30   VLAN0030                         active    Fa0/6
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
10   enet  100010     1500  -      -      -        -    -        0      0
30   enet  100030     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

Remote SPAN VLANs
------------------------------------------------------------------------------

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------
```

### R1
```sh
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
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
no ipv6 cef
!
!
!
!
license udi pid CISCO1941/K9 sn FTX1524SC8U
!
!
!
!
!
!
!
!
!
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
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 172.17.10.1 255.255.255.0
!
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 172.17.30.1 255.255.255.0
!
interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
ip flow-export version 9
!
!
!
no cdp run
!
!
!
!
!
line con 0
!
line aux 0
!
line vty 0 4
 login
!
!
!
end
```

### S1

```sh
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname S1
!
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
 switchport mode trunk
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
!
!
!
!
!
line con 0
!
line vty 0 4
 login
line vty 5 15
 login
!
!
!
!
end
```


## Layer 3 Switch

![Logical Layout](/img/logical-layout_L3S.png)

```sh
version 12.2
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname MLS
!
!
!
!
!
!
ip routing
!
ipv6 unicast-routing
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
 switchport trunk native vlan 999
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport nonegotiate
!
interface GigabitEthernet0/2
 no switchport
 ip address 209.165.200.225 255.255.255.252
 duplex auto
 speed auto
 ipv6 address 2001:DB8:ACAD:A::1/64
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 mac-address 0002.16a2.cb01
 ip address 192.168.10.254 255.255.255.0
 ipv6 address 2001:DB8:ACAD:10::1/64
 --More-- 
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on GigabitEthernet0/1 (999), with S1 GigabitEthernet0/1 (99).

!
interface Vlan20
 mac-address 0002.16a2.cb02
 ip address 192.168.20.254 255.255.255.0
 ipv6 address 2001:DB8:ACAD:20::1/64
!
interface Vlan30
 mac-address 0002.16a2.cb03
 ip address 192.168.30.254 255.255.255.0
 ipv6 address 2001:DB8:ACAD:30::1/64
!
interface Vlan99
 mac-address 0002.16a2.cb04
 ip address 192.168.99.254 255.255.255.0
!
ip classless
!
ip flow-export version 9
!
ipv6 route ::/0 GigabitEthernet0/2 2001:DB8:ACAD:A::2
!
!
 --More-- !
!
!
!
!
line con 0
 logging synchronous
!
line aux 0
!
line vty 0 4
 login
!
!
!
!
end
```

### S1

```sh
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname S1
!
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport trunk native vlan 999
 switchport mode trunk
!
interface FastEthernet0/2
 switchport trunk native vlan 999
 switchport mode trunk
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
 switchport trunk native vlan 99
 switchport mode trunk
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan99
 ip address 192.168.99.1 255.255.255.0
!
ip default-gateway 192.168.99.254
!
!
!
!
!
!
line con 0
!
line vty 0 4
 login
line vty 5 15
 login
!
!
!
!
end
```




## Trouble Shoot InterVlan Routing

![Logical Layout](/img/logical-layout_TSIV.png)




# Antwortenblatt

![Antworten Blatt](/img/Antwortenblatt/A1.png)  
![Antworten Blatt](/img/Antwortenblatt/A2.png)  
![Antworten Blatt](/img/Antwortenblatt/A3.png)  
![Antworten Blatt](/img/Antwortenblatt/A4.png)  
![Antworten Blatt](/img/Antwortenblatt/A5.png)  
![Antworten Blatt](/img/Antwortenblatt/A6.png)  
![Antworten Blatt](/img/Antwortenblatt/A7.png)  
![Antworten Blatt](/img/Antwortenblatt/A8.png)  
![Antworten Blatt](/img/Antwortenblatt/A9.png)  
![Antworten Blatt](/img/Antwortenblatt/A10.png)  
![Antworten Blatt](/img/Antwortenblatt/A11.png)  
![Antworten Blatt](/img/Antwortenblatt/A12.png)  
![Antworten Blatt](/img/Antwortenblatt/A13.png)  
![Antworten Blatt](/img/Antwortenblatt/A14.png)  
![Antworten Blatt](/img/Antwortenblatt/A15.png)  
