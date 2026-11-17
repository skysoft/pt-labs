# Lab 1.1.1 - Wat zegt de interface?

CCNA 200-301 v2.0 - leerdoel 1.1. Configuratie- en interpretatielab.

Packet Tracer-bestand: [1.1.1_wat_zegt_de_interface.pkt](1.1.1_wat_zegt_de_interface.pkt)

**Leerdoel (CCNA 200-301 v2.0, 1.1):** interface- en kabelproblemen diagnosticeren (koper en glasvezel): collisions, errors, duplex- en speed-mismatch, afstand, interfacestatus, signaalniveaus, pin-out en kabeltypes.

## Scenario
Je bent nieuw bij de beheerafdeling. Voordat je storingen gaat oplossen, leer je wat de switches je vertellen over hun poorten en kabels. Dit is een **configuratie- en interpretatielab**. Een deel doe je in Packet Tracer, een deel op papier.

**Let op:** Packet Tracer is een simulatie zonder echte kabels. Collisions, CRC-fouten, kabellengte en signaalniveaus kun je hier niet echt veroorzaken. Daarom krijg je in deel 4 en 5 uitvoer van echte apparatuur om te interpreteren.

## Topologie (VLAN 10, 10.11.10.0/24)
| Apparaat | Poort | Aangesloten op |
|---|---|---|
| R1 (gateway .1) | G0/0 | SW1 Fa0/1 |
| SW1 (.2) | Fa0/24 | SW2 Fa0/24 |
| SW1 | Fa0/2 / Fa0/3 | PC1 (.11) / PC3 (.13) |
| SW2 (.3) | Fa0/1 | PC2 (.12) |
| SW2 | Fa0/2 | SW-BUREAU, met daarachter PC4 (.14) |

## Toegang
- Switches en router: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt** of **Desktop → IP Configuration**.
- Packet Tracer kent geen uitvoerfilters zoals `| include`. Gebruik in plaats daarvan bijvoorbeeld `show interfaces fa0/24 status`.

## Opdrachten

### Deel 1 - De status van een interface
1. Bekijk op SW1 en SW2 `show interfaces status` en `show ip interface brief`. Vul de tabel in voor SW1 Fa0/2, Fa0/3, Fa0/4, Fa0/24 en SW2 Fa0/2:

| Switch/poort | Status (`show interfaces status`) | Status/Protocol (`show ip interface brief`) | Betekenis en mogelijke oorzaak |
|---|---|---|---|
| | | | |

2. Wat is het verschil tussen *administratively down*, *down/down* (notconnect) en *err-disabled*? Hoe herstel je elk ervan?
3. Zoek met `show interfaces status` en `show logging` uit waarom SW2 Fa0/2 is uitgeschakeld.
4. Zet SW1 Fa0/3 aan en controleer dat PC3 de gateway kan pingen.
5. Wat zou *up/down* betekenen (status up, protocol down)? Noem twee mogelijke oorzaken.

### Deel 2 - Klopt de bekabeling met de documentatie?
1. Gebruik `show cdp neighbors` (en `show cdp neighbors detail`) op SW1, SW2 en R1 om vast te stellen welk apparaat op welke poort zit.
2. SW2 toont SW-BUREAU in de CDP-tabel, terwijl de poort err-disabled is. Hoe kan dat? Kijk een paar minuten later opnieuw en let op de kolom *Holdtme*.
3. Welke apparaten zie je **niet** met CDP? Hoe vind je toch op welke poort een pc zit? (Tip: ping eerst vanaf de pc's, bekijk dan `show mac address-table`. Het MAC-adres van een pc vind je met `ipconfig /all`.)
4. Teken de topologie zoals jij die hebt vastgesteld en vergelijk die met de tabel hierboven.

### Deel 3 - Speed en duplex
1. Bekijk de kolommen *Duplex* en *Speed* in `show interfaces status`. Wat betekent de **a-** voor `a-full`?
2. Zet op SW1 Fa0/24 de snelheid en duplex vast: `speed 100`, `duplex half`. Wat toont `show interfaces status` nu op SW1, en wat op SW2? Wat heeft de auto-kant (SW2) gekozen?
3. Zet nu ook SW2 Fa0/24 vast, maar anders: `speed 100`, `duplex full`. Wat gebeurt er met de link in Packet Tracer?
4. **Theorie:** als één kant vast staat en de andere kant op auto, welke snelheid en duplex kiest de auto-kant? Waarom geeft dat bij een vaste kant op *full* een probleem?
5. Op echte apparatuur blijft een link met een duplex-mismatch wél up. Welke tellers lopen dan op aan de **half**-duplex-kant, en welke aan de **full**-duplex-kant? Welke CDP-melding verschijnt er in de log?
6. Zet Fa0/24 op beide switches terug op autonegotiation (`speed auto`, `duplex auto`) en controleer dat de link weer werkt.

### Deel 4 - Tellers lezen (uitvoer van echte switches)
Stel voor elke uitvoer een diagnose: wat is er waarschijnlijk aan de hand, en wat ga je controleren?

**A** - poort naar een printer:
```
GigabitEthernet1/0/7 is up, line protocol is up (connected)
  Half-duplex, 100Mb/s, media type is 10/100/1000BaseTX
     118342 packets input, 14203311 bytes, 0 no buffer
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     203117 packets output, 99231874 bytes, 0 underruns
     0 output errors, 18234 collisions, 2 interface resets
     0 babbles, 1907 late collision, 0 deferred
```

**B** - de andere kant van dezelfde verbinding als A (de printer zelf meldt):
```
Full-duplex, 100Mb/s
  input errors 4211, CRC 3980, runts 231
```

**C** - poort naar een werkplek op de derde verdieping:
```
GigabitEthernet1/0/22 is up, line protocol is up (connected)
  Full-duplex, 1000Mb/s, media type is 10/100/1000BaseTX
     8812331 packets input, 9019233122 bytes, 0 no buffer
     0 runts, 0 giants, 0 throttles
     52301 input errors, 52288 CRC, 13 frame, 0 overrun, 0 ignored
     0 output errors, 0 collisions, 0 interface resets
```

**D** - een trunk met een MTU-verschil:
```
     0 runts, 6620 giants, 0 throttles
     6620 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
```

### Deel 5 - Kabels en glasvezel (theorie)
1. Wanneer gebruik je een **straight-through** en wanneer een **crossover**-kabel? Wat doet **Auto-MDIX**? (Bekijk in Packet Tracer welk kabeltype tussen SW1 en SW2 zit, en welk tussen R1 en SW1.)
2. Wat is het verschil tussen **T568A** en **T568B**? Welke aders (pinnen) worden bij 100BASE-TX gebruikt, en welke bij 1000BASE-T?
3. Wat is de maximale lengte van een koperverbinding (Cat5e/Cat6) bij 1 Gb/s? Wat gebeurt er met de tellers als een kabel te lang is?
4. Vergelijk **multimode (MMF)** en **singlemode (SMF)** glasvezel: kern, lichtbron, afstand en typische toepassing. Noem bij 1 Gb/s en 10 Gb/s een SFP-type voor elk (bijvoorbeeld 1000BASE-SX/LX, 10GBASE-SR/LR).
5. Een glasvezel-uplink gaat steeds even down. Je ziet:
```
Switch# show interfaces TenGigabitEthernet1/1/1 transceiver detail
                              Optical   High Alarm  High Warn  Low Warn   Low Alarm
           Port       Lane    Power     Threshold   Threshold  Threshold  Threshold
           ---------  ----    -------   ---------   ---------  ---------  ---------
Transmit   Te1/1/1    N/A      -2.3        1.7        -1.3       -7.3       -11.3
Receive    Te1/1/1    N/A     -13.9        2.0        -1.0       -9.9       -13.9
```
   Wat zegt dit over het signaal, en welke oorzaken controleer je (noem er minstens drie)?

## Reflectie
1. Welke van de onderwerpen uit dit leerdoel kun je in een simulatie wél zien, en welke niet? Waarom?
2. Met welke drie commando's begin jij voortaan als iemand zegt "het netwerk is traag"?

Je docent bespreekt daarna je antwoorden.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
