# Lab 2.2.2 - Bellen en draadloos

CCNA 200-301 v2.0 - leerdoel 2.2. Configuratielab.

Packet Tracer-bestand: [2.2.2_bellen_en_draadloos.pkt](2.2.2_bellen_en_draadloos.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.2):** Layer 2-poortinstellingen configureren voor eindapparaten: VoIP-telefoon en wireless access points (standalone en controller-based).

## Scenario
Fysiotherapiepraktijk In Beweging krijgt IP-telefonie en wifi. Op switch SW1 komen:
- een **IP-telefoon** (TEL1) met de pc van de balie (PC1) erachter: PC1 is aangesloten op de PC-poort van de telefoon;
- een **standalone AP** (AP1) dat zelf twee SSID's uitzendt: *Personeel* (VLAN 10) en *Gasten* (VLAN 50); het beheeradres van AP1 zit in VLAN 40;
- een **controller-based (lightweight) AP** (AP2) dat al het wifi-verkeer via een CAPWAP-tunnel naar de wireless controller WLC1 stuurt.

Alle VLAN's bestaan al op SW1 en CORE. De uplink van SW1 naar CORE is klaar. CORE is de gateway (SVI's .1) voor alle VLAN's, en is ook de DHCP-server voor het voice VLAN.

## VLAN's
| VLAN | Naam | Subnet |
|---|---|---|
| 10 | PERSONEEL | 192.168.10.0/24 |
| 40 | AP-BEHEER | 192.168.40.0/24 |
| 45 | WLC | 192.168.45.0/24 |
| 50 | GASTEN | 192.168.50.0/24 |
| 99 | BEHEER (native uplink) | 192.168.99.0/24 |
| 150 | VOICE | 192.168.150.0/24 (DHCP op CORE) |

## Poortenplan SW1
| Poort | Apparaat | Ontwerp |
|---|---|---|
| Gi0/1 | uplink CORE | trunk (al klaar) |
| Fa0/1 | TEL1 + PC1 | data-VLAN 10, voice VLAN 150, PortFast |
| Fa0/2 | AP1 (standalone) | trunk, native VLAN 40, allowed 10,40,50, geen DTP, PortFast voor trunks |
| Fa0/3 | AP2 (lightweight) | access VLAN 40, PortFast |

| Apparaat | Adres(sen) |
|---|---|
| TEL1 | via DHCP in VLAN 150 |
| PC1 | 192.168.10.11 |
| AP1 | beheer 192.168.40.11 (ongetagd, Gi0/0); SSID Personeel 192.168.10.21 (Gi0/0.10, VLAN 10); SSID Gasten 192.168.50.21 (Gi0/0.50, VLAN 50) |
| AP2 | 192.168.40.12 |
| WLC1 | 192.168.45.10 (op CORE Fa0/1) |

**Let op - nabootsing in Packet Tracer:**
- AP1 is een **router** die het verkeer van een standalone AP nabootst: ongetagd beheerverkeer op Gi0/0, en per SSID een getagde subinterface (Gi0/0.10 en Gi0/0.50). Pingen doe je in de CLI van AP1.
- AP2 is een pc die het IP-verkeer van een lightweight AP nabootst, en WLC1 is een server. Er is geen echte CAPWAP-tunnel.
- SW1 is een 3560-24PS en levert **PoE**: de telefoon krijgt zijn stroom van de switch.

## Toegang
- Switches en AP1: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een poort doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.

## Opdrachten

### Deel 1 - Theorie
1. Een IP-telefoon en een pc delen één switchpoort. Hoe houdt de switch spraak- en dataverkeer uit elkaar? Wat is getagd en wat niet?
2. Hoe weet een Cisco-telefoon welk voice VLAN hij moet gebruiken?
3. Wat is het verschil tussen een standalone (autonomous) AP en een lightweight AP? Waar wordt het wifi-verkeer op het bekabelde netwerk gezet?
4. Waarom heeft een standalone AP een trunk nodig, en een lightweight AP (local mode) niet? Wanneer heeft een lightweight AP toch een trunk nodig (tip: FlexConnect)?

### Deel 2 - Poort voor telefoon en pc (Fa0/1)
1. Bekijk eerst `show power inline` en `show cdp neighbors` op SW1. Waar haalt TEL1 zijn stroom vandaan, en hoeveel?
2. Configureer Fa0/1 volgens het poortenplan.
3. Controleer met `show interfaces FastEthernet0/1 switchport` (*Access Mode VLAN* en *Voice VLAN*) en `show vlan brief`. In welke VLAN's staat Fa0/1?
4. Kijk op CORE met `show ip dhcp binding` of TEL1 een adres heeft gekregen. In welk subnet? Bekijk ook `show mac address-table` op SW1: in welke VLAN's zie je MAC-adressen op Fa0/1?
5. Ping vanaf PC1 naar 192.168.10.1.

### Deel 3 - Poort voor de standalone AP (Fa0/2)
1. Configureer Fa0/2 volgens het poortenplan.
2. Test vanuit de CLI van AP1: `ping 192.168.40.1` (beheer), `ping 192.168.10.1` (SSID Personeel) en `ping 192.168.50.1` (SSID Gasten).
3. Bekijk `show interfaces trunk` en `show spanning-tree interface FastEthernet0/2 detail`.

### Deel 4 - Poort voor de lightweight AP (Fa0/3)
1. Configureer Fa0/3 volgens het poortenplan.
2. Test vanaf AP2 of hij de controller bereikt: `ping 192.168.45.10`.

### Deel 5 - Reflectie
1. Waarom hoeven de VLAN's 10 en 50 niet op de poort van AP2 te staan, terwijl gebruikers op AP2 wel in die VLAN's terechtkomen?
2. Wat gebeurt er met AP1 als de native VLAN op Fa0/2 op 1 blijft staan? Probeer het (`no switchport trunk native vlan 40`) en zet het terug.
3. Een AP met 802.3at (PoE+) krijgt op een 802.3af-poort te weinig stroom. Wat merk je dan in de praktijk?

Je docent bespreekt daarna je antwoorden.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
