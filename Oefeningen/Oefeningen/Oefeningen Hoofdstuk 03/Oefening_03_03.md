# 03_03

<b>Hoofdstuk 03</b> &nbsp;·&nbsp; Codes, overloading en recursie &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#f03e3e">●●●</span> &nbsp;·&nbsp; ±2 uur

## Leerdoel

Na deze oefening kan je codes zoals `CPU-01` maken met `String.format` en ontleden met `indexOf` en `substring`, en een volledige regel codes controleren voor je ze gebruikt.

Je leert twee methodes met dezelfde naam schrijven die verschillen in hun parameters (overloading), en een methode die zichzelf oproept (recursie).

Daarnaast oefen je op een zoektocht die je met zes geneste lussen niet kan aanpassen, maar met recursie wel: dezelfde methode zoekt alle builds, of enkel de builds die een paar onderdelen van de klant behouden.

## Opdracht

De configurator van Bit & Byte uit 02_03 werkt, maar de klant kiest zijn onderdelen met zes keer een nummer, en de verkoper kan die nummers niet doorgeven aan het magazijn. Daar gebruiken ze codes zoals `GPU-02`. En een build die niet compatibel is, krijgt geen voorstel om ze te verbeteren.

Jouw taak is om de configurator met codes te laten werken, en een voorstel te laten doen.

Het programma moet, net als in 02_03:

1. een build controleren op de vijf regels, en alle geldige builds binnen een budget zoeken;

en daarnaast:

2. de bedragen en het voedingsadvies in methodes hebben;
3. elk onderdeel met een code tonen, in een tabel;
4. een build lezen uit één regel codes;
5. een build tonen als één regel codes;
6. alle builds zoeken met recursie;
7. een voorstel doen voor een build die niet compatibel is.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 02_03 naar een nieuwe map `Oefening_03_03`. Deze regels blijven precies dezelfde:

| Nr | Regel | Fout als |
|---|---|---|
| 1 | Socket | de socket van de processor verschilt van die van het moederbord |
| 2 | Geheugen | het geheugentype verschilt van dat van het moederbord |
| 3 | Formfactor | een ATX-moederbord in een mATX-kast |
| 4 | Lengte | de videokaart is langer dan de kast toelaat |
| 5 | Vermogen | de voeding levert minder dan het advies |

**Voedingsadvies** = (watt processor + watt videokaart + 75 W) × 1,3, naar boven afgerond op een veelvoud van 50 W. Alle bedragen in cent.

> [!TIP]
> Start je programma één keer met de build 1, 1, 1, 1, 1, 2 en een budget van 1250 euro, en bewaar de uitvoer. Na punt 2 moet je programma exact hetzelfde tonen.

---

### 2. Bedragen en advies in een methode

Een bedrag in cent tonen staat dertien keer in je code, met dezelfde drie stukken. Het voedingsadvies reken je twee keer uit. Zet ze in een methode:

| Methode | Geeft terug |
|---|---|
| `static String euro(int cent)` | het bedrag als tekst: 128400 geeft <code style="color:#845ef7">1284,00 euro</code> |
| `static int advies(int wattProcessor, int wattVideokaart)` | het voedingsadvies in watt |

Gebruik `String.format` met `%d` en `%02d` (leerboek 1.8, *Opgemaakte tekst*). Je programma toont daarna exact hetzelfde als in punt 1.

> [!WARNING]
> `advies` gebruikt de constante `REST` van 75 W. Een methode ziet de lokale variabelen van `main` niet: zet de constante bovenaan de klasse, als `static final int REST = 75;` (leerboek 1.2, *Scope*).

Test je programma en controleer:

- <code style="color:#845ef7">Onderdelen   : 1284,00 euro</code> en <code style="color:#845ef7">  btw 21 %   : 222,84 euro</code>;
- <code style="color:#845ef7">Geldige builds: 18 van 144</code>.

---

### 3. Codes en een tabel

Elk onderdeel krijgt een code: de soort, een streepje, en het nummer met twee cijfers.

| Soort | Code |
|---|---|
| processor | `CPU-01` tot `CPU-03` |
| moederbord | `MB-01`, `MB-02` |
| geheugen | `RAM-01`, `RAM-02` |
| videokaart | `GPU-01`, `GPU-02` |
| voeding | `PSU-01` tot `PSU-03` |
| behuizing | `KAST-01`, `KAST-02` |

Toon de lijsten bij de start als tabel:

```text
Processors:
CPU-01   Ryzen 5 7600X        199,00 euro   105 W
CPU-02   Ryzen 7 7800X3D      389,00 euro   120 W
CPU-03   Core i5-14600K       299,00 euro   125 W
Moederborden:
MB-01    B650 Tomahawk        219,00 euro
MB-02    B760M DS3H DDR4      119,00 euro
...
Videokaarten:
GPU-01   GeForce RTX 4070     589,00 euro   200 W, 267 mm
GPU-02   Radeon RX 7800 XT    499,00 euro   263 W, 320 mm
...
Behuizingen:
KAST-01  Compact M             79,00 euro   videokaart tot 300 mm
KAST-02  Midi Air              99,00 euro   videokaart tot 360 mm
```

Zes lussen die bijna hetzelfde doen, worden zo één lus over de soorten, met een lus over de onderdelen erin. Daarvoor zet je alle gegevens bovenaan de klasse, als klassevariabelen met `static` (leerboek 1.2, *Scope*), en maak je per soort een rij in een tweedimensionale array (leerboek 1.7):

```java
static String[] codes = {"CPU", "MB", "RAM", "GPU", "PSU", "KAST"};
static String[][] namen = {cpuNamen, mbNamen, ramNamen, gpuNamen, psuNamen, kastNamen};
static int[][] prijzen = {cpuPrijzen, mbPrijzen, ramPrijzen, gpuPrijzen, psuPrijzen, kastPrijzen};
```

| Methode | Geeft terug |
|---|---|
| `static String code(int soort, int i)` | de code: `code(3, 1)` geeft `"GPU-02"` |
| `static String info(int soort, int i)` | de extra gegevens: watt, lengte of maximale lengte, of `""` |

> [!WARNING]
> Een klassevariabele kan enkel naar klassevariabelen verwijzen die er al boven staan. Zet `namen` en `prijzen` onder de zes losse arrays, anders geeft de compiler `illegal forward reference`.

> [!TIP]
> Bij een onderdeel zonder extra gegevens zet je ook geen drie spaties na `euro`. Zo eindigt geen enkele regel op een spatie.

---

### 4. Een build uit één regel codes

De klant typt zijn build voortaan als zes codes op één regel, in eender welke volgorde, in kleine of grote letters:

```text
Build (6 codes): CPU-01 CPU-02 MB-01 RAM-03 HDD-01 GPU-x PSU-01
Twee keer een processor: CPU-01 en CPU-02.
Bestaat niet: RAM-03.
Onbekende code: HDD-01.
Bestaat niet: GPU-x.
Geen geheugen gekozen.
Geen videokaart gekozen.
Geen behuizing gekozen.
Build (6 codes): cpu-01 mb-01 ram-01 gpu-01 psu-01 kast-02
```

Je programma toont alle fouten van de regel, niet enkel de eerste, en vraagt de regel opnieuw tot er geen fout meer in staat.

| Situatie | Wat je programma toont |
|---|---|
| een code met een onbekende soort, of zonder streepje | <code style="color:#845ef7">Onbekende code: HDD-01.</code> |
| een nummer dat niet bestaat of geen getal is | <code style="color:#845ef7">Bestaat niet: RAM-03.</code> |
| een soort die al gekozen is | <code style="color:#845ef7">Twee keer een processor: CPU-01 en CPU-02.</code> |
| een soort die ontbreekt | <code style="color:#845ef7">Geen behuizing gekozen.</code> |

Schrijf daarvoor vier methodes:

| Methode | Geeft terug |
|---|---|
| `static boolean isGetal(String tekst)` | `true` als de tekst niet leeg is en enkel uit cijfers bestaat |
| `static int soortVan(String code)` | 0 tot 5 voor de soort voor het streepje, of -1 |
| `static int nummerVan(String code)` | het getal na het streepje, of -1 |
| `static int[] leesBuild(String regel)` | de zes gekozen indexen, of `null` als er een fout was |

Een build is een array van zes indexen, in de volgorde van de soorten: `{processor, moederbord, geheugen, videokaart, voeding, behuizing}`. De build `CPU-01 MB-01 RAM-01 GPU-01 PSU-01 KAST-02` is `{0, 0, 0, 0, 0, 1}`.

> [!TIP]
> Begin met een build vol `-1`: nog niets gekozen. Een `-1` die na de lus over de codes overblijft, is een soort die ontbreekt.

> [!WARNING]
> Tekst vergelijk je met `equals`, nooit met `==` (leerboek 1.8). Met `==` is elke code onbekend, ook `CPU-01`.

> [!NOTE]
> Typt de klant twee spaties na elkaar, dan geeft `split(" ")` een leeg stuk. Sla lege stukken over, anders krijg je `Onbekende code: .`

---

### 5. Een build als één regel codes

Schrijf een tweede methode `code`, met een andere parameter: `static String code(int[] build)`. Ze geeft de hele build terug als codes, gescheiden door een spatie. Dezelfde naam, andere parameters: Java kiest de juiste methode aan de hand van wat je meegeeft (leerboek 1.9, *Overloading in het kort*).

| Oproep | Geeft terug |
|---|---|
| `code(3, 1)` | `"GPU-02"` |
| `code(new int[] {2, 1, 1, 1, 1, 1})` | `"CPU-03 MB-02 RAM-02 GPU-02 PSU-02 KAST-02"` |

Gebruik de nieuwe methode op drie plaatsen:

- bovenaan het buildrapport: <code style="color:#845ef7">Build        : CPU-01 MB-01 RAM-01 GPU-01 PSU-01 KAST-02</code>;
- in de lijst binnen het budget: <code style="color:#845ef7">1170,00 euro: CPU-03 MB-02 RAM-02 GPU-02 PSU-02 KAST-02</code>;
- onder de goedkoopste geldige build: <code style="color:#845ef7">  CPU-03 MB-02 RAM-02 GPU-02 PSU-02 KAST-02</code>.

Schrijf er ook een methode `static int prijs(int[] build)` bij, die de prijs van een build optelt met een lus over de soorten.

> [!WARNING]
> Overloading werkt enkel als de parameterlijsten verschillen. Twee methodes `code(int, int)`, ook met andere parameternamen, geven de fout `method code(int,int) is already defined`.

---

### 6. Alle builds met recursie

De zes geneste lussen doorlopen alle 144 combinaties. Vervang ze door één recursieve methode (leerboek 1.9, *Recursie*):

```java
static int zoek(int soort, int[] build, int budget)
```

| Geval | Wat de methode doet |
|---|---|
| `soort` is 6: alle zes soorten zijn gekozen | het basisgeval: tel de combinatie, controleer de build, en geef 1 terug als ze geldig is en binnen het budget past, anders 0 |
| `soort` is kleiner dan 6 | probeer elk onderdeel van die soort: zet het in `build[soort]`, en roep `zoek(soort + 1, build, budget)` op |

De methode geeft het aantal geldige builds binnen het budget terug, en toont ze. Het aantal combinaties, het aantal geldige builds en de goedkoopste houd je bij in klassevariabelen.

Schrijf er ook een methode `static boolean isGeldig(int[] build)` bij die de vijf regels controleert.

Je programma toont daarna hetzelfde als in punt 5:

```text
Budget in euro: 1250
1234,00 euro: CPU-01 MB-01 RAM-01 GPU-02 PSU-02 KAST-02
1200,00 euro: CPU-03 MB-02 RAM-02 GPU-01 PSU-01 KAST-01
...
6 geldige build(s) binnen 1250,00 euro.
Geldige builds: 18 van 144
Goedkoopste geldige build: 1170,00 euro
  CPU-03 MB-02 RAM-02 GPU-02 PSU-02 KAST-02
```

> [!WARNING]
> Roep je `zoek(soort, ...)` op in plaats van `zoek(soort + 1, ...)`, dan wordt het probleem nooit kleiner. De methode roept zichzelf op tot de stack vol is: `StackOverflowError`.

> [!TIP]
> Met een array `int[] aantallen` per soort zou je nog altijd zes lussen nodig hebben. De recursie heeft er één, ongeacht hoeveel soorten er zijn: komt er een zevende soort bij, dan verandert er niets aan `zoek`.

---

### 7. Alles samen: het buildrapport

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Is de build van de klant niet compatibel, dan doet het buildrapport een voorstel: de goedkoopste geldige build met **dezelfde processor en dezelfde videokaart**. Die twee kiest een klant meestal bewust; de rest mag veranderen.

```text
Compatibel      : nee (4 problemen)
Voorstel        : CPU-02 MB-01 RAM-01 GPU-02 PSU-02 KAST-02
                  1424,00 euro (160,00 euro meer)
Anders          : MB-01 in plaats van MB-02, PSU-02 in plaats van PSU-01, KAST-02 in plaats van KAST-01
==============================
```

Is de build compatibel, dan staat er <code style="color:#845ef7">Voorstel        : geen, deze build is compatibel</code>.

| Regel | Betekenis |
|---|---|
| Voorstel | de codes van de goedkoopste geldige build met dezelfde processor en videokaart |
| het bedrag | de prijs van het voorstel, en het verschil met de build van de klant, met <code style="color:#845ef7">meer)</code> of <code style="color:#845ef7">minder)</code> |
| Anders | elk onderdeel dat verandert, met <code style="color:#845ef7">in plaats van</code> |

Breid `zoek` uit met een parameter `int[] vast`. Een `-1` betekent dat de soort vrij is; een ander getal, dat de soort vastligt op dat onderdeel. Voor het voorstel geef je `{c, -1, -1, g, -1, -1}` mee, voor de zoektocht binnen het budget zes keer `-1`.

Schrijf ook een methode `static String verschillen(int[] build, int[] voorstel)` die de regel na *Anders* opbouwt met een `StringBuilder`.

> [!IMPORTANT]
> Het voorstel en de zoektocht binnen het budget gebruiken dezelfde klassevariabelen. Zet de goedkoopste prijs terug op `Integer.MAX_VALUE` voor elke zoektocht, en het aantal combinaties en geldige builds op 0.

Test je programma en controleer:

- de build `CPU-02 MB-02 RAM-01 GPU-02 PSU-01 KAST-01`: het voorstel hierboven;
- de build `CPU-03 MB-01 RAM-01 GPU-01 PSU-01 KAST-01`: een goedkoper voorstel, <code style="color:#845ef7">1200,00 euro (164,00 euro minder)</code>, met <code style="color:#845ef7">MB-02 in plaats van MB-01, RAM-02 in plaats van RAM-01</code>;
- de build `CPU-01 MB-01 RAM-01 GPU-01 PSU-01 KAST-02` met een budget van 1200 euro: geen voorstel, en daarna nog altijd <code style="color:#845ef7">Geldige builds: 18 van 144</code>.

> [!NOTE]
> De gegevens van een onderdeel staan verspreid over vijf arrays, en de soort is een getal van 0 tot 5. Vanaf hoofdstuk 04 wordt elk onderdeel een object, en in hoofdstuk 06 wordt elke regel een klasse achter de interface `CompatibiliteitsRegel`.
