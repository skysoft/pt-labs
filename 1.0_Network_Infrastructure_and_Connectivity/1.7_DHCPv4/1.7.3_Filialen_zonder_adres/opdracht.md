# Lab 1.7.3 - Filialen zonder adres

CCNA 200-301 v2.0 - leerdoel 1.7. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met het ontwerp.

Packet Tracer-bestand: [1.7.3_filialen_zonder_adres.pkt](1.7.3_filialen_zonder_adres.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.7):** DHCPv4-client, -server en -relay op IOS-apparaten troubleshooten.

## Scenario
Een bedrijf beheert DHCP centraal: router R-DC in het datacenter is de **DHCP-server** voor alle filialen. Filiaal 1 zit direct aan R-DC, filiaal 2 zit twee routers verder (via R-MID). De filiaalrouters sturen DHCP-verzoeken door als **DHCP-relay**.

Klachten:
- *PC-B1 (filiaal 1):* "Ik heb een adres dat met 169.254 begint."
- *PC-B2 (filiaal 2):* "Ik ook."

Er zitten **3 fouten** in het netwerk.

## Ontwerp (klopt)
| Apparaat | Interface | IP-adres | Opmerking |
|---|---|---|---|
| R-DC | G0/1 (naar R-BR1) | 10.71.1.1/30 | DHCP-server |
| R-DC | G0/2 (naar R-MID) | 10.71.2.1/30 | DHCP-server |
| R-BR1 | G0/1 (naar R-DC) | 10.71.1.2/30 | |
| R-BR1 | G0/0 (filiaal 1) | 172.17.51.1/24 | DHCP-relay naar 10.71.1.1 |
| R-MID | G0/0 (naar R-DC) | 10.71.2.2/30 | |
| R-MID | G0/1 (naar R-BR2) | 10.71.3.1/30 | |
| R-BR2 | G0/1 (naar R-MID) | 10.71.3.2/30 | |
| R-BR2 | G0/0 (filiaal 2) | 172.17.52.1/24 | DHCP-relay naar 10.71.2.1 |
| PC-B1 | Fa0 | DHCP (filiaal 1) | |
| PC-B2 | Fa0 | DHCP (filiaal 2) | |

DHCP-pools op R-DC: filiaal 1 en filiaal 2, elk .100 t/m .254, gateway .1, DNS 8.8.8.8.

Routing (statisch): R-DC kent alle filiaalnetwerken en transitlinks. De filiaalrouters en R-MID hebben een default route richting R-DC; R-MID kent daarnaast filiaal 2.

Op R-MID staat een ACL die ongewenst verkeer uit filiaal 2 tegenhoudt; normaal bedrijfsverkeer (waaronder DHCP) moet erdoor.

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: **Desktop → Command Prompt**. Een nieuw adres aanvragen: `ipconfig /renew`.

## Opdrachten

### Deel 1 - Documentatie analyseren
1. Beschrijf de weg van een DHCPDISCOVER van PC-B2 naar R-DC, en de weg van de DHCPOFFER terug. Welke bron- en doeladressen en welke UDP-poorten horen daarbij?
2. Wat is het veld *giaddr*, wie vult het in, en waarom heeft de server het nodig?

### Deel 2 - Baseline
Noteer het adres van PC-B1 en PC-B2 (`ipconfig`, en daarna `ipconfig /renew`). Ping vanaf R-DC naar 172.17.51.1 en 172.17.52.1.

### Deel 3 - Verkennen en vergelijken
- R-DC: `show ip dhcp pool`, `show ip dhcp binding`, `show ip route`
- Relay-routers: `show ip interface <int> | include Helper`, `show running-config`
- R-MID: `show access-lists`, `show ip interface <int> | include access list`
- Wil je de DHCP-berichten zelf zien? Gebruik de **Simulation**-modus van Packet Tracer met een filter op DHCP.

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's. Test na elke wijziging opnieuw.

### Deel 5 - Verificatie
- PC-B1 en PC-B2 hebben een adres uit hun eigen bereik met de juiste gateway.
- `show ip dhcp binding` op R-DC toont beide leases.
- PC-B1 kan PC-B2 pingen.

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
