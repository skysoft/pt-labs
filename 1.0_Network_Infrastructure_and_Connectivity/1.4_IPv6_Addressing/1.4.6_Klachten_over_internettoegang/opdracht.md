# Lab 1.4.6 - Klachten over internettoegang

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.6_klachten_over_internettoegang.pkt](1.4.6_klachten_over_internettoegang.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten.

## Scenario
Een bedrijf is via router R1 met de provider (ISP) verbonden. Van de ISP heeft het bedrijf het IPv6-prefix **2001:db8:6::/48** gekregen. Bij IPv6 is geen NAT nodig: alle apparaten krijgen een globaal routeerbaar adres uit dit prefix. PC2 en PC3 gebruiken SLAAC; PC1 en de webserver hebben een statisch adres.

INET-PC staat voor "een willekeurige computer op het internet".

Klachten:
- *PC2 (LAN-B):* "Collega's in het bedrijf bereik ik wel, internet niet."
- *PC3 (LAN-C):* "Ik bereik helemaal niets."
- *Klant op internet (INET-PC):* "Jullie website op 2001:db8:6:a::80 is onbereikbaar."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IPv6-adres | Opmerking |
|---|---|---|---|
| R1 | G0/0 (LAN-A) | 2001:db8:6:a::1/64, link-local FE80::1 | |
| R1 | G0/2.20 (LAN-B, VLAN 20) | 2001:db8:6:b::1/64 | SLAAC |
| R1 | G0/2.30 (LAN-C, VLAN 30) | 2001:db8:6:c::1/64 | SLAAC |
| R1 | G0/1 (WAN) | 2001:db8:ffff:6::2/64 | |
| ISP | G0/1 | 2001:db8:ffff:6::1/64 | gateway van R1 |
| ISP | G0/0 | 2001:db8:ffff:100::1/64 | "internet" |
| PC1 | Fa0 | 2001:db8:6:a::11/64 (statisch) | gateway FE80::1 |
| SRV-WEB | Fa0 | 2001:db8:6:a::80/64 (statisch) | gateway FE80::1 |
| PC2 | Fa0 | via SLAAC (LAN-B) | |
| PC3 | Fa0 | via SLAAC (LAN-C) | |
| INET-PC | Fa0 | 2001:db8:ffff:100::10/64 | gateway 2001:db8:ffff:100::1 |

LAN-B en LAN-C lopen via switch SW-B over een trunk naar R1 (router-on-a-stick). De ISP routeert alleen 2001:db8:6::/48 naar het bedrijf. De ISP-router is van de provider: je mag hem bekijken, maar niet aanpassen.

## Toegang
- Routers en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Hosts: **Desktop → Command Prompt** of **Desktop → IP Configuration**.
- Wacht na het openen van het bestand ongeveer 20 seconden: zo lang duurt het voordat PC2 en PC3 hun SLAAC-adres hebben.
- Heb je iets aangepast op R1, zet dan PC2 of PC3 even uit en weer aan (tabblad **Physical**, aan/uit-knop) en wacht weer ongeveer 20 seconden.

## Opdrachten

### Deel 1 - Documentatie analyseren
Noem de belangrijkste soorten IPv6-unicastadressen (met hun prefix) en geef bij elk aan of het op internet gerouteerd wordt. Welke soort hoort volgens het adresplan op de LAN's?

### Deel 2 - Baseline
- Ping vanaf PC1, PC2, PC3 en SRV-WEB naar 2001:db8:ffff:100::10 (INET-PC).
- Ping vanaf INET-PC naar 2001:db8:6:a::80.
- Ping intern tussen PC1, PC2 en PC3.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan. Classificeer elk adres dat je tegenkomt.
- Router: `show ipv6 interface brief`, `show ipv6 interface <int>`, `show ipv6 route`, `show running-config`
- Host: `ipv6config /all`, `tracert <bestemming>`

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's. Een statisch adres op een host pas je aan in IP Configuration.

### Deel 5 - Verificatie
- PC1, PC2, PC3 en SRV-WEB kunnen 2001:db8:ffff:100::10 pingen.
- INET-PC kan 2001:db8:6:a::80 pingen.
- Alle adressen vallen binnen 2001:db8:6::/48.

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
