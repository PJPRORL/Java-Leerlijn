# 02_03

<b>Hoofdstuk 02</b> &nbsp;·&nbsp; Tabellen en geneste lussen &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#f03e3e">●●●</span> &nbsp;·&nbsp; ±2 uur

## Leerdoel

Na deze oefening kan je een tabel bewaren in een tweedimensionale array, en ze gebruiken om twee dingen met elkaar te vergelijken.

Je leert geneste lussen schrijven, tot zes diep, en met `continue` een combinatie overslaan zodra je weet dat ze niet deugt.

Daarnaast oefen je op invoer die je opnieuw vraagt tot ze klopt, met `do-while`, en op het zoeken naar het beste element in een reeks die je niet bewaart.

## Opdracht

De configurator van Bit & Byte uit 01_03 rekent juist, maar de klant moet elke prijs en elk vermogen zelf intypen, en hij weet niet of zijn onderdelen samen passen. Het team wil dat de configurator de onderdelen kent, de compatibiliteit controleert, en zelf voorstellen doet binnen een budget.

Jouw taak is om de configurator de catalogus en de compatibiliteitsregels te laten kennen.

Het programma moet, net als in 01_03:

1. een buildrapport tonen met de prijs in cent, de btw tot op de cent en het voedingsadvies;

en daarnaast:

2. de onderdelen per soort tonen;
3. een build laten kiezen uit de lijsten, met invoer die opnieuw gevraagd wordt tot ze klopt;
4. de vijf compatibiliteitsregels controleren;
5. tellen hoeveel van alle mogelijke builds geldig zijn;
6. de goedkoopste geldige build vinden;
7. alle geldige builds binnen een budget tonen.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 01_03 naar een nieuwe map `Oefening_02_03`. Deze rekenregels blijven precies dezelfde:

| Wat | Hoe |
|---|---|
| bedragen | in cent, als `int`; tonen in euro met een komma en twee cijfers |
| zonder btw | `(onderdelen * 100 + 60) / 121` |
| voedingsadvies | (watt processor + watt videokaart + 75 W) × 1,3, naar boven op 50 W |
| het buildrapport | dezelfde labels: *Onderdelen*, *zonder btw*, *btw 21 %*, *Vermogen*, *Advies*, *Voeding* |

---

### 2. De onderdelen in arrays

Bewaar de onderdelen van de catalogus in arrays, per soort. De prijzen in cent:

| Soort | Onderdelen |
|---|---|
| processor | Ryzen 5 7600X, 19900, 105 W · Ryzen 7 7800X3D, 38900, 120 W · Core i5-14600K, 29900, 125 W |
| moederbord | B650 Tomahawk, 21900 · B760M DS3H DDR4, 11900 |
| geheugen | 32 GB DDR5-6000, 10900 · 16 GB DDR4-3200, 4500 |
| videokaart | GeForce RTX 4070, 58900, 200 W, 267 mm · Radeon RX 7800 XT, 49900, 263 W, 320 mm |
| voeding | Voeding 550 W, 6900 · Voeding 750 W, 10900 · Voeding 850 W, 13900 |
| behuizing | Compact M, 7900, videokaart tot 300 mm · Midi Air, 9900, videokaart tot 360 mm |

Toon bij de start alle lijsten. In dit punt blijft de rest van je programma zoals in 01_03.

```text
Bit & Byte - configurator
Processors:
1. Ryzen 5 7600X - 199,00 euro - 105 W
2. Ryzen 7 7800X3D - 389,00 euro - 120 W
3. Core i5-14600K - 299,00 euro - 125 W
Moederborden:
1. B650 Tomahawk - 219,00 euro
2. B760M DS3H DDR4 - 119,00 euro
...
Behuizingen:
1. Compact M - 79,00 euro - videokaart tot 300 mm
2. Midi Air - 99,00 euro - videokaart tot 360 mm
```

> [!TIP]
> Een onderdeel heeft meerdere gegevens: naam, prijs, vermogen. Bewaar elk gegeven in een eigen array, met dezelfde index voor hetzelfde onderdeel: `cpuNamen[2]`, `cpuPrijzen[2]` en `cpuWatt[2]` horen samen.

---

### 3. Een build kiezen

De klant typt geen prijzen meer: hij kiest per soort een nummer uit de lijst. Vraag het nummer opnieuw zolang het niet in de lijst staat:

```text
Processor (1-3): 4
Processor (1-3): 0
Processor (1-3): 1
Moederbord (1-2): 1
Geheugen (1-2): 3
Geheugen (1-2): 1
Videokaart (1-2): 1
Voeding (1-3): 1
Behuizing (1-2): 2
```

Haal de prijzen en de vermogens daarna uit de arrays, en toon het buildrapport van 01_03, met bovenaan de gekozen onderdelen:

```text
==============================
BIT & BYTE - BUILDRAPPORT
==============================
Processor    : Ryzen 5 7600X
Moederbord   : B650 Tomahawk
Geheugen     : 32 GB DDR5-6000
Videokaart   : GeForce RTX 4070
Voeding      : Voeding 550 W
Behuizing    : Midi Air
------------------------------
Onderdelen   : 1284,00 euro
  zonder btw : 1061,16 euro
  btw 21 %   : 222,84 euro
Vermogen     : 380 W
Advies       : 500 W
Voeding      : 550 W (volstaat, marge 50 W)
==============================
```

> [!TIP]
> Een vraag die je herhaalt tot het antwoord klopt, is het schoolvoorbeeld van `do-while`: je stelt ze minstens één keer (leerboek 1.6).

---

### 4. De compatibiliteitstabel

Of een processor op een moederbord past, hangt af van de socket. Bit & Byte krijgt dat van zijn leverancier als een tabel, en die bewaar je zoals ze is, in een tweedimensionale array van `boolean` (leerboek 1.7, *Meerdimensionale arrays*):

| Past op | B650 Tomahawk | B760M DS3H DDR4 |
|---|---|---|
| Ryzen 5 7600X | ja | nee |
| Ryzen 7 7800X3D | ja | nee |
| Core i5-14600K | nee | ja |

Maak op dezelfde manier een tabel voor het geheugen op het moederbord, en een voor het moederbord in de behuizing:

| Geheugen past op | B650 Tomahawk | B760M DS3H DDR4 |
|---|---|---|
| 32 GB DDR5-6000 | ja | nee |
| 16 GB DDR4-3200 | nee | ja |

| Moederbord past in | Compact M | Midi Air |
|---|---|---|
| B650 Tomahawk | nee | ja |
| B760M DS3H DDR4 | ja | ja |

Controleer daarmee de vijf regels, in deze volgorde, en vervang de laatste regel van het rapport door dit blok:

```text
------------------------------
1. Socket       : ok
2. Geheugentype : ok
3. Formfactor   : ok
4. Lengte       : ok
5. Voeding      : 550 W (volstaat, marge 50 W)
Compatibel      : ja
==============================
```

| Regel | Fout als | Wat het rapport dan toont |
|---|---|---|
| 1. Socket | de socket-tabel zegt nee | <code style="color:#845ef7">past niet</code> |
| 2. Geheugentype | de geheugen-tabel zegt nee | <code style="color:#845ef7">past niet</code> |
| 3. Formfactor | de behuizing-tabel zegt nee | <code style="color:#845ef7">past niet</code> |
| 4. Lengte | de videokaart is langer dan de kast toelaat | <code style="color:#845ef7">te lang (320 mm, kast tot 300 mm)</code> |
| 5. Voeding | de voeding levert minder dan het advies | de zin van 01_03: <code style="color:#845ef7">te zwak, 50 W tekort</code> |

De laatste regel telt de problemen: <code style="color:#845ef7">Compatibel      : ja</code>, of <code style="color:#845ef7">nee (1 probleem)</code>, <code style="color:#845ef7">nee (4 problemen)</code>.

| Keuzes | Wat het rapport toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">5. Voeding      : 550 W (te zwak, 50 W tekort)</code><br><code style="color:#845ef7">Compatibel      : nee (1 probleem)</code> |
| <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">1. Socket       : past niet</code><br><code style="color:#845ef7">3. Formfactor   : past niet</code><br><code style="color:#845ef7">4. Lengte       : te lang (320 mm, kast tot 300 mm)</code><br><code style="color:#845ef7">Compatibel      : nee (4 problemen)</code> |
| <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">5. Voeding      : 550 W (volstaat, marge 0 W)</code><br><code style="color:#845ef7">Compatibel      : ja</code> |

> [!NOTE]
> Waarom een tabel, en geen socket als tekst bij elk onderdeel? Teksten vergelijken leer je pas in hoofdstuk 03. En een tabel van de leverancier mag je niet zelf gaan herberekenen: ze zegt wat er past, ook in gevallen die je niet uit één eigenschap kan afleiden.

> [!WARNING]
> De index in de tabel is de rij, dan de kolom: `socketPast[processor][moederbord]`. Draai je ze om, dan compileert je code nog, maar vraag je de verkeerde cel. Bij de Core i5-14600K op het B650 Tomahawk-bord krijg je dan zelfs een `ArrayIndexOutOfBoundsException`, want de tabel heeft maar 2 kolommen.

---

### 5. Alle combinaties

Hoeveel verschillende builds kan een klant samenstellen, en hoeveel daarvan zijn geldig? Doorloop alle combinaties met zes geneste lussen, één per soort, en tel ze. Een combinatie is geldig als ze de vijf regels van punt 4 doorstaat.

Toon na het rapport:

<code style="color:#845ef7">Geldige builds: 18 van 144</code>

> [!TIP]
> Zodra je weet dat een combinatie niet deugt, moet je de rest van de ronde niet meer uitrekenen. `continue` gaat meteen naar de volgende combinatie (leerboek 1.6).

> [!WARNING]
> Zes lussen hebben zes tellers nodig, elk met een andere naam. Gebruik je in een binnenste lus per ongeluk de teller van een buitenste, dan compileert je code, maar telt ze nonsens.

---

### 6. De goedkoopste geldige build

Zoek tijdens dezelfde lussen de goedkoopste geldige build, en toon ze:

```text
Goedkoopste geldige build: 1170,00 euro
  Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, Radeon RX 7800 XT, Voeding 750 W, Midi Air
```

Je bewaart niet alle builds. Je onthoudt enkel de laagste prijs tot nu toe, en de zes indexen die erbij horen.

> [!TIP]
> Welke beginwaarde geef je de laagste prijs, zodat de eerste geldige build zeker goedkoper is? Kijk in naslag 10.2 naar `Integer.MAX_VALUE`.

> [!NOTE]
> Valt het je op dat de goedkoopste build de duurdere videokaart heeft? De Radeon RX 7800 XT is goedkoper dan de GeForce RTX 4070, maar vraagt een sterkere voeding en past enkel in de Midi Air. Precies daarom laat Bit & Byte de configurator rekenen.

---

### 7. Alles samen: alle geldige builds onder een budget

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Een klant zegt hoeveel hij wil uitgeven, en de configurator stelt voor wat binnen dat budget past. In dit laatste punt combineer je de arrays, de tabellen, de geneste lussen en het rekenen in cent.

Vraag na het rapport het budget: <code style="color:#845ef7">Budget in euro: </code>. Toon tijdens dezelfde lussen van punt 5 elke geldige build die niet meer kost dan het budget, met haar prijs, in de volgorde waarin je lussen ze vinden. Daaronder hoeveel het er zijn, en daarna, zoals in punt 5 en 6, de telling en de goedkoopste.

```text
Budget in euro: 1250
1234,00 euro: Ryzen 5 7600X, B650 Tomahawk, 32 GB DDR5-6000, Radeon RX 7800 XT, Voeding 750 W, Midi Air
1200,00 euro: Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, GeForce RTX 4070, Voeding 550 W, Compact M
1220,00 euro: Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, GeForce RTX 4070, Voeding 550 W, Midi Air
1240,00 euro: Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, GeForce RTX 4070, Voeding 750 W, Compact M
1170,00 euro: Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, Radeon RX 7800 XT, Voeding 750 W, Midi Air
1200,00 euro: Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, Radeon RX 7800 XT, Voeding 850 W, Midi Air
6 geldige build(s) binnen 1250,00 euro.
Geldige builds: 18 van 144
Goedkoopste geldige build: 1170,00 euro
  Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, Radeon RX 7800 XT, Voeding 750 W, Midi Air
```

Past er niets in het budget, dan toont je programma <code style="color:#845ef7">Geen geldige build binnen 1100,00 euro.</code>

> [!IMPORTANT]
> Het budget lees je in als `double` en zet je meteen om naar cent met `Math.round`, zoals in 01_03. Een build van precies het budget telt mee.

Test je configurator en controleer:

- met een budget van <code style="color:#e64980;font-weight:600">1300</code>: <code style="color:#845ef7">11 geldige build(s) binnen 1300,00 euro.</code>;
- met een budget van <code style="color:#e64980;font-weight:600">1100</code>: geen enkele build;
- met een budget van <code style="color:#e64980;font-weight:600">1170</code>: precies één build, de goedkoopste.

> [!NOTE]
> De omzetting van cent naar euro staat nu meer dan tien keer in je code. Elke kopie is een plaats waar een tikfout kan zitten. In hoofdstuk 03 maak je er één methode van.
