# Lab 1.7.1 - Nieuwe glasvezelaansluiting

CCNA 200-301 v2.0 - leerdoel 1.7. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met het ontwerp.

Packet Tracer-bestand: [1.7.1_nieuwe_glasvezelaansluiting.pkt](1.7.1_nieuwe_glasvezelaansluiting.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.7):** DHCPv4-client, -server en -relay op IOS-apparaten troubleshooten.

## Scenario
Een klein kantoor heeft een nieuwe glasvezelaansluiting gekregen. De bedrijfsrouter EDGE haalt zijn WAN-adres en default gateway automatisch via **DHCP** bij de router van de provider (ISP). EDGE vertaalt het kantoor-LAN via PAT naar het WAN-adres. Achter de ISP-router staat de server **INTERNET** (8.8.8.8): gebruik dat als testadres.

In dit lab beheer je ook de DHCP-server van de provider.

Na de installatie komen deze klachten binnen:
- *PC1:* "Het internet werkt niet."
- *Monteur:* "De router van de klant krijgt geen adres van ons."

Er zitten **3 fouten** in het netwerk.

## Ontwerp (klopt)
| Apparaat | Interface | IP-adres | Opmerking |
|---|---|---|---|
| ISP | G0/1 (klant) | 100.64.71.1/29 | DHCP-server voor de klant |
| ISP | G0/2 (internet) | 8.8.8.1/24 | naar INTERNET |
| EDGE | G0/1 (WAN) | via DHCP van de ISP | NAT outside |
| EDGE | G0/0 (LAN) | 192.168.71.1/24 | NAT inside |
| PC1 | Fa0 | 192.168.71.10/24 | gateway 192.168.71.1 |

Afspraken met de provider (DHCP-pool voor de klantlink):
- Uitgedeeld bereik: 100.64.71.2 t/m 100.64.71.6
- Default gateway: 100.64.71.1
- DNS-server: 8.8.8.8

EDGE heeft **geen** handmatig ingestelde routes: de default route komt uit DHCP.

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC1: klik op de pc en open **Desktop → Command Prompt**.

## Opdrachten

### Deel 1 - Documentatie analyseren
1. Welke gegevens moet een DHCP-client naast een IP-adres en subnetmasker minimaal ontvangen om het internet te kunnen bereiken?
2. Hoe installeert een IOS-router met `ip address dhcp` zijn default route, en met welke administratieve afstand?

### Deel 2 - Baseline
Ping vanaf PC1 naar 192.168.71.1 en 8.8.8.8, en vanaf EDGE naar 100.64.71.1 en 8.8.8.8.

### Deel 3 - Verkennen en vergelijken
- EDGE: `show ip interface brief`, `show dhcp lease`, `show ip route`, `show running-config`
- ISP: `show ip dhcp pool`, `show ip dhcp binding`, `show running-config`

Een nieuwe DHCP-aanvraag op EDGE doe je met `shutdown` en daarna `no shutdown` op de WAN-interface:
```
EDGE(config)# interface GigabitEthernet0/1
EDGE(config-if)# shutdown
EDGE(config-if)# no shutdown
```
Wacht daarna even (of druk op **Fast Forward Time** onderaan het scherm) en kijk met `show dhcp lease`.

Let op, dit is een eigenaardigheid van Packet Tracer: verander je `ip dhcp excluded-address` terwijl de pool al bestaat, geef dan in de pool het `network`-commando opnieuw. Pas dan gebruikt de DHCP-server de nieuwe uitsluitingen.

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's. Test na elke wijziging opnieuw.

### Deel 5 - Verificatie
- `show dhcp lease` op EDGE toont een adres uit 100.64.71.2-.6 en gateway 100.64.71.1.
- `show ip route` op EDGE toont de default route via 100.64.71.1.
- PC1 kan 8.8.8.8 pingen.

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
