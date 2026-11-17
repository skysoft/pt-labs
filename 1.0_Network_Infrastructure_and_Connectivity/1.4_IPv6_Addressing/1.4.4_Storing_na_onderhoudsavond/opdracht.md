# Lab 1.4.4 - Storing na onderhoudsavond

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.4_storing_na_onderhoudsavond.pkt](1.4.4_storing_na_onderhoudsavond.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten.

## Scenario
Een bedrijf heeft twee locaties (LAN-A bij R1 en LAN-B bij R2), verbonden via een WAN-link. Alle hosts zijn statisch geconfigureerd. De bedrijfsstandaard schrijft voor dat hosts hun gateway instellen op het **link-local-adres FE80::1** van hun router. Ook de statische routes tussen de routers gebruiken link-local next hops.

Omdat alle hosts statisch zijn, staan Router Advertisements op de LAN-interfaces bewust uit (`ipv6 nd ra suppress`).

Na een onderhoudsavond, waarbij onder andere R2 opnieuw is geconfigureerd, komen deze klachten binnen:
- *PC1:* "Het kantoor werkt, maar de PC's in LAN-B zijn onbereikbaar."
- *PC2:* "Ik bereik niets buiten mijn eigen LAN."
- *PC3 en PC4:* "Wij bereiken niets buiten ons eigen LAN."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | Global unicast | Link-local | Gateway |
|---|---|---|---|---|
| R1 | G0/0 (LAN-A) | 2001:db8:4:a::1/64 | FE80::1 | - |
| R1 | G0/1 (WAN) | 2001:db8:4:ff::1/64 | FE80::1 | - |
| R2 | G0/1 (WAN) | 2001:db8:4:ff::2/64 | FE80::2 | - |
| R2 | G0/0 (LAN-B) | 2001:db8:4:b::1/64 | FE80::1 | - |
| PC1 | Fa0 | 2001:db8:4:a::11/64 | automatisch | FE80::1 |
| PC2 | Fa0 | 2001:db8:4:a::12/64 | automatisch | FE80::1 |
| PC3 | Fa0 | 2001:db8:4:b::13/64 | automatisch | FE80::1 |
| PC4 | Fa0 | 2001:db8:4:b::14/64 | automatisch | FE80::1 |

Routing (statisch, met link-local next hops):
- R1: route naar 2001:db8:4:b::/64 via G0/1, FE80::2
- R2: route naar 2001:db8:4:a::/64 via G0/1, FE80::1

## Toegang
- Routers: klik op de router en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's: **Desktop → Command Prompt** of **Desktop → IP Configuration** (deel IPv6 Configuration, staat op **Static**).

## Opdrachten

### Deel 1 - Documentatie analyseren
1. Waarom kan elke router op elke interface hetzelfde link-local-adres FE80::1 gebruiken zonder dat dit een conflict geeft?
2. Waarom moet je bij een link-local-adres als next hop altijd ook de interface opgeven?

### Deel 2 - Baseline-connectiviteit
Maak een pingmatrix: ping vanaf elke PC naar elke andere PC en naar 2001:db8:4:a::1 en 2001:db8:4:b::1. Noteer per combinatie OK of FAIL.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan.
- Router: `show ipv6 interface brief`, `show ipv6 interface <int>`, `show ipv6 route`, `show ipv6 neighbors`, `show running-config`
- PC: `ipv6config /all`, of kijk in IP Configuration

Tip: op een router kun je een link-local-adres pingen met `ping FE80::1`; IOS vraagt dan om de uitgaande interface.

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's. De gateway van een PC pas je aan in IP Configuration.

### Deel 5 - Verificatie
Herhaal de pingmatrix. Alle pings moeten nu slagen, en elke PC heeft FE80::1 als default gateway.

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
