# Lab 1.3.4 - Nieuw netwerk productielocatie

CCNA 200-301 v2.0 - leerdoel 1.3. Troubleshootlab: maak het adresplan, vind en herstel de storing zodat het netwerk overeenkomt met het ontwerp.

Packet Tracer-bestand: [1.3.4_nieuw_netwerk_productielocatie.pkt](1.3.4_nieuw_netwerk_productielocatie.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.3):** IPv4-adresconfiguratie, -toewijzing en subnetting troubleshooten.

## Scenario
Een klant heeft voor de nieuwe productielocatie één adresblok gekregen: **10.10.0.0/24**. Dat blok moet zo zuinig mogelijk worden verdeeld over vier netwerken. Een collega heeft het netwerk opgebouwd, maar bij de oplevering werkt niet alles. Er is geen adresplan achtergelaten, alleen de ontwerpeisen hieronder.

Er zitten **2 fouten** in het netwerk.

## Ontwerpeisen
| Netwerk | Locatie | Aantal hosts nodig | Router-interface |
|---|---|---|---|
| LAN1 | Productie | 60 | R1 G0/0 |
| LAN2 | Kantoor | 28 | R1 G0/1 |
| LAN3 | Magazijn | 12 | R2 G0/0 |
| LINK | R1 G0/2 - R2 G0/1 | 2 | - |

Regels:
- Gebruik voor elk netwerk het **kleinste** subnet dat groot genoeg is.
- Wijs de subnetten toe van **groot naar klein**, beginnend bij 10.10.0.0.
- De router krijgt altijd het **eerste** bruikbare adres van een LAN.
- Op de LINK krijgt R1 het eerste en R2 het tweede bruikbare adres.

## Eindapparaten
| PC | Netwerk | IP-adres | Gateway |
|---|---|---|---|
| PC1 | LAN1 | 10.10.0.10/26 | 10.10.0.1 |
| PC2 | LAN2 | 10.10.0.90/27 | 10.10.0.65 |
| PC3 | LAN3 | 10.10.0.108/28 | 10.10.0.97 |

Routing: R1 heeft een statische route naar LAN3, R2 heeft een default route naar R1.

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: klik op de pc en open **Desktop → Command Prompt** of **Desktop → IP Configuration**.

## Opdrachten

### Deel 1 - Adresplan maken (op papier)
Maak op basis van de ontwerpeisen het adresplan en vul deze tabel in:

| Netwerk | Prefix | Subnetmasker | Netwerkadres | Eerste bruikbaar | Laatste bruikbaar | Broadcast |
|---|---|---|---|---|---|---|
| LAN1 | | | | | | |
| LAN2 | | | | | | |
| LAN3 | | | | | | |
| LINK | | | | | | |

Hoeveel adressen van het /24-blok blijven over?

### Deel 2 - Baseline
Ping vanaf elke PC naar de andere PC's en naar de eigen gateway. Noteer OK of FAIL.

Tip: de eerste ping naar een nieuw adres kan één time-out geven terwijl ARP het MAC-adres opzoekt. Kijk naar het totaal.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met jouw adresplan.
- Router: `show ip interface brief`, `show running-config`, `show ip route`, `show ip route connected`
- PC: `ipconfig`, `tracert <bestemming>`

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's.

### Deel 5 - Verificatie
- `show ip route connected` toont op beide routers precies de subnetten uit jouw plan.
- Alle PC's kunnen elkaar pingen.
- Vanaf PC2 loopt `tracert 10.10.0.108` via 10.10.0.65 en 10.10.0.114.

### Deel 6 - Reflectie
1. Welke fouten heb je gevonden? Geef per fout het commando dat de doorslag gaf.
2. Welke fout was het lastigst te vinden, en waarom?
3. Waarom kon PC1 de gateways van LAN2 en LAN3 wel pingen, maar PC2 en PC3 hun eigen gateway niet?
4. Hoe had deze storing voorkomen kunnen worden?

## Documentatie

| # | Apparaat | Symptoom | Gevonden met | Oorzaak | Oplossing |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |

Je docent bespreekt daarna nog een aantal verdiepingsvragen met je.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
