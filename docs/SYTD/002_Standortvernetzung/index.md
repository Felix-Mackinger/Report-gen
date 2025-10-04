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



