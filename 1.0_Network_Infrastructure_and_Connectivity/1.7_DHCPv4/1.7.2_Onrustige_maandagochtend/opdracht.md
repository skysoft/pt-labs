# Lab 1.7.2 - Onrustige maandagochtend

CCNA 200-301 v2.0 - leerdoel 1.7. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met het ontwerp.

Packet Tracer-bestand: [1.7.2_onrustige_maandagochtend.pkt](1.7.2_onrustige_maandagochtend.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.7):** DHCPv4-client, -server en -relay op IOS-apparaten troubleshooten.

## Scenario
Router R1 routeert tussen drie VLAN's (router-on-a-stick) en is voor alle drie de **DHCP-server**. De netwerkprinter heeft een vast adres buiten het DHCP-bereik. Op maandagochtend komen deze klachten binnen:
- *PC20 (Sales):* "Ik heb wel een adres, maar ik kom nergens buiten mijn eigen afdeling."
- *PC10 (Staff):* "Printen lukt niet meer, de printer staat niet op zijn vaste adres."
- *Gasten:* "Eén van ons heeft geen netwerk, de anderen wel."

De switch is correct geconfigureerd. Er zitten **3 fouten** in het netwerk.

## Ontwerp (klopt)
| VLAN | Naam | Subnet | Gateway (R1) | DHCP-bereik | Clients |
|---|---|---|---|---|---|
| 10 | STAFF | 172.17.10.0/24 | G0/0.10 - 172.17.10.1 | .100 t/m .254 | PC10, PRN10 |
| 20 | SALES | 172.17.20.0/24 | G0/0.20 - 172.17.20.1 | .100 t/m .254 | PC20 |
| 30 | GASTEN | 172.17.30.0/24 | G0/0.30 - 172.17.30.1 | .100 t/m .254 | GAST1, GAST2, GAST3 |

DNS-server voor alle pools: 8.8.8.8.

Printer **PRN10** heeft altijd het vaste adres **172.17.10.50**/24, gateway 172.17.10.1, DNS 8.8.8.8.

Beveiligingsbeleid: proxy-ARP staat op alle subinterfaces van R1 uit (`no ip proxy-arp`). Dat is geen fout.

## Toegang
- R1 en SW1: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: **Desktop → Command Prompt** of **Desktop → IP Configuration**.
- Printer PRN10: tabblad **Config**. Onder **Settings** staan de gateway en DNS, onder **FastEthernet0** het adres (DHCP of statisch).
- Een nieuw adres aanvragen op een pc: `ipconfig /release` en daarna `ipconfig /renew`. Krijg je `DHCP request failed`, probeer het dan nog een keer.

## Opdrachten

### Deel 1 - Documentatie analyseren
1. Welke opties moet elke pool minimaal meegeven?
2. Een vast adres binnen een DHCP-netwerk kan op twee manieren: statisch op het apparaat, of als reservering op de DHCP-server. Wat zijn de voor- en nadelen van beide?

### Deel 2 - Baseline
Noteer op elke client het IP-adres, het masker en de default gateway (`ipconfig`). Ping vanaf elke pc naar de eigen gateway en naar PC10.

### Deel 3 - Verkennen en vergelijken
- R1: `show ip dhcp pool`, `show ip dhcp binding`, `show ip dhcp conflict`, `show running-config`
- Client: `ipconfig /all` (MAC-adres), `arp -a`

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's. Vraag na elke wijziging op de betrokken client opnieuw een adres aan.

Let op, dit is een eigenaardigheid van Packet Tracer: verander je `ip dhcp excluded-address` terwijl de pool al bestaat, geef dan in de pool het `network`-commando opnieuw. Pas dan gebruikt de DHCP-server de nieuwe uitsluitingen.

### Deel 5 - Verificatie
- Elke client heeft een adres uit het juiste bereik, het juiste masker en de juiste gateway.
- PRN10 heeft 172.17.10.50.
- Alle clients kunnen elkaar pingen.

### Deel 6 - Reflectie
1. Welke fouten heb je gevonden? Geef per fout het commando dat de doorslag gaf.
2. Welke fout was het lastigst te vinden, en waarom?
3. Waarom had maar één gast geen netwerk en de anderen wel?
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
