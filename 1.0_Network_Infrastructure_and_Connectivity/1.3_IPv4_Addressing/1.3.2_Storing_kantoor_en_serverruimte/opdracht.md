# Lab 1.3.2 - Storing kantoor en serverruimte

CCNA 200-301 v2.0 - leerdoel 1.3. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.3.2_storing_kantoor_en_serverruimte.pkt](1.3.2_storing_kantoor_en_serverruimte.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.3):** IPv4-adresconfiguratie, -toewijzing en subnetting troubleshooten.

## Scenario
Je werkt bij de servicedesk van een klein bedrijf met twee locaties: het kantoor (LAN-A) en de serverruimte (LAN-B). De locaties zijn met elkaar verbonden via een WAN-link tussen R1 en R2. Na een verhuizing van een aantal werkplekken komen deze klachten binnen:
- *PC2:* "Ik kan server SRV1 niet bereiken, maar mijn collega op PC1 wel."
- *PC3:* "Bij mij werkt SRV1 ook niet. Zelfs de router aan de andere kant (R2) reageert niet."
- *PC4:* "Ik kan SRV1 naast me gewoon bereiken, maar niets op kantoor."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IP-adres | Masker | Gateway |
|---|---|---|---|---|
| R1 | G0/0 (LAN-A) | 172.16.1.1 | /24 | - |
| R1 | G0/1 (WAN) | 10.0.12.1 | /30 | - |
| R2 | G0/0 (LAN-B) | 172.16.2.1 | /24 | - |
| R2 | G0/1 (WAN) | 10.0.12.2 | /30 | - |
| PC1 | Fa0 | 172.16.1.11 | /24 | 172.16.1.1 |
| PC2 | Fa0 | 172.16.1.12 | /24 | 172.16.1.1 |
| PC3 | Fa0 | 172.16.1.13 | /24 | 172.16.1.1 |
| SRV1 | Fa0 | 172.16.2.10 | /24 | 172.16.2.1 |
| PC4 | Fa0 | 172.16.2.20 | /24 | 172.16.2.1 |

Routing: statische routes. R1 heeft een route naar LAN-B, R2 heeft een route naar LAN-A.
De switches SW-A en SW-B hebben geen configuratie en doen niet mee aan de storing.

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's en server: klik op het apparaat en open **Desktop → Command Prompt** of **Desktop → IP Configuration**.

## Opdrachten

### Deel 1 - Documentatie analyseren
Bereken voor elk subnet uit het adresplan het netwerkadres, het eerste en laatste bruikbare adres en het broadcastadres. Controleer of alle adressen in het plan logisch in hun subnet vallen.

### Deel 2 - Baseline-connectiviteit
Maak een pingmatrix: ping vanaf elke PC en SRV1 naar alle andere eindapparaten en naar 172.16.1.1, 172.16.2.1 en 10.0.12.1. Noteer per combinatie OK of FAIL.

Tip: de eerste ping naar een nieuw adres kan één of twee time-outs geven terwijl ARP de MAC-adressen opzoekt. Kijk naar het totaal, of ping nog een keer.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan.
- PC/server: `ipconfig`, `ipconfig /all`, `arp -a`, `tracert <bestemming>`
- Router: `show ip interface brief`, `show running-config`, `show ip route`, `show ip arp`, `show interfaces`

### Deel 4 - Fouten vinden en herstellen
Noteer voor elke fout:
1. het symptoom (wat werkt niet?)
2. het apparaat en het commando waarmee je de fout vond
3. de oorzaak
4. het herstel

Op de PC's en de server herstel je het adres, het masker en de gateway in **Desktop → IP Configuration**.

### Deel 5 - Verificatie
Herhaal de pingmatrix en voer vanaf PC2 een `tracert 172.16.2.10` uit. Alle pings moeten slagen en de traceroute moet via 172.16.1.1 en 10.0.12.2 lopen.

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
