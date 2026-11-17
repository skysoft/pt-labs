# Lab 2.1.5 - Routeren op de switch

CCNA 200-301 v2.0 - leerdoel 2.1. Configuratielab.

Packet Tracer-bestand: [2.1.5_routeren_op_de_switch.pkt](2.1.5_routeren_op_de_switch.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.1):** netwerkinfrastructuur verbinden: Layer 3 physical interfaces en switch virtual interfaces (SVI).

## Scenario
Bij tandartspraktijk De Witte Kies staat een multilayer switch (MLS1, een 3560) die tot nu toe alleen als laag-2-switch werd gebruikt. Hij moet de **gateway** worden voor alle VLAN's en via een **routed port** verbonden worden met de internetrouter EDGE. Daarnaast komt er een nieuw VLAN voor de printers.

EDGE is al klaar: hij is verbonden met de provider (router INTERNET, met daarachter een server op 8.8.8.8), doet NAT voor 192.168.0.0/16 en heeft een route naar 192.168.0.0/16 via 10.0.0.2.

## Ontwerp
| VLAN | Naam | Subnet | Gateway (SVI op MLS1) |
|---|---|---|---|
| 10 | BALIE | 192.168.10.0/24 | 192.168.10.1 |
| 20 | BEHANDELKAMERS | 192.168.20.0/24 | 192.168.20.1 |
| 30 | PRINTERS (nieuw) | 192.168.30.0/24 | 192.168.30.1 |

| Link | Ontwerp |
|---|---|
| MLS1 Gi0/1 - EDGE G0/0 | routed port, 10.0.0.0/30 (EDGE .1, MLS1 .2); default route van MLS1 via EDGE |
| MLS1 Gi0/2 - ASW1 Gi0/1 | trunk (bestaat al), moet ook VLAN 30 dragen |

| Apparaat | Poort | VLAN | Adres |
|---|---|---|---|
| PC1 | MLS1 Fa0/1 | 10 | 192.168.10.11/24 |
| PC2 | MLS1 Fa0/2 | 20 | 192.168.20.11/24 |
| PC3 | ASW1 Fa0/1 | 20 | 192.168.20.12/24 |
| PRN1 | ASW1 Fa0/2 | 30 | 192.168.30.10/24 |

PRN1 is in Packet Tracer een pc die de printer voorstelt, zodat je er ook vanaf kunt pingen.

## Toegang
- Routers en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een poort doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.
- Packet Tracer kent geen uitvoerfilters zoals `| include`.

## Opdrachten

### Deel 1 - Theorie
1. Wat is het verschil tussen een SVI, een routed port (`no switchport`) en een subinterface op een router?
2. Aan welke voorwaarden moet voldaan zijn voordat een SVI **up/up** is?
3. Wat doet `ip routing` op een multilayer switch?

### Deel 2 - Uitgangssituatie
1. Bekijk op MLS1 `show ip route` en `show running-config` (zoek naar "routing"). Wat valt je op?
2. Ping vanaf PC1 naar zijn gateway en naar PC2.

### Deel 3 - SVI's configureren
1. Zet routering aan op MLS1.
2. Maak de SVI's voor VLAN 10, 20 en 30 aan met de adressen uit het ontwerp.
3. Bekijk `show ip interface brief`. Welke SVI is niet up/up? Verklaar dat met de voorwaarden uit deel 1 (tip: `show vlan brief` en `show interfaces trunk`).
4. Ping vanaf PC1 naar PC2 en PC3.

### Deel 4 - Routed port naar EDGE
1. Maak van MLS1 Gi0/1 een routed port met het adres uit het ontwerp.
2. Configureer een default route op MLS1 naar EDGE.
3. Vergelijk `show interfaces GigabitEthernet0/1 switchport` voor en na. Staat Gi0/1 nog in `show spanning-tree`?
4. Ping vanaf PC1 naar 10.0.0.1 en naar 8.8.8.8.

### Deel 5 - Het printer-VLAN
1. Zorg dat PRN1 in VLAN 30 terechtkomt: VLAN 30 moet op ASW1 bestaan, de poort van PRN1 moet in VLAN 30 staan, en de trunk moet VLAN 30 aan beide kanten doorlaten.
2. Kijk na elke stap naar de status van SVI 30 op MLS1 (`show interfaces vlan 30`). Is hij up/up als alle voorwaarden kloppen? Zo niet: probeer `shutdown` en `no shutdown` op de SVI.
3. Ping vanaf PRN1 naar 192.168.30.1, PC1 en 8.8.8.8.

### Deel 6 - Reflectie
1. Waarom gebruik je voor de link naar EDGE liever een routed port dan een access-poort met een SVI in een apart VLAN?
2. Wat gebeurt er met SVI 10 als je Fa0/1 (PC1) op MLS1 uitzet? Probeer het en zet het terug.
3. Waarom heeft EDGE een route naar 192.168.0.0/16 nodig, en wat zou er gebeuren zonder die route?

Je docent bespreekt daarna je antwoorden.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
