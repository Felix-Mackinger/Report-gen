---
title: "004_Standortvernetzung_IPv4_Erweitert"
author: "Felix Mackinger"
klasse: "4AHITS"
subject: "SYTD"
date: "2025-10-14"
---


# Aufgabenstellung

[Aufgabenstellung](https://felix-mackinger.github.io/Report-gen/SYTD/docs/SYTD/004_Standortvernetzung_IPv4_Erweitert/img/Übung_03-Standortvernetzung_IPv6.pdf)


![Logical Layout](/img/logical-layout.png)


# Aufgabe 1 *Wien Berlin*

## Config Router

router1
```c
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname router1
!
!
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
!
!
!
no ip cef
no ipv6 cef
!
!
!
username netadmin privilege 15 password 0 cisco
!
!
license udi pid CISCO2911/K9 sn FTX15244K6F-
!
!
!
!
!
!
!
!
!
no ip domain-lookup
ip domain-name labor-lan.net
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
 description ####  link_lan_wien  ###
 ip address 192.168.21.254 255.255.255.0
 duplex auto
 speed Auto
 no shutdown
!
interface GigabitEthernet0/1
 description
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
!
interface Serial0/2/0
 description ####  link_wien2berlin  ###
 bandwidth 2048
 ip address 192.168.250.1 255.255.255.0
 encapsulation ppp
 clock rate 2000000
 no shutdown
!
interface Serial0/2/1
 description ####  link_wien2london  ###
 bandwidth 2048
 ip address 192.168.251.1 255.255.255.0
 encapsulation ppp
 clock rate 2000000
 no shutdown
!
interface Serial0/3/0
 description ####  link_wien2london  ###
 bandwidth 2048
 ip address 192.168.252.1 255.255.255.0
 encapsulation ppp
 clock rate 2000000
 no shutdown
!
interface Serial0/3/1
 no ip address
 clock rate 2000000
 shutdown
!
interface Vlan1
 no ip address
!
ip classless
!
ip route 192.168.21.0 255.255.255.0 GigabitEthernet0/0 
ip route 192.168.101.0 255.255.255.0 192.168.250.2 
ip route 192.168.102.0 255.255.255.0 192.168.251.2 
ip route 192.168.103.0 255.255.255.0 192.168.252.2 
!
!
ip flow-export version 9
!
!
!
!
!
!
!
line con 0
 login local
!
line aux 0
 login local
!
line vty 0 4
 login local
 transport input ssh
!
!
!
end
```

router2
```c
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname router2
!
!
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
!
!
!
no ip cef
no ipv6 cef
!
!
!
username netadmin privilege 15 password 0 cisco
!
!
license udi pid CISCO2911/K9 sn FTX15244K6F-
!
!
!
!
!
!
!
!
!
no ip domain-lookup
ip domain-name labor-lan.net
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
 description ####  link_lan_berlin  ###
 ip address 192.168.101.254 255.255.255.0
 duplex auto
 speed Auto
 no shutdown
!
interface GigabitEthernet0/1
 description
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
!
interface Serial0/3/0
 description ####  link_berlin2wien  ###
 bandwidth 2048
 ip address 192.168.250.2 255.255.255.0
 encapsulation ppp
 no shutdown
!
interface Serial0/3/1
 no ip address
 clock rate 2000000
 shutdown
!
interface Vlan1
 no ip address
!
ip classless
!
ip route 192.168.101.0 255.255.255.0 GigabitEthernet0/0 
ip route 192.168.21.0 255.255.255.0 192.168.250.1 
ip route 192.168.102.0 255.255.255.0 192.168.250.1
ip route 192.168.103.0 255.255.255.0 192.168.250.1
!
!
ip flow-export version 9
!
!
!
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
```

## Config Switch

switch02
```c
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname switch02
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
no ip domain-lookup
ip domain-name labor-lan.net
!
username netadmin privilege 1 password 7 0822455D0A16
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
interface range FastEthernet0/1 - 23
 switchport mode Access
 spanning-tree portfast
!
interface FastEthernet0/24
 description ###  Uplink-2-W-R-01 ###
 switchport mode trunk
 switchport trunk allowed vlan all
 spanning-tree portfast
!
interface GigabitEthernet0/1
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport mode trunk
!
!
interface vlan 1
 ip address 192.168.21.201 255.255.255.0
!
ip default-gateway 192.168.21.254
!
!
!
!
line con 0
 exec-timeout 15 0
 login local
line vty 0 4
 exec-timeout 15 0
 transport input ssh
 login local
line vty 5 15
 exec-timeout 15 0
 transport input ssh
 login local
!
!
!
!
end
```

switch05
```c
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname switch05
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
no ip domain-lookup
ip domain-name labor-lan.net
!
username netadmin privilege 1 password 7 0822455D0A16
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
interface range FastEthernet0/1 - 23
 switchport mode Access
 spanning-tree portfast
!
interface FastEthernet0/24
 description ###  Uplink-2-router2 ###
 switchport mode trunk
 spanning-tree portfast
!
interface GigabitEthernet0/1
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport mode trunk
!
!
interface vlan 1
 ip address 192.168.101.201 255.255.255.0
!
ip default-gateway 192.168.101.254
!
!
!
!
line con 0
 exec-timeout 15 0
 login local
line vty 0 4
 exec-timeout 15 0
 transport input ssh
 login local
line vty 5 15
 exec-timeout 15 0
 transport input ssh
 login local
!
!
!
!
end
```


## Ping check

Router1 -> Router 2  
![Router1-Router2](/img/pingrouter1-2.png)

Switch02 -> Switch05  
![Switch02-Switch05](/img/pingswitch02-05.png)

PC03 -> PC06 & PC07  
![PC03-PC06_7](/img/pingpc03-pc06_7.png)

PC06 -> PC03 & PC04  
![PC06-PC03_4](/img/pingpc06-pc03_4.png)


# Aufgabe 2 *London Erweiterung*

## Config Router

```c
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname router3
!
!
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
!
!
!
no ip cef
no ipv6 cef
!
!
!
username netadmin privilege 15 password 0 cisco
!
!
license udi pid CISCO2911/K9 sn FTX15244K6F-
!
!
!
!
!
!
!
!
!
no ip domain-lookup
ip domain-name labor-lan.net
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
 description ####  link_lan_london  ###
 ip address 192.168.102.254 255.255.255.0
 duplex auto
 speed Auto
 no shutdown
!
interface GigabitEthernet0/1
 description
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
!
interface Serial0/3/0
 description ####  link_london2wien  ###
 bandwidth 2048
 ip address 192.168.251.2 255.255.255.0
 encapsulation ppp
 no shutdown
!
interface Serial0/3/1
 no ip address
 clock rate 2000000
 shutdown
!
interface Vlan1
 no ip address
!
ip classless
!
ip route 192.168.102.0 255.255.255.0 GigabitEthernet0/0 
ip route 192.168.21.0 255.255.255.0 192.168.250.1 
ip route 192.168.101.0 255.255.255.0 192.168.250.1
ip route 192.168.103.0 255.255.255.0 192.168.250.1
!
!
ip flow-export version 9
!
!
!
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
```

## Config Switch

```c
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname switch05
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
no ip domain-lookup
ip domain-name labor-lan.net
!
username netadmin privilege 1 password 7 0822455D0A16
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
interface range FastEthernet0/1 - 23
 switchport mode Access
 spanning-tree portfast
!
interface FastEthernet0/24
 description ###  Uplink-2-router3 ###
 switchport mode trunk
 spanning-tree portfast
!
interface GigabitEthernet0/1
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport mode trunk
!
!
interface vlan 1
 ip address 192.168.102.201 255.255.255.0
!
ip default-gateway 192.168.102.254
!
!
!
!
line con 0
 exec-timeout 15 0
 login local
line vty 0 4
 exec-timeout 15 0
 transport input ssh
 login local
line vty 5 15
 exec-timeout 15 0
 transport input ssh
 login local
!
!
!
!
end
```

## Ping check

Router1 -> Router 3  
![Router1-Router3](/img/pingrouter1-3.png)

Reply von Gateway: Destination Host unreachable
-->
```
Switch02 -> Switch06  
![Switch02-Switch06](/img/pingswitch02-06.png)

PC03 -> PC09 & PC10  
![PC03-PC09_10](/img/pingpc03-pc09_10.png)

PC09 -> PC03 & PC04  
![PC09-PC03_4](/img/pingpc09-pc03_4.png)
```


# Aufgabe 3 *Paris Erweiterung*

## Config Router

```c
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname router4
!
!
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
!
!
!
no ip cef
no ipv6 cef
!
!
!
username netadmin privilege 15 password 0 cisco
!
!
license udi pid CISCO2911/K9 sn FTX15244K6F-
!
!
!
!
!
!
!
!
!
no ip domain-lookup
ip domain-name labor-lan.net
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
 description ####  link_lan_paris  ###
 ip address 192.168.103.254 255.255.255.0
 duplex auto
 speed Auto
 no shutdown
!
interface GigabitEthernet0/1
 description
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
!
interface Serial0/3/0
 description ####  link_paris2wien  ###
 bandwidth 2048
 ip address 192.168.252.2 255.255.255.0
 encapsulation ppp
 no shutdown
!
interface Serial0/3/1
 no ip address
 clock rate 2000000
 shutdown
!
interface Vlan1
 no ip address
!
ip classless
!
ip route 192.168.103.0 255.255.255.0 GigabitEthernet0/0 
ip route 192.168.21.0 255.255.255.0 192.168.250.1 
ip route 192.168.101.0 255.255.255.0 192.168.250.1
ip route 192.168.102.0 255.255.255.0 192.168.250.1
!
!
ip flow-export version 9
!
!
!
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
```

## Config Switch

```c
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname switch06
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
no ip domain-lookup
ip domain-name labor-lan.net
!
username netadmin privilege 1 password 7 0822455D0A16
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
interface range FastEthernet0/1 - 23
 switchport mode Access
 spanning-tree portfast
!
interface FastEthernet0/24
 description ###  Uplink-2-router4 ###
 switchport mode trunk
 spanning-tree portfast
!
interface GigabitEthernet0/1
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport mode trunk
!
!
interface vlan 1
 ip address 192.168.103.201 255.255.255.0
!
ip default-gateway 192.168.103.254
!
!
!
!
line con 0
 exec-timeout 15 0
 login local
line vty 0 4
 exec-timeout 15 0
 transport input ssh
 login local
line vty 5 15
 exec-timeout 15 0
 transport input ssh
 login local
!
!
!
!
end
```

## Ping check

Router1 -> Router 4  
![Router1-Router4](/img/pingrouter1-4.png)

Reply von Gateway: Destination Host unreachable
-->
```
Switch02 -> Switch07  
![Switch02-Switch07](/img/pingswitch02-07.png)

PC03 -> PC11 & PC12  
![PC03-PC11_12](/img/pingpc03-pc11_12.png)

PC11 -> PC03 & PC04  
![PC11-PC03_4](/img/pingpc11-pc03_4.png)
```