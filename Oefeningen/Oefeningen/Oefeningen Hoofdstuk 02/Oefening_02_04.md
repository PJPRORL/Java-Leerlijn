# 02_04

<b>Hoofdstuk 02</b> &nbsp;·&nbsp; Arrays en een menu &nbsp;·&nbsp; Dierenasiel &nbsp;·&nbsp; niveau <span style="color:#37b24d">●○○</span> &nbsp;·&nbsp; ±75 min

## Leerdoel

Na deze oefening kan je een reeks gegevens bewaren in arrays en ze doorlopen met een lus.

Je leert met `if` en `else` een verkeerde keuze weigeren, en je bouwt een menu dat blijft draaien tot de gebruiker stopt, met een `switch` in een lus.

Daarnaast oefen je op wat je met een lus over een array allemaal kan: zoeken, tellen, optellen en het kleinste of grootste element vinden.

## Opdracht

Het programma van Dierenasiel Pootjes uit 01_04 maakt een dierfiche, maar de vrijwilliger moet elk dier zelf intypen, en na één fiche stopt het programma. Het asiel wil een programma dat de dieren kent, adopties bijhoudt, en een overzicht geeft.

Jouw taak is om het programma de dieren van het asiel te laten kennen, en het te laten draaien tot de vrijwilliger stopt.

Het programma moet, net als in 01_04:

1. een dierfiche kunnen tonen, met levensfase, voer en adoptieprijs;

en daarnaast:

2. de zes dieren van het asiel kunnen tonen;
3. een adoptie kunnen weigeren als het dier niet bestaat of niet beschikbaar is;
4. een menu tonen tot de vrijwilliger stopt;
5. alle dieren tot een maximumleeftijd kunnen zoeken;
6. een statistiek van het asiel kunnen tonen;
7. een asieloverzicht kunnen tonen.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 01_04 naar een nieuwe map `Oefening_02_04`. Na deze oefening moet dit nog precies hetzelfde werken als vandaag:

| Wat | Hoe het nu werkt |
|---|---|
| de dierfiche | dezelfde lijnen, met dezelfde labels en dezelfde berekeningen: leeftijd in jaren en maanden, levensfase, voer per dag, adoptieprijs |
| een naam na een getal | ook met een spatie, zonder het Enter-probleem |

---

### 2. Het asiel in arrays

Bewaar de zes dieren van het asiel in vijf arrays: namen, soortnummers (1 = hond, 2 = kat, 3 = konijn), leeftijden in maanden, gewichten en statussen.

| Id | Naam | Soort | Leeftijd | Gewicht | Status |
|---|---|---|---|---|---|
| 1 | Rex | 1 | 30 | 28.5 | 0 |
| 2 | Luna | 2 | 7 | 3.1 | 0 |
| 3 | Max | 1 | 110 | 31.0 | 0 |
| 4 | Pluis | 3 | 14 | 2.2 | 1 |
| 5 | Mimi | 2 | 50 | 4.4 | 0 |
| 6 | Bobby | 1 | 4 | 6.0 | 0 |

De status is een getal: 0 = <code style="color:#845ef7">beschikbaar</code>, 1 = <code style="color:#845ef7">gereserveerd</code>, 2 = <code style="color:#845ef7">geadopteerd</code>.

Toon de dieren bij de start met een `for`-lus. Kies het woord voor de status met een `switch` (leerboek 1.5). Daarna vraagt het programma niet langer naar naam, soort, leeftijd en gewicht, maar enkel naar het nummer van het dier, en de naam van wie het adopteert. De dierfiche blijft zoals in 01_04; enkel de laatste regel wordt <code style="color:#845ef7">Adoptant     : ...</code> in plaats van de verzorger.

```text
Dierenasiel - onthaal
1. Rex (hond) - 30 maanden - 28.5 kg - beschikbaar
2. Luna (kat) - 7 maanden - 3.1 kg - beschikbaar
3. Max (hond) - 110 maanden - 31.0 kg - beschikbaar
4. Pluis (konijn) - 14 maanden - 2.2 kg - gereserveerd
5. Mimi (kat) - 50 maanden - 4.4 kg - beschikbaar
6. Bobby (hond) - 4 maanden - 6.0 kg - beschikbaar
Dier (1-6): 2
Adoptant: Lotte Peeters
==============================
DIERENASIEL - DIERFICHE
==============================
Naam         : Luna
...
Adoptieprijs : 90 euro
Adoptant     : Lotte Peeters
==============================
```

> [!TIP]
> Laat de lus lopen tot `namen.length`, niet tot 6. Komt er een dier bij, dan moet je de lus niet aanpassen.

---

### 3. Een adoptie weigeren

Controleer voor je iets uitrekent of verandert, in deze volgorde:

| Situatie | Wat je programma toont |
|---|---|
| het nummer ligt niet tussen 1 en 6 | <code style="color:#845ef7">Dier 9 bestaat niet.</code> |
| het dier is gereserveerd | <code style="color:#845ef7">Pluis is gereserveerd.</code> |
| het dier is al geadopteerd | <code style="color:#845ef7">Luna is al geadopteerd.</code> |

In die gevallen toont je programma geen fiche, en vraagt het ook geen adoptant. Lukt de adoptie wel, dan zet je de status in de array op 2.

> [!WARNING]
> Controleer het nummer voor je in een array kijkt. `statussen[8]` bestaat niet: je programma stopt dan met een `ArrayIndexOutOfBoundsException` (leerboek 1.7).

---

### 4. Een menu in een lus

Laat je programma een menu tonen, tot de vrijwilliger `0` kiest:

```text
--- Menu ---
1. Overzicht
2. Adopteren
0. Stoppen
Keuze: 
```

| Keuze | Wat je programma doet |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> | de dieren tonen, zoals in punt 2 |
| <code style="color:#e64980;font-weight:600">2</code> | een adoptie, zoals in punt 3 |
| <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Tot ziens!</code> tonen en stoppen |
| iets anders | <code style="color:#845ef7">Ongeldige keuze.</code> tonen, en het menu opnieuw tonen |

Test je programma en controleer:

- adopteer Luna; probeer ze daarna een tweede keer te adopteren: <code style="color:#845ef7">Luna is al geadopteerd.</code>;
- of het overzicht daarna <code style="color:#845ef7">2. Luna (kat) - 7 maanden - 3.1 kg - geadopteerd</code> toont.

---

### 5. Zoeken tot een maximumleeftijd

Voeg aan het menu de keuze <code style="color:#845ef7">3. Zoeken tot een maximumleeftijd</code> toe. Je programma vraagt <code style="color:#845ef7">Maximumleeftijd in maanden: </code> en toont alle dieren die hoogstens zo oud zijn:

| Maximumleeftijd | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">12</code> | <code style="color:#845ef7">2. Luna - 7 maanden</code><br><code style="color:#845ef7">6. Bobby - 4 maanden</code><br><code style="color:#845ef7">2 dier(en) tot 12 maanden.</code> |
| <code style="color:#e64980;font-weight:600">14</code> | <code style="color:#845ef7">2. Luna - 7 maanden</code><br><code style="color:#845ef7">4. Pluis - 14 maanden</code><br><code style="color:#845ef7">6. Bobby - 4 maanden</code><br><code style="color:#845ef7">3 dier(en) tot 14 maanden.</code> |
| <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Geen dieren tot 2 maanden.</code> |

---

### 6. Statistiek

Voeg de keuze <code style="color:#845ef7">4. Statistiek</code> toe. Doorloop de arrays één keer, en toon:

<code style="color:#845ef7">Gemiddeld gewicht: 12.53 kg</code><br>
<code style="color:#845ef7">Jongste: Bobby (4 maanden)</code><br>
<code style="color:#845ef7">Oudste: Max (110 maanden)</code><br>
<code style="color:#845ef7">Beschikbaar: 5</code>

Rond het gemiddelde gewicht af op twee cijfers na de komma.

> [!TIP]
> Onthoud voor het jongste en het oudste dier de *index*, niet de leeftijd. Met de index vind je daarna zowel de naam als de leeftijd terug.

---

### 7. Alles samen: het asieloverzicht

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Op het einde van de dag wil het asiel weten waar elk dier staat, wat de adopties opgebracht hebben, en welke dieren extra aandacht nodig hebben. In dit laatste punt combineer je de arrays, de lus, de `if` en de adopties van punt 3.

Voeg de keuze <code style="color:#845ef7">5. Asieloverzicht</code> toe. Bij de start van het programma toont ze:

```text
--- Asieloverzicht ---
1. Rex: beschikbaar
2. Luna: beschikbaar
3. Max: beschikbaar
4. Pluis: gereserveerd
5. Mimi: beschikbaar
6. Bobby: beschikbaar
Beschikbaar: 5, gereserveerd: 1, geadopteerd: 0
Opbrengst adopties: 0 euro
Moeilijk te plaatsen:
- Max
```

| Regel | Betekenis |
|---|---|
| de tellingen | hoeveel dieren elke status hebben |
| Opbrengst adopties | de som van de adoptieprijzen van de geadopteerde dieren, met de seniorkorting van 01_04 |
| Moeilijk te plaatsen | elk beschikbaar dier van 96 maanden of ouder; is er geen, dan <code style="color:#845ef7">- niemand</code> |

> [!IMPORTANT]
> Het overzicht toont de toestand van dit moment. Na een adoptie moet het kloppen, zonder dat je iets opnieuw intypt.

Test je overzicht en controleer:

- adopteer Max en Luna, en vraag dan het overzicht: <code style="color:#845ef7">Beschikbaar: 3, gereserveerd: 1, geadopteerd: 2</code>, <code style="color:#845ef7">Opbrengst adopties: 165 euro</code> en <code style="color:#845ef7">- niemand</code> onder *Moeilijk te plaatsen*;
- of een adoptie die geweigerd werd, niets verandert aan het overzicht.

> [!NOTE]
> De adoptieprijs reken je nu op twee plaatsen uit: op de fiche en in het overzicht. Verandert de prijs van een kat, dan moet je twee plaatsen aanpassen. In hoofdstuk 03 maak je er één methode van.
