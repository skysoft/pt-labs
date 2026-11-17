# Lab 2.1.4 - Storing na het weekendonderhoud

CCNA 200-301 v2.0 - leerdoel 2.1. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [2.1.4_storing_na_het_weekendonderhoud.pkt](2.1.4_storing_na_het_weekendonderhoud.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.1):** netwerkinfrastructuur verbinden: Layer 2/Layer 3 LACP port-channel (EtherChannel).

## Scenario
Bij logistiek bedrijf De Vrachtbrief is in het weekend een nieuwe distributieswitch geplaatst. Alle uplinks zijn verdubbeld en gebundeld. Maandagochtend komen de meldingen binnen:
- "De monitoring meldt dat de bundel Po1 tussen DSW1 en ASW1 (begane grond) down is."
- "De monitoring meldt dat de uplink van ASW2 maar voor de helft werkt: één van de twee links is niet actief."
- "Niemand kan de server SRV1 bereiken."

**Er zitten 4 fouten in dit lab.**

## Ontwerp
| Bundel | Poorten | Protocol | Soort |
|---|---|---|---|
| Po1 | ASW1 Gi0/1-2 - DSW1 Gi0/1-2 | LACP: ASW1 **active**, DSW1 passive | L2-trunk |
| Po2 | ASW2 Fa0/23-24 - DSW1 Fa0/1-2 | LACP: ASW2 **active**, DSW1 passive | L2-trunk |
| Po12 | DSW1 Fa0/23-24 - DSW2 Fa0/23-24 | LACP: beide **active** | L3, 10.0.12.0/30 (DSW1 .1, DSW2 .2) |

- Alle L2-trunks: 802.1Q, geen DTP, native VLAN 99, allowed VLAN's **10,20,99**.
- Elke bundel heeft **twee actieve members**.
- ASW1 en ASW2 zijn 2960's, DSW1 en DSW2 zijn 3560 multilayer switches.

| Apparaat | Poort | VLAN | Adres | Gateway |
|---|---|---|---|---|
| PC1 | ASW1 Fa0/1 | 10 | 192.168.10.11/24 | 192.168.10.1 (DSW1) |
| PC2 | ASW2 Fa0/1 | 20 | 192.168.20.11/24 | 192.168.20.1 (DSW1) |
| ASW1 / ASW2 | Vlan99 | 99 | 192.168.99.2 / .3 | 192.168.99.1 (DSW1) |
| SRV1 | DSW2 Fa0/1 | 50 | 192.168.50.10/24 | 192.168.50.1 (DSW2) |

DSW1 en DSW2 routeren via statische routes over Po12.

## Toegang
- Switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's en server: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een poort doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.
- **Tips voor de 3560 in Packet Tracer:** configureer members één voor één (geen `interface range`). Een member uit een groep halen doe je met `no channel-group` (zonder nummer).

## Handige commando's
`show etherchannel summary`, `show etherchannel port-channel`, `show interfaces trunk`, `show interfaces <poort> switchport`, `show running-config`, `show spanning-tree vlan <nr>`, `show ip route`

## Opdrachten
1. Test de meldingen en noteer de symptomen.
2. Zoek en herstel de 4 fouten. Houd een **foutenlog** bij:

| # | Apparaat | Symptoom | Oorzaak | Oplossing (commando's) | Hoe geverifieerd |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |

3. Toon aan het eind met `show etherchannel summary` op DSW1 aan dat alle drie de bundels twee actieve members hebben, en dat PC1 en PC2 SRV1 bereiken.

Je docent bespreekt daarna je foutenlog.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
