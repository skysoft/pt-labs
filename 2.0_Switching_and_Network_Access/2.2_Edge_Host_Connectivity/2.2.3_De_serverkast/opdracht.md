# Lab 2.2.3 - De serverkast

CCNA 200-301 v2.0 - leerdoel 2.2. Configuratielab.

Packet Tracer-bestand: [2.2.3_de_serverkast.pkt](2.2.3_de_serverkast.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.2):** Layer 2-poortinstellingen configureren voor eindapparaten (VLAN, port channel en LACP): virtualized hosts en network appliances.

## Scenario
Accountantskantoor De Balans krijgt een eigen serverkast met switch SW-SRV. Daarop komen:
- **HV1**, een hypervisor met de virtuele machines *VM-WEB* (VLAN 60) en *VM-TEST* (VLAN 61). Het beheeradres van de hypervisor zelf zit in VLAN 69 en gaat ongetagd over de kabel; de VM's gaan getagd.
- **NAS1**, een opslagapparaat met **twee** netwerkpoorten, die samen een LACP-bundel (802.3ad) vormen. De NAS is al zo ingesteld; de switch nog niet.

De uplink van SW-SRV naar CORE en alle VLAN's zijn al klaar. CORE is de gateway (SVI's .1) voor alle VLAN's. PC1 op CORE is een werkplek van een medewerker.

## VLAN's
| VLAN | Naam | Subnet |
|---|---|---|
| 10 | KANTOOR | 192.168.10.0/24 |
| 60 | VM-PRODUCTIE | 192.168.60.0/24 |
| 61 | VM-TEST | 192.168.61.0/24 |
| 69 | HV-BEHEER | 192.168.69.0/24 |
| 70 | OPSLAG | 192.168.70.0/24 |
| 99 | BEHEER (native uplink) | 192.168.99.0/24 |

## Poortenplan SW-SRV (2960)
| Poort | Apparaat | Ontwerp |
|---|---|---|
| Gi0/1 | uplink CORE | trunk (al klaar) |
| Fa0/1 | HV1 | trunk, native VLAN 69, allowed 60,61,69, geen DTP, PortFast voor trunks |
| Fa0/2 - Fa0/3 | NAS1 (poort 1 + 2) | Port-channel 2, LACP active, access VLAN 70, PortFast |

| Apparaat | Adres(sen) |
|---|---|
| HV1 | beheer 192.168.69.10 (ongetagd); VM-WEB 192.168.60.11 (VLAN 60); VM-TEST 192.168.61.11 (VLAN 61) |
| NAS1 | 192.168.70.10 (over beide poorten) |
| PC1 | 192.168.10.11 (op CORE) |

**Let op - nabootsing in Packet Tracer:** pc's en servers in Packet Tracer kunnen niet taggen en geen LACP-bundel maken. Daarom:
- **HV1** is een router: Gi0/0 is de NIC van de hypervisor (beheer, ongetagd), en de subinterfaces Gi0/0.60 en Gi0/0.61 zijn de VM's.
- **NAS1** is een switch met een LACP-bundel over Fa0/1-2 en een beheeradres als NAS-adres. **Pas NAS1 niet aan**; je mag er wel show-commando's op uitvoeren.

## Toegang
- Switches en HV1: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC1: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een poort doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.
- Packet Tracer kent geen `show lacp neighbor` en geen `lacp rate fast`. Gebruik `show etherchannel summary`, `show etherchannel port-channel` en `show cdp neighbors`.

## Opdrachten

### Deel 1 - Theorie
1. Waarom heeft een hypervisor meestal een trunk nodig? Wie doet aan de kant van de server het taggen?
2. Wat is het voordeel van een LACP-bundel naar een NAS of server ten opzichte van één kabel? En ten opzichte van twee losse kabels?
3. Wat doet `spanning-tree portfast trunk`, en waarom mag dat hier wel?

### Deel 2 - Uitgangssituatie
1. Ping vanaf PC1 naar 192.168.69.10, 192.168.60.11 en 192.168.70.10.
2. Bekijk op SW-SRV `show etherchannel summary`, en op NAS1 ook. Wat is de status van de bundel op NAS1, en waarom?

### Deel 3 - Hypervisor (Fa0/1)
1. Configureer Fa0/1 volgens het poortenplan.
2. Ping vanaf PC1 naar het beheeradres van HV1 en naar beide VM's.
3. Bekijk `show mac address-table` op SW-SRV. In welke VLAN's zie je het MAC-adres van HV1 op Fa0/1?

### Deel 4 - NAS (Fa0/2 - Fa0/3)
1. Configureer de bundel naar NAS1 volgens het poortenplan.
2. Controleer met `show etherchannel summary` en `show etherchannel port-channel` op SW-SRV en op NAS1. Welke flags zie je?
3. Ping vanaf PC1 naar NAS1.
4. **Kabel eruit:** start op PC1 `ping -t 192.168.70.10`, verwijder één van de twee kabels tussen SW-SRV en NAS1 (gum, rechts in de werkbalk, of *Delete*) en kijk wat er gebeurt. Bekijk `show etherchannel summary`. Sluit de kabel daarna weer aan (gekruiste kabel, dezelfde poorten) en stop de ping met Ctrl+C.

### Deel 5 - Reflectie
1. Wat zou er gebeuren als je op Fa0/2-3 `channel-group 2 mode on` gebruikt, terwijl de NAS LACP doet? En met `mode passive`? Probeer het (eerst `no channel-group 2`), bekijk `show etherchannel summary` op beide apparaten, en zet het daarna terug op `active`.
2. Een collega wil de NAS ook in VLAN 60 beschikbaar maken. Wat moet er aan beide kanten veranderen?

Je docent bespreekt daarna je antwoorden.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
