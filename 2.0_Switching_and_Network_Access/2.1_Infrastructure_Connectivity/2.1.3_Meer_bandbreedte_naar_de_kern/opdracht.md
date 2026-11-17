# Lab 2.1.3 - Meer bandbreedte naar de kern

CCNA 200-301 v2.0 - leerdoel 2.1. Configuratielab.

Packet Tracer-bestand: [2.1.3_meer_bandbreedte_naar_de_kern.pkt](2.1.3_meer_bandbreedte_naar_de_kern.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.1):** netwerkinfrastructuur verbinden: Layer 2/Layer 3 LACP port-channel (EtherChannel).

## Scenario
Bij ingenieursbureau De Rekenlat zijn alle verbindingen tussen de switches **verdubbeld**: er ligt nu steeds een tweede kabel. Toch is er niets sneller geworden, en de beheerder ziet dat één van de twee kabels door spanning tree wordt geblokkeerd. Jouw taak is de dubbele verbindingen te bundelen met **LACP**:
- tussen access-switch ASW1 (2960) en distributieswitch DSW1 (3560): een **L2-EtherChannel** die als trunk werkt;
- tussen de distributieswitches DSW1 en DSW2 (beide 3560 multilayer switches): een **L3-EtherChannel** (routed link).

VLAN's, SVI's en statische routes zijn al geconfigureerd.

## Verbindingen en ontwerp
| Link | Poorten | Port-channel | Ontwerp |
|---|---|---|---|
| ASW1 - DSW1 | Gi0/1-2 aan beide kanten | Po1 | LACP (ASW1 active, DSW1 passive); trunk 802.1Q zonder DTP; native VLAN 99; allowed 10,20,99 |
| DSW1 - DSW2 | Fa0/23-24 aan beide kanten | Po12 | LACP (beide active); routed: DSW1 10.0.12.1/30, DSW2 10.0.12.2/30 |

## Adressen
| Apparaat | Interface | Adres | Opmerking |
|---|---|---|---|
| PC1 | ASW1 Fa0/1 (VLAN 10) | 192.168.10.11/24 | gateway .1 (DSW1) |
| PC2 | ASW1 Fa0/2 (VLAN 20) | 192.168.20.11/24 | gateway .1 (DSW1) |
| ASW1 | Vlan99 | 192.168.99.2/24 | gateway .1 (DSW1) |
| DSW1 | Vlan10 / 20 / 99 | 192.168.10.1 / 20.1 / 99.1 | |
| DSW2 | Vlan50 | 192.168.50.1/24 | serverlan |
| SRV1 | DSW2 Fa0/1 (VLAN 50) | 192.168.50.10/24 | server |

## Toegang
- Switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's en server: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een poort doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.
- **Let op bij de 3560 (DSW1, DSW2):** in Packet Tracer werkt `interface range` op deze switches niet betrouwbaar. Configureer de members één voor één.

## Opdrachten

### Deel 1 - Theorie
1. Waarom levert een tweede kabel tussen twee switches zonder EtherChannel geen extra bandbreedte op?
2. Welke combinaties van LACP-modi (`active`, `passive`) vormen een bundel? En wat doet `mode on`?
3. Welke instellingen moeten op alle members van een bundel gelijk zijn?
4. Wat is het verschil tussen een L2- en een L3-EtherChannel? Waar komt bij een L3-EtherChannel het IP-adres te staan?

### Deel 2 - Uitgangssituatie
1. Bekijk op ASW1 én DSW1 `show spanning-tree vlan 1`. Welke switch is root, en welke poorten zijn geblokkeerd?
2. Bekijk op DSW1 `show ip interface brief` en `show ip route static`. Waarom zijn de SVI's niet up/up? Waarom staat de route naar 192.168.50.0/24 niet in de routingtabel?
3. Ping vanaf PC1 naar zijn gateway en naar SRV1.

### Deel 3 - L2-EtherChannel ASW1 - DSW1
1. Bundel Gi0/1-2 op ASW1 (LACP **active**) en op DSW1 (LACP **passive**) tot Po1: zet eerst `channel-group 1 mode ...` op de members.
2. Configureer daarna Po1 aan beide kanten als trunk volgens het ontwerp. Op DSW1 (3560) moet je eerst `switchport trunk encapsulation dot1q` invoeren. Bekijk daarna de running-config van Gi0/1: wat valt je op?
3. Controleer met `show etherchannel summary` en `show interfaces trunk`. Staan de members op **(P)**? Zo niet: vergelijk de trunkinstellingen van de members met die van Po1 (`show interfaces gi0/1 switchport`) en maak ze gelijk.
4. Hoe ziet spanning tree de uplink nu (`show spanning-tree vlan 10`)?

### Deel 4 - L3-EtherChannel DSW1 - DSW2
1. Maak van Fa0/23-24 op beide switches routed poorten (`no switchport`), en bundel ze met LACP (**active**) tot Po12.
2. Geef Po12 het IP-adres uit het ontwerp.
3. Controleer met `show etherchannel summary` (let op de letters achter Po12) en `show ip interface brief`.

### Deel 5 - Verificatie en analyse
1. Ping vanaf PC1 en PC2 naar SRV1. Controleer `show ip route static` op DSW1 opnieuw.
2. Bekijk `show etherchannel port-channel` op DSW1. Welke members staan op *Active*, en welke op *Passive*?
3. **Kabel eruit:**
   - Verwijder in Packet Tracer de kabel ASW1 Gi0/1 - DSW1 Gi0/1 (gum-gereedschap).
   - Ping direct daarna vanaf PC1 naar 192.168.50.10. Gaat er verkeer verloren? Bekijk `show etherchannel summary` op ASW1.
   - Leg de kabel daarna weer terug (koperen crossover tussen dezelfde poorten).
4. Bekijk `show etherchannel load-balance`. Op basis waarvan wordt het verkeer verdeeld? Wat betekent dat voor het verkeer van één pc?

### Deel 6 - Reflectie
1. Wat gebeurt er volgens de theorie als beide kanten van een bundel op `passive` staan?
2. Bij proef 3 ging de link meteen down. Wat zou er gebeurd zijn bij een defecte kabel of een mediaconverter waarbij de link "up" blijft, als de bundel met `channel-group ... mode on` was gemaakt? Waarom is `mode on` dus riskanter dan LACP?

Je docent bespreekt daarna je antwoorden.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
