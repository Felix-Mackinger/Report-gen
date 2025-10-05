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

![Ping von PC-BR-A zu PC-BR-B R-BR-1](/img/ping-BR.png)

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

```

#### R-S-1
```console

```

#### S-BR-1
```console

```

#### S-S-1
```console

```


