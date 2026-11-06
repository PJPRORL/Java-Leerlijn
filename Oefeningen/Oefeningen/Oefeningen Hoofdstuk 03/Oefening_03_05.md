# 03_05

<b>Hoofdstuk 03</b> &nbsp;·&nbsp; Tekst ontleden &nbsp;·&nbsp; Fietsverhuur &nbsp;·&nbsp; niveau <span style="color:#f08c00">●●○</span> &nbsp;·&nbsp; ±90 min

## Leerdoel

Na deze oefening kan je een tijd zoals `09:45` ontleden met `split`, controleren of elk stuk een getal is, en het omzetten met `Integer.parseInt`.

Je leert de tarieven van een programma in methodes met een returnwaarde zetten, en tekst opbouwen met `StringBuilder` en `String.format`.

Daarnaast oefen je op een invoer die pas aanvaard wordt als élk deel klopt: één fout deel, en er wordt niets verhuurd.

## Opdracht

Fietsverhuur Trapper werkt met het programma uit 02_05. Een tijd typt de baliemedewerker als twee getallen, en op het scherm verschijnt `9:5` voor vijf over negen. Een klant kiest hij met een nummer dat enkel het programma kent. En aan de balie gaat het snel: hij wil een verhuring op één regel typen.

Jouw taak is om het programma tekst te laten lezen en schrijven.

Het programma moet, net als in 02_05:

1. fietsen verhuren en terugbrengen met controles, een huurcontract en een overzicht per klant;

en daarnaast:

2. de tarieven in methodes hebben;
3. elke tijd tonen als `09:45`;
4. een tijd als tekst lezen;
5. een klant op nummer of naam vinden;
6. het overzicht per klant op één regel per klant tonen;
7. een verhuring uit één regel invoer lezen en een huurbon tonen.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 02_05 naar een nieuwe map `Oefening_03_05`. Deze regels blijven precies dezelfde:

| Type | Per begonnen uur | Maximum per dag | Waarborg |
|---|---|---|---|
| stadsfiets | 3 euro | 15 euro | 50 euro |
| e-bike | 6 euro | 30 euro | 150 euro |
| bakfiets | 8 euro | 40 euro | 100 euro |

| Regel | Waarde |
|---|---|
| Te laat | terug na 20:00 uur: 10 euro boete |
| Per klant | hoogstens 2 fietsen tegelijk |
| Terugbrengen | de fiets wordt pas weer beschikbaar na een <code style="color:#e64980;font-weight:600">j</code> |

> [!TIP]
> Start je programma één keer met een verhuring van fiets 3 aan Sara om 9:45, en breng ze terug om 13:10. Bewaar het huurcontract. Na punt 2 moet je programma, op het contractnummer na, exact hetzelfde contract tonen.

---

### 2. De tarieven in methodes

Zet elke regel die een waarde geeft in een eigen methode met een returnwaarde (leerboek 1.9):

| Methode | Geeft terug |
|---|---|
| `static String typeNaam(int type)` | <code style="color:#845ef7">stadsfiets</code>, <code style="color:#845ef7">e-bike</code> of <code style="color:#845ef7">bakfiets</code> |
| `static String statusNaam(int status)` | <code style="color:#845ef7">beschikbaar</code>, <code style="color:#845ef7">verhuurd</code> of <code style="color:#845ef7">in herstelling</code> |
| `static int uurtarief(int type)`, `dagmaximum(int type)`, `waarborg(int type)` | het bedrag in euro |
| `static int begonnenUren(int duur)` | het aantal begonnen uren voor een duur in minuten |
| `static int huurprijs(int type, int duur)` | het kleinste van begonnen uren × uurtarief en het dagmaximum |

Je programma toont daarna hetzelfde als in punt 1.

> [!WARNING]
> `huurprijs(int type, int duur)` heeft twee parameters van hetzelfde type. Roep je ze in de omgekeerde volgorde op, dan geeft de compiler geen fout, maar rekent je programma met een fiets van type 205.

Test je programma en controleer:

- het contract van fiets 3, van 9:45 tot 13:10: <code style="color:#845ef7">Huurprijs     : 24 euro</code>;
- <code style="color:#845ef7">Fiets         : e-bike Stromer (6 euro per uur, max 30 euro)</code>.

---

### 3. Tijden als 09:45

Schrijf een methode `static String tijd(int minuten)` die een tijd in minuten na middernacht teruggeeft als tekst: altijd twee cijfers voor het uur en twee voor de minuten. Gebruik `String.format` met `%02d` (leerboek 1.8, *Opgemaakte tekst*).

| Oproep | Geeft terug |
|---|---|
| `tijd(545)` | `"09:05"` |
| `tijd(510)` | `"08:30"` |
| `tijd(790)` | `"13:10"` |

Gebruik ze overal waar je programma een tijd toont: na een verhuring, op het contract en in het overzicht per klant.

> [!TIP]
> In `%02d` zegt de `2` hoe breed het getal is, en de `0` dat de lege plaatsen nullen worden in plaats van spaties.

Test je programma en controleer:

- een verhuring van fiets 3 om 9 uur 5: <code style="color:#845ef7">Fiets 3 verhuurd aan Sara Janssens om 09:05, contract FV-2026-</code> met vier cijfers;
- het overzicht per klant: <code style="color:#845ef7">- fiets 4, e-bike Cube, sinds 08:30</code>;
- het contract daarna: <code style="color:#845ef7">Vertrek       : 09:05</code> en <code style="color:#845ef7">Duur          : 4 u 5 min (5 begonnen uren)</code>.

---

### 4. Een tijd als tekst

De baliemedewerker typt een tijd voortaan als één stuk tekst. Vraag ze opnieuw tot ze klopt:

```text
Vertrek (uu:mm): 9u45
Geen geldige tijd: 9u45.
Vertrek (uu:mm): 25:00
Geen geldige tijd: 25:00.
Vertrek (uu:mm): 9:5
Geen geldige tijd: 9:5.
Vertrek (uu:mm): 9:45
Fiets 3 verhuurd aan Sara Janssens om 09:45, contract FV-2026-9673.
```

| Een geldige tijd | Voorbeeld |
|---|---|
| een uur, een `:`, en twee cijfers voor de minuten | <code style="color:#e64980;font-weight:600">9:45</code> en <code style="color:#e64980;font-weight:600">09:45</code> zijn allebei goed |
| het uur van 0 tot en met 23, de minuten van 0 tot en met 59 | <code style="color:#e64980;font-weight:600">25:00</code> en <code style="color:#e64980;font-weight:600">9:60</code> zijn fout |

Schrijf daarvoor drie methodes:

| Methode | Geeft terug |
|---|---|
| `static boolean isGetal(String tekst)` | `true` als de tekst niet leeg is en enkel uit cijfers bestaat |
| `static int minuten(String tijd)` | de tijd in minuten na middernacht, of -1 als ze niet geldig is |
| `static int vraagTijd(Scanner sc, String wat)` | vraagt de tijd tot ze klopt, en geeft de minuten terug |

Gebruik `vraagTijd(sc, "Vertrek")` bij het verhuren en `vraagTijd(sc, "Terug")` bij het terugbrengen.

> [!TIP]
> `tijd.split(":")` geeft een array met het uur en de minuten (leerboek 1.8). Klopt de vorm, dan heeft die array precies twee elementen. Controleer met `isGetal`, en zet pas dan om met `Integer.parseInt` (naslag 10.2).

> [!WARNING]
> `Integer.parseInt("4a")` stopt je programma met een `NumberFormatException`. Daarom controleer je eerst teken per teken met `Character.isDigit` (naslag 10.1).

Test je programma en controleer:

- de vier tijden hierboven;
- fiets 3 terug om <code style="color:#e64980;font-weight:600">13:10</code>: <code style="color:#845ef7">Huurprijs     : 24 euro</code>;
- fiets 4 terug om <code style="color:#e64980;font-weight:600">8:00</code>: <code style="color:#845ef7">De terugkomst ligt niet na het vertrek.</code>

---

### 5. Een klant op nummer of naam

Elke klant heeft een klantnummer. Bewaar de nummers in een array naast de namen:

| Klantnummer | Naam |
|---|---|
| F001 | Sara Janssens |
| F002 | Tom Wouters |

Bij het verhuren vraagt je programma <code style="color:#845ef7">Klant (nummer of naam): </code>. De medewerker typt het nummer of de volledige naam, in kleine of grote letters. Een klant die niet bestaat, geeft <code style="color:#845ef7">Onbekende klant: Jan.</code>

Schrijf een methode `static int zoekKlant(String[] nummers, String[] namen, String tekst)` die de index van de klant teruggeeft, of -1. Gebruik `equalsIgnoreCase` (leerboek 1.8).

Op het huurcontract staat de klant voortaan met zijn nummer: <code style="color:#845ef7">Klant         : F002 Tom Wouters</code>.

> [!WARNING]
> Tekst vergelijk je met `equals` of `equalsIgnoreCase`, nooit met `==`. Met `==` vindt je programma de klant `F001` niet, ook als je precies `F001` typt (leerboek 1.8).

> [!IMPORTANT]
> Een naam bevat een spatie: lees ze met `nextLine`. Het fietsnummer las je met `nextInt`, dus staat er nog een Enter klaar. Haal die eerst weg met een extra `sc.nextLine()`.

Test je programma en controleer:

- fiets 3 aan <code style="color:#e64980;font-weight:600">f001</code>: <code style="color:#845ef7">Fiets 3 verhuurd aan Sara Janssens om 09:45, contract FV-2026-</code> met vier cijfers;
- daarna fiets 1 aan <code style="color:#e64980;font-weight:600">Tom Wouters</code>, en fiets 2 aan <code style="color:#e64980;font-weight:600">tom wouters</code>: <code style="color:#845ef7">Tom Wouters heeft al 2 fietsen.</code>

---

### 6. Het overzicht op één regel per klant

Toon het overzicht per klant voortaan met één regel per klant, met een komma tussen de fietsen:

```text
F001 Sara Janssens: fiets 3 (e-bike Stromer, sinds 09:45)
F002 Tom Wouters: fiets 1 (stadsfiets Gazelle, sinds 09:00), fiets 4 (e-bike Cube, sinds 08:30)
```

Heeft een klant geen fietsen, dan staat er <code style="color:#845ef7">F001 Sara Janssens: geen fietsen</code>.

Schrijf een methode `static String fietsenVan(int klant, int[] huurders, int[] typeNummers, String[] merken, int[] vertrekken)` die het stuk na de dubbele punt opbouwt met een `StringBuilder`, en teruggeeft.

> [!TIP]
> Zet de komma *voor* elke fiets, behalve voor de eerste die je toevoegt. Of je al iets toevoegde, zie je aan `sb.length()`. De index van de fiets zegt dat niet: de eerste fiets van Tom is fiets 4.

---

### 7. Alles samen: de huurbon uit één regel invoer

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Bij het verhuren typt de medewerker nu alles op één regel: het fietsnummer, het klantnummer en de vertrektijd, gescheiden door een spatie. Lukt de verhuring, dan toont je programma een huurbon:

```text
Verhuring (fiets klant uu:mm): 3 F001 09:45
==============================
FIETSVERHUUR - HUURBON
==============================
Contract      : FV-2026-1455
Klant         : F001 Sara Janssens
Fiets         : 3 e-bike Stromer
Vertrek       : 09:45
Tarief        : 6 euro per begonnen uur, max 30 euro
Nu te betalen : 150 euro (waarborg)
==============================
```

Controleer in deze volgorde, en toon bij de eerste fout één melding:

| Situatie | Wat je programma toont |
|---|---|
| niet precies drie delen | <code style="color:#845ef7">Gebruik de vorm fiets klant uu:mm, zoals 3 F001 09:45.</code> |
| het fietsnummer is geen getal of bestaat niet | <code style="color:#845ef7">Fiets 9 bestaat niet.</code> |
| de fiets is verhuurd of in herstelling | <code style="color:#845ef7">Fiets 3 is al verhuurd.</code> of <code style="color:#845ef7">Fiets 6 is in herstelling.</code> |
| het klantnummer bestaat niet | <code style="color:#845ef7">Onbekende klant: F003.</code> |
| de klant heeft al 2 fietsen | <code style="color:#845ef7">Tom Wouters heeft al 2 fietsen.</code> |
| de tijd is niet geldig | <code style="color:#845ef7">Geen geldige tijd: 25:00.</code> |

Schrijf een methode `static String huurbon(String contract, String klant, int fiets, int type, String merk, int vertrek)` die de huurbon opbouwt met een `StringBuilder`, en teruggeeft. Gebruik ze ook voor de tarieven: `String.format` met `%d`.

> [!IMPORTANT]
> Verander de arrays pas als alle controles gelukt zijn. Zet je de status al op verhuurd voor je de klant controleert, dan blijft de fiets verhuurd bij een onbekende klant, aan niemand.

> [!NOTE]
> In één regel typt de medewerker het klantnummer, niet de naam. Een naam bevat een spatie, en dan heeft de regel vier delen.

Test je programma en controleer:

- de verhuring hierboven, en daarna <code style="color:#e64980;font-weight:600">3 F002 10:00</code>: <code style="color:#845ef7">Fiets 3 is al verhuurd.</code>;
- <code style="color:#e64980;font-weight:600">1 F003 10:00</code> en <code style="color:#e64980;font-weight:600">1 F002 25:00</code>: telkens één melding, en fiets 1 blijft beschikbaar;
- <code style="color:#e64980;font-weight:600">1 f002 9:00</code>: een huurbon met <code style="color:#845ef7">Vertrek       : 09:00</code> en <code style="color:#845ef7">Nu te betalen : 50 euro (waarborg)</code>;
- <code style="color:#e64980;font-weight:600">1 Tom Wouters 9:00</code>: de melding over de vorm;
- het overzicht per klant daarna: <code style="color:#845ef7">F002 Tom Wouters: fiets 1 (stadsfiets Gazelle, sinds 09:00), fiets 4 (e-bike Cube, sinds 08:30)</code>.

> [!NOTE]
> Het fietsnummer, de klant en de tijd ontleed je in `main` één voor één. In hoofdstuk 04 maak je een klasse `Verhuring`, die zelf weet hoe ze uit een regel tekst ontstaat.
