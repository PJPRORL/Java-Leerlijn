# 02_06

<b>Hoofdstuk 02</b> &nbsp;·&nbsp; Tabellen en geneste lussen &nbsp;·&nbsp; Concertzaal &nbsp;·&nbsp; niveau <span style="color:#f03e3e">●●●</span> &nbsp;·&nbsp; ±2 uur

## Leerdoel

Na deze oefening kan je een zaalplan bewaren in een tweedimensionale array, en het doorlopen met geneste lussen.

Je leert met een gelabelde `break` twee lussen tegelijk verlaten zodra je gevonden hebt wat je zocht, en met `do-while` een vraag herhalen tot het antwoord klopt.

Daarnaast oefen je op zoeken in een tabel: de eerste vrije plaats, en een reeks vrije plaatsen naast elkaar.

## Opdracht

Concertzaal De Klankkast maakt haar tickets met het programma uit 01_06. Maar het programma weet niet welke plaatsen al verkocht zijn: aan de balie gebeurt het dat twee mensen hetzelfde ticket krijgen. En een groep die samen wil zitten, moet zelf het zaalplan afzoeken.

Jouw taak is om het programma het zaalplan te laten kennen.

Het programma moet, net als in 01_06:

1. een ticket opmaken met plaats, categorie, jongerenkorting, servicekost en btw tot op de cent;

en daarnaast:

2. het zaalplan tonen;
3. weigeren een plaats te verkopen die niet bestaat of al bezet is;
4. tonen hoeveel plaatsen er per categorie vrij zijn;
5. de beste vrije plaats vinden;
6. een reeks vrije plaatsen naast elkaar vinden voor een groep;
7. een groep plaatsen en één groepsticket opmaken.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 01_06 naar een nieuwe map `Oefening_02_06`. Deze regels blijven precies dezelfde:

| Wat | Hoe |
|---|---|
| plaatsnummer naar rij en stoel | rij = (nummer − 1) / 20, met 0 = `A`; stoel = (nummer − 1) % 20 + 1 |
| categorie en basisprijs | rij A–C 65,00 · rij D–G 49,00 · rij H–J 35,00 euro, in cent |
| jongerenkorting | 20 % op de basisprijs voor wie jonger is dan 26 |
| servicekost | 2,50 euro per ticket |
| btw | 6 % in het ticket, met `(ticket * 100 + 53) / 106` |
| het ticket | dezelfde labels als in 01_06 |

---

### 2. Het zaalplan

Bewaar het zaalplan als een tweedimensionale array van `boolean`: 10 rijen van 20 stoelen, `true` voor een bezette plaats (leerboek 1.7). Leg het aantal rijen vast in een constante, naast het aantal stoelen per rij.

Bij de start zijn deze plaatsen al verkocht:

| Rij | Bezet |
|---|---|
| A | stoel 1 tot en met 8, en 13 tot en met 20 |
| B | stoel 5 tot en met 10 |
| C | stoel 12 |
| D | stoel 1 tot en met 4 |
| F | de hele rij |
| H | stoel 10 |
| J | stoel 19 en 20 |

Bewaar die als een array van plaatsnummers, en zet ze met een lus in het zaalplan. Toon het zaalplan bij de start, met `X` voor bezet en `.` voor vrij, en daaronder het aantal vrije plaatsen:

```text
Concertzaal - ticketbalie
   12345678901234567890
A  XXXXXXXX....XXXXXXXX
B  ....XXXXXX..........
C  ...........X........
D  XXXX................
E  ....................
F  XXXXXXXXXXXXXXXXXXXX
G  ....................
H  .........X..........
I  ....................
J  ..................XX
Vrij: 150 van 200
```

Daarna gaat het programma verder zoals in 01_06.

> [!TIP]
> Van een plaatsnummer naar een rij en een stoel ken je uit 01_06. Voor de array tel je ook de stoel vanaf 0: `bezet[rij][stoel - 1]`.

> [!TIP]
> Een regel van het zaalplan bouw je op in een `String`: begin met de letter en twee spaties, en plak er in een lus een `X` of een `.` aan.

---

### 3. Een plaats kiezen

Vraag het plaatsnummer opnieuw zolang de plaats niet bestaat of al bezet is:

```text
Plaatsnummer (1 - 200): 0
Plaats 0 bestaat niet.
Plaatsnummer (1 - 200): 201
Plaats 201 bestaat niet.
Plaatsnummer (1 - 200): 52
Plaats C12 is al bezet.
Plaatsnummer (1 - 200): 33
Leeftijd: 19
```

Na het ticket is de plaats bezet. Toon op het einde van het programma het zaalplan opnieuw: plaats B13 heeft nu een `X`, en er zijn nog 149 plaatsen vrij.

> [!WARNING]
> Controleer eerst of het nummer bestaat, en pas dan of de plaats bezet is. In de omgekeerde volgorde vraag je naar `bezet[10][0]` bij plaats 201, en stopt je programma met een `ArrayIndexOutOfBoundsException`.

---

### 4. Vrije plaatsen per categorie

Toon na het ticket hoeveel plaatsen er in elke categorie nog vrij zijn, en wat ze samen opbrengen als ze allemaal verkocht worden, zonder korting en zonder servicekost:

| Na een ticket op plaats | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">33</code> | <code style="color:#845ef7">Categorie 1: 36 vrij</code><br><code style="color:#845ef7">Categorie 2: 56 vrij</code><br><code style="color:#845ef7">Categorie 3: 57 vrij</code><br><code style="color:#845ef7">Nog te verkopen: 7079,00 euro</code> |

> [!TIP]
> Bewaar de drie tellers in een array van 3, met de categorie min 1 als index. Dan heb je geen drie aparte variabelen en geen `if` per categorie nodig.

---

### 5. De beste vrije plaats

De beste plaats is de eerste vrije plaats als je het zaalplan leest zoals een boek: rij A van links naar rechts, dan rij B, enzovoort. Toon ze onder de categorieën:

| Na een ticket op plaats | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">33</code> | <code style="color:#845ef7">Beste vrije plaats: A9 (categorie 1)</code> |
| <code style="color:#e64980;font-weight:600">9</code> | <code style="color:#845ef7">Beste vrije plaats: A10 (categorie 1)</code> |

Is er geen enkele plaats vrij, dan toont je programma <code style="color:#845ef7">De zaal is uitverkocht.</code>

> [!TIP]
> Een gewone `break` stopt enkel de binnenste lus: je zoekt dan verder in de volgende rij. Met een label voor de buitenste lus stop je ze allebei (leerboek 1.6, *break en continue*).

---

### 6. Plaatsen naast elkaar

Een groep wil samen zitten, in dezelfde rij, op stoelen naast elkaar. Vraag de grootte van de groep, opnieuw zolang die niet tussen 1 en 8 ligt: <code style="color:#845ef7">Groepsgrootte (1-8): </code>.

Zoek dan, op dezelfde manier als in punt 5, de eerste reeks vrije stoelen naast elkaar die groot genoeg is:

| Na een ticket op plaats 33, met een groep van | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">4 plaatsen naast elkaar: A9 tot A12</code> |
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">5 plaatsen naast elkaar: B14 tot B18</code> |
| <code style="color:#e64980;font-weight:600">8</code> | <code style="color:#845ef7">8 plaatsen naast elkaar: C1 tot C8</code> |

Is er nergens zo'n reeks, dan toont je programma <code style="color:#845ef7">Geen 8 plaatsen naast elkaar.</code>

> [!TIP]
> Tel in elke rij hoeveel vrije stoelen je na elkaar tegenkomt. Een bezette stoel zet de teller terug op 0; een nieuwe rij ook. Haalt de teller de grootte van de groep, dan ben je klaar, en weet je ook waar de reeks begint.

> [!WARNING]
> Een groep van 5 past niet in B1 tot B4 en ook niet in B11 tot B12: de stoelen van B13 zijn verkocht. Een reeks die over een bezette stoel loopt, of over het einde van een rij, telt niet.

---

### 7. Alles samen: vier plaatsen naast elkaar

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

In dit laatste punt plaats je de groep echt: je programma vraagt de leeftijd van elke persoon, zet de plaatsen op bezet, en maakt één groepsticket.

```text
Groepsgrootte (1-8): 4
Leeftijd persoon 1: 19
Leeftijd persoon 2: 30
Leeftijd persoon 3: 22
Leeftijd persoon 4: 45
==============================
CONCERTZAAL - GROEPSTICKET
==============================
Concert      : The Lunar Echoes (2026-12-04)
Plaatsen     : A9 tot A12 (categorie 1)
Tickets      : 4 x 65,00 euro
Korting -26  : 26,00 euro
Servicekost  : 10,00 euro
Te betalen   : 244,00 euro
Waarvan btw  : 13,25 euro (6 % op de tickets)
==============================
```

| Regel | Betekenis |
|---|---|
| Korting -26 | 20 % van de basisprijs, voor elke persoon jonger dan 26 apart afgerond, en dan opgeteld |
| Servicekost | 2,50 euro per persoon |
| Waarvan btw | 6 % in de tickets na korting, samen, zoals in 01_06 |

Vindt je programma geen reeks die groot genoeg is, dan vraagt het geen leeftijden en verandert er niets. Het zaalplan op het einde toont de groep als vier `X` naast elkaar.

> [!IMPORTANT]
> Alle bedragen in cent, zoals in 01_06. En de plaatsen worden pas bezet nadat alle leeftijden gekend zijn.

Test je programma en controleer:

- na een ticket op plaats 33 en een groep van 4: rij A van het zaalplan is volledig bezet, en <code style="color:#845ef7">Vrij: 145 van 200</code>;
- een groep van 5 personen van 30 jaar voor concert 2: <code style="color:#845ef7">Plaatsen     : B14 tot B18 (categorie 1)</code>, <code style="color:#845ef7">Korting -26  : 0,00 euro</code> en <code style="color:#845ef7">Te betalen   : 337,50 euro</code>;
- of het zaalplan op het einde zowel het ticket van punt 3 als de groep toont.

> [!NOTE]
> Het zaalplan tonen staat nu twee keer in je code, de categorie reken je op vier plaatsen uit, en de basisprijs op drie. In hoofdstuk 03 maak je er methodes van, en verdwijnt de herhaling.
