# Lab 1.4.3 - Nieuwe werkplekken zonder adres

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.3_nieuwe_werkplekken_zonder_adres.pkt](1.4.3_nieuwe_werkplekken_zonder_adres.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten.

## Scenario
Een bedrijf heeft drie afdelingen: LAN-A en LAN-B bij router R1, en LAN-C bij router R2. Alle PC's configureren zichzelf automatisch via **SLAAC**. Er is niets handmatig ingesteld op de PC's, en er is geen DHCPv6-server.

Na het aansluiten van nieuwe werkplekken komen deze klachten binnen:
- *PC1 en PC2 (LAN-A):* "We hebben geen netwerk, alleen elkaar kunnen we soms nog bereiken."
- *PC3 (LAN-B):* "Ik heb geen IPv6-adres dat met 2001 begint."
- *PC4 (LAN-C):* "Ik bereik niemand."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IPv6-adres | Opmerking |
|---|---|---|---|
| R1 | G0/0 (LAN-A) | 2001:db8:3:a::1/64 | SLAAC voor PC1, PC2 |
| R1 | G0/1 (LAN-B) | 2001:db8:3:b::1/64 | SLAAC voor PC3 |
| R1 | G0/2 (link) | 2001:db8:3:ff::1/64 | |
| R2 | G0/2 (link) | 2001:db8:3:ff::2/64 | |
| R2 | G0/0 (LAN-C) | 2001:db8:3:c::1/64 | SLAAC voor PC4 |
| PC1 t/m PC4 | Fa0 | via SLAAC | gateway via Router Advertisement |

Routing: R1 heeft een statische route naar LAN-C, R2 heeft een default route naar R1. Beide routers routeren IPv6.

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: **Desktop → Command Prompt** of **Desktop → IP Configuration** (deel IPv6 Configuration, staat op **Automatic**).
- Heb je iets aangepast op een router, laat de PC dan opnieuw beginnen: zet in IP Configuration IPv6 even op **Static** en daarna weer op **Automatic**.

## Opdrachten

### Deel 1 - Documentatie analyseren
Beschrijf voor elk LAN welk adres een PC via SLAAC verwacht te krijgen: welk prefix en welke prefixlengte, en waar de interface-ID vandaan komt. Welke informatie heeft een PC daarvoor nodig, en van wie krijgt de PC die?

### Deel 2 - Symptomen
Noteer op elke PC de uitvoer van `ipv6config`. Ping daarna vanaf elke PC naar alle routeradressen uit het adresplan.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan.
- Router: `show ipv6 interface brief`, `show ipv6 interface <int>`, `show ipv6 route`, `show running-config`
- PC: `ipv6config /all`

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's.

### Deel 5 - Verificatie
- Elke PC heeft een global unicast-adres uit het juiste prefix en een default gateway.
- Alle PC's kunnen elkaar pingen.

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
