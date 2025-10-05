---
title: "002_Standortvernetzung"
author: "Felix Mackinger"
klasse: "4AHITS"
subject: "SYTD"
date: "2025-10-01"
---


# Aufgabenstellung

[Aufgabenstellung](https://felix-mackinger.github.io/Report-gen/SYTD/002_Standortvernetzung/img/Übung_02-Standortvernetzung_IPv4.pdf)


### **Vorgehensweise**

Beide Router wurden mit Hostname, Domain und Benutzerzugang eingerichtet.
Die Interfaces erhielten folgende Adressen:

![Logical Layout](/img/logical-layout.png)

* **R-BR-1:**

  * Fa0/0 → 192.168.1.1/24
  * Fa0/1 → 10.0.0.1/30

* **R-S-1:**

  * Fa0/0 → 192.168.2.1/24
  * Fa0/1 → 10.0.0.2/30

Für die Verbindung der LANs wurden statische Routen gesetzt:

```console
R-BR-1: ip route 192.168.2.0 255.255.255.0 10.0.0.2
R-S-1: ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

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
no ipv6 cef
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
 ipv6 address 2001:ABC:DB8:ACAD::1/64
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
ip route 192.168.2.0 255.255.255.0 10.0.0.2
ip route 10.0.0.0 255.255.255.252 10.0.0.2
!
ip flow-export version 9
!
!
!
no cdp run
!
banner motd ^C!KEIN ZUGRIFF ERLAUBT!^^C
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
no ipv6 cef
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
 ipv6 address 2001:ABC:DB8:ACAD:12::1/64
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
ip route 192.168.1.0 255.255.255.0 10.0.0.1
ip route 10.0.0.0 255.255.255.252 10.0.0.1
!
ip flow-export version 9
!
!
!
no cdp run
!
banner motd ^C!KEIN ZUGRIFF ERLAUBT!^^C
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
version 15.0
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname S-BR-1
!
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
ip domain-name braunau.at
!
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode access
 spanning-tree portfast
!
interface FastEthernet0/2
 switchport mode access
 spanning-tree portfast
!
interface FastEthernet0/3
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/4
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/5
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/6
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/7
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/8
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/9
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/10
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/11
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/12
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/13
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/14
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/15
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/16
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/17
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/18
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/19
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/20
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/21
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/22
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/23
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/24
 switchport mode access
 spanning-tree portfast
 no shutdown
!
interface GigabitEthernet0/1
 switchport mode access
 spanning-tree portfast
!
interface GigabitEthernet0/2
 switchport mode access
 spanning-tree portfast
!
interface Vlan1
 description LAN-Mgmt
 ip address 192.168.1.2 255.255.255.0
!
ip default-gateway 192.168.1.1
!
no cdp run
!
banner motd ^C!KEIN ZUGRIFF ERLAUBT!^^C
!
!
!
!
!
line con 0
 login local
!
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
line vty 5 15
 exec-timeout 15 0
 login local
 transport input ssh
!
!
!
!
end
```

#### S-S-1
```console
version 15.0
service timestamps log datetime msec
service timestamps debug datetime msec
service password-encryption
!
hostname S-S-1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
ip domain-name schaerding.at
!
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport mode access
 spanning-tree portfast
!
interface FastEthernet0/2
 switchport mode access
 spanning-tree portfast
!
interface FastEthernet0/3
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/4
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/5
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/6
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/7
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/8
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/9
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/10
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/11
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/12
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/13
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/14
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/15
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/16
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/17
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/18
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/19
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/20
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/21
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/22
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/23
 switchport mode access
 spanning-tree portfast
 shutdown
!
interface FastEthernet0/24
 switchport mode access
 spanning-tree portfast
!
interface GigabitEthernet0/1
 switchport mode access
 spanning-tree portfast
!
interface GigabitEthernet0/2
 switchport mode access
 spanning-tree portfast
!
interface Vlan1
 description LAN-Mgmt
 ip address 192.168.2.2 255.255.255.0
!
ip default-gateway 192.168.2.1
!
no cdp run
!
banner motd ^C!KEIN ZUGRIFF ERLAUBT!^^C
!
!
!
line con 0
 login local
!
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
line vty 5 15
 exec-timeout 15 0
 login local
 transport input ssh
!
!
!
!
end
```

---

### PING Tests

![Ping von PC-A zu Switch Router](/img/ping-BR.png)

![Ping von PC-B zu Switch Router](/img/ping-S.png)


---


### **Fehleranalyse**

Zunächst war kein Ping zwischen den Standorten möglich.
Ursache war eine falsche Subnetzmaske (**/32 statt /24**) in den statischen Routen.
Nach der Korrektur funktionierte das Routing einwandfrei.

---

### **Ergebnis**

Nach der Anpassung konnten beide LANs erfolgreich miteinander kommunizieren.
Die Verbindung über das **10.0.0.0/30-Netz** war stabil und die Standortvernetzung funktionierte laut Aufgabenstellung.



