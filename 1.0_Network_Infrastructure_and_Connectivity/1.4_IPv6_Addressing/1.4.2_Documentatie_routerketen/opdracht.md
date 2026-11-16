# Lab 1.4.2 - Documentatie routerketen

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.2_documentatie_routerketen.pkt](1.4.2_documentatie_routerketen.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten.

## Scenario
Drie routers zijn in een keten verbonden: R1 - R2 - R3. Volgens de netwerkstandaard van het bedrijf krijgen alle routerinterfaces hun interface-ID via **modified EUI-64**. De PC's krijgen hun adres via SLAAC.

De documentatie bevat alleen de prefixen en de MAC-adressen. De volledige adressen moet je zelf afleiden. PC1 kan PC3 niet bereiken.

Er zitten **3 fouten** in het netwerk.

## Documentatie (klopt)

### Prefixen
| Netwerk | Prefix |
|---|---|
| LAN1 (R1 G0/0 - PC1) | 2001:db8:2:1::/64 |
| Link R1-R2 (R1 G0/1 - R2 G0/1) | 2001:db8:2:12::/64 |
| Link R2-R3 (R2 G0/2 - R3 G0/1) | 2001:db8:2:23::/64 |
| LAN3 (R3 G0/0 - PC3) | 2001:db8:2:3::/64 |

### MAC-adressen
| Apparaat | Interface | MAC-adres |
|---|---|---|
| R1 | G0/0 | 50:54:00:12:01:01 |
| R1 | G0/1 | 50:54:00:12:01:02 |
| R2 | G0/1 | 50:54:00:12:02:01 |
| R2 | G0/2 | 50:54:00:12:02:02 |
| R3 | G0/1 | 50:54:00:12:03:01 |
| R3 | G0/0 | 50:54:00:12:03:02 |
| PC1 | Fa0 | 50:54:00:12:10:01 |
| PC3 | Fa0 | 50:54:00:12:30:01 |

### Routing (statisch)
- R1: default route naar het EUI-64-adres van R2 G0/1
- R2: route naar LAN1 via het EUI-64-adres van R1 G0/1, route naar LAN3 via het EUI-64-adres van R3 G0/1
- R3: default route naar het EUI-64-adres van R2 G0/2

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: **Desktop → Command Prompt** of **Desktop → IP Configuration**.

## Opdrachten

### Deel 1 - Adressen berekenen (op papier)
Bereken voor elke interface uit de MAC-tabel:
1. de modified EUI-64-interface-ID
2. het volledige global unicast-adres (prefix + interface-ID)
3. het link-local-adres

Noteer de stappen van één berekening volledig uit.

### Deel 2 - Baseline
Ping vanaf elke router naar het adres van de buurrouter op elke link, en van PC1 naar PC3. Noteer OK of FAIL.

### Deel 3 - Verkennen en vergelijken
Vergelijk de werkelijke adressen en routes met jouw berekening.
- Router: `show ipv6 interface brief`, `show ipv6 interface <int>`, `show interfaces <int>` (MAC-adres), `show ipv6 route`, `show ipv6 neighbors`, `show running-config`
- PC: `ipv6config /all`, `tracert <bestemming>`

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's.

### Deel 5 - Verificatie
- Alle buurrouters kunnen elkaar pingen op hun EUI-64-adres.
- Alle adressen komen overeen met jouw berekening.
- PC1 kan PC3 pingen.

### Deel 6 - Reflectie
1. Welke fouten heb je gevonden? Geef per fout het commando dat de doorslag gaf.
2. Welke fout was het lastigst te vinden, en waarom?
3. Werkte alles meteen nadat je één fout had opgelost, of kwam er daarna een nieuw probleem aan het licht? Leg uit.
4. Hoe had deze storing voorkomen kunnen worden?

## Documentatie

| # | Apparaat | Symptoom | Gevonden met | Oorzaak | Oplossing |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

Je docent bespreekt daarna nog een aantal verdiepingsvragen met je.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
