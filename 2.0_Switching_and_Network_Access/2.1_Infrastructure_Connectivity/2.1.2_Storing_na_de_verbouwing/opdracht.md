# Lab 2.1.2 - Storing na de verbouwing

CCNA 200-301 v2.0 - leerdoel 2.1. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [2.1.2_storing_na_de_verbouwing.pkt](2.1.2_storing_na_de_verbouwing.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.1):** netwerkinfrastructuur verbinden (switch-to-switch en switch-to-router): 802.1Q-trunks.

## Scenario
Groothandel Het Pakhuis is verbouwd. Tijdens de verbouwing zijn de switches losgekoppeld en door een uitzendkracht opnieuw aangesloten en deels opnieuw geconfigureerd. Sindsdien komen er meldingen binnen:
- "Op de eerste verdieping (SW-A) kan niemand meer bij de printers en het internet (de gateway)."
- "Verkoop komt nergens meer uit."
- "In het magazijn werkt de pc niet meer."
- "Ik kan SW-B niet meer beheren via 192.168.99.4."

**Er zitten 4 fouten in dit lab.** R1 is correct geconfigureerd als router-on-a-stick; de fouten zitten in de switches.

## Ontwerp
- Alle verbindingen tussen switches en naar R1 zijn **802.1Q-trunks**: altijd trunk (geen DTP), native VLAN **99**, allowed VLAN's **10,20,30,99**.
- Alle switches kennen alle VLAN's.

| VLAN | Naam | Subnet | Gateway (R1) |
|---|---|---|---|
| 10 | KANTOOR | 192.168.10.0/24 | .1 |
| 20 | VERKOOP | 192.168.20.0/24 | .1 |
| 30 | MAGAZIJN | 192.168.30.0/24 | .1 |
| 99 | BEHEER (native) | 192.168.99.0/24 | .1 |

| Apparaat | Poort | VLAN | Adres |
|---|---|---|---|
| PC-A1 | SW-A Fa0/1 | 10 | 192.168.10.11 |
| PC-A2 | SW-A Fa0/2 | 20 | 192.168.20.11 |
| PC-B1 | SW-B Fa0/1 | 10 | 192.168.10.12 |
| PC-B2 | SW-B Fa0/2 | 30 | 192.168.30.11 |
| SW-KERN / SW-A / SW-B | Vlan99 | 99 | 192.168.99.2 / .3 / .4 |

Verbindingen: R1 G0/0 - SW-KERN Fa0/1, SW-KERN Gi0/1 - SW-A Gi0/1, SW-KERN Gi0/2 - SW-B Gi0/1.

## Toegang
- Router en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een VLAN doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.

## Handige commando's
`show interfaces trunk`, `show interfaces <poort> switchport`, `show vlan brief`, `show cdp neighbors detail`, `show logging`, `show spanning-tree vlan <nr>`

## Opdrachten
1. Test de meldingen en noteer de symptomen.
2. Zoek en herstel de 4 fouten. Houd een **foutenlog** bij:

| # | Apparaat | Symptoom | Oorzaak | Oplossing (commando's) | Hoe geverifieerd |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |

3. Toon aan het eind aan dat elke pc zijn gateway en de pc's in de andere VLAN's bereikt, en dat alle switches via VLAN 99 bereikbaar zijn.

Je docent bespreekt daarna je foutenlog.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
