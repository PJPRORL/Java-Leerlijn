# 02_01

<b>Hoofdstuk 02</b> &nbsp;·&nbsp; Arrays en een menu &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#37b24d">●○○</span> &nbsp;·&nbsp; ±75 min

## Leerdoel

Na deze oefening kan je een reeks gegevens bewaren in arrays en ze doorlopen met een lus.

Je leert met `if` en `else` een verkeerde invoer weigeren, en je bouwt een menu dat blijft draaien tot de gebruiker stopt, met een `switch` in een lus.

Daarnaast oefen je op wat je met een lus over een array allemaal kan: zoeken, tellen, optellen en het kleinste of grootste element vinden.

## Opdracht

De kassa van Bit & Byte uit 01_01 werkt, maar de verkoper moet bij elke verkoop de naam, het merk, de prijs en de voorraad zelf intypen. Daar gaat het geregeld mis. En na elke verkoop stopt het programma.

Jouw taak is om de kassa de catalogus te laten kennen, en ze te laten draaien tot de verkoper stopt.

Het programma moet, net als in 01_01:

1. een verkoop kunnen afrekenen op een kassabon;

en daarnaast:

2. de catalogus van zes artikelen kunnen tonen;
3. een verkoop kunnen weigeren als het artikel niet bestaat of de voorraad te klein is;
4. een menu tonen tot de verkoper stopt;
5. alle artikelen tot een maximumprijs kunnen zoeken;
6. een statistiek van de catalogus kunnen tonen;
7. een voorraadoverzicht kunnen tonen.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 01_01 naar een nieuwe map `Oefening_02_01`. Na deze oefening moet dit nog precies hetzelfde werken als vandaag:

| Wat | Hoe het nu werkt |
|---|---|
| de kassabon | dezelfde lijnen, met dezelfde labels en dezelfde berekeningen: subtotaal, verzendkost onder 50 euro, te betalen, waarvan btw |
| de voorraad na verkoop | met de status <code style="color:#845ef7">uitverkocht</code>, <code style="color:#845ef7">bijna uitverkocht</code> of <code style="color:#845ef7">op voorraad</code> |
| de naam van de klant | na het aantal, ook als die een spatie bevat |

> [!TIP]
> Start je programma één keer met de invoer uit 01_01, en bewaar wat je kassabon toont. Na deze oefening moet een verkoop van 2 stuks Ryzen 5 7600X aan Lotte Peeters exact dezelfde bon geven.

---

### 2. De catalogus in arrays

Bewaar de zes artikelen van de catalogus in vijf arrays: één voor de namen, één voor de merken, één voor de soorten, één voor de prijzen en één voor de voorraden (leerboek 1.7).

| Id | Naam | Merk | Soort | Prijs | Voorraad |
|---|---|---|---|---|---|
| 1 | B650 Tomahawk | MSI | moederbord | 219.00 | 12 |
| 2 | Ryzen 5 7600X | AMD | processor | 199.00 | 3 |
| 3 | Ryzen 7 7800X3D | AMD | processor | 389.00 | 5 |
| 4 | GeForce RTX 4070 | ASUS | videokaart | 589.00 | 0 |
| 5 | Radeon RX 7800 XT | Sapphire | videokaart | 499.00 | 2 |
| 6 | NH-D15 | Noctua | koeler | 109.00 | 7 |

Het Id is de plaats in de arrays plus 1: artikel 1 staat op index 0.

Toon de catalogus bij de start met een `for`-lus. Daarna vraagt de kassa niet langer naar naam, merk, prijs en voorraad, maar enkel naar het nummer van het artikel. De rest van de verkoop blijft zoals in 01_01.

```text
Bit & Byte - kassa
1. B650 Tomahawk (MSI, moederbord) - 219.0 euro - 12 stuks
2. Ryzen 5 7600X (AMD, processor) - 199.0 euro - 3 stuks
3. Ryzen 7 7800X3D (AMD, processor) - 389.0 euro - 5 stuks
4. GeForce RTX 4070 (ASUS, videokaart) - 589.0 euro - 0 stuks
5. Radeon RX 7800 XT (Sapphire, videokaart) - 499.0 euro - 2 stuks
6. NH-D15 (Noctua, koeler) - 109.0 euro - 7 stuks
Artikel (1-6): 2
Aantal: 2
Klant: Lotte Peeters
==============================
BIT & BYTE - KASSABON
==============================
Klant       : Lotte Peeters
Artikel     : Ryzen 5 7600X (AMD)
...
```

> [!TIP]
> Laat de lus lopen tot `namen.length`, niet tot 6. Komt er later een artikel bij, dan moet je de lus niet aanpassen.

> [!NOTE]
> Je voorraad heette in 01_01 `voorraad`. Geef de array een andere naam, bijvoorbeeld `voorraden`, zodat de variabele van één artikel haar naam kan houden.

---

### 3. Een verkoop weigeren

Tot nu toe verkocht je kassa alles. Controleer voor je iets uitrekent of verandert, in deze volgorde:

| Situatie | Wat je programma toont |
|---|---|
| het nummer ligt niet tussen 1 en 6 | <code style="color:#845ef7">Artikel 9 bestaat niet.</code> |
| het aantal is kleiner dan 1 | <code style="color:#845ef7">Het aantal moet minstens 1 zijn.</code> |
| het aantal is groter dan de voorraad | <code style="color:#845ef7">Niet genoeg op voorraad: nog 2 stuks.</code> |

In die gevallen toont je programma geen kassabon, en vraagt het ook de naam van de klant niet.

Lukt de verkoop wel, dan verlaag je de voorraad in de array. Zo weet de kassa het bij de volgende verkoop.

> [!WARNING]
> Controleer het nummer voor je in een array kijkt. `voorraden[8]` bestaat niet: je programma stopt dan met een `ArrayIndexOutOfBoundsException` (leerboek 1.7).

Test je programma en controleer:

- <code style="color:#e64980;font-weight:600">9</code> en <code style="color:#e64980;font-weight:600">0</code>: telkens <code style="color:#845ef7">Artikel ... bestaat niet.</code>, zonder dat je programma naar het aantal vraagt;
- artikel <code style="color:#e64980;font-weight:600">5</code> met aantal <code style="color:#e64980;font-weight:600">3</code>: <code style="color:#845ef7">Niet genoeg op voorraad: nog 2 stuks.</code>;
- artikel <code style="color:#e64980;font-weight:600">4</code> met aantal <code style="color:#e64980;font-weight:600">1</code>: <code style="color:#845ef7">Niet genoeg op voorraad: nog 0 stuks.</code>;
- artikel <code style="color:#e64980;font-weight:600">5</code> met aantal <code style="color:#e64980;font-weight:600">2</code>: een kassabon met <code style="color:#845ef7">Voorraad na verkoop: 0 (uitverkocht)</code>.

---

### 4. Een menu in een lus

Een kassa die na één verkoop stopt, heeft weinig zin. Laat je programma een menu tonen, tot de verkoper `0` kiest:

```text
--- Menu ---
1. Catalogus
2. Verkopen
0. Stoppen
Keuze: 
```

| Keuze | Wat je programma doet |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> | de catalogus tonen, zoals in punt 2 |
| <code style="color:#e64980;font-weight:600">2</code> | een verkoop, zoals in punt 3 |
| <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Tot ziens!</code> tonen en stoppen |
| iets anders | <code style="color:#845ef7">Ongeldige keuze.</code> tonen, en het menu opnieuw tonen |

Kies de juiste lus: het menu moet minstens één keer verschijnen (leerboek 1.6). Kies met een `switch` wat er bij elke keuze gebeurt (leerboek 1.5).

> [!TIP]
> Sinds Java 14 schrijf je een `switch` ook met pijlen: `case 1 -> { ... }`. Dan heb je geen `break` meer nodig, en kan je die ook niet vergeten (leerboek 1.5, *Moderne switch*).

Test je programma en controleer:

- of de catalogus na een verkoop van 2 Ryzen 5 7600X <code style="color:#845ef7">2. Ryzen 5 7600X (AMD, processor) - 199.0 euro - 1 stuks</code> toont;
- of <code style="color:#e64980;font-weight:600">7</code> <code style="color:#845ef7">Ongeldige keuze.</code> geeft, en het menu daarna opnieuw verschijnt.

---

### 5. Zoeken tot een maximumprijs

Voeg aan het menu de keuze <code style="color:#845ef7">3. Zoeken tot een maximumprijs</code> toe. Je programma vraagt <code style="color:#845ef7">Maximumprijs: </code> en toont alle artikelen die hoogstens zoveel kosten. Daaronder staat hoeveel het er waren:

| Maximumprijs | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">200</code> | <code style="color:#845ef7">2. Ryzen 5 7600X - 199.0 euro</code><br><code style="color:#845ef7">6. NH-D15 - 109.0 euro</code><br><code style="color:#845ef7">2 artikel(en) tot 200.0 euro.</code> |
| <code style="color:#e64980;font-weight:600">199,00</code> | <code style="color:#845ef7">2. Ryzen 5 7600X - 199.0 euro</code><br><code style="color:#845ef7">6. NH-D15 - 109.0 euro</code><br><code style="color:#845ef7">2 artikel(en) tot 199.0 euro.</code> |
| <code style="color:#e64980;font-weight:600">100</code> | <code style="color:#845ef7">Geen artikelen tot 100.0 euro.</code> |

> [!NOTE]
> *Hoogstens* 199 euro betekent dat een artikel van precies 199 euro meedoet.

---

### 6. Statistiek

Voeg de keuze <code style="color:#845ef7">4. Statistiek</code> toe. Doorloop de arrays één keer, en toon:

<code style="color:#845ef7">Gemiddelde prijs: 334.0 euro</code><br>
<code style="color:#845ef7">Goedkoopste: NH-D15 (109.0 euro)</code><br>
<code style="color:#845ef7">Duurste: GeForce RTX 4070 (589.0 euro)</code><br>
<code style="color:#845ef7">Stuks op voorraad: 29</code>

Rond de gemiddelde prijs af op twee cijfers na de komma.

> [!TIP]
> Onthoud voor de goedkoopste en de duurste de *index*, niet de prijs. Met de index vind je daarna zowel de naam als de prijs terug.

Test je programma en controleer:

- of na een verkoop van 7 stuks NH-D15 <code style="color:#845ef7">Stuks op voorraad: 22</code> verschijnt, terwijl de goedkoopste nog altijd de NH-D15 is.

---

### 7. Alles samen: het voorraadoverzicht

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Het magazijn wil bij het sluiten van de winkel weten wat er nog ligt en wat het moet bijbestellen. In dit laatste punt combineer je de arrays, de lus, de `if` en de verkopen van punt 3.

Voeg de keuze <code style="color:#845ef7">5. Voorraadoverzicht</code> toe. Bij de start van het programma toont ze:

```text
--- Voorraadoverzicht ---
1. B650 Tomahawk: 12 (op voorraad)
2. Ryzen 5 7600X: 3 (op voorraad)
3. Ryzen 7 7800X3D: 5 (op voorraad)
4. GeForce RTX 4070: 0 (uitverkocht)
5. Radeon RX 7800 XT: 2 (bijna uitverkocht)
6. NH-D15: 7 (op voorraad)
Voorraadwaarde: 6931.0 euro
Uitverkocht: 1
Bijbestellen:
- GeForce RTX 4070
- Radeon RX 7800 XT
```

| Regel | Betekenis |
|---|---|
| de status | dezelfde drie statussen als op de kassabon, nu met een `if`-ladder in plaats van ternary operators |
| Voorraadwaarde | de som van prijs × voorraad over alle artikelen |
| Uitverkocht | het aantal artikelen met voorraad 0 |
| Bijbestellen | elk artikel met een voorraad van 2 of minder, elk op een eigen regel; is er geen, dan <code style="color:#845ef7">- niets</code> |

> [!IMPORTANT]
> Het overzicht toont de voorraad van dit moment. Na een verkoop moet ze kloppen met wat er verkocht is, zonder dat je iets opnieuw intypt.

Test je overzicht en controleer:

- verkoop 2 stuks Ryzen 5 7600X en 5 stuks NH-D15, en vraag dan het overzicht: <code style="color:#845ef7">2. Ryzen 5 7600X: 1 (bijna uitverkocht)</code>, <code style="color:#845ef7">6. NH-D15: 2 (bijna uitverkocht)</code>, <code style="color:#845ef7">Voorraadwaarde: 5988.0 euro</code>, en vier artikelen onder <code style="color:#845ef7">Bijbestellen:</code>;
- of een verkoop die geweigerd werd, niets verandert aan het overzicht.

> [!NOTE]
> De kassabon staat nu in het midden van een `switch`, met de berekeningen ertussen. Je hoofdprogramma wordt lang en moeilijk te lezen. In hoofdstuk 03 haal je stukken eruit, in methodes.
