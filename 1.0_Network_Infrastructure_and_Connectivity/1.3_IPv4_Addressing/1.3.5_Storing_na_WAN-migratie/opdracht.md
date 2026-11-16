# Lab 1.3.5 - Storing na WAN-migratie

CCNA 200-301 v2.0 - leerdoel 1.3. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.3.5_storing_na_wan-migratie.pkt](1.3.5_storing_na_wan-migratie.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.3):** IPv4-adresconfiguratie, -toewijzing en subnetting troubleshooten.

## Scenario
Drie routers zijn in een keten met elkaar verbonden: R1 - R2 - R3. Om adressen te besparen zijn de WAN-links van /29 naar /30 gebracht en opnieuw geadresseerd volgens het adresplan hieronder. Na de migratie kan PC1 PC3 niet meer bereiken.

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IP-adres | Prefix | Verbonden met |
|---|---|---|---|---|
| R1 | G0/0 | 192.168.1.1 | /24 | PC1 |
| R1 | G0/1 | 10.0.0.1 | /30 | R2 G0/1 |
| R2 | G0/1 | 10.0.0.2 | /30 | R1 G0/1 |
| R2 | G0/2 | 10.0.0.5 | /30 | R3 G0/1 |
| R3 | G0/1 | 10.0.0.6 | /30 | R2 G0/2 |
| R3 | G0/0 | 192.168.3.1 | /24 | PC3 |
| PC1 | Fa0 | 192.168.1.10 | /24 | gateway 192.168.1.1 |
| PC3 | Fa0 | 192.168.3.10 | /24 | gateway 192.168.3.1 |

Routing (statisch):
- R1: default route naar R2
- R2: route naar 192.168.1.0/24 via R1 en naar 192.168.3.0/24 via R3
- R3: default route naar R2

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: klik op de pc en open **Desktop → Command Prompt** of **Desktop → IP Configuration**.

## Opdrachten

### Deel 1 - Documentatie analyseren
Bereken voor elk subnet uit het adresplan het subnetmasker, het netwerkadres, de bruikbare adressen en het broadcastadres.

### Deel 2 - Baseline
Ping vanaf elke router naar het adres van de buurrouter op elke link, en ping van PC1 naar PC3. Noteer OK of FAIL.

Tip: de eerste ping naar een nieuw adres kan één time-out geven terwijl ARP het MAC-adres opzoekt. Kijk naar het totaal.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan.
- `show ip interface brief`
- `show running-config`
- `show ip route`
- `show ip route static`

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's. Let op de meldingen die IOS geeft als je een commando invoert.

### Deel 5 - Verificatie
- Alle buurrouters kunnen elkaar pingen.
- `show ip route` op elke router toont routes naar beide LAN's (direct of statisch).
- PC1 kan PC3 pingen en `tracert 192.168.3.10` loopt via 192.168.1.1, 10.0.0.2 en 10.0.0.6.

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
