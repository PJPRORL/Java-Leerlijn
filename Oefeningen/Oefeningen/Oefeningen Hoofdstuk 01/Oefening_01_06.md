# 01_06

<b>Hoofdstuk 01</b> &nbsp;·&nbsp; Rekenen zonder afrondingsfouten &nbsp;·&nbsp; Concertzaal &nbsp;·&nbsp; niveau <span style="color:#f03e3e">●●●</span> &nbsp;·&nbsp; ±90 min

## Leerdoel

Na deze oefening kan je bedragen exact bewaren en optellen, door in cent te rekenen met gehele getallen in plaats van met `double`.

Je leert afronden met enkel gehele getallen, en je rekent met een `char` om een plaatsnummer om te zetten naar een rij en een stoel.

Daarnaast oefen je op de voorrang van operatoren en op typecasting: fouten die geen foutmelding geven, maar wel een verkeerde plaats of een cent te veel.

## Opdracht

Concertzaal De Klankkast verkoopt haar tickets aan een balie. De zaal heeft 10 rijen, `A` tot en met `J`, met elk 20 stoelen: 200 plaatsen, genummerd van 1 tot en met 200. De boekhouding aanvaardt geen bedragen die een cent afwijken. Daarom rekent de zaal in cent.

Jouw taak is om het programma te schrijven dat een ticket opmaakt.

Het programma moet:

1. een plaatsnummer kunnen omzetten naar een rij en een stoel;
2. de categorie en de basisprijs van een plaats kunnen bepalen, in cent;
3. kunnen tonen waarom een `double` daarvoor niet geschikt is;
4. de jongerenkorting en de servicekost kunnen aanrekenen;
5. de btw in het ticket kunnen uitrekenen, tot op de cent juist;
6. alles samen kunnen tonen op één ticket.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Maak een nieuwe map `Oefening_01_06` met een bestand `src/Main.java`, zoals in 01_04. Het programma toont één regel:

<code style="color:#845ef7">Concertzaal - ticketbalie</code>

---

### 2. Van plaatsnummer naar rij en stoel

Vraag een plaatsnummer: <code style="color:#845ef7">Plaatsnummer (1 - 200): </code>. Zet het om naar een rij en een stoel:

- plaats 1 tot en met 20 ligt in rij `A`, plaats 21 tot en met 40 in rij `B`, enzovoort;
- de stoel is het nummer binnen de rij, van 1 tot en met 20;
- de plaatscode is de rij gevolgd door de stoel, zonder spatie: plaats 52 is `C12`.

Leg het aantal stoelen per rij vast in een constante. Bewaar de rij als een `char`.

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">52</code> | <code style="color:#845ef7">Plaats: C12 (rij C, stoel 12)</code> |
| <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Plaats: A1 (rij A, stoel 1)</code> |
| <code style="color:#e64980;font-weight:600">20</code> | <code style="color:#845ef7">Plaats: A20 (rij A, stoel 20)</code> |
| <code style="color:#e64980;font-weight:600">21</code> | <code style="color:#845ef7">Plaats: B1 (rij B, stoel 1)</code> |
| <code style="color:#e64980;font-weight:600">200</code> | <code style="color:#845ef7">Plaats: J20 (rij J, stoel 20)</code> |

> [!TIP]
> Een `char` is in Java ook een getal (leerboek 1.3, *Typecasting*). `'A' + 2` is een `int`; met een cast naar `char` wordt het weer een letter.

> [!WARNING]
> Plaats 20 hoort nog bij rij `A`. Deel je `20 / 20`, dan kom je in rij `B` uit. Tel van 0 voor je deelt: plaats 1 tot en met 20 worden 0 tot en met 19.

> [!WARNING]
> `rij + stoel` met een `char` en een `int` geeft geen tekst, maar een getal: `'C' + 12` is 79. Zorg dat er een `String` in de som zit voor de `char` erbij komt.

---

### 3. Categorie en basisprijs, in cent

De rij bepaalt de categorie en de basisprijs, inclusief 6 % btw:

| Rijen | Categorie | Basisprijs |
|---|---|---|
| A tot en met C | 1 | 65,00 euro |
| D tot en met G | 2 | 49,00 euro |
| H tot en met J | 3 | 35,00 euro |

Bewaar de basisprijs als een `int` in cent: 65,00 euro is `6500`. Toon ze in euro, met een komma en altijd twee cijfers na de komma:

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">52</code> | <code style="color:#845ef7">Categorie: 1</code><br><code style="color:#845ef7">Basisprijs: 65,00 euro</code> |
| <code style="color:#e64980;font-weight:600">61</code> | <code style="color:#845ef7">Categorie: 2</code><br><code style="color:#845ef7">Basisprijs: 49,00 euro</code> |
| <code style="color:#e64980;font-weight:600">140</code> | <code style="color:#845ef7">Categorie: 2</code><br><code style="color:#845ef7">Basisprijs: 49,00 euro</code> |
| <code style="color:#e64980;font-weight:600">141</code> | <code style="color:#845ef7">Categorie: 3</code><br><code style="color:#845ef7">Basisprijs: 35,00 euro</code> |

Je hebt geen `printf` en geen `if` nodig. De gehele deling en de rest door 100 geven je de euro en de cent; een rest kleiner dan 10 krijgt een extra nul.

> [!TIP]
> De categorie kan je bepalen met de rij als getal van 0 tot en met 9, die je in punt 2 al uitrekende. Dan heb je geen vergelijking met letters nodig.

---

### 4. Waarom geen double

Een groep van drie jongeren koopt tickets in categorie 2: per ticket 49 euro min 20 % korting, plus 2,50 euro servicekost. Reken het totaal één keer uit met `double` en één keer in cent, en toon beide:

<code style="color:#845ef7">Groep met double: 125.10000000000001 euro</code><br>
<code style="color:#845ef7">Groep in cent: 125,10 euro</code>

> [!NOTE]
> Een `double` bewaart een getal in het tweetallig stelsel. 0,8 is daarin een oneindig lange breuk, zoals 1/3 in het tiendelig stelsel. Daarom rekent de boekhouding niet met `double`. In het werkveld gebruik je `BigDecimal` (naslag 10.2); in dit hoofdstuk volstaan gehele getallen.

---

### 5. Jongerenkorting en servicekost

Vraag de leeftijd van de koper: <code style="color:#845ef7">Leeftijd: </code>. Wie jonger is dan 26, krijgt 20 % korting op de basisprijs, afgerond op de cent. Op elk ticket komt 2,50 euro servicekost, zonder korting. Leg de servicekost in cent vast in een constante.

| Invoer: plaats en leeftijd | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">52</code> en <code style="color:#e64980;font-weight:600">19</code> | <code style="color:#845ef7">Korting: 13,00 euro</code><br><code style="color:#845ef7">Servicekost: 2,50 euro</code><br><code style="color:#845ef7">Te betalen: 54,50 euro</code> |
| <code style="color:#e64980;font-weight:600">52</code> en <code style="color:#e64980;font-weight:600">26</code> | <code style="color:#845ef7">Korting: 0,00 euro</code><br><code style="color:#845ef7">Servicekost: 2,50 euro</code><br><code style="color:#845ef7">Te betalen: 67,50 euro</code> |
| <code style="color:#e64980;font-weight:600">75</code> en <code style="color:#e64980;font-weight:600">22</code> | <code style="color:#845ef7">Korting: 9,80 euro</code><br><code style="color:#845ef7">Servicekost: 2,50 euro</code><br><code style="color:#845ef7">Te betalen: 41,70 euro</code> |

> [!TIP]
> Afronden met een gehele deling doe je door er eerst de helft van de deler bij op te tellen: `(a + 50) / 100` rondt `a` af op honderdtallen. Probeer het met 149 en 150.

---

### 6. Btw met gehele getallen

Het ticket, de basisprijs min de korting, bevat 6 % btw. Reken uit hoeveel, zonder `double`:

- de prijs zonder btw is het ticket gedeeld door 1,06, afgerond op de cent;
- de btw is het verschil.

Toon de btw onder het bedrag:

| Invoer: plaats en leeftijd | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">52</code> en <code style="color:#e64980;font-weight:600">19</code> | <code style="color:#845ef7">Btw (6 %): 2,94 euro</code> |
| <code style="color:#e64980;font-weight:600">52</code> en <code style="color:#e64980;font-weight:600">40</code> | <code style="color:#845ef7">Btw (6 %): 3,68 euro</code> |
| <code style="color:#e64980;font-weight:600">75</code> en <code style="color:#e64980;font-weight:600">22</code> | <code style="color:#845ef7">Btw (6 %): 2,22 euro</code> |
| <code style="color:#e64980;font-weight:600">181</code> en <code style="color:#e64980;font-weight:600">30</code> | <code style="color:#845ef7">Btw (6 %): 1,98 euro</code> |

Gedeeld door 1,06 is hetzelfde als maal 100 en gedeeld door 106. Een gehele deling kapt af; jij wil afronden op de dichtstbijzijnde cent.

> [!NOTE]
> De servicekost valt niet onder de 6 %. Reken de btw enkel uit over het ticket.

---

### 7. Alles samen: het ticket

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

In dit laatste punt zet je alles samen op één ticket. Je programma vraagt eerst voor welk concert:

| Nummer | Artiest | Datum |
|---|---|---|
| 1 | The Lunar Echoes | 2026-12-04 |
| 2 | Brasserie Blues Band | 2026-12-12 |
| 3 | Noor & de Strijkers | 2027-01-09 |

Daarna vraagt het het plaatsnummer en de leeftijd, en toont enkel nog het ticket. De regels van punt 2 tot en met 6 verdwijnen; wat ze uitrekenden, staat op het ticket.

```text
Concertzaal - ticketbalie
Concert (1, 2 of 3): 1
Plaatsnummer (1 - 200): 52
Leeftijd: 19
==============================
CONCERTZAAL - TICKET
==============================
Concert      : The Lunar Echoes (2026-12-04)
Plaats       : C12 (rij C, stoel 12)
Categorie    : 1
Basisprijs   : 65,00 euro
Korting -26  : 13,00 euro
Servicekost  : 2,50 euro
Te betalen   : 54,50 euro
Waarvan btw  : 2,94 euro (6 % op het ticket)
==============================
```

> [!IMPORTANT]
> Elk bedrag op het ticket komt uit een berekening in cent. Er staat geen enkele `double` in je programma.

Test je ticket en controleer:

- met concert <code style="color:#e64980;font-weight:600">2</code>, plaats <code style="color:#e64980;font-weight:600">75</code>, leeftijd <code style="color:#e64980;font-weight:600">40</code>: <code style="color:#845ef7">Plaats       : D15 (rij D, stoel 15)</code>, <code style="color:#845ef7">Te betalen   : 51,50 euro</code> en <code style="color:#845ef7">Waarvan btw  : 2,77 euro (6 % op het ticket)</code>;
- met concert <code style="color:#e64980;font-weight:600">3</code>, plaats <code style="color:#e64980;font-weight:600">200</code>, leeftijd <code style="color:#e64980;font-weight:600">25</code>: <code style="color:#845ef7">Plaats       : J20 (rij J, stoel 20)</code>, <code style="color:#845ef7">Korting -26  : 7,00 euro</code> en <code style="color:#845ef7">Te betalen   : 30,50 euro</code>;
- of <code style="color:#845ef7">Korting -26  : 0,00 euro</code> met de nul verschijnt bij iemand van 26 of ouder.

> [!NOTE]
> Je schrijft de omzetting van cent naar euro nu vijf keer uit, telkens met een andere variabele. Dat voelt verkeerd, en dat is het ook. In hoofdstuk 03 maak je er één methode van.
