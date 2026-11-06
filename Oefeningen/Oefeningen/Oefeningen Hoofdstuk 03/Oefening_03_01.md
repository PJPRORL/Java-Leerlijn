# 03_01

<b>Hoofdstuk 03</b> &nbsp;·&nbsp; Opsplitsen in methodes &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#37b24d">●○○</span> &nbsp;·&nbsp; ±75 min

## Leerdoel

Na deze oefening kan je een lang `main` opsplitsen in methodes, met en zonder returnwaarde, en arrays als parameter doorgeven.

Je leert tekst vergelijken en doorzoeken zonder op hoofdletters te letten, en getallen netjes in kolommen zetten met `printf` en `String.format`.

Daarnaast oefen je op een methode die geen tekst toont, maar tekst opbouwt en teruggeeft: dan beslist wie de methode oproept wat er met die tekst gebeurt.

## Opdracht

De kassa van Bit & Byte uit 02_01 werkt, maar `main` is meer dan honderd regels lang. De voorraadstatus staat er twee keer in, het afronden op de cent vier keer. En de verkopers klagen dat bedragen als `199.0 euro` op het scherm staan.

Jouw taak is om de kassa op te splitsen in methodes, en ze daarna netter te laten tonen en beter te laten zoeken.

Het programma moet, net als in 02_01:

1. een menu tonen met de catalogus, verkopen, zoeken tot een maximumprijs, een statistiek en een voorraadoverzicht;

en daarnaast:

2. de herhaalde stukken in methodes hebben;
3. elk bedrag met twee cijfers na de komma tonen;
4. de catalogus als tabel tonen;
5. artikelen zoeken op naam of merk;
6. de berekeningen over de catalogus in methodes hebben;
7. een artikelfiche tonen.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 02_01 naar een nieuwe map `Oefening_03_01`. Deze werking blijft dezelfde:

| Wat | Hoe het nu werkt |
|---|---|
| het menu | blijft draaien tot <code style="color:#e64980;font-weight:600">0</code>, met <code style="color:#845ef7">Ongeldige keuze.</code> bij een onbekende keuze |
| een verkoop | eerst de controles, dan de kassabon en een lagere voorraad |
| zoeken, statistiek, voorraadoverzicht | met dezelfde regels als in 02_01 |

> [!TIP]
> Start je programma één keer met een verkoop van 2 stuks Ryzen 5 7600X aan Lotte Peeters, en vraag daarna de statistiek en het voorraadoverzicht. Bewaar die uitvoer. Na punt 2 moet je programma exact hetzelfde tonen.

---

### 2. Wat dubbel staat, in een methode

Haal drie stukken uit `main`, en zet ze als methode in dezelfde klasse, onder `main` (leerboek 1.9):

| Methode | Wat ze doet | Waar je ze gebruikt |
|---|---|---|
| `static void toonMenu()` | toont het menu en de vraag <code style="color:#845ef7">Keuze: </code> | bovenaan de lus |
| `static String status(int voorraad)` | geeft <code style="color:#845ef7">uitverkocht</code>, <code style="color:#845ef7">bijna uitverkocht</code> of <code style="color:#845ef7">op voorraad</code> terug | op de kassabon en in het voorraadoverzicht |
| `static double rond(double bedrag)` | rondt af op de cent: `Math.round(bedrag * 100) / 100.0` | overal waar je nu afrondt |

Je programma toont daarna exact hetzelfde als in punt 1. Dat is de bedoeling: je verandert hoe de code is opgebouwd, niet wat ze doet.

> [!NOTE]
> `main` is `static`. Vanuit `main` kan je zonder object enkel andere `static` methodes oproepen (leerboek 1.9, *Valkuilen*). Waarom, zie je in deel 2.

> [!TIP]
> Een methode die iets teruggeeft, toont zelf niets. `status` geeft een `String` terug; wie de methode oproept, plakt die tekst op de bon of in het overzicht.

Test je programma en controleer:

- of de verkoop van 2 Ryzen 5 7600X nog altijd <code style="color:#845ef7">Voorraad na verkoop: 1 (bijna uitverkocht)</code> toont;
- of de statistiek en het voorraadoverzicht hetzelfde zijn als de uitvoer die je bewaarde.

---

### 3. Bedragen met twee cijfers na de komma

Schrijf een methode `static String euro(double bedrag)`. Ze geeft het bedrag terug als tekst, met twee cijfers na de komma en het woord `euro` erachter. Gebruik `String.format` met `%.2f` (leerboek 1.8, *Opgemaakte tekst*).

| Oproep | Geeft terug |
|---|---|
| `euro(199.0)` | <code style="color:#845ef7">199,00 euro</code> |
| `euro(69.07)` | <code style="color:#845ef7">69,07 euro</code> |
| `euro(0.0)` | <code style="color:#845ef7">0,00 euro</code> |

Gebruik de methode voor elk bedrag dat je programma toont: in de catalogus, op de kassabon, bij het zoeken, in de statistiek en in het voorraadoverzicht.

```text
Aantal      : 2 x 199,00 euro
Subtotaal   : 398,00 euro
Verzendkost : 0,00 euro
Te betalen  : 398,00 euro
Waarvan btw : 69,07 euro
```

> [!NOTE]
> `String.format` gebruikt de taalinstellingen van je computer. Op een Belgische Windows krijg je een komma. Rekenen doe je nog altijd met een `double`: `euro` verandert enkel hoe je het bedrag toont.

Test je programma en controleer:

- <code style="color:#845ef7">1. B650 Tomahawk (MSI, moederbord) - 219,00 euro - 12 stuks</code> in de catalogus;
- zoeken tot <code style="color:#e64980;font-weight:600">100</code>: <code style="color:#845ef7">Geen artikelen tot 100,00 euro.</code>;
- de statistiek: <code style="color:#845ef7">Gemiddelde prijs: 334,00 euro</code> en <code style="color:#845ef7">Goedkoopste: NH-D15 (109,00 euro)</code>.

---

### 4. De catalogus als tabel

Toon de catalogus als tabel, met kolommen die netjes onder elkaar staan:

```text
Nr  Artikel            Merk      Soort          Prijs  Stuks
 1  B650 Tomahawk      MSI       moederbord    219,00     12
 2  Ryzen 5 7600X      AMD       processor     199,00      3
 3  Ryzen 7 7800X3D    AMD       processor     389,00      5
 4  GeForce RTX 4070   ASUS      videokaart    589,00      0
 5  Radeon RX 7800 XT  Sapphire  videokaart    499,00      2
 6  NH-D15             Noctua    koeler        109,00      7
```

Schrijf daarvoor twee methodes zonder returnwaarde:

| Methode | Wat ze toont |
|---|---|
| `static void toonKop()` | de regel met de titels |
| `static void toonRij(int nr, String naam, String merk, String soort, double prijs, int voorraad)` | één artikel |

Gebruik ze ook bij het zoeken tot een maximumprijs: de gevonden artikelen verschijnen dan in dezelfde tabel, met de kop erboven. Daaronder blijft <code style="color:#845ef7">3 artikel(en) tot 300,00 euro.</code> staan.

| In de opmaak | Betekent |
|---|---|
| `%-18s` | tekst, links uitgelijnd, 18 tekens breed |
| `%2d` | geheel getal, rechts uitgelijnd, 2 tekens breed |
| `%8.2f` | kommagetal, 2 cijfers na de komma, 8 tekens breed |
| `%n` | een nieuwe regel |

> [!TIP]
> De kop en de rij gebruiken dezelfde breedtes. Schrijf de kop met `%s` op dezelfde plaatsen: `"%2s  %-18s %-9s %-11s %8s %6s%n"`.

> [!WARNING]
> Elke `%` in de opmaak hoort bij één waarde, in dezelfde volgorde. Geef je een `double` mee waar `%d` staat, dan stopt je programma met een `IllegalFormatConversionException`.

---

### 5. Zoeken op naam of merk

Voeg aan het menu de keuze <code style="color:#845ef7">6. Zoeken op naam of merk</code> toe. Je programma vraagt <code style="color:#845ef7">Zoek: </code> en toont elk artikel waarvan de naam of het merk die tekst bevat, in de tabel van punt 4. Hoofdletters spelen geen rol, en spaties voor of na de zoektekst ook niet.

| Zoek | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">ryzen</code> | artikel 2 en 3, en <code style="color:#845ef7">2 artikel(en) gevonden.</code> |
| <code style="color:#e64980;font-weight:600">AMD</code> | artikel 2 en 3: het merk telt ook |
| <code style="color:#e64980;font-weight:600">  noctua </code> | artikel 6, en <code style="color:#845ef7">1 artikel(en) gevonden.</code> |
| <code style="color:#e64980;font-weight:600">xyz</code> | <code style="color:#845ef7">Niets gevonden voor xyz.</code> |
| niets, enkel Enter | <code style="color:#845ef7">Geef minstens een teken in.</code> |

Schrijf een methode `static boolean bevat(String tekst, String stuk)` die `true` teruggeeft als `stuk` in `tekst` voorkomt, zonder op hoofdletters te letten. Gebruik `toLowerCase` en `contains` (leerboek 1.8).

> [!WARNING]
> De keuze lees je met `nextInt`, de zoektekst met `nextLine`. Na `nextInt` staat de Enter nog klaar: zonder een extra `sc.nextLine()` krijg je meteen een lege zoektekst (leerboek 1.4, *Het Enter-probleem*).

---

### 6. Rekenen in methodes

De statistiek en het voorraadoverzicht rekenen met lussen in `main`. Zet die berekeningen in methodes die een array als parameter krijgen:

| Methode | Geeft terug |
|---|---|
| `static double gemiddelde(double[] getallen)` | het gemiddelde, afgerond op de cent |
| `static int kleinste(double[] getallen)` | de *index* van het kleinste getal |
| `static int grootste(double[] getallen)` | de *index* van het grootste getal |
| `static double voorraadwaarde(double[] prijzen, int[] voorraden)` | de som van prijs × voorraad, afgerond op de cent |

De statistiek en het voorraadoverzicht tonen daarna hetzelfde als in punt 3.

> [!NOTE]
> Bij een array krijgt de methode een kopie van de *referentie*: ze kijkt naar dezelfde array als `main` (leerboek 1.9, *Parameters zijn kopieën*). Na een verkoop rekent `voorraadwaarde` dus met de nieuwe voorraad.

Test je programma en controleer:

- de statistiek bij de start: <code style="color:#845ef7">Gemiddelde prijs: 334,00 euro</code>, <code style="color:#845ef7">Goedkoopste: NH-D15 (109,00 euro)</code>, <code style="color:#845ef7">Duurste: GeForce RTX 4070 (589,00 euro)</code>;
- het voorraadoverzicht bij de start: <code style="color:#845ef7">Voorraadwaarde: 6931,00 euro</code>;
- na een verkoop van 2 stuks Radeon RX 7800 XT: <code style="color:#845ef7">Voorraadwaarde: 5933,00 euro</code>.

---

### 7. Alles samen: de artikelfiche als tekst

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Voeg de keuze <code style="color:#845ef7">7. Artikelfiche</code> toe. Je programma vraagt <code style="color:#845ef7">Artikel (naam of een stuk ervan): </code> en toont de fiche van dat artikel:

```text
================================
RADEON RX 7800 XT
================================
Merk     : Sapphire
Soort    : videokaart
Prijs    : 499,00 euro
Voorraad : 2 (bijna uitverkocht)
Waarde   : 998,00 euro
================================
```

| Regel | Betekenis |
|---|---|
| de titel | de naam in hoofdletters |
| Voorraad | met de status van punt 2 |
| Waarde | prijs × voorraad, afgerond op de cent |
| het kader | een rij `=` die precies zo lang is als de langste regel van de fiche |

Het artikel zoek je zo:

| Wat de verkoper typt | Wat je programma toont |
|---|---|
| de volledige naam, in kleine of grote letters | de fiche van dat artikel |
| een stuk van precies één naam, zoals <code style="color:#e64980;font-weight:600">rx</code> | de fiche van dat artikel |
| een stuk van meer namen, zoals <code style="color:#e64980;font-weight:600">ryzen</code> | <code style="color:#845ef7">Meerdere artikelen: Ryzen 5 7600X, Ryzen 7 7800X3D.</code> |
| een stuk van geen enkele naam | <code style="color:#845ef7">Niets gevonden voor hdd.</code> |

Schrijf daarvoor twee methodes:

- `static int zoekIndex(String[] namen, String naam)`: de index van het artikel met precies die naam, zonder op hoofdletters te letten, of `-1`. Gebruik `equalsIgnoreCase` (leerboek 1.8).
- `static String fiche(String naam, String merk, String soort, double prijs, int voorraad)`: de volledige fiche als één `String`, met een `\n` na elke regel. De methode toont zelf niets; `main` toont de tekst met `System.out.print`.

> [!TIP]
> Maak eerst elke regel als `String`. Dan ken je hun `length()`, en daarmee de breedte van het kader. Een rij van `n` keer `=` maak je met `"=".repeat(n)` (naslag 10.1), of met een lus. Plak de regels daarna aan elkaar met een `StringBuilder` (leerboek 1.8).

> [!IMPORTANT]
> Zoek eerst op de volledige naam, en pas daarna op een stuk. Wie <code style="color:#e64980;font-weight:600">ryzen 5 7600x</code> typt, wil die ene fiche, ook al zou een stuk ervan bij meer namen passen.

Test je programma en controleer:

- <code style="color:#e64980;font-weight:600">ryzen 5 7600x</code>: de fiche van de Ryzen 5 7600X, met een kader van 26 tekens en <code style="color:#845ef7">Waarde   : 597,00 euro</code>;
- <code style="color:#e64980;font-weight:600">rx</code>: de fiche hierboven, met een kader van 32 tekens;
- na een verkoop van 1 Radeon RX 7800 XT, <code style="color:#e64980;font-weight:600">radeon</code>: <code style="color:#845ef7">Voorraad : 1 (bijna uitverkocht)</code> en <code style="color:#845ef7">Waarde   : 499,00 euro</code>.

> [!NOTE]
> De methodes van dit hoofdstuk krijgen vijf of zes parameters mee: naam, merk, soort, prijs en voorraad horen samen, maar staan in vijf arrays. In hoofdstuk 04 worden ze één object `Artikel`, en krijgt een methode één parameter.
