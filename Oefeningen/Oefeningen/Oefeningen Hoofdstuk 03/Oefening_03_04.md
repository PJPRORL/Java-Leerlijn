# 03_04

<b>Hoofdstuk 03</b> &nbsp;·&nbsp; Opsplitsen in methodes &nbsp;·&nbsp; Dierenasiel &nbsp;·&nbsp; niveau <span style="color:#37b24d">●○○</span> &nbsp;·&nbsp; ±75 min

## Leerdoel

Na deze oefening kan je een lang `main` opsplitsen in methodes, met en zonder returnwaarde, en arrays als parameter doorgeven.

Je leert tekst vergelijken en doorzoeken zonder op hoofdletters te letten, en waarden netjes in kolommen zetten met `printf` en `String.format`.

Daarnaast oefen je op een methode die geen tekst toont, maar tekst opbouwt en teruggeeft: dan beslist wie de methode oproept wat er met die tekst gebeurt.

## Opdracht

Het onthaal van dierenasiel Pootjes werkt met het programma uit 02_04, maar `main` is lang geworden. De soort van een dier staat er drie keer in als ternary operator, de adoptieprijs twee keer. En een leeftijd van `110 maanden` zegt de bezoekers weinig.

Jouw taak is om het programma op te splitsen in methodes, en het daarna duidelijker te laten tonen en beter te laten zoeken.

Het programma moet, net als in 02_04:

1. een menu tonen met het overzicht, adopteren, zoeken tot een maximumleeftijd, een statistiek en een asieloverzicht;

en daarnaast:

2. de herhaalde stukken in methodes hebben;
3. leeftijd en gewicht als leesbare tekst tonen;
4. het overzicht als tabel tonen;
5. dieren zoeken op een stukje van hun naam;
6. de berekeningen over het asiel in methodes hebben;
7. een dierfiche tonen.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 02_04 naar een nieuwe map `Oefening_03_04`. Deze werking blijft dezelfde:

| Wat | Hoe het nu werkt |
|---|---|
| het menu | blijft draaien tot <code style="color:#e64980;font-weight:600">0</code>, met <code style="color:#845ef7">Ongeldige keuze.</code> bij een onbekende keuze |
| een adoptie | eerst de controles, dan de dierfiche, en de status wordt <code style="color:#845ef7">geadopteerd</code> |
| zoeken, statistiek, asieloverzicht | met dezelfde regels als in 02_04 |

> [!TIP]
> Start je programma één keer met een adoptie van Luna door Ines Maes, en vraag daarna het overzicht, de statistiek en het asieloverzicht. Bewaar die uitvoer. Na punt 2 moet je programma exact hetzelfde tonen.

---

### 2. Wat dubbel staat, in een methode

Haal deze stukken uit `main`, en zet ze als methode in dezelfde klasse, onder `main` (leerboek 1.9):

| Methode | Geeft terug |
|---|---|
| `static void toonMenu()` | niets: toont het menu en de vraag <code style="color:#845ef7">Keuze: </code> |
| `static String soortNaam(int soortNummer)` | <code style="color:#845ef7">hond</code>, <code style="color:#845ef7">kat</code> of <code style="color:#845ef7">konijn</code> |
| `static String statusNaam(int status)` | <code style="color:#845ef7">beschikbaar</code>, <code style="color:#845ef7">gereserveerd</code> of <code style="color:#845ef7">geadopteerd</code> |
| `static int adoptieprijs(int soortNummer, int leeftijd)` | de prijs in euro, met de helft voor een senior |
| `static String levensfase(int leeftijd)` | <code style="color:#845ef7">jong</code>, <code style="color:#845ef7">volwassen</code> of <code style="color:#845ef7">senior</code> |
| `static int voer(double gewicht)` | het voer per dag in gram |

Je programma toont daarna exact hetzelfde als in punt 1. Dat is de bedoeling: je verandert hoe de code is opgebouwd, niet wat ze doet.

> [!TIP]
> Een `switch` kan ook een waarde teruggeven: `return switch (soortNummer) { case 1 -> "hond"; ... };` (leerboek 1.5, *Moderne switch*). Vergeet de `default` niet: zonder geeft de compiler een fout.

Test je programma en controleer:

- of de adoptie van Luna de dierfiche met <code style="color:#845ef7">Adoptieprijs : 90 euro</code> toont;
- of het asieloverzicht daarna <code style="color:#845ef7">Opbrengst adopties: 90 euro</code> geeft.

---

### 3. Leeftijd en gewicht als tekst

Schrijf twee methodes die een `String` teruggeven:

| Methode | Voorbeelden |
|---|---|
| `static String leeftijdTekst(int maanden)` | 30 → `"2 jaar en 6 maanden"` · 7 → `"7 maanden"` · 1 → `"1 maand"` · 12 → `"1 jaar"` · 13 → `"1 jaar en 1 maand"` |
| `static String kg(double gewicht)` | 28.5 → `"28,5 kg"` · 31.0 → `"31,0 kg"` |

Voor `kg` gebruik je `String.format` met `%.1f` (leerboek 1.8, *Opgemaakte tekst*).

Gebruik ze overal waar je programma een leeftijd of een gewicht toont: in het overzicht, op de dierfiche, bij het zoeken en in de statistiek.

```text
1. Rex (hond) - 2 jaar en 6 maanden - 28,5 kg - beschikbaar
2. Luna (kat) - 7 maanden - 3,1 kg - beschikbaar
3. Max (hond) - 9 jaar en 2 maanden - 31,0 kg - beschikbaar
```

> [!NOTE]
> `String.format` gebruikt de taalinstellingen van je computer. Op een Belgische Windows krijg je een komma.

Test je programma en controleer:

- de dierfiche van Luna: <code style="color:#845ef7">Leeftijd     : 7 maanden (jong)</code> en <code style="color:#845ef7">Gewicht      : 3,1 kg</code>;
- de statistiek: <code style="color:#845ef7">Gemiddeld gewicht: 12,5 kg</code> en <code style="color:#845ef7">Oudste: Max (9 jaar en 2 maanden)</code>.

> [!TIP]
> In het asiel is er geen dier van precies 1 maand of 1 jaar. Test die gevallen met een tijdelijke regel bovenaan `main`, zoals `System.out.println(leeftijdTekst(13));`, en haal ze daarna weer weg.

---

### 4. Het overzicht als tabel

Toon het overzicht als tabel, met kolommen die netjes onder elkaar staan:

```text
Nr  Naam   Soort   Leeftijd              Gewicht  Status
 1  Rex    hond    2 jaar en 6 maanden   28,5 kg  beschikbaar
 2  Luna   kat     7 maanden              3,1 kg  beschikbaar
 3  Max    hond    9 jaar en 2 maanden   31,0 kg  beschikbaar
 4  Pluis  konijn  1 jaar en 2 maanden    2,2 kg  gereserveerd
 5  Mimi   kat     4 jaar en 2 maanden    4,4 kg  beschikbaar
 6  Bobby  hond    4 maanden              6,0 kg  beschikbaar
```

Schrijf daarvoor twee methodes zonder returnwaarde:

| Methode | Wat ze toont |
|---|---|
| `static void toonKop()` | de regel met de titels |
| `static void toonRij(int nr, String naam, int soortNummer, int leeftijd, double gewicht, int status)` | één dier |

Gebruik ze ook bij het zoeken tot een maximumleeftijd: de gevonden dieren verschijnen dan in dezelfde tabel, met de kop erboven. Daaronder blijft <code style="color:#845ef7">3 dier(en) tot 24 maanden.</code> staan.

| In de opmaak | Betekent |
|---|---|
| `%-20s` | tekst, links uitgelijnd, 20 tekens breed |
| `%8s` | tekst, rechts uitgelijnd, 8 tekens breed |
| `%2d` | geheel getal, rechts uitgelijnd, 2 tekens breed |
| `%n` | een nieuwe regel |

> [!TIP]
> De leeftijd en het gewicht zijn hier al tekst, dankzij punt 3. Je zet ze in de tabel met `%s`, niet met `%d` of `%f`.

---

### 5. Zoeken op een stukje van de naam

Voeg aan het menu de keuze <code style="color:#845ef7">6. Zoeken op naam</code> toe. Je programma vraagt <code style="color:#845ef7">Zoek: </code> en toont elk dier waarvan de naam die tekst bevat, in de tabel van punt 4. Hoofdletters spelen geen rol, en spaties voor of na de zoektekst ook niet.

| Zoek | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">u</code> | Luna en Pluis, en <code style="color:#845ef7">2 dier(en) gevonden.</code> |
| <code style="color:#e64980;font-weight:600">BO</code> | Bobby, en <code style="color:#845ef7">1 dier(en) gevonden.</code> |
| <code style="color:#e64980;font-weight:600">  max </code> | Max |
| <code style="color:#e64980;font-weight:600">zz</code> | <code style="color:#845ef7">Geen dier met zz in de naam.</code> |
| niets, enkel Enter | <code style="color:#845ef7">Geef minstens een letter in.</code> |

Schrijf een methode `static boolean bevat(String tekst, String stuk)` die `true` teruggeeft als `stuk` in `tekst` voorkomt, zonder op hoofdletters te letten. Gebruik `toLowerCase` en `contains` (leerboek 1.8).

> [!WARNING]
> De keuze lees je met `nextInt`, de zoektekst met `nextLine`. Na `nextInt` staat de Enter nog klaar: zonder een extra `sc.nextLine()` krijg je meteen een lege zoektekst (leerboek 1.4, *Het Enter-probleem*).

---

### 6. Rekenen in methodes

De statistiek en het asieloverzicht rekenen met lussen in `main`. Zet die berekeningen in methodes die een array als parameter krijgen:

| Methode | Geeft terug |
|---|---|
| `static double gemiddelde(double[] getallen)` | het gemiddelde, afgerond op twee cijfers na de komma |
| `static int kleinste(int[] getallen)` | de *index* van het kleinste getal |
| `static int grootste(int[] getallen)` | de *index* van het grootste getal |
| `static int opbrengst(int[] soortNummers, int[] leeftijden, int[] statussen)` | de som van de adoptieprijzen van alle geadopteerde dieren |

De statistiek en het asieloverzicht tonen daarna hetzelfde als in punt 3.

> [!NOTE]
> Bij een array krijgt de methode een kopie van de *referentie*: ze kijkt naar dezelfde array als `main` (leerboek 1.9, *Parameters zijn kopieën*). Na een adoptie rekent `opbrengst` dus met de nieuwe status.

Test je programma en controleer:

- de statistiek bij de start: <code style="color:#845ef7">Jongste: Bobby (4 maanden)</code> en <code style="color:#845ef7">Beschikbaar: 5</code>;
- na de adoptie van Max en van Rex: <code style="color:#845ef7">Opbrengst adopties: 225 euro</code>.

---

### 7. Alles samen: de dierfiche als tekst

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Voeg de keuze <code style="color:#845ef7">7. Dierfiche</code> toe. Je programma vraagt <code style="color:#845ef7">Dier (naam of een stuk ervan): </code> en toont de fiche van dat dier:

```text
==========================================
REX
==========================================
Soort    : hond
Leeftijd : 2 jaar en 6 maanden (volwassen)
Gewicht  : 28,5 kg
Voer     : 570 gram per dag
Prijs    : 150 euro
Status   : beschikbaar
==========================================
```

| Regel | Betekenis |
|---|---|
| de titel | de naam in hoofdletters |
| Leeftijd | als tekst, met de levensfase |
| Prijs | de adoptieprijs |
| het kader | een rij `=` die precies zo lang is als de langste regel van de fiche |

Het dier zoek je zo:

| Wat de medewerker typt | Wat je programma toont |
|---|---|
| de volledige naam, in kleine of grote letters | de fiche van dat dier |
| een stuk van precies één naam | de fiche van dat dier |
| een stuk van meer namen, zoals <code style="color:#e64980;font-weight:600">lu</code> | <code style="color:#845ef7">Meerdere dieren: Luna, Pluis.</code> |
| een stuk van geen enkele naam | <code style="color:#845ef7">Geen dier met zz in de naam.</code> |

Schrijf daarvoor twee methodes:

- `static int zoekIndex(String[] namen, String naam)`: de index van het dier met precies die naam, zonder op hoofdletters te letten, of `-1`. Gebruik `equalsIgnoreCase` (leerboek 1.8).
- `static String fiche(String naam, int soortNummer, int leeftijd, double gewicht, int status)`: de volledige fiche als één `String`, met een `\n` na elke regel. De methode toont zelf niets; `main` toont de tekst met `System.out.print`.

> [!TIP]
> Maak eerst elke regel als `String`. Dan ken je hun `length()`, en daarmee de breedte van het kader. Een rij van `n` keer `=` maak je met `"=".repeat(n)` (naslag 10.1), of met een lus. Plak de regels daarna aan elkaar met een `StringBuilder` (leerboek 1.8).

> [!IMPORTANT]
> Zoek eerst op de volledige naam, en pas daarna op een stuk. Wie <code style="color:#e64980;font-weight:600">Max</code> typt, wil de fiche van Max, ook als er later een dier bij komt met *max* in de naam.

Test je programma en controleer:

- <code style="color:#e64980;font-weight:600">x</code>: <code style="color:#845ef7">Meerdere dieren: Rex, Max.</code>;
- <code style="color:#e64980;font-weight:600">Pluis</code>: een fiche met <code style="color:#845ef7">Status   : gereserveerd</code> en <code style="color:#845ef7">Prijs    : 40 euro</code>;
- na de adoptie van Max, <code style="color:#e64980;font-weight:600">max</code>: <code style="color:#845ef7">Status   : geadopteerd</code>, <code style="color:#845ef7">Prijs    : 75 euro</code>, en een kader van 39 tekens.

> [!NOTE]
> De methodes van dit hoofdstuk krijgen vijf of zes parameters mee: naam, soort, leeftijd, gewicht en status horen samen, maar staan in vijf arrays. In hoofdstuk 04 worden ze één object `Dier`, en krijgt een methode één parameter.
