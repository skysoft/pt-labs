# Lab 1.4.7 - Adresplan nieuwe vestiging

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.7_adresplan_nieuwe_vestiging.pkt](1.4.7_adresplan_nieuwe_vestiging.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten.

## Scenario
Voor een nieuwe vestiging heeft het bedrijf het prefix **2001:db8:acad::/48** gekregen. Een collega heeft het netwerk op basis van de ontwerpregels hieronder opgebouwd, maar er is geen adresplan achtergelaten. PC1, PC2 en PC3 kunnen elkaar niet allemaal bereiken.

Er zitten **3 fouten** in het netwerk.

## Ontwerpregels
1. Elk LAN krijgt een **/64**. Het LAN achter router Rn gebruikt subnet-ID **n0** (hexadecimaal), dus R1 krijgt 2001:db8:acad:10::/64, R2 :20:, enzovoort.
2. De router krijgt op elk LAN interface-ID **::1**. De PC's krijgen hun adres via SLAAC.
3. Point-to-point-links tussen routers krijgen een **/127** (RFC 6164) uit 2001:db8:acad:ffff::/64. Begin bij ::2 en ga telkens naar de volgende vrije /127.
4. Op een link krijgt de router met het laagste nummer het laagste adres.

## Topologie
| Link | Interfaces |
|---|---|
| LAN R1 | R1 G0/0 - PC1 |
| LAN R2 | R2 G0/0 - PC2 |
| LAN R3 | R3 G0/0 - PC3 |
| Link 1 | R1 G0/1 - R2 G0/1 |
| Link 2 | R2 G0/2 - R3 G0/1 |

Routing (statisch): R1 en R3 hebben een default route naar R2. R2 heeft routes naar de LAN's van R1 en R3.

## Toegang
- Routers: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: **Desktop → Command Prompt** of **Desktop → IP Configuration** (IPv6 staat op **Automatic**).
- Wacht na het openen van het bestand ongeveer 20 seconden: zo lang duurt het voordat de PC's hun SLAAC-adres hebben.
- Heb je iets aangepast op een router, zet dan de PC in dat LAN even uit en weer aan (tabblad **Physical**, aan/uit-knop) en wacht weer ongeveer 20 seconden.

## Opdrachten

### Deel 1 - Adresplan maken (op papier)
Maak op basis van de ontwerpregels het adresplan:

| Netwerk | Prefix | Adres(sen) routers |
|---|---|---|

Hoeveel /64-subnetten zitten er in een /48? En hoeveel /127-links passen er in één /64?

### Deel 2 - Baseline
Ping vanaf elke router naar de buurrouter(s), en tussen alle PC's. Noteer OK of FAIL.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met jouw adresplan.
- Router: `show ipv6 interface brief`, `show ipv6 interface <int>`, `show ipv6 route`, `show running-config`
- PC: `ipv6config /all`, `tracert <bestemming>`

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's.

### Deel 5 - Verificatie
- Alle routerinterfaces hebben het adres uit jouw plan.
- Alle PC's hebben een SLAAC-adres uit het juiste /64 en kunnen elkaar pingen.

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
