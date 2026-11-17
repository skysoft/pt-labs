# Lab 1.4.9 - Eindlab Drukkerij Het Blad

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.9_eindlab_drukkerij_het_blad.pkt](1.4.9_eindlab_drukkerij_het_blad.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten. Dit is het integrale eindlab van leerdoel 1.4.

## Scenario
Je begint als netwerkbeheerder bij *Drukkerij Het Blad*. Het bedrijf is net volledig overgestapt op IPv6, maar de installateur heeft het werk slordig opgeleverd. Het **ontwerpdocument** hieronder klopt. Het netwerk wijkt er op meerdere plaatsen van af.

Je opdracht: **laat het netwerk exact overeenkomen met het ontwerp**, zodat alle eindapparaten elkaar en het internet kunnen bereiken.

Er zitten **7 fouten** in het netwerk.

## Ontwerpdocument (klopt)

Bedrijfsprefix van de ISP: **2001:db8:c0de::/48**

### Hoofdkantoor (HQ)
| Netwerk | VLAN | Prefix | Gateway | Adrestoewijzing |
|---|---|---|---|---|
| STAFF | 10 | 2001:db8:c0de:10::/64 | R-HQ G0/0.10 - ::1, link-local FE80::1 | SLAAC |
| SERVERS | 20 | 2001:db8:c0de:20::/64 | R-HQ G0/0.20 - ::1, link-local FE80::1 | statisch |

### Filiaal (BR)
| Netwerk | Prefix | Gateway | Adrestoewijzing |
|---|---|---|---|
| BRANCH-LAN | 2001:db8:c0de:30::/64 | R-BR G0/0 - ::1, link-local FE80::1 | SLAAC, printer statisch |

### Verbindingen
| Link | Prefix | Adressen |
|---|---|---|
| HQ-BR | 2001:db8:c0de:ffff::2/127 | R-HQ G0/1 ::2 - R-BR G0/1 ::3 |
| Internet | 2001:db8:ffff:c0::/64 (van de ISP) | ISP G0/1 ::1 (link-local FE80::1) - R-HQ G0/2 ::2 |

### Eindapparaten
| Apparaat | Locatie | IPv6-adres | Gateway |
|---|---|---|---|
| HQ-PC1 | VLAN 10 | SLAAC | via RA |
| HQ-SRV | VLAN 20 | 2001:db8:c0de:20::10/64 | FE80::1 |
| BR-PC1 | filiaal | SLAAC | via RA |
| BR-PRN (printer) | filiaal | 2001:db8:c0de:30::100/64 | FE80::1 |
| INET-PC | "internet" | 2001:db8:ffff:100::10/64 | 2001:db8:ffff:100::1 |

### Routing
- R-HQ: default route via G0/2 naar de link-local FE80::1 van de ISP, en een statische route naar BRANCH-LAN via R-BR (::3).
- R-BR: default route naar R-HQ (::2).
- Beide bedrijfsrouters routeren IPv6.

De ISP-router is van de provider. Je mag hem bekijken, maar niet aanpassen. De switches SW-HQ en SW-BR zijn correct.

## Toegang
- Routers en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's en server: **Desktop → Command Prompt** of **Desktop → IP Configuration**.
- Printer BR-PRN: tabblad **Config → FastEthernet0** (IPv6-deel). De printer heeft geen Command Prompt: test hem door hem vanaf andere apparaten te pingen.
- Wacht na het openen van het bestand ongeveer 20 seconden: zo lang duurt het voordat de SLAAC-hosts hun adres hebben.
- Heb je iets aangepast op een router, zet dan HQ-PC1 of BR-PC1 even uit en weer aan (tabblad **Physical**, aan/uit-knop) en wacht weer ongeveer 20 seconden.

## Opdrachten

### Deel 1 - Plan van aanpak
Kies een methode (bottom-up, top-down, divide-and-conquer of follow-the-path) en leg in 3-5 zinnen uit waarom.

### Deel 2 - Baseline
Maak een pingmatrix tussen HQ-PC1, HQ-SRV, BR-PC1, BR-PRN en INET-PC, en naar alle gateways. Noteer OK of FAIL.

### Deel 3 - Documentatie vergelijken
Verzamel van elk apparaat de werkelijke IPv6-configuratie en zet die naast het ontwerpdocument. Markeer elke afwijking.

Handige commando's:
- IOS: `show ipv6 interface brief`, `show ipv6 interface <int>`, `show ipv6 route`, `show ipv6 neighbors`, `show running-config`
- Host: `ipv6config /all`, `tracert <bestemming>`

### Deel 4 - Fouten herstellen (foutenlog)
Houd een foutenlog bij:

| # | Apparaat | Symptoom | Gevonden met | Oorzaak | Herstel | Getest? |
|---|---|---|---|---|---|---|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |
| 6 | | | | | | |
| 7 | | | | | | |

Test na **elke** wijziging opnieuw.

### Deel 5 - Eindverificatie
- Alle eindapparaten kunnen elkaar pingen.
- HQ-PC1, HQ-SRV, BR-PC1 en BR-PRN zijn vanaf INET-PC (2001:db8:ffff:100::10) bereikbaar, en de PC's en de server kunnen INET-PC pingen.
- Alle adressen, prefixlengtes, link-local-adressen en routes komen overeen met het ontwerpdocument.

### Deel 6 - Reflectie
1. Welke fout vond je het lastigst en waarom?
2. Welke fouten verborgen een andere fout? Geef een voorbeeld.
3. Welke drie maatregelen zou je nemen om dit soort fouten in de toekomst te voorkomen?
4. Welke verschillen tussen IPv4- en IPv6-troubleshooting ben je in dit lab tegengekomen?

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
