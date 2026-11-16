# Lab 1.3.1 - Storing kantoor en magazijn

CCNA 200-301 v2.0 - leerdoel 1.3. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.3.1_storing_kantoor_en_magazijn.pkt](1.3.1_storing_kantoor_en_magazijn.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.3):** IPv4-adresconfiguratie, -toewijzing en subnetting troubleshooten.

## Scenario
Je werkt bij de servicedesk van een klein bedrijf met een kantoor (LAN-A) en een magazijn (LAN-B). De twee locaties zijn met elkaar verbonden via een WAN-link tussen R1 en R2.

Sinds een collega "even snel" wat wijzigingen heeft gedaan, komen deze klachten binnen:
- *PC1:* "Ik kan de printer in het magazijn (PC3) niet meer bereiken."
- *PC2:* "Alles in het kantoor werkt, maar het magazijn is onbereikbaar."
- *PC4:* "Ik kan helemaal niets meer bereiken."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IP-adres | Masker | Gateway |
|---|---|---|---|---|
| R1 | G0/0 (LAN-A) | 192.168.10.1 | /26 (255.255.255.192) | - |
| R1 | G0/1 (WAN) | 10.0.12.1 | /30 | - |
| R2 | G0/0 (LAN-B) | 192.168.10.65 | /26 (255.255.255.192) | - |
| R2 | G0/1 (WAN) | 10.0.12.2 | /30 | - |
| PC1 | Fa0 | 192.168.10.10 | /26 | 192.168.10.1 |
| PC2 | Fa0 | 192.168.10.40 | /26 | 192.168.10.1 |
| PC3 | Fa0 | 192.168.10.70 | /26 | 192.168.10.65 |
| PC4 | Fa0 | 192.168.10.100 | /26 | 192.168.10.65 |

Routing: statische routes. R1 heeft een route naar LAN-B, R2 heeft een route naar LAN-A.
De switches SW-A en SW-B hebben geen configuratie en doen niet mee aan de storing.

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: klik op de pc en open **Desktop → Command Prompt** of **Desktop → IP Configuration**.

## Opdrachten

### Deel 1 - Documentatie analyseren
Bereken voor elk subnet uit het adresplan het netwerkadres, het eerste en laatste bruikbare adres, het broadcastadres en het aantal hosts.

### Deel 2 - Baseline-connectiviteit
Maak een pingmatrix: ping vanaf elke PC naar elke andere PC en naar alle gateways. Noteer per combinatie OK of FAIL.

Tip: de eerste ping naar een nieuw adres kan één time-out geven terwijl ARP het MAC-adres opzoekt. Kijk naar het totaal.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan.
- Router: `show ip interface brief`, `show running-config`, `show ip route`, `show ip arp`
- PC: `ipconfig`, `ipconfig /all`, `arp -a`, `tracert <bestemming>`

### Deel 4 - Fouten vinden en herstellen
Noteer voor elke fout:
1. het symptoom (wat werkt niet?)
2. het apparaat en het commando waarmee je de fout vond
3. de oorzaak
4. het herstel

Op de PC's herstel je het adres, het masker en de gateway in **Desktop → IP Configuration**.

### Deel 5 - Verificatie
Herhaal de pingmatrix. Alle pings moeten nu slagen.

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
