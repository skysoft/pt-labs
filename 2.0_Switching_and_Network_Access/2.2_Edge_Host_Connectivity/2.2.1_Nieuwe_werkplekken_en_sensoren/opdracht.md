# Lab 2.2.1 - Nieuwe werkplekken en sensoren

CCNA 200-301 v2.0 - leerdoel 2.2. Configuratielab.

Packet Tracer-bestand: [2.2.1_nieuwe_werkplekken_en_sensoren.pkt](2.2.1_nieuwe_werkplekken_en_sensoren.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.2):** Layer 2-poortinstellingen configureren voor eindapparaten (VLAN, PoE): desktop, printer en IoT-apparaten.

## Scenario
Hotel De Gouden Sleutel krijgt op de receptie een nieuwe switch (SW1). Daarop komen twee werkplekken, een printer, een beveiligingscamera en een klimaatsensor. Het beleid van het hotel:
- elk type apparaat in een **eigen VLAN** (IoT-apparaten nooit in hetzelfde VLAN als werkplekken);
- elke poort krijgt een **description**;
- eindapparaat-poorten zijn **vaste access-poorten** met **PortFast**;
- **ongebruikte poorten** gaan uit en komen in parkeer-VLAN 999.

De uplink van SW1 naar de kernswitch CORE en het beheer-VLAN 99 zijn al geconfigureerd. CORE (een 3560) is de gateway (SVI's) voor alle VLAN's.

## VLAN's
| VLAN | Naam | Subnet | Gateway (CORE) |
|---|---|---|---|
| 10 | WERKPLEKKEN | 192.168.10.0/24 | .1 |
| 20 | PRINTERS | 192.168.20.0/24 | .1 |
| 30 | IOT | 192.168.30.0/24 | .1 |
| 99 | BEHEER (native op trunk) | 192.168.99.0/24 | .1 |
| 999 | PARKEER | - | - |

## Poortenplan SW1 (2960)
| Poort | Apparaat | VLAN | Adres |
|---|---|---|---|
| Gi0/1 | uplink CORE (trunk, al klaar) | - | - |
| Fa0/1 | PC1 | 10 | 192.168.10.11 |
| Fa0/2 | PC2 | 10 | 192.168.10.12 |
| Fa0/3 | PRN1 (printer) | 20 | 192.168.20.10 |
| Fa0/4 | CAM1 (IP-camera) | 30 | 192.168.30.10 |
| Fa0/5 | SENS1 (klimaatsensor) | 30 | 192.168.30.11 |
| Fa0/6 - Fa0/24, Gi0/2 | ongebruikt | 999 | - |

PRN1, CAM1 en SENS1 zijn in Packet Tracer pc's die het apparaat voorstellen, zodat je er ook vanaf kunt pingen.

## Toegang
- Switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een poort doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.

## Opdrachten

### Deel 1 - Theorie
1. Waarom zet je een eindapparaat-poort vast op `switchport mode access` in plaats van de standaardinstelling?
2. Wat doet PortFast, en waarom alleen op poorten naar eindapparaten?
3. Waarom zet je IoT-apparaten in een eigen VLAN?
4. Waarom zet je ongebruikte poorten uit, en waarom ook nog in een apart VLAN?

### Deel 2 - PoE
De camera en de klimaatsensor krijgen in het echt stroom via de switch (Power over Ethernet). **SW1 (2960-24TT) heeft geen PoE**; CORE (3560-24PS, "PS" = PoE) wel. Zoek op en beantwoord:
1. Welke PoE-standaarden zijn er (802.3af, 802.3at, 802.3bt) en hoeveel vermogen leveren ze per poort?
2. Wat doen `power inline auto`, `power inline never` en `power inline static max <mW>` op een Cisco-switch?
3. Wat is het PoE-budget van een switch? Wat gebeurt er als het budget op is?
4. Met welk commando bekijk je het PoE-verbruik per poort? Probeer het op SW1 en op CORE. Wat is het totale budget van CORE, en hoeveel watt mag één poort maximaal leveren?

### Deel 3 - Uitgangssituatie
1. Bekijk `show vlan brief` en `show interfaces status` op SW1. In welk VLAN staan de poorten nu?
2. Bekijk `show interfaces FastEthernet0/1 switchport`. Wat is de *administrative mode*?
3. Ping vanaf PC1 naar zijn gateway.

### Deel 4 - Configureren op SW1
1. Maak de VLAN's 10, 20, 30 en 999 aan met de juiste namen.
2. Configureer alle eindapparaat-poorten volgens het poortenplan en het beleid (description, vaste access-poort, VLAN, PortFast). Gebruik waar het kan `interface range`.
3. Zet de ongebruikte poorten volgens het beleid.

### Deel 5 - Verificatie
1. Bekijk opnieuw `show vlan brief` en `show interfaces status`. Klopt alles met het poortenplan?
2. Ping vanaf elk apparaat naar zijn gateway.
3. Ping vanaf CAM1 naar PC1. Lukt dat? Wat zegt dat over de scheiding tussen de VLAN's, en wat zou je extra nodig hebben om IoT echt af te schermen?
4. Bekijk `show spanning-tree interface FastEthernet0/4 detail`. Waaraan zie je dat PortFast actief is?

### Deel 6 - Reflectie
1. Een collega sluit een kleine switch aan op Fa0/6. Wat gebeurt er, en waarom?
2. Waarom is `show interfaces status` handig bij het documenteren van een switch?

Je docent bespreekt daarna je antwoorden.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
