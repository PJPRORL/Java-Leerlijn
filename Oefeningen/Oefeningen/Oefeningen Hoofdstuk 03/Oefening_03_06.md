# 03_06

<b>Hoofdstuk 03</b> &nbsp;·&nbsp; Codes, overloading en recursie &nbsp;·&nbsp; Concertzaal &nbsp;·&nbsp; niveau <span style="color:#f03e3e">●●●</span> &nbsp;·&nbsp; ±2 uur

## Leerdoel

Na deze oefening kan je een plaatscode zoals `C12` ontleden met `charAt` en `substring`, en controleren voor je ze gebruikt.

Je leert twee methodes met dezelfde naam schrijven die verschillen in hun parameters (overloading), en een methode die zichzelf oproept (recursie).

Daarnaast oefen je op een rapport dat je volledig opbouwt als tekst, met kolommen uit `String.format`, voor je er iets van toont.

## Opdracht

Concertzaal De Klankkast werkt met het programma uit 02_06. De medewerker typt een plaatsnummer van 1 tot 200, maar op het ticket staat `C12`: de klant zegt *C12*, en de medewerker moet zelf rekenen. Na de verkoop toont het programma enkel het zaalplan, zonder te zeggen waar nog plaats is.

Jouw taak is om het programma met plaatscodes te laten werken, en het een zaalrapport te laten maken.

Het programma moet, net als in 02_06:

1. een ticket en een groepsticket opmaken, met het zaalplan, de vrije plaatsen per categorie en de beste vrije plaats;

en daarnaast:

2. de herhaalde stukken in methodes hebben;
3. een plaats lezen als code, zoals `C12`;
4. de vrije plaatsen per categorie als tabel tonen;
5. één plaats en een reeks plaatsen controleren met dezelfde methodenaam;
6. vrije plaatsen na elkaar tellen met recursie;
7. een zaalrapport tonen.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 02_06 naar een nieuwe map `Oefening_03_06`. Deze regels blijven precies dezelfde:

| Wat | Hoe |
|---|---|
| categorie en basisprijs | rij A–C 65,00 · rij D–G 49,00 · rij H–J 35,00 euro, in cent |
| jongerenkorting | 20 % op de basisprijs voor wie jonger is dan 26 |
| servicekost | 2,50 euro per ticket |
| btw | 6 % in het ticket, met `(ticket * 100 + 53) / 106` |
| een groep | 1 tot 8 personen, op stoelen naast elkaar in dezelfde rij |

> [!TIP]
> Start je programma één keer met concert 1, plaats 33, leeftijd 40, en een groep van 4 met leeftijden 19, 30, 22 en 45. Bewaar de uitvoer. Na punt 2 moet je programma exact hetzelfde tonen.

---

### 2. Wat dubbel staat, in een methode

Haal deze stukken uit `main`, en zet ze als methode in de klasse (leerboek 1.9):

| Methode | Geeft terug |
|---|---|
| `static String euro(int cent)` | het bedrag als tekst: 6750 geeft <code style="color:#845ef7">67,50 euro</code> |
| `static int korting(int basisprijs, int leeftijd)` | de jongerenkorting in cent |
| `static int btw(int ticket)` | de btw in cent |
| `static int categorie(int rijIndex)` | 1, 2 of 3 |
| `static int basisprijs(int categorie)` | 6500, 4900 of 3500 |
| `static String plaatsCode(int rijIndex, int stoel)` | de code: `plaatsCode(1, 13)` geeft `"B13"` |
| `static void toonZaalplan(boolean[][] bezet)` | niets: toont het zaalplan en het aantal vrije plaatsen |

De constanten voor het aantal rijen, het aantal stoelen per rij en de servicekost zet je bovenaan de klasse, als `static final` (leerboek 1.2, *Scope*): de methodes hebben ze nodig.

Je programma toont daarna exact hetzelfde als in punt 1.

> [!WARNING]
> Vergeet je `static` voor een methode, dan kan `main` ze niet oproepen: `non-static method euro(int) cannot be referenced from a static context`.

---

### 3. Een plaats als code

De medewerker typt de plaats voortaan als code: een rijletter en een stoelnummer, in kleine of grote letters. Vraag ze opnieuw tot ze klopt en vrij is:

```text
Plaats (bv. C12): K3
Geen geldige plaats: K3.
Plaats (bv. C12): c0
Geen geldige plaats: c0.
Plaats (bv. C12): C12
Plaats C12 is al bezet.
Plaats (bv. C12): b13
Leeftijd: 40
```

| Een geldige plaats | Voorbeeld |
|---|---|
| een letter van A tot en met J | <code style="color:#e64980;font-weight:600">K3</code> is fout |
| daarna een stoelnummer van 1 tot en met 20, met enkel cijfers | <code style="color:#e64980;font-weight:600">c0</code> en <code style="color:#e64980;font-weight:600">C21</code> zijn fout |

Schrijf daarvoor twee methodes:

| Methode | Geeft terug |
|---|---|
| `static int rijVan(String code)` | de rij-index: 0 voor `A` tot 9 voor `J`, of -1 |
| `static int stoelVan(String code)` | het stoelnummer van 1 tot 20, of -1 |

> [!TIP]
> `code.charAt(0)` geeft de letter, `code.substring(1)` het nummer (leerboek 1.8). Met `Character.toUpperCase` (naslag 10.1) maak je van `b` een `B`; daarna is `letter - 'A'` de rij-index, zoals in 01_06.

> [!WARNING]
> Zonder `toUpperCase` is `b13` geen geldige plaats: `'b'` ligt niet tussen `'A'` en `'J'`.

---

### 4. Vrije plaatsen per categorie als tabel

Toon de vrije plaatsen per categorie als tabel, met wat ze samen opbrengen:

```text
Categorie  Vrij     Prijs      Samen
1            36     65,00    2340,00
2            56     49,00    2744,00
3            57     35,00    1995,00
Totaal      149              7079,00
```

| Kolom | Breedte |
|---|---|
| Categorie | 9 tekens, links uitgelijnd |
| Vrij | 5 |
| Prijs | 9, met 2 cijfers na de komma |
| Samen | 10, met 2 cijfers na de komma |

Gebruik `printf`. Een bedrag in cent zet je om met `/ 100.0`, zodat je een `double` krijgt voor `%9.2f`.

> [!WARNING]
> Elke `%` in de opmaak hoort bij één waarde. Geef je een `double` mee waar `%d` staat, dan stopt je programma met een `IllegalFormatConversionException`.

---

### 5. Eén plaats of een reeks: dezelfde naam

Schrijf twee methodes met dezelfde naam, die verschillen in hun parameters (leerboek 1.9, *Overloading in het kort*):

| Methode | Geeft `true` als |
|---|---|
| `static boolean isVrij(boolean[][] bezet, int rij, int stoel)` | die ene plaats vrij is |
| `static boolean isVrij(boolean[][] bezet, int rij, int start, int aantal)` | `aantal` plaatsen naast elkaar vrij zijn, vanaf `start` |

De tweede gebruikt de eerste, in een lus. Zoek de plaatsen voor een groep voortaan zo: probeer in elke rij elke startplaats, tot `isVrij` voor de hele groep `true` geeft.

Je programma vindt daarna dezelfde plaatsen als in 02_06:

| Groep | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Plaatsen     : A9 tot A12 (categorie 1)</code> |
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Plaatsen     : B14 tot B18 (categorie 1)</code> |
| <code style="color:#e64980;font-weight:600">8</code> | <code style="color:#845ef7">Plaatsen     : C1 tot C8 (categorie 1)</code> |

> [!WARNING]
> Een reeks van 8 die op stoel 14 begint, loopt tot stoel 21, en die bestaat niet. Laat de startplaats lopen zolang `start + aantal <= 20`, anders stopt je programma met `ArrayIndexOutOfBoundsException: Index 20 out of bounds for length 20`.

---

### 6. Vrije plaatsen na elkaar, met recursie

Schrijf een recursieve methode (leerboek 1.9, *Recursie*):

```java
static int vrijVanaf(boolean[][] bezet, int rij, int stoel)
```

Ze geeft het aantal vrije plaatsen na elkaar terug, vanaf `stoel` naar rechts:

| Geval | Geeft terug |
|---|---|
| `stoel` is 20, het einde van de rij, of de plaats is bezet | 0: het basisgeval |
| anders | 1 + `vrijVanaf(bezet, rij, stoel + 1)` |

Gebruik ze op twee plaatsen:

- de tweede `isVrij` wordt één regel: `return vrijVanaf(bezet, rij, start) >= aantal;`
- een methode `static int langsteReeks(boolean[][] bezet, int rij)` geeft de langste reeks vrije plaatsen in een rij.

Toon onder de beste vrije plaats de grootste groep die nog samen kan zitten, en in welke rij: <code style="color:#845ef7">Grootste groep die nog samen kan zitten: 20 (rij E)</code>.

> [!WARNING]
> Zonder de test op het einde van de rij kijkt de methode naar stoel 21, en stopt je programma met `ArrayIndexOutOfBoundsException`. Een recursieve methode heeft een basisgeval voor élke manier waarop ze moet stoppen.

---

### 7. Alles samen: het zaalrapport

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Vervang het zaalplan op het einde door een zaalrapport:

```text
============================================
CONCERTZAAL - ZAALRAPPORT
The Lunar Echoes (2026-12-04)
============================================
   12345678901234567890  vrij  langste reeks
A  XXXXXXXXXXXXXXXXXXXX     0  -
B  ....XXXXXX..X.......    13  B14-B20 (7)
C  ...........X........    19  C1-C11 (11)
D  XXXX................    16  D5-D20 (16)
E  ....................    20  E1-E20 (20)
F  XXXXXXXXXXXXXXXXXXXX     0  -
G  ....................    20  G1-G20 (20)
H  .........X..........    19  H11-H20 (10)
I  ....................    20  I1-I20 (20)
J  ..................XX    18  J1-J18 (18)
--------------------------------------------
Categorie 1: 32 vrij van 60
Categorie 2: 56 vrij van 80
Categorie 3: 57 vrij van 60
Bezetting: 55 van 200 (27,5 %)
============================================
```

| Regel | Betekenis |
|---|---|
| vrij | het aantal vrije plaatsen in die rij |
| langste reeks | de eerste langste reeks vrije plaatsen, met de codes van de eerste en de laatste plaats, en de lengte; <code style="color:#845ef7">-</code> als de rij vol is |
| Bezetting | het aantal verkochte plaatsen, en het percentage met één cijfer na de komma |

Schrijf een methode `static String zaalrapport(boolean[][] bezet, String concert)` die het hele rapport opbouwt met een `StringBuilder` en `String.format`, en teruggeeft. `main` toont het met `System.out.print`.

> [!TIP]
> Waar de langste reeks begint, vind je met `vrijVanaf`: de eerste stoel waarvoor `vrijVanaf` gelijk is aan `langsteReeks`.

> [!WARNING]
> Een procentteken in `String.format` schrijf je als `%%`. Met één `%` voor het haakje stopt je programma met `UnknownFormatConversionException: Conversion = ')'`.

Test je programma en controleer:

- het rapport hierboven, na een ticket op B13 en een groep van 4;
- concert <code style="color:#e64980;font-weight:600">3</code>, plaats <code style="color:#e64980;font-weight:600">E5</code>, en een groep van 8: <code style="color:#845ef7">Plaatsen     : B11 tot B18 (categorie 1)</code>, en in het rapport <code style="color:#845ef7">E  ....X...............    19  E6-E20 (15)</code> en <code style="color:#845ef7">Bezetting: 59 van 200 (29,5 %)</code>.

> [!NOTE]
> Het zaalplan is één array van `boolean`, voor één concert. In hoofdstuk 04 wordt een concert een object met een eigen zaalplan, en in hoofdstuk 05 krijgt elk soort plaats een eigen klasse.
