---
title: "005_VLAN-Trunks-Access-Point"
author: "Felix Mackinger"
klasse: "4AHITS"
subject: "SYTD"
date: "2025-10-23"
---


# Aufgabenstellung

[Aufgabenstellung](https://felix-mackinger.github.io/Report-gen/SYTD/docs/SYTD/005_VLAN-Trunks-Access-Ports/img/Übung_05-VLANs_Trunks_und_Access_Ports.html)


![Logical Layout](/img/logical-layout.png)


## Aufgabe 1 

### Router Config

router1
```c
version 15.1
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
username admin privilege 15 password 0 cisco
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
ip ssh version 1
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
 speed auto
!
interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Serial0/2/0
 description ####  link_wien2berlin  ###
 bandwidth 2048
 ip address 192.168.250.1 255.255.255.0
 encapsulation ppp
 clock rate 2000000
!
interface Serial0/2/1
 description ####  link_wien2london  ###
 bandwidth 2048
 ip address 192.168.251.1 255.255.255.0
 encapsulation ppp
 clock rate 2000000
!
interface Serial0/3/0
 description ####  link_wien2london  ###
 bandwidth 2048
 ip address 192.168.252.1 255.255.255.0
 encapsulation ppp
 clock rate 2000000
!
interface Serial0/3/1
 no ip address
 clock rate 2000000
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
ip route 192.168.21.0 255.255.255.0 GigabitEthernet0/0 
ip route 192.168.101.0 255.255.255.0 192.168.250.2 
ip route 192.168.102.0 255.255.255.0 192.168.251.2 
ip route 192.168.103.0 255.255.255.0 192.168.252.2 
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
!
end
```

Anpassung von Router1, line security -> exec timeout

Router on a Stick wurde noch ausgelassen



### Switch Config

Switch01
```c
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname switch01
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
vtp mode client
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
 description ###  Uplink-2-Switch01 ###
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

Und noch den Befehl 
```c
vtp mode server
```


Switch02
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
interface range FastEthernet0/1 - 22
 switchport mode Access
 spanning-tree portfast
!
interface FastEthernet0/23
 description ###  Uplink-2-Switch01 ###
 switchport mode trunk
 switchport trunk allowed vlan all
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
 ip address 192.168.21.202 255.255.255.0
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

Und noch den Befehl 
```c
vtp mode client
```



Vlan config

```c
enable
configure terminal
!
vlan 1
 name vlan_MGMT
vlan 10
 name vlan_blue
vlan 11
 name vlan_green
vlan 12
 name vlan_red
vlan 13
 name vlan_yellow
vlan 14
 name vlan_black

!
interface range fastEthernet0/1 - 22
 switchport mode access
 switchport access vlan 1
 exit

interface range fastEthernet0/2 - 5
 switchport mode access
 switchport access vlan 10
 exit

!
interface range fastEthernet0/6 - 10
 switchport mode access
 switchport access vlan 11
 exit

! 
interface range fastEthernet0/11 - 15
 switchport mode access
 switchport access vlan 12
 exit

! 
interface range fastEthernet0/16 - 20
 switchport mode access
 switchport access vlan 13
 exit

!
interface fastEthernet0/21
 switchport mode access
 switchport access vlan 14
 exit

! 
end
write memory

show vlan brief

```





### Ping 

PC01 -> PC03 & PC02  
![PC01-PC02-5](/img/Pingcheck.png)

Pings gehen derzeit nur VLAN intern.


| VLAN-ID | VLAN-Name   | Beschreibung / Zweck        | IP-Bereich        | Gateway (Router) | Switch01 Ports           | Switch02 Ports           |
|----------|-------------|-----------------------------|-------------------|------------------|---------------------------|---------------------------|
| 1        | vlan_MGMT   | Management-VLAN (Switches)  | 192.168.21.0/24   | 192.168.21.254   | Fa0/1, Fa0/22–23          | Fa0/1, Fa0/22             |
| 10       | vlan_blue   | Abteilung Blue (Clients)    | 192.168.21.0/24   | 192.168.21.254   | Fa0/2–5                   | Fa0/2–5                   |
| 11       | vlan_green  | Abteilung Green (Clients)   | 192.168.21.0/24   | 192.168.21.254   | Fa0/6–10                  | Fa0/6–10                  |
| 12       | vlan_red    | Abteilung Red (Clients)     | 192.168.21.0/24   | 192.168.21.254   | Fa0/11–15                 | Fa0/11–15                 |
| 13       | vlan_yellow | Abteilung Yellow (Clients)  | 192.168.21.0/24   | 192.168.21.254   | Fa0/16–20                 | Fa0/16–20                 |
| 14       | vlan_black  | Abteilung Black (Clients)   | 192.168.21.0/24   | 192.168.21.254   | Fa0/21                    | Fa0/21                    |
| —        | —           | **Trunk-Uplinks**           | —                 | —                | Fa0/24 (→ Switch02), Gi0/1–2 | Fa0/23 (→ Switch01), Fa0/24 (→ Router), Gi0/1–2 |
