# Lab 1.4.8 - Nieuwe clients zonder configuratie

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.8_nieuwe_clients_zonder_configuratie.pkt](1.4.8_nieuwe_clients_zonder_configuratie.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten.

## Scenario
Een bedrijf heeft twee locaties. De clients CL1 t/m CL3 halen hun IPv6-configuratie automatisch op.

- **LAN-A (R1):** CL1 en CL2 krijgen hun adres **en** DNS-gegevens via **stateful DHCPv6** van R1 (IP Configuration: IPv6 op **DHCP**).
- **LAN-B (R2):** CL3 maakt zijn adres zelf via **SLAAC** en haalt alleen de DNS-gegevens via **stateless DHCPv6** bij R2 (IP Configuration: IPv6 op **Automatic**).

Klachten:
- *CL1 en CL2:* "Na het opstarten hebben we geen bruikbaar IPv6-adres."
- *CL3:* "Ik heb wel een adres, maar geen DNS-server en geen domeinnaam."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IPv6-adres | Opmerking |
|---|---|---|---|
| R1 | G0/0 (LAN-A) | 2001:db8:8:a::1/64 | DHCPv6-server, stateful (M-flag) |
| R1 | G0/1 (WAN) | 2001:db8:8:ff::1/64 | |
| R2 | G0/1 (WAN) | 2001:db8:8:ff::2/64 | |
| R2 | G0/0 (LAN-B) | 2001:db8:8:b::1/64 | SLAAC + DHCPv6-server, stateless (O-flag) |
| CL1, CL2 | Fa0 | via DHCPv6 uit 2001:db8:8:a::/64 | LAN-A, via switch SW-A |
| CL3 | Fa0 | via SLAAC uit 2001:db8:8:b::/64 | LAN-B |

DHCPv6-gegevens voor beide LAN's:
- DNS-server: 2001:db8:8:a::53
- Domeinnaam: korrel.lan

Routing: R1 heeft een statische route naar LAN-B, R2 heeft een default route naar R1.

## Toegang
- Routers en switch: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Clients: **Desktop → Command Prompt** of **Desktop → IP Configuration**.
- Wacht na het openen van het bestand ongeveer 20 seconden: zo lang duurt het voordat de clients hun configuratie hebben.
- Een client opnieuw laten beginnen: zet in IP Configuration het IPv6-deel op **Static** en daarna weer op **DHCP** (CL1, CL2) of **Automatic** (CL3). Krijgt de client daarna nog steeds oude gegevens, sla je werk dan op en open het bestand opnieuw.

## Opdrachten

### Deel 1 - Documentatie analyseren
Leg uit wat de M-flag en de O-flag in een Router Advertisement betekenen. Welke combinatie hoort bij LAN-A en welke bij LAN-B?

### Deel 2 - Symptomen
Noteer op elke client de uitvoer van `ipv6config /all` (adres, gateway, DNS-server, DNS-suffix). Ping vanaf elke client naar het global adres van de gateway en naar de andere clients.

### Deel 3 - Verkennen en vergelijken
Verzamel van elk apparaat de werkelijke configuratie en vergelijk die met het adresplan.
- Router: `show ipv6 interface <int>` (let op de laatste regels: hoe krijgen hosts hun adres en andere gegevens?), `show ipv6 dhcp pool`, `show ipv6 dhcp binding`, `show ipv6 dhcp interface`, `show running-config`, `show ipv6 route`
- Client: `ipv6config /all`, `tracert <bestemming>`

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's. Test na elke wijziging opnieuw.

### Deel 5 - Verificatie
- CL1 en CL2 hebben een adres uit 2001:db8:8:a::/64 en zijn te zien in `show ipv6 dhcp binding` op R1.
- Alle clients tonen in `ipv6config /all` de DNS-server 2001:db8:8:a::53 en het DNS-suffix korrel.lan.
- Alle clients kunnen elkaar pingen.

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
