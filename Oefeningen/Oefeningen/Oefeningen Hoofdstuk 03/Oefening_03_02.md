# 03_02

<b>Hoofdstuk 03</b> &nbsp;·&nbsp; Tekst ontleden &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#f08c00">●●○</span> &nbsp;·&nbsp; ±90 min

## Leerdoel

Na deze oefening kan je een regel invoer zoals `2x3` ontleden met `split`, controleren of een stuk tekst een getal is, en het omzetten met `Integer.parseInt`.

Je leert de vaste regels van een programma in methodes met een returnwaarde zetten, en tekst opbouwen met `StringBuilder` en `String.format`.

Daarnaast oefen je op een invoer die pas aanvaard wordt als élk deel klopt: één fout deel, en er verandert niets.

## Opdracht

Het magazijn van Bit & Byte werkt met het programma uit 02_02. Voor elke bestellijn vraagt het twee keer een getal, en de besteldag moet als nummer. De magazijniers willen sneller werken: een volledige bestelling op één regel, zoals ze die van de klant krijgen.

Jouw taak is om het programma tekst te laten lezen en schrijven.

Het programma moet, net als in 02_02:

1. bestellingen afhandelen met controles, korting, verzendkost, spaarpunten, dozen, een leverdag en een pakbon;

en daarnaast:

2. de vaste regels in methodes hebben;
3. de pakbon in kolommen tonen;
4. een bestellijn als tekst lezen, zoals `2x3`;
5. de besteldag en de leverdag als weekdag lezen en tonen;
6. de bestelling samenvatten op één regel;
7. een volledige bestelling uit één regel invoer lezen.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 02_02 naar een nieuwe map `Oefening_03_02`. Deze regels blijven precies dezelfde:

| Regel | Waarde |
|---|---|
| Bestellijnen | hoogstens 5 per bestelling; hetzelfde artikel opnieuw telt bij de bestaande lijn bij |
| Aantal per artikel | 1 tot en met 20 |
| Staffelkorting per lijn | vanaf 5 stuks 5 %, vanaf 10 stuks 10 % |
| Verzendkost | 4,99 euro als de lijnbedragen samen onder 50 euro liggen; anders gratis |
| Spaarpunten | 1 punt per volle 10 euro van de lijnbedragen |
| Dozen | 3 artikelen per doos, over de hele bestelling |
| Levertermijn | alles op voorraad: 1 werkdag; anders 5 werkdagen |
| Bevestigen | pas na een <code style="color:#e64980;font-weight:600">j</code> verandert de voorraad |

> [!TIP]
> Start je programma één keer met een bestelling van 2 Ryzen 5 7600X en 6 NH-D15 op dinsdag, en bewaar de pakbon. Na punt 2 moet je programma, op het bestelnummer na, exact dezelfde pakbon tonen.

---

### 2. De vaste regels in methodes

Zet elke regel die een waarde uitrekent in een eigen methode met een returnwaarde (leerboek 1.9):

| Methode | Geeft terug |
|---|---|
| `static int korting(int aantal)` | het kortingspercentage: 0, 5 of 10 |
| `static double verzendkost(double lijnbedragen)` | 4.99 of 0.0 |
| `static int aantalDozen(int stuks)` | het aantal dozen |
| `static int inLaatsteDoos(int stuks)` | hoeveel stuks in de laatste doos zitten |
| `static double rond(double bedrag)` | het bedrag, afgerond op de cent |

Je programma toont daarna hetzelfde als in punt 1.

> [!WARNING]
> Een methode ziet de lokale variabelen van `main` niet. Gebruiken `aantalDozen` en `inLaatsteDoos` de constante `PER_DOOS`, zet die dan bovenaan de klasse, buiten `main`: `static final int PER_DOOS = 3;` (leerboek 1.2, *Scope*).

Test je programma en controleer:

- de bestelling van 2 Ryzen 5 7600X en 6 NH-D15: <code style="color:#845ef7">Dozen        : 3 (laatste doos: 2 stuks)</code> en <code style="color:#845ef7">Spaarpunten  : 101</code>;
- een bestelling van 10 B650 Tomahawk: <code style="color:#845ef7">Dozen        : 4 (laatste doos: 1 stuk)</code>.

---

### 3. De pakbon in kolommen

Toon de catalogus, het te betalen bedrag en de pakbon met `printf`, zodat de bedragen twee cijfers na de komma hebben en de kolommen netjes onder elkaar staan:

```text
1. B650 Tomahawk       219,00 euro - 12 op voorraad
2. Ryzen 5 7600X       199,00 euro -  3 op voorraad
...
Te betalen: 1019,30 euro
Bevestigen (j/n): j
======================================================
BIT & BYTE - PAKBON
======================================================
Bestelnummer : BB-2026-49843
Artikel            Aantal     Prijs  Korting    Bedrag
Ryzen 5 7600X           2    199,00      0 %    398,00
NH-D15                  6    109,00      5 %    621,30
------------------------------------------------------
Lijnbedragen : 1019,30 euro
Verzendkost  : 0,00 euro
Totaal       : 1019,30 euro
Spaarpunten  : 101
Dozen        : 3 (laatste doos: 2 stuks)
Levertermijn : 1 werkdag
Leverdag     : werkdag 3 (deze week)
======================================================
```

| Kolom | Breedte |
|---|---|
| Artikel | 18 tekens, links uitgelijnd |
| Aantal | 6 |
| Prijs | 9, met 2 cijfers na de komma |
| Korting | 6 voor het getal, dan een spatie en `%` |
| Bedrag | 9, met 2 cijfers na de komma |

De strepen zijn 54 tekens lang: precies zo breed als de tabel.

> [!WARNING]
> In een opmaak betekent `%` het begin van een plaats voor een waarde. Een gewoon procentteken schrijf je als `%%`. Met één `%` stopt je programma bij de eerste lijn van de pakbon.

---

### 4. Een bestellijn als tekst

De magazijnier typt een bestellijn voortaan als één stuk tekst: het artikel, een `x`, en het aantal. Hij mag spaties zetten, en de `x` mag een hoofdletter zijn. Een lege regel betekent dat de bestelling klaar is.

```text
Lijn (artikel x aantal, leeg = klaar): 2x2
Lijn (artikel x aantal, leeg = klaar): 6 x 6
Lijn (artikel x aantal, leeg = klaar): 2X1
Bijgeteld bij lijn 1: nu 3 stuks.
Lijn (artikel x aantal, leeg = klaar): 
```

Controleer de regel in deze volgorde, en toon bij de eerste fout één melding:

| Situatie | Wat je programma toont |
|---|---|
| geen `x`, of niets voor of na de `x`, zoals <code style="color:#e64980;font-weight:600">2x</code> | <code style="color:#845ef7">Gebruik de vorm artikel x aantal, zoals 2x3.</code> |
| een stuk is geen getal, zoals <code style="color:#e64980;font-weight:600">twee x 3</code> | <code style="color:#845ef7">Geen geldig getal: twee.</code> |
| het artikel bestaat niet, zoals <code style="color:#e64980;font-weight:600">9x1</code> | <code style="color:#845ef7">Artikel 9 bestaat niet.</code> |
| het aantal ligt niet tussen 1 en 20, zoals <code style="color:#e64980;font-weight:600">3x25</code> | <code style="color:#845ef7">Het aantal moet tussen 1 en 20 liggen.</code> |

Schrijf daarvoor vier methodes:

| Methode | Geeft terug |
|---|---|
| `static boolean isGetal(String tekst)` | `true` als de tekst niet leeg is en enkel uit cijfers bestaat |
| `static String controleerLijn(String lijn, int aantalArtikelen, int maxPerArtikel)` | de melding van de eerste fout, of `""` als de regel klopt |
| `static int artikelVan(String lijn)` | de index van het artikel: `2x3` geeft 1 |
| `static int aantalVan(String lijn)` | het aantal: `2x3` geeft 3 |

> [!TIP]
> `lijn.toLowerCase().split("x")` geeft een array met de stukken voor en na de `x` (leerboek 1.8). Klopt de vorm, dan heeft die array precies twee elementen. Haal met `trim()` de spaties eraf, controleer met `isGetal`, en zet pas dan om met `Integer.parseInt` (naslag 10.2).

> [!WARNING]
> `Integer.parseInt("twee")` stopt je programma met een `NumberFormatException`. Daarom controleer je eerst teken per teken met `Character.isDigit` (naslag 10.1).

> [!IMPORTANT]
> De lijnen lees je met `nextLine`, de vraag *Nog een bestelling* met `next`. Na `next` staat de Enter nog klaar. Zonder een extra `sc.nextLine()` leest de volgende bestelling meteen een lege lijn, en is ze leeg voor de magazijnier iets typt.

Test je programma en controleer:

- de lijnen hierboven: een pakbon met <code style="color:#845ef7">Ryzen 5 7600X           3    199,00      0 %    597,00</code> en <code style="color:#845ef7">Totaal       : 1218,30 euro</code>;
- daarna <code style="color:#e64980;font-weight:600">j</code> voor nog een bestelling, en <code style="color:#e64980;font-weight:600">2x1</code>: je programma vraagt een nieuwe lijn, en toont niet meteen <code style="color:#845ef7">Lege bestelling: er is niets besteld.</code>

---

### 5. Weekdagen als tekst

De besteldag typt de magazijnier voortaan als weekdag: de afkorting of het volledige woord, in kleine of grote letters. Vraag ze opnieuw tot ze klopt:

```text
Besteldag (ma-vr): za
Geen werkdag: za.
Besteldag (ma-vr): Dinsdag
```

Op de pakbon staat de leverdag ook als weekdag: <code style="color:#845ef7">Leverdag     : woensdag (deze week)</code>.

| Methode | Geeft terug |
|---|---|
| `static int dagNummer(String dag)` | 1 voor <code style="color:#e64980;font-weight:600">ma</code> of <code style="color:#e64980;font-weight:600">maandag</code>, tot 5 voor <code style="color:#e64980;font-weight:600">vr</code> of <code style="color:#e64980;font-weight:600">vrijdag</code>; -1 voor al de rest |
| `static String dagNaam(int dag)` | <code style="color:#845ef7">maandag</code> tot <code style="color:#845ef7">vrijdag</code> |

> [!TIP]
> Een `switch` werkt ook met tekst, en kan meer waarden per `case` hebben: `case "ma", "maandag" -> 1;` (leerboek 1.5, *Moderne switch*). Zet de tekst eerst in kleine letters.

Test je programma en controleer:

- een bestelling van 10 B650 Tomahawk op <code style="color:#e64980;font-weight:600">vr</code>: <code style="color:#845ef7">Leverdag     : maandag (volgende week)</code>.

---

### 6. De bestelling op één regel

Na de pakbon toont je programma de bestelling nog eens op één regel, voor het logboek van het magazijn:

`BB-2026-44211: 2 x Ryzen 5 7600X, 6 x NH-D15 = 1019,30 euro`

Het bestelnummer is bij jou anders; de rest, <code style="color:#845ef7">: 2 x Ryzen 5 7600X, 6 x NH-D15 = 1019,30 euro</code>, moet kloppen.

Schrijf een methode `static String samenvatting(String bestelnummer, String[] namen, int[] lijnArtikel, int[] lijnAantal, int aantalLijnen, double totaal)` die die regel opbouwt met een `StringBuilder`, en teruggeeft.

> [!WARNING]
> Tussen twee lijnen staat een komma, maar na de laatste niet. Zet de komma *voor* elke lijn behalve de eerste, dan hoef je achteraf niets weg te halen (leerboek 1.8, oefening 5).

---

### 7. Alles samen: de bestelbon uit één regel invoer

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

De magazijnier typt nu de hele bestelling op één regel, met een komma tussen de lijnen. Een lege regel betekent: geen bestelling.

```text
Bestelling (bv. 2x3, 6x2): 2x2, 6x6, 2x1
Besteldag (ma-vr): di
Te betalen: 1218,30 euro
```

Je programma controleert elk deel met de methodes van punt 4. Is er een fout, dan toont het een melding met het foute deel ervoor, en daaronder <code style="color:#845ef7">Er is niets besteld: verbeter de regel.</code> Daarna vraagt het de regel opnieuw.

```text
Bestelling (bv. 2x3, 6x2): 2x2, 9x1, 1 x twee
9x1: Artikel 9 bestaat niet.
1 x twee: Geen geldig getal: twee.
Er is niets besteld: verbeter de regel.
```

Ook de regels die over meer delen gaan, controleer je:

| Situatie | Wat je programma toont |
|---|---|
| een zesde verschillend artikel | <code style="color:#845ef7">4x1: maximaal 5 lijnen per bestelling.</code> |
| hetzelfde artikel, samen meer dan 20 stuks | <code style="color:#845ef7">1x1: te veel, maximaal 20 stuks per artikel.</code> |

> [!IMPORTANT]
> Een regel met een fout bestelt niets, ook niet de delen die wel klopten. Begin daarom bij elke nieuwe poging opnieuw met nul lijnen.

Test je programma en controleer:

- <code style="color:#e64980;font-weight:600">2x2, 6x6, 2x1</code>: een pakbon met dezelfde lijnen als in punt 4, en als laatste regel het bestelnummer met <code style="color:#845ef7">: 3 x Ryzen 5 7600X, 6 x NH-D15 = 1218,30 euro</code>;
- <code style="color:#e64980;font-weight:600">2x2, 9x1, 1 x twee</code> en daarna <code style="color:#e64980;font-weight:600">5x2</code>: een pakbon met enkel <code style="color:#845ef7">Radeon RX 7800 XT       2    499,00      0 %    998,00</code>, zonder Ryzen;
- <code style="color:#e64980;font-weight:600">1x20, 1x1</code>: de melding over 20 stuks per artikel;
- een lege regel: <code style="color:#845ef7">Lege bestelling: er is niets besteld.</code>

> [!NOTE]
> `controleerLijn`, `artikelVan` en `aantalVan` ontleden dezelfde regel drie keer. Een methode kan maar één waarde teruggeven. In hoofdstuk 04 maak je een object `Bestellijn` dat artikel en aantal samen bijhoudt, en ontleed je de regel één keer.
