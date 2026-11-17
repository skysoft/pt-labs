# Lab 2.1.1 - Twee verdiepingen koppelen

CCNA 200-301 v2.0 - leerdoel 2.1. Configuratielab.

Packet Tracer-bestand: [2.1.1_twee_verdiepingen_koppelen.pkt](2.1.1_twee_verdiepingen_koppelen.pkt)

**Leerdoel (CCNA 200-301 v2.0, 2.1):** netwerkinfrastructuur verbinden (switch-to-switch en switch-to-router): fysieke L2-interfaces en 802.1Q-trunks.

## Scenario
Administratiekantoor Het Grootboek heeft twee verdiepingen. Op elke verdieping staat een switch (SW1 beneden, SW2 boven). SW2 is verbonden met router R1, die al is geconfigureerd als router-on-a-stick. Op beide verdiepingen zitten medewerkers van **Administratie (VLAN 10)** en **Verkoop (VLAN 20)**. **VLAN 99** is het beheer-VLAN en ook het native VLAN.

De switches zijn net uit de doos. Alleen dit is ingesteld: hostname, `vtp mode transparent`, het beheeradres op `interface Vlan99` en `ip default-gateway`. Jij configureert de VLAN's, de access-poorten en de trunks.

## Poortenplan
| Switch | Poort | Verbonden met | Functie |
|---|---|---|---|
| SW1 | Gi0/1 | SW2 Gi0/1 | trunk |
| SW1 | Fa0/1 | PC1 | access VLAN 10 |
| SW1 | Fa0/2 | PC2 | access VLAN 20 |
| SW2 | Gi0/2 | R1 G0/0 | trunk naar router |
| SW2 | Gi0/1 | SW1 Gi0/1 | trunk |
| SW2 | Fa0/1 | PC3 | access VLAN 10 |
| SW2 | Fa0/2 | PC4 | access VLAN 20 |

## VLAN's en adressen
| VLAN | Naam | Subnet | Gateway (R1) |
|---|---|---|---|
| 10 | ADMINISTRATIE | 192.168.10.0/24 | 192.168.10.1 |
| 20 | VERKOOP | 192.168.20.0/24 | 192.168.20.1 |
| 99 | BEHEER (native) | 192.168.99.0/24 | 192.168.99.1 |

| Apparaat | Adres |
|---|---|
| PC1 / PC3 | 192.168.10.11 / 192.168.10.12 |
| PC2 / PC4 | 192.168.20.11 / 192.168.20.12 |
| SW1 / SW2 (Vlan99) | 192.168.99.2 / 192.168.99.3 |

R1 is al correct geconfigureerd; je hoeft hem niet aan te passen.

## Toegang
- Router en switches: klik op het apparaat en open het tabblad **CLI**. Er is geen wachtwoord.
- Pc's: **Desktop → Command Prompt**.
- Na een wijziging aan een poort duurt het ongeveer 30 seconden (spanning tree) voordat er verkeer doorgaat. Met **Fast Forward Time** (onderaan) gaat dat sneller. De eerste ping na een wijziging kan mislukken door ARP; ping dan nog een keer.

## Opdrachten

### Deel 1 - Theorie
1. Wat doet DTP? Welke combinaties van `dynamic auto`, `dynamic desirable`, `trunk` en `access` leveren een trunk op?
2. Wat is het native VLAN, en waarom kies je daarvoor liever niet VLAN 1?
3. Waarom zou je `switchport nonegotiate` en een beperkte lijst `allowed vlan` instellen?

### Deel 2 - Uitgangssituatie
1. Bekijk op SW1 `show interfaces GigabitEthernet0/1 switchport`. Wat is de *administrative mode* en de *operational mode*?
2. Bekijk `show interfaces trunk` en `show ip interface brief`. Wat is de status van `Vlan99`, en waarom?
3. Ping vanaf PC1 naar PC3 en naar zijn gateway. Verklaar het resultaat.

### Deel 3 - Configureren
1. Maak op beide switches de VLAN's 10, 20 en 99 aan, met de namen uit de tabel.
2. Configureer de access-poorten volgens het poortenplan.
3. Configureer de trunk tussen SW1 en SW2 met deze instellingen:
   - altijd trunk, zonder DTP-onderhandeling;
   - native VLAN 99;
   - alleen de VLAN's 10, 20 en 99.
4. Configureer op SW2 de trunk naar R1 met dezelfde instellingen.

### Deel 4 - Verificatie
1. Bekijk `show interfaces trunk` op beide switches. Welke VLAN's staan in de laatste twee blokken, en wat betekenen die blokken?
2. Bekijk opnieuw de status van `Vlan99`. Wat is er veranderd, en waarom?
3. Ping vanaf PC1 naar:
   - PC3 (zelfde VLAN, andere verdieping);
   - PC4 (ander VLAN);
   - 192.168.99.3 (SW2).
4. Ping vanaf SW1 naar 192.168.99.1 en 192.168.99.3.

### Deel 5 - Reflectie
1. Hoe loopt een ping van PC2 naar PC3? Noem alle links, en of het frame daar getagd is.
2. Wat gebeurt er met het verkeer van VLAN 20 als je VLAN 20 uit de allowed list van de trunk SW1-SW2 haalt? Probeer het en zet het daarna terug.
3. Wat gebeurt er als je op SW1 het native VLAN weer op 1 zet? Kijk naar de meldingen op de console (`show logging`), naar `show interfaces trunk` en naar de ping van SW1 naar SW2. Zet het daarna terug.

Je docent bespreekt daarna je antwoorden.

## Opnieuw beginnen
Open het originele `.pkt`-bestand opnieuw. Sla je eigen werk op onder een andere naam.
