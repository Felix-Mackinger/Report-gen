---
title: "003_Standortvernetzung-IPv6"
author: "Felix Mackinger"
klasse: "4AHITS"
subject: "SYTD"
date: "2025-10-05"
---


# Aufgabenstellung

[Aufgabenstellung](https://felix-mackinger.github.io/Report-gen/SYTD/003_Standortvernetzung/img/Übung_03-Standortvernetzung_IPv6.pdf)


### **Vorgehensweise**

Da von der vorhergehenden Aufgabe die Umgebung schon gestellt ist, wurde diese verwendet.

![Logical Layout](/img/logical-layout.png)

* **R-BR-1:**

  * Fa0/0 → 2001:ABC:DB8:ACA1:11::1/64
  * Fa0/1 → 2001:BC:DB8:ACAD:1::1/64


* **R-S-1:**

  * Fa0/0 → 2001:ABC:DB8:ACA2:12::1/64
  * Fa0/1 → 2001:BC:DB8:ACAD:1::2/64
 

Für die Verbindung der LANs wurden statische Routen gesetzt:

```console
R-BR-1: ipv6 route 2001:ABC:DB8:ACA2:12::/64 2001:BC:DB8:ACAD:1::2
R-S-1: ipv6 route 2001:ABC:DB8:ACA1:11::/64 2001:BC:DB8:ACAD:1::1
```

---


### PING Tests

![Ping von PC-BR-A zu PC-BR-B R-BR-1](/img/ping-Br.png)

![Ping von PC-S-A zu PC-S-B R-S-1](/img/ping-S.png)


---


### **Fehleranalyse**

Zunächst war kein Ping zwischen den Standorten möglich.
Ursache war das bei den Statischen Routen `R-BR-1: ipv6 route 2001:ABC:DB8:ACAD:11::/64 2001:BC:DB8:ACAD:1::2`                 
                                                                                ^^  <- Die 11 entfernt wurde durch die Präfix länge.

Deswegen ist ein Loop im `2001:BC:DB8:ACAD:1::/64` entstanden.
Nach der Umstellung auf diese Netze `2001:ABC:DB8:ACA1:11::/64` `2001:ABC:DB8:ACA2:12::/64` funktionierte das Routing einwandfrei.

---

### Code

#### R-BR-1
```console
version 15.1
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname R-BR-1
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
ipv6 cef
!
!
!
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
license udi pid CISCO2811/K9 sn FTX1017H5VE-
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
ip domain-name braunau.at
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface FastEthernet0/0
 description Verbindung zu S-BR-1
 ip address 192.168.1.1 255.255.255.0
 duplex auto
 speed auto
 ipv6 address 2001:ABC:DB8:ACA1:11::1/64
 ipv6 enable
!
interface FastEthernet0/1
 description Verbindung zu R-S-1
 ip address 10.0.0.1 255.255.255.252
 duplex auto
 speed auto
 ipv6 address 2001:BC:DB8:ACAD:1::1/64
 ipv6 enable
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
ip route 10.0.0.0 255.255.255.252 10.0.0.2 
ip route 192.168.2.0 255.255.255.0 10.0.0.2 
!
ip flow-export version 9
!
ipv6 route 2001:ABC:DB8:ACA2::/64 2001:BC:DB8:ACAD:1::2
!
!
no cdp run
!
banner motd ^C!KEIN ZUGRIFF ERLAUBT!^C
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

#### R-S-1
```console
version 15.1
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname R-S-1
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
ipv6 cef
!
!
!
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
license udi pid CISCO2811/K9 sn FTX1017ZKX1-
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
ip domain-name schaerding.at
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface FastEthernet0/0
 description Verbindung zu S-S-1
 ip address 192.168.2.1 255.255.255.0
 duplex auto
 speed auto
 ipv6 address 2001:ABC:DB8:ACA2:12::1/64
 ipv6 enable
!
interface FastEthernet0/1
 description Verbindung zu R-BR-1
 ip address 10.0.0.2 255.255.255.252
 duplex auto
 speed auto
 ipv6 address 2001:BC:DB8:ACAD:1::2/64
 ipv6 enable
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
ip route 10.0.0.0 255.255.255.252 10.0.0.1 
ip route 192.168.1.0 255.255.255.0 10.0.0.1 
!
ip flow-export version 9
!
ipv6 route 2001:ABC:DB8:ACA1::/64 2001:BC:DB8:ACAD:1::1
!
!
no cdp run
!
banner motd ^C!KEIN ZUGRIFF ERLAUBT!^C
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

#### S-BR-1
```console
gleich
```

#### S-S-1
```console
gleich
```


