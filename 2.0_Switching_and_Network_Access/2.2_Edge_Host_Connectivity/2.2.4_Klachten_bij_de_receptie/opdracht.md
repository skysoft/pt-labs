# Lab 2.2.4 - Klachten bij de receptie

CCNA 200-301 v2.0 - leerdoel 2.2. Troubleshootlab.

Packet Tracer-bestand: [2.2.4_klachten_bij_de_receptie.pkt](2.2.4_klachten_bij_de_receptie.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.2):** Layer 2-poortinstellingen voor eindapparaten configureren en troubleshooten.

## Scenario
Camping De Zeven Linden heeft een nieuwe switch (SW1) bij de receptie. Een stagiair heeft de poorten ingesteld. Sindsdien:
- "De balie-pc (aangesloten achter de IP-telefoon) heeft geen netwerk. De telefoon doet het ook niet."
- "AP1 is niet meer te beheren, terwijl het wifi van AP1 gewoon werkt."
- "AP2 meldt zich niet aan bij de wireless controller."
- "De camera bij de slagboom is offline."

De printer werkt wel.

**Er zitten 4 fouten in dit lab.** CORE is correct; de fouten zitten op SW1.

## Ontwerp
| VLAN | Naam | Subnet (gateway .1 op CORE) |
|---|---|---|
| 10 | PERSONEEL | 192.168.10.0/24 |
| 20 | PRINTERS | 192.168.20.0/24 |
| 30 | IOT | 192.168.30.0/24 |
| 40 | AP-BEHEER | 192.168.40.0/24 |
| 45 | WLC | 192.168.45.0/24 |
| 50 | GASTEN | 192.168.50.0/24 |
| 99 | BEHEER (native uplink) | 192.168.99.0/24 |
| 150 | VOICE | 192.168.150.0/24 (DHCP op CORE) |

| Poort SW1 | Apparaat | Ontwerp | Adres(sen) |
|---|---|---|---|
| Gi0/1 | uplink CORE | trunk, native 99 | - |
| Fa0/1 | IP-telefoon TEL1 + PC1 | data-VLAN 10, voice VLAN 150, PortFast | TEL1 via DHCP; PC1 192.168.10.11 |
| Fa0/2 | AP1 standalone | trunk, native 40, allowed 10,40,50, geen DTP, PortFast voor trunks | beheer 192.168.40.11; SSID's 192.168.10.21 (VLAN 10) en 192.168.50.21 (VLAN 50) |
| Fa0/3 | AP2 lightweight | access VLAN 40, PortFast | 192.168.40.12 |
| Fa0/4 | CAM1 (camera) | access VLAN 30, PortFast | 192.168.30.10 |
| Fa0/5 | PRN1 (printer) | access VLAN 20, PortFast | 192.168.20.10 |

WLC1 (192.168.45.10) hangt aan CORE.

**Let op - nabootsing in Packet Tracer (zoals in lab 2.2.2):**
- TEL1 is een echte IP-telefoon (7960) die PoE van SW1 krijgt; PC1 zit op de PC-poort van de telefoon.
- AP1 is een **router**: Gi0/0 is het ongetagde beheeradres, de subinterfaces Gi0/0.10 en Gi0/0.50 zijn de SSID's. Pingen doe je in de CLI van AP1.
- AP2, CAM1 en PRN1 zijn pc's; WLC1 is een server.

## Toegang
- Switches en AP1: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt**.
- Na een wijziging duurt het even voordat spanning tree een poort doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.

## Testcommando's
- PC1, CAM1, PRN1: `ping <gateway>`
- AP1 (CLI): `ping 192.168.40.1` (beheer), `ping 192.168.10.1` (SSID Personeel), `ping 192.168.50.1` (SSID Gasten)
- AP2: `ping 192.168.45.10`
- CORE: `show ip dhcp binding` (heeft de telefoon een adres?)
- SW1: `show interfaces status`, `show interfaces <poort> switchport`, `show interfaces trunk`, `show vlan brief`, `show mac address-table`, `show power inline`

## Opdrachten
1. Test de meldingen en noteer de symptomen.
2. Zoek en herstel de 4 fouten. Houd een **foutenlog** bij:

| # | Poort | Symptoom | Oorzaak | Oplossing (commando's) | Hoe geverifieerd |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |

3. Controleer aan het eind elke poort van SW1 tegen het ontwerp.

Je docent bespreekt daarna je foutenlog.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
