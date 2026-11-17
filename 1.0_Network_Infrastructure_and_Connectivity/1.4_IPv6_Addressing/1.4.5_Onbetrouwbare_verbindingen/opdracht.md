# Lab 1.4.5 - Onbetrouwbare verbindingen

CCNA 200-301 v2.0 - leerdoel 1.4. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.4.5_onbetrouwbare_verbindingen.pkt](1.4.5_onbetrouwbare_verbindingen.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.4):** IPv6-adresconfiguratie, -toewijzing en prefixgrootte troubleshooten.

## Scenario
Het kantoor-LAN 2001:db8:5:1::/64 hangt aan router R1 via switch SW1. Alle hosts zijn statisch geconfigureerd, met gateway FE80::1. Het adres 2001:db8:5:ffff::1 op R1 (Loopback1) staat voor "het internet".

Na een migratie komen vage klachten binnen:
- *PC1:* "Sommige collega's bereik ik wel, andere niet, en de fileserver doet raar."
- *PC2 of PC3:* "Mijn netwerk werkt niet, maar mijn collega heeft nergens last van."
- *Meerdere gebruikers:* "Het internet werkt soms wel en soms niet."

Er zitten **3 fouten** in het netwerk.

## Adresplan (de documentatie klopt)
| Apparaat | Interface | IPv6-adres | Gateway |
|---|---|---|---|
| R1 | G0/0 (LAN) | 2001:db8:5:1::1/64, link-local FE80::1 | - |
| R1 | Loopback1 ("internet") | 2001:db8:5:ffff::1/128 | - |
| PC1 | Fa0 | 2001:db8:5:1::11/64 | FE80::1 |
| PC2 | Fa0 | 2001:db8:5:1::12/64 | FE80::1 |
| PC3 | Fa0 | 2001:db8:5:1::13/64 | FE80::1 |
| PC4 | Fa0 | 2001:db8:5:1::14/64 | FE80::1 |
| SRV1 | Fa0 | 2001:db8:5:1::50/64 (alleen dit adres) | FE80::1 |

Ieder apparaat heeft precies **één** global unicast-adres. Router Advertisements staan op R1 bewust uit, omdat alle hosts statisch zijn.

## Toegang
- R1 en SW1: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- PC's en server: **Desktop → Command Prompt** of **Desktop → IP Configuration** (deel IPv6 Configuration, staat op **Static**).

## Opdrachten

### Deel 1 - Documentatie analyseren
Beschrijf hoe een IPv6-host controleert of een adres al in gebruik is voordat hij het gaat gebruiken (Duplicate Address Detection). Welke berichten worden daarbij verstuurd, en naar welk adres?

### Deel 2 - Symptomen verzamelen
Ping vanaf elke host naar 2001:db8:5:ffff::1, SRV1 en de andere hosts. Herhaal elke ping een paar keer (`ping -n 4`). Noteer per test OK, FAIL of WISSELEND.

### Deel 3 - Verkennen en vergelijken
Maak een tabel met per host: het global adres, het MAC-adres en de switchpoort. Vergelijk die met het adresplan.
- Host: `ipv6config /all`
- R1: `show ipv6 interface brief`, `show ipv6 interface g0/0`, `show ipv6 neighbors`, `show logging`, `show running-config`
- SW1: `show mac address-table`, `show interfaces status`

Tip: met `clear ipv6 neighbors` op R1 maak je de neighbor-tabel leeg. Ping daarna opnieuw en kijk welk MAC-adres R1 nu bij een adres leert.

### Deel 4 - Fouten vinden en herstellen
Noteer per fout: het symptoom, het commando waarmee je de fout vond, de oorzaak en de herstelcommando's. Een adres op een host pas je aan in IP Configuration.

### Deel 5 - Verificatie
- Ieder apparaat heeft precies het adres uit het adresplan.
- Alle hosts kunnen elkaar, SRV1 en 2001:db8:5:ffff::1 steeds pingen.

### Deel 6 - Reflectie
1. Welke fouten heb je gevonden? Geef per fout het commando dat de doorslag gaf.
2. Welke fout was het lastigst te vinden, en waarom?
3. Waarom werkten sommige pings de ene keer wel en de andere keer niet?
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
