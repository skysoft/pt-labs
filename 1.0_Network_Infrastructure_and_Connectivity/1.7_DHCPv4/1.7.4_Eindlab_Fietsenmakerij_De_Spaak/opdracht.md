# Lab 1.7.4 - Eindlab Fietsenmakerij De Spaak

CCNA 200-301 v2.0 - leerdoel 1.7. Integraal eindlab: laat het netwerk exact overeenkomen met het ontwerpdocument.

Packet Tracer-bestand: [1.7.4_eindlab_fietsenmakerij_de_spaak.pkt](1.7.4_eindlab_fietsenmakerij_de_spaak.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.7):** DHCPv4-client, -server en -relay op IOS-apparaten troubleshooten. Dit is het integrale eindlab van leerdoel 1.7.

## Scenario
*Fietsenmakerij De Spaak* heeft een hoofdvestiging (HQ) en een werkplaats (filiaal). Alle adressen worden via DHCP uitgedeeld: de provider geeft de bedrijfsrouter EDGE zijn WAN-adres, en EDGE is DHCP-server voor het hele bedrijf. Het filiaal gebruikt een DHCP-relay. Ook de beheerinterface van de switch haalt zijn adres via DHCP.

Achter de ISP-router staat de server **INTERNET** (8.8.8.8): gebruik dat als testadres. In dit lab beheer je ook de DHCP-server van de provider.

Niets werkt zoals het hoort. Je opdracht: **laat het netwerk exact overeenkomen met het ontwerpdocument**.

Er zitten **5 fouten** in het netwerk.

## Ontwerpdocument (klopt)

### Provider
| Apparaat | Interface | IP-adres | Opmerking |
|---|---|---|---|
| ISP | G0/1 | 100.64.74.1/29 | DHCP-server voor EDGE: bereik .2-.6, gateway .1, DNS 8.8.8.8 |
| ISP | G0/2 | 8.8.8.1/24 | naar INTERNET |

### EDGE (bedrijfsrouter, DHCP-server, PAT)
| Interface | IP-adres | Netwerk | DHCP-pool (bereik .100-.254) |
|---|---|---|---|
| G0/2 (WAN) | via DHCP van de ISP | - | - |
| G0/0.10 | 10.74.10.1/24 | VLAN 10 STAFF | STAFF, gateway 10.74.10.1 |
| G0/0.99 | 10.74.99.1/24 | VLAN 99 BEHEER | BEHEER, gateway 10.74.99.1 |
| G0/1 | 10.74.255.1/30 | link naar R-BR | - |
| - | - | 10.74.30.0/24 (filiaal) | FILIAAL, gateway 10.74.30.1 |

EDGE: de default route komt uit DHCP. Voor het filiaal is er een statische route via R-BR. PAT voor alle adressen in 10.74.0.0/16. Beveiligingsbeleid: proxy-ARP staat uit op de subinterfaces (`no ip proxy-arp`); dat is geen fout.

### Filiaal
| Apparaat | Interface | IP-adres | Opmerking |
|---|---|---|---|
| R-BR | G0/1 | 10.74.255.2/30 | default route naar EDGE |
| R-BR | G0/0 | 10.74.30.1/24 | DHCP-relay naar 10.74.255.1 |

### Switch en eindapparaten
| Apparaat | Interface | Adres | VLAN |
|---|---|---|---|
| SW-HQ | Gi0/1 | trunk naar EDGE, VLAN 10 en 99 | - |
| SW-HQ | Vlan99 (beheer) | via DHCP | 99 |
| HQ-PC1 | Fa0 (SW-HQ Fa0/1) | via DHCP | 10 |
| BR-PC1 | Fa0 (SW-BR Fa0/1) | via DHCP | filiaal |

## Toegang
- Routers en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: **Desktop → Command Prompt**. Een nieuw adres aanvragen: `ipconfig /renew`. Krijg je `DHCP request failed`, probeer het dan nog een keer.
- Een IOS-client (EDGE G0/2, SW-HQ Vlan99) opnieuw laten aanvragen: `shutdown` / `no shutdown` op de interface.
- Eigenaardigheid van Packet Tracer: `ip address dhcp` op een VLAN-interface van een switch blijft alleen in de configuratie staan als die VLAN-interface **up/up** is.

## Opdrachten

### Deel 1 - Plan van aanpak
Kies een methode (bottom-up, top-down, divide-and-conquer of follow-the-path) en leg in 3-5 zinnen uit waarom.

### Deel 2 - Baseline
Noteer van elke DHCP-client (EDGE G0/2, SW-HQ Vlan99, HQ-PC1, BR-PC1) het adres, het masker en de gateway. Ping vanaf de PC's naar hun gateway, naar elkaar en naar 8.8.8.8.

### Deel 3 - Documentatie vergelijken
Vergelijk de werkelijke configuratie met het ontwerpdocument en markeer elke afwijking.

Handige commando's:
- IOS-server: `show ip dhcp pool`, `show ip dhcp binding`
- IOS-client: `show dhcp lease`, `show ip interface brief`, `show ip route`
- Relay: `show ip interface <int> | include Helper`
- Switch: `show interfaces trunk`, `show vlan brief`

### Deel 4 - Fouten herstellen (foutenlog)
| # | Apparaat | Symptoom | Gevonden met | Oorzaak | Herstel | Getest? |
|---|---|---|---|---|---|---|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |

Test na **elke** wijziging opnieuw.

### Deel 5 - Eindverificatie
- Alle DHCP-clients hebben een adres uit het juiste bereik met de juiste gateway.
- HQ-PC1 en BR-PC1 pingen elkaar, SW-HQ (Vlan99) en 8.8.8.8.

### Deel 6 - Reflectie
1. Welke fout vond je het lastigst en waarom?
2. Welke fouten verborgen een andere fout? Geef een voorbeeld.
3. Welke drie maatregelen zou je nemen om dit soort fouten in de toekomst te voorkomen?
4. Welke DHCP-problemen zie je aan de client en welke alleen aan de server of relay?

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
