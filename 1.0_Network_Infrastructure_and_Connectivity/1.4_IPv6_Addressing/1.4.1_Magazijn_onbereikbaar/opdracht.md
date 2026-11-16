# Lab 1.4.1 - Magazijn onbereikbaar

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.1_magazijn_onbereikbaar.pkt](1.4.1_magazijn_onbereikbaar.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten.

## Scenario
Een bedrijf heeft een kantoor (LAN-A) en een magazijn (LAN-B), verbonden via een WAN-link tussen R1 en R2. Het netwerk draait volledig op IPv6. Alle adressen zijn statisch ingesteld.

Sinds een collega de IPv6-configuratie heeft "opgeschoond", komen deze klachten binnen:
- *PC2:* "Het magazijn is onbereikbaar, maar in het kantoor werkt alles."
- *PC3:* "Ik kan helemaal niets bereiken."
- *PC4:* "Collega PC3 naast me is onbereikbaar en het kantoor ook."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IPv6-adres | Gateway |
|---|---|---|---|
| R1 | G0/0 (LAN-A) | 2001:db8:1:a::1/64 | - |
| R1 | G0/1 (WAN) | 2001:db8:1:ff::1/64 | - |
| R2 | G0/0 (LAN-B) | 2001:db8:1:b::1/64 | - |
| R2 | G0/1 (WAN) | 2001:db8:1:ff::2/64 | - |
| PC1 | Fa0 | 2001:db8:1:a::10/64 | 2001:db8:1:a::1 |
| PC2 | Fa0 | 2001:db8:1:a::20/64 | 2001:db8:1:a::1 |
| PC3 | Fa0 | 2001:db8:1:b::30/64 | 2001:db8:1:b::1 |
| PC4 | Fa0 | 2001:db8:1:b:1::40/64 | 2001:db8:1:b::1 |

Routing: statische IPv6-routes. R1 heeft een route naar LAN-B, R2 heeft een route naar LAN-A.

## Toegang
- Routers en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: **Desktop → Command Prompt** of **Desktop → IP Configuration** (deel IPv6 Configuration). De IPv6-gateway staat ook onder **Config → Settings**.

## Opdrachten

### Deel 1 - Documentatie analyseren
Geef voor elk subnet uit het adresplan het prefix en het bereik van de adressen die erin vallen. Schrijf alle adressen uit het plan ook volledig uit (zonder afkortingen).

### Deel 2 - Baseline-connectiviteit
Maak een pingmatrix: ping vanaf elke PC naar elke andere PC en naar alle gateways. Noteer per combinatie OK of FAIL.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan.
- Router: `show ipv6 interface brief`, `show ipv6 interface <int>`, `show ipv6 route`, `show ipv6 neighbors`, `show running-config`
- PC: `ipv6config`, `tracert <bestemming>`, en het prefix in **IP Configuration**

### Deel 4 - Fouten vinden en herstellen
Noteer voor elke fout:
1. het symptoom (wat werkt niet?)
2. het apparaat en het commando waarmee je de fout vond
3. de oorzaak
4. het herstelcommando

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
