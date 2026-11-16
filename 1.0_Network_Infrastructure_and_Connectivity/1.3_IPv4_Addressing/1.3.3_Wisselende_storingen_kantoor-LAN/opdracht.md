# Lab 1.3.3 - Wisselende storingen kantoor-LAN

CCNA 200-301 v2.0 - leerdoel 1.3. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.3.3_wisselende_storingen_kantoor-lan.pkt](1.3.3_wisselende_storingen_kantoor-lan.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.3):** IPv4-adresconfiguratie, -toewijzing en subnetting troubleshooten.

## Scenario
Het kantoor-LAN 192.168.30.0/24 hangt aan router R1. R1 is via G0/1 verbonden met de internetprovider (router ISP) en vertaalt het LAN via PAT naar zijn internetadres. Werkplekken en servers hebben een statisch adres, gasten en nieuwe laptops krijgen hun adres via DHCP van R1. Gebruik **8.8.8.8** als testadres voor het internet.

Sinds vorige week komen er vreemde, wisselende klachten binnen:
- *PC1:* "Het internet werkt soms wel en soms niet."
- *PC2:* "Mijn verbinding valt steeds weg. Soms antwoordt er wel iets, soms niet."
- *SRV1-beheerder:* "Sinds die nieuwe laptop (PC5) is aangesloten, klopt er iets niet met de adressen. Op R1 staan meldingen over mijn serveradres."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IP-adres | Masker | Gateway |
|---|---|---|---|---|
| R1 | G0/0 (LAN) | 192.168.30.1 | /24 | - |
| R1 | G0/1 (internet) | via DHCP van de provider | - | via DHCP |
| SW1 | Vlan1 (beheer) | 192.168.30.2 | /24 | 192.168.30.1 |
| PC1 | Fa0 | 192.168.30.11 | /24 | 192.168.30.1 |
| PC2 | Fa0 | 192.168.30.12 | /24 | 192.168.30.1 |
| PC3 | Fa0 | 192.168.30.13 | /24 | 192.168.30.1 |
| PC4 | Fa0 | 192.168.30.14 | /24 | 192.168.30.1 |
| SRV1 | Fa0 | 192.168.30.50 | /24 | 192.168.30.1 |
| PC5 | Fa0 | DHCP | /24 | via DHCP |

Afspraak voor DHCP: R1 deelt **alleen .100 t/m .200** uit. Alle andere adressen zijn gereserveerd voor statische apparaten.

De internetverbinding van R1 (G0/1, PAT) is correct geconfigureerd. De router **ISP** en de server **INTERNET** (8.8.8.8) horen bij de provider: daar hoef je niets aan te veranderen.

## Toegang
- R1 en SW1: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's en server: klik op het apparaat en open **Desktop → Command Prompt** of **Desktop → IP Configuration**.

## Opdrachten

### Deel 1 - Documentatie analyseren
Bereken voor het LAN het netwerkadres, het bruikbare bereik en het broadcastadres. Welke adressen zijn volgens de afspraken statisch, en welke dynamisch?

### Deel 2 - Symptomen verzamelen
Ping vanaf elke host naar 8.8.8.8, 192.168.30.1, SRV1 en de andere hosts, met `ping -n 10`. Noteer per test het percentage packet loss. Let ook op de **TTL** in de antwoorden.

### Deel 3 - Verkennen en vergelijken
Maak een tabel met per host het IP-adres, het MAC-adres en de switchpoort, en vergelijk die met het adresplan.
- Host: `ipconfig`, `ipconfig /all`, `arp -a`
- R1: `show ip interface brief`, `show running-config`, `show ip arp`, `show logging`, `show ip dhcp binding`, `show ip dhcp conflict`, `show ip nat translations`, `show interfaces GigabitEthernet0/0`
- SW1: `show mac address-table`, `show interfaces status`

Tip: de MAC-adrestabel van SW1 raakt na een paar minuten zonder verkeer leeg. Ping eerst vanaf de hosts, en kijk dan meteen.

### Deel 4 - Fouten vinden en herstellen
Noteer voor elke fout:
1. het symptoom
2. het apparaat en het commando waarmee je de fout vond
3. de oorzaak
4. het herstel

Handige commando's:
```
# PC of server (Command Prompt)
arp -d                        ARP-cache legen
ipconfig /release             DHCP-adres teruggeven
ipconfig /renew               nieuw DHCP-adres aanvragen
# R1
clear arp-cache
clear ip dhcp conflict *
```
Op de PC's en de server herstel je het adres, het masker en de gateway in **Desktop → IP Configuration**.

### Deel 5 - Verificatie
- `ping -n 10 8.8.8.8` vanaf elke host geeft 0% packet loss (na de eerste ping, die door ARP een time-out kan geven).
- `show ip dhcp conflict` op R1 is leeg en `show logging` toont geen nieuwe conflictmeldingen.
- Alle adressen komen overeen met het adresplan en de DHCP-afspraken.

### Deel 6 - Reflectie
1. Welke fouten heb je gevonden? Geef per fout het commando dat de doorslag gaf.
2. Welke fout was het lastigst te vinden, en waarom?
3. Waarom waren de storingen in dit lab wisselend in plaats van constant?
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
