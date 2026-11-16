# Lab 1.3.7 - Eindlab Bakkerij De Korrel

CCNA 200-301 v2.0 - leerdoel 1.3. Integraal eindlab: laat het netwerk exact overeenkomen met het ontwerpdocument.

Packet Tracer-bestand: [1.3.7_eindlab_bakkerij_de_korrel.pkt](1.3.7_eindlab_bakkerij_de_korrel.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.3):** IPv4-adresconfiguratie, -toewijzing en subnetting troubleshooten. Dit is het integrale eindlab van leerdoel 1.3.

## Scenario
Je begint als nieuwe netwerkbeheerder bij *Bakkerij De Korrel*. De vorige beheerder heeft het netwerk half afgemaakt opgeleverd. Er is gelukkig een correct **ontwerpdocument** (zie hieronder), maar het netwerk zelf wijkt er op meerdere plaatsen van af.

Je opdracht: **laat het netwerk exact overeenkomen met het ontwerp**, zodat alle eindapparaten elkaar en het internet kunnen bereiken.

Er zitten **7 fouten** in het netwerk.

## Ontwerpdocument (klopt)

### Hoofdkantoor (HQ)
| Netwerk | VLAN | Subnet | Gateway | Opmerking |
|---|---|---|---|---|
| STAFF | 10 | 10.20.10.0/24 | R-HQ G0/0.10 - 10.20.10.1 | DHCP (.50-.254) |
| SERVERS | 20 | 10.20.20.0/26 | R-HQ G0/0.20 - 10.20.20.1 | statisch |

### Filiaal (BR)
| Netwerk | Subnet | Gateway | Opmerking |
|---|---|---|---|
| BRANCH-LAN | 10.20.30.0/25 | R-BR G0/0 - 10.20.30.1 | DHCP via relay naar R-HQ (.50-.126, excl. .100) |

### Verbindingen
| Link | Subnet | Adressen |
|---|---|---|
| WAN HQ-BR | 10.255.0.0/30 | R-HQ G0/1 .1 - R-BR G0/1 .2 |
| Provider | 203.0.113.0/29 (publiek, van ISP) | ISP G0/1 .1 - R-HQ G0/2 .2 (PAT) |

Achter de ISP-router staat de server **INTERNET** met het adres **8.8.8.8**: gebruik dat als testadres.

### Eindapparaten
| Apparaat | Locatie | IP-adres | Gateway |
|---|---|---|---|
| HQ-PC1 | VLAN 10 (SW-HQ Fa0/1) | DHCP | via DHCP |
| HQ-SRV | VLAN 20 (SW-HQ Fa0/2) | 10.20.20.10/26 | 10.20.20.1 |
| BR-PC1 | filiaal (SW-BR Fa0/1) | DHCP | via DHCP |
| BR-PRN (printer) | filiaal (SW-BR Fa0/2) | 10.20.30.100/25 | 10.20.30.1 |
| INET-PC | klant van de provider | 198.51.100.10/24 | 198.51.100.1 |

### Routing en NAT
- R-HQ: default route naar de ISP (203.0.113.1) en een statische route naar het filiaal-LAN via R-BR.
- R-BR: default route naar R-HQ.
- R-HQ doet PAT (overload) op G0/2 voor **alle** interne adressen in 10.20.0.0/16.
- R-HQ is de DHCP-server voor STAFF en BRANCH-LAN.

De router ISP en de server INTERNET zijn van de provider. Je mag ze bekijken, maar niet aanpassen.

## Toegang
- Routers en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's en server: klik op het apparaat en open **Desktop → Command Prompt** of **Desktop → IP Configuration**.
- De printer BR-PRN heeft geen Command Prompt. Je stelt hem in via het tabblad **Config → FastEthernet0** (adres en masker) en **Config → Settings** (gateway). Test hem door hem vanaf een ander apparaat te pingen.
- Een DHCP-client vraagt een nieuw adres aan met `ipconfig /renew`. Krijg je `DHCP request failed`, probeer het dan nog een keer: de eerste aanvraag na een herstel kan mislukken.

## Opdrachten

### Deel 1 - Plan van aanpak
Kies een methode (bottom-up, top-down, divide-and-conquer of follow-the-path) en leg in 3-5 zinnen uit waarom.

### Deel 2 - Baseline
Maak een pingmatrix tussen HQ-PC1, HQ-SRV, BR-PC1, BR-PRN en INET-PC, naar alle gateways en naar 8.8.8.8. Noteer OK of FAIL.

### Deel 3 - Documentatie vergelijken
Verzamel van elk apparaat de werkelijke IPv4-configuratie en zet die naast het ontwerpdocument. Markeer elke afwijking.

Handige commando's:
- IOS: `show ip interface brief`, `show running-config`, `show ip route`, `show ip dhcp binding`, `show ip dhcp pool`, `show ip nat translations`, `show ip nat statistics`, `show access-lists`, `show vlan brief`, `show interfaces trunk`
- PC: `ipconfig`, `ipconfig /all`, `arp -a`, `tracert`

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
- HQ-PC1, HQ-SRV en BR-PC1 kunnen 8.8.8.8 en INET-PC (198.51.100.10) pingen.
- HQ-PC1 en BR-PC1 hebben een lease uit het juiste bereik.
- De hele configuratie komt overeen met het ontwerpdocument.

### Deel 6 - Reflectie
1. Welke fout vond je het lastigst en waarom?
2. Welke fouten verborgen een andere fout? Geef een voorbeeld.
3. Welke drie maatregelen zou je nemen om dit soort fouten in de toekomst te voorkomen?
4. Hoe kan een AI-assistent helpen bij dit soort troubleshooting, en waar moet je zelf kritisch blijven? (koppeling met leerdoel 5.1/5.2)

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
