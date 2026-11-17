# Lab 2.1.6 - Eindlab Bouwmarkt De Spijker

CCNA 200-301 v2.0 - leerdoel 2.1. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [2.1.6_eindlab_bouwmarkt_de_spijker.pkt](2.1.6_eindlab_bouwmarkt_de_spijker.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.1):** netwerkinfrastructuur verbinden (switch-to-switch en switch-to-router): L2/L3-interfaces, 802.1Q-trunks, LACP-EtherChannel en SVI's.

## Scenario
Bouwmarkt De Spijker heeft een nieuw netwerk laten aanleggen door een installateur die halverwege is gestopt. Het netwerk is "af", maar vrijwel niets werkt zoals het hoort. Jij krijgt het ontwerp en moet het netwerk werkend opleveren.

**Er zitten 6 fouten in dit lab.** EDGE en INTERNET zijn correct geconfigureerd.

## Ontwerp
| Link | Ontwerp |
|---|---|
| DSW1 Gi0/1 - EDGE G0/0 | **routed port**, 10.0.0.0/30 (EDGE .1, DSW1 .2); DSW1 default route via EDGE |
| DSW1 Fa0/1-2 - ASW1 Fa0/23-24 | **Po1**, LACP (beide active), L2-trunk |
| DSW1 Gi0/2 - ASW2 Gi0/1 | L2-trunk |

- Alle trunks: 802.1Q, geen DTP, native VLAN **99**, allowed **10,20,30,99**.
- DSW1 (3560) is de multilayer switch en de gateway (SVI) voor alle VLAN's. ASW1 en ASW2 (2960) zijn laag-2-switches.
- EDGE doet NAT naar het internet (router INTERNET, test: server 8.8.8.8) en heeft een route naar 192.168.0.0/16 via DSW1.

| VLAN | Naam | Subnet | Gateway (DSW1) |
|---|---|---|---|
| 10 | KASSA | 192.168.10.0/24 | .1 |
| 20 | KANTOOR | 192.168.20.0/24 | .1 |
| 30 | MAGAZIJN | 192.168.30.0/24 | .1 |
| 99 | BEHEER (native) | 192.168.99.0/24 | .1 |

| Apparaat | Poort | VLAN | Adres |
|---|---|---|---|
| PC1 | ASW1 Fa0/1 | 10 | 192.168.10.11 |
| PC2 | ASW1 Fa0/2 | 20 | 192.168.20.11 |
| PC3 | ASW2 Fa0/1 | 30 | 192.168.30.11 |
| PC4 | ASW2 Fa0/2 | 10 | 192.168.10.12 |
| ASW1 / ASW2 | Vlan99 | 99 | 192.168.99.2 / .3 |

## Toegang
- Routers en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een poort doorlaat. Druk een paar keer op **Fast Forward Time** (onderaan). De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.
- Werkt het na je laatste fix nog steeds niet, terwijl `show interfaces trunk`, `show etherchannel summary` en `show ip interface brief` wel kloppen? Sla je werk op en open het opnieuw: Packet Tracer heeft soms een herstart nodig.
- Tips voor de 3560 (DSW1) in Packet Tracer: configureer members één voor één (geen `interface range`). Een member uit een groep halen doe je met `no channel-group` (zonder nummer). Packet Tracer kent geen uitvoerfilters zoals `| include`.

## Opdrachten
1. Test systematisch: welke host bereikt wat (gateway, andere VLAN's, 10.0.0.1, 8.8.8.8, beheeradressen)? Noteer de symptomen.
2. Zoek en herstel de 6 fouten. Houd een **foutenlog** bij:

| # | Apparaat | Symptoom | Oorzaak | Oplossing (commando's) | Hoe geverifieerd |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |
| 6 | | | | | |

3. Oplevering: toon aan dat alle pc's elkaar en 8.8.8.8 bereiken, dat beide access-switches via VLAN 99 bereikbaar zijn, en dat `show etherchannel summary` en `show interfaces trunk` overeenkomen met het ontwerp.

### Reflectie
1. Welke fout was het lastigst te vinden, en waarom?
2. Welke fouten verborgen andere fouten?
3. Maak een korte checklist (5 tot 8 punten) die je voortaan gebruikt bij het opleveren van switch-to-switch- en switch-to-router-verbindingen.

Je docent bespreekt daarna je foutenlog.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
