# Lab 1.3.6 - Geen internetverbinding

CCNA 200-301 v2.0 - leerdoel 1.3. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.3.6_geen_internetverbinding.pkt](1.3.6_geen_internetverbinding.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.3):** IPv4-adresconfiguratie, -toewijzing en subnetting troubleshooten.

## Scenario
Een bedrijf heeft een kantoornetwerk en een gastennetwerk. Router R1 verbindt beide met de provider (router ISP). Van de ISP heeft het bedrijf het blok **203.0.113.0/29** gekregen. R1 vertaalt de interne adressen via PAT (overload) naar het WAN-adres. De webserver SRV-WEB is via static NAT van buitenaf bereikbaar op een eigen adres.

Achter de ISP-router staat de server **INTERNET** met het adres **8.8.8.8**: gebruik dat als testadres voor het internet. INET-PC staat voor "een klant van dezelfde provider", waarmee je test of het bedrijf van buitenaf bereikbaar is.

Klachten:
- *Kantoor (PC1):* "We hebben geen internet."
- *Gastennetwerk (GUEST1):* "Ook geen internet."
- *Klant (INET-PC):* "Jullie website op 203.0.113.3 is onbereikbaar."

Intern werkt alles. Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IP-adres | Prefix | Opmerking |
|---|---|---|---|---|
| R1 | G0/0 (kantoor) | 172.20.10.1 | /24 | NAT inside |
| R1 | G0/2 (gasten) | 192.168.50.1 | /24 | NAT inside |
| R1 | G0/1 (WAN) | 203.0.113.2 | /29 | NAT outside, PAT-adres |
| ISP | G0/1 | 203.0.113.1 | /29 | gateway van R1 |
| ISP | G0/0 | 198.51.100.1 | /24 | klantennetwerk provider |
| ISP | G0/2 | 8.8.8.1 | /24 | naar INTERNET |
| PC1 | Fa0 | 172.20.10.11 | /24 | gateway 172.20.10.1 |
| SRV-WEB | Fa0 | 172.20.10.100 | /24 | gateway 172.20.10.1, publiek: **203.0.113.3** |
| GUEST1 | Fa0 | 192.168.50.20 | /24 | gateway 192.168.50.1 |
| INET-PC | Fa0 | 198.51.100.10 | /24 | gateway 198.51.100.1 |
| INTERNET | Fa0 | 8.8.8.8 | /24 | gateway 8.8.8.1 |

NAT-beleid: **alle** interne adressen van het bedrijf worden vertaald.

De router ISP en de server INTERNET zijn van de provider. Je mag ze bekijken, maar niet aanpassen.

## Toegang
- Routers en switch: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's en servers: klik op het apparaat en open **Desktop → Command Prompt** of **Desktop → IP Configuration**.

## Opdrachten

### Deel 1 - Documentatie analyseren
Bereken voor elk subnet uit het adresplan het netwerkadres, het bruikbare bereik en het broadcastadres. Geef bij elk subnet aan of het een privé- of een publiek bereik is.

### Deel 2 - Baseline
- Ping vanaf PC1, SRV-WEB en GUEST1 naar 8.8.8.8 en naar 198.51.100.10 (INET-PC).
- Ping vanaf INET-PC naar 203.0.113.2 en 203.0.113.3.
- Ping intern tussen PC1 en GUEST1.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan en het NAT-beleid.
- R1: `show ip interface brief`, `show running-config`, `show ip nat translations`, `show ip nat statistics`, `show access-lists`, `show ip route`
- Hosts: `ipconfig`, `tracert <bestemming>`

Tip: start een ping en bekijk direct daarna `show ip nat translations` op R1. Wordt het adres vertaald, en naar welk adres?

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's.

Handige commando's:
```
# R1
clear ip nat translation *
```
Op de PC's herstel je het adres, het masker en de gateway in **Desktop → IP Configuration**.

### Deel 5 - Verificatie
- PC1, SRV-WEB en GUEST1 kunnen 8.8.8.8 en 198.51.100.10 pingen.
- INET-PC kan 203.0.113.3 pingen.
- Alle adressen komen overeen met het adresplan.

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
