# Lab 1.1.2 - Klachten over het netwerk

CCNA 200-301 v2.0 - leerdoel 1.1. Troubleshootlab: vind en herstel de storing zodat het netwerk weer overeenkomt met de documentatie.

Packet Tracer-bestand: [1.1.2_klachten_over_het_netwerk.pkt](1.1.2_klachten_over_het_netwerk.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.1):** interface- en kabelproblemen diagnosticeren.

## Scenario
Na een verbouwing zijn er bij de servicedesk meerdere meldingen binnengekomen. De monteurs hebben kabels verlegd en poorten opnieuw ingesteld, en de afdeling heeft zelf een switch in de vergaderzaal neergezet. In het netwerk zitten **5 fouten** op laag 1 en 2 (interface en kabel). Vind ze, los ze op en documenteer ze.

## Topologie (alles in VLAN 10, 10.12.10.0/24)
| Apparaat | Poort | Hoort aangesloten op |
|---|---|---|
| R1 (gateway .1) | G0/0 | SW-KERN Fa0/1 |
| SW-KERN (.2) | Gi0/1 | SW-VERDIEPING Gi0/1 (uplink A) |
| SW-KERN | Gi0/2 | SW-VERDIEPING Gi0/2 (reserve-uplink, kabel ligt er, poorten staan uit) |
| SW-KERN | Fa0/24 | SW-MAGAZIJN Fa0/24 |
| SW-VERDIEPING (.3) | Fa0/2 | PC-VERD1 (.21) |
| SW-VERDIEPING | Fa0/4 | PC-VERD2 (.22) |
| SW-VERDIEPING | Fa0/11 | SW-ZAAL (afdelingsswitch), met daarachter PC-ZAAL (.23) |
| SW-MAGAZIJN (.4) | Fa0/1 / Fa0/2 | PC-MAG1 (.31) / PC-MAG2 (.32) |

Ontwerpregels:
- Alle switchpoorten staan op autonegotiation (speed en duplex auto) en Auto-MDIX.
- De afdelingsswitch SW-ZAAL is goedgekeurd: de poort ervoor hoort gewoon te werken.

## Toegang
- Switches en router: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt**.
- **Een kabel omzetten of vervangen:** in Packet Tracer kun je een kabel verwijderen (gum-gereedschap of Delete) en een nieuwe kabel trekken. Met de muis boven een kabel zie je welk type het is. Dat mag, maar je kunt een probleem vaak ook in de configuratie oplossen. Beschrijf wat je in een echt gebouw zou doen.
- Na een wijziging duurt het ongeveer 30 seconden (spanning tree) voordat een poort verkeer doorlaat. Met **Fast Forward Time** (onderaan) gaat dat sneller.

## Meldingen
1. "Ik bereik de verdiepingsswitch nog wel, maar verder helemaal niets." (PC-VERD1)
2. "PC-VERD2 heeft sinds de verbouwing helemaal geen netwerk."
3. "In de vergaderzaal werkt niets." (PC-ZAAL)
4. "PC-MAG1 in het magazijn heeft geen verbinding."
5. "Het hele magazijn is sinds de verbouwing onbereikbaar. De monitoring meldt dat de verbinding tussen SW-KERN en het magazijn down is."

## Werkwijze
Noteer per fout:
| # | Apparaat/poort | Symptoom | Oorzaak | Oplossing | Hoe getest |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |

**Nuttige commando's:**
- `show interfaces status`, `show interfaces <poort> status`, `show ip interface brief`
- `show interfaces <poort>` (let op duplex, speed en tellers)
- `show running-config` (let op de interfaceconfiguratie)
- `show cdp neighbors`, `show mac address-table`
- `show logging`
- op een pc: `ping -n 20 10.12.10.1`, `ipconfig /all`

Packet Tracer kent geen uitvoerfilters zoals `| include`.

## Eindcontrole
- Alle pc's pingen de gateway 10.12.10.1 twintig keer zonder verlies.
- Alle switchpoorten voldoen aan de ontwerpregels (auto/auto, Auto-MDIX).
- Geen enkele poort staat nog err-disabled of onbedoeld uit.

## Reflectie
1. Welke fout verborg een andere fout? Hoe merkte je dat?
2. Welke fout zou je met een kabeltester hebben gevonden, en welke niet?
3. In een echt netwerk gaat een link met een duplex-mismatch meestal níet down. Wat merk je dan wel, en in welke tellers?

Je docent bespreekt daarna je antwoorden.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
