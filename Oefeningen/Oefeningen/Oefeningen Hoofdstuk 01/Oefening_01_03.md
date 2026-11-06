# 01_03

<b>Hoofdstuk 01</b> &nbsp;·&nbsp; Rekenen zonder afrondingsfouten &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#f03e3e">●●●</span> &nbsp;·&nbsp; ±90 min

## Leerdoel

Na deze oefening kan je bedragen exact bewaren en optellen, door in cent te rekenen met gehele getallen in plaats van met `double`.

Je leert afronden en naar boven afronden met enkel gehele getallen, en je ziet waarom de voorrang van operatoren daarbij telt.

Daarnaast oefen je op het verschil tussen afkappen en afronden bij typecasting: een fout die geen foutmelding geeft, maar wel een cent verkeerd.

## Opdracht

Klanten van Bit & Byte stellen online hun eigen pc samen. Voor ze bestellen, wil Bit & Byte hun twee dingen vertellen: wat de build kost, met de btw apart, en of de voeding sterk genoeg is. Het boekhoudprogramma aanvaardt geen bedragen die een cent afwijken. Daarom rekent het team in cent.

Jouw taak is om het rekenwerk achter de configurator te schrijven.

Het programma moet:

1. de prijs van een build in cent kunnen optellen en tonen in euro;
2. kunnen tonen waarom een `double` daarvoor niet geschikt is;
3. een bedrag in euro kunnen omzetten naar cent;
4. de btw in een bedrag kunnen uitrekenen, tot op de cent juist;
5. het voedingsadvies voor een build kunnen geven;
6. alles samen kunnen tonen in één buildrapport.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Maak een nieuwe map `Oefening_01_03` met een bestand `src/Main.java`, zoals in 01_01. Het programma toont één regel:

<code style="color:#845ef7">Bit & Byte - configurator</code>

---

### 2. Bedragen in cent

De voorbeeldbuild bestaat uit deze onderdelen uit de catalogus:

| Code | Onderdeel | Prijs |
|---|---|---|
| CPU-01 | Ryzen 5 7600X | 199.00 |
| MB-01 | B650 Tomahawk | 219.00 |
| RAM-01 | 32 GB DDR5-6000 | 109.00 |
| GPU-01 | GeForce RTX 4070 | 589.00 |
| PSU-01 | Voeding 550 W | 69.00 |
| KAST-02 | Midi Air | 99.00 |

Bewaar elke prijs als een `int` in cent: 199.00 euro is `19900`. Tel ze op, en toon het totaal in euro, met een komma en altijd twee cijfers na de komma:

<code style="color:#845ef7">Onderdelen: 1284,00 euro</code>

Je hebt geen `printf` en geen `if` nodig. De gehele deling en de rest door 100 geven je de euro en de cent.

> [!WARNING]
> 5 cent moet `,05` worden, niet `,5`. Een rest kleiner dan 10 heeft een extra nul nodig. Die kies je met de ternary operator.

> [!WARNING]
> Bij `"Onderdelen: " + totaal / 100 + ","` rekent Java eerst de deling uit, en plakt dan aan elkaar: `/` en `%` gaan voor `+` (leerboek 1.3, *Voorrang*). Met een ternary operator in je tekst heb je wel haakjes nodig.

---

### 3. Waarom geen double

Bij de build horen twee accessoires: kabelbinders van 0,10 euro en een schroevenset van 0,20 euro. Tel ze één keer op als `double` en één keer in cent, en toon beide onder de onderdelen:

<code style="color:#845ef7">Accessoires met double: 0.30000000000000004 euro</code><br>
<code style="color:#845ef7">Accessoires in cent: 0,30 euro</code>

> [!NOTE]
> Een `double` bewaart een getal in het tweetallig stelsel. 0,1 is daarin een oneindig lange breuk, zoals 1/3 in het tiendelig stelsel. Daarom rekent het boekhoudprogramma niet met `double`. In het werkveld gebruik je `BigDecimal` (naslag 10.2); in dit hoofdstuk volstaan gehele getallen.

---

### 4. Een bedrag inlezen

Een klant typt een prijs in euro, zoals `19,99`. Lees die in als `double` en zet ze om naar cent, op twee manieren:

- met een cast: `(int) (prijs * 100)`;
- met `Math.round`, en daarna een cast naar `int`.

Toon beide resultaten:

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">19,99</code> | <code style="color:#845ef7">Afgekapt: 1998 cent</code><br><code style="color:#845ef7">Afgerond: 1999 cent</code> |
| <code style="color:#e64980;font-weight:600">4,35</code> | <code style="color:#845ef7">Afgekapt: 434 cent</code><br><code style="color:#845ef7">Afgerond: 435 cent</code> |
| <code style="color:#e64980;font-weight:600">0,29</code> | <code style="color:#845ef7">Afgekapt: 28 cent</code><br><code style="color:#845ef7">Afgerond: 29 cent</code> |
| <code style="color:#e64980;font-weight:600">219,00</code> | <code style="color:#845ef7">Afgekapt: 21900 cent</code><br><code style="color:#845ef7">Afgerond: 21900 cent</code> |

> [!IMPORTANT]
> Vanaf nu zet je elk ingelezen bedrag om met `Math.round`. Een cast kapt af, en `19.99 * 100` is in een `double` net iets minder dan 1999.

> [!TIP]
> `Math.round` geeft bij een `double` een `long` terug (naslag 10.2). Een `long` past niet zomaar in een `int`: daarvoor heb je toch nog een cast nodig.

---

### 5. Btw met gehele getallen

Reken uit hoeveel van de onderdelen uit punt 2 btw is, zonder `double`:

- de prijs zonder btw is het totaal gedeeld door 1,21, afgerond op de cent;
- de btw is het verschil.

Toon beide onder de vorige regels:

<code style="color:#845ef7">Zonder btw: 1061,16 euro</code><br>
<code style="color:#845ef7">Btw: 222,84 euro</code>

Gedeeld door 1,21 is hetzelfde als maal 100 en gedeeld door 121. Een gehele deling kapt af; jij wil afronden op de dichtstbijzijnde cent.

> [!TIP]
> Afronden met een gehele deling doe je door er eerst de helft van de deler bij op te tellen: `(a + 50) / 100` rondt `a` af op honderdtallen. Probeer het met 149 en 150.

> [!WARNING]
> `totaal * 100` wordt groot. Een `int` gaat tot ongeveer 2,1 miljard (leerboek 1.2): voor deze bedragen is dat ruim genoeg, maar een build van meer dan 214 000 euro zou zonder foutmelding een verkeerd getal geven.

---

### 6. Het voedingsadvies

Vraag het vermogen van de processor, van de videokaart en van de voeding, in watt:

<code style="color:#845ef7">Watt processor: </code><br>
<code style="color:#845ef7">Watt videokaart: </code><br>
<code style="color:#845ef7">Watt voeding: </code>

Het voedingsadvies van Bit & Byte:

1. Tel het vermogen van processor en videokaart op, plus 75 W voor de rest van de pc.
2. Neem daar 1,3 keer van: 30 % reserve.
3. Rond naar boven af op een veelvoud van 50 W.

Toon daarna het vermogen, het advies en het oordeel over de voeding:

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">105</code> <code style="color:#e64980;font-weight:600">200</code> <code style="color:#e64980;font-weight:600">550</code> | <code style="color:#845ef7">Vermogen: 380 W</code><br><code style="color:#845ef7">Advies: 500 W</code><br><code style="color:#845ef7">Voeding: 550 W (volstaat, marge 50 W)</code> |
| <code style="color:#e64980;font-weight:600">120</code> <code style="color:#e64980;font-weight:600">263</code> <code style="color:#e64980;font-weight:600">550</code> | <code style="color:#845ef7">Vermogen: 458 W</code><br><code style="color:#845ef7">Advies: 600 W</code><br><code style="color:#845ef7">Voeding: 550 W (te zwak, 50 W tekort)</code> |
| <code style="color:#e64980;font-weight:600">125</code> <code style="color:#e64980;font-weight:600">263</code> <code style="color:#e64980;font-weight:600">850</code> | <code style="color:#845ef7">Vermogen: 463 W</code><br><code style="color:#845ef7">Advies: 650 W</code><br><code style="color:#845ef7">Voeding: 850 W (volstaat, marge 200 W)</code> |
| <code style="color:#e64980;font-weight:600">100</code> <code style="color:#e64980;font-weight:600">175</code> <code style="color:#e64980;font-weight:600">400</code> | <code style="color:#845ef7">Vermogen: 350 W</code><br><code style="color:#845ef7">Advies: 500 W</code><br><code style="color:#845ef7">Voeding: 400 W (te zwak, 100 W tekort)</code> |

Reken enkel met gehele getallen: maal 1,3 is maal 13 en gedeeld door 10.

> [!TIP]
> Naar boven afronden met een gehele deling: tel er de deler min 1 bij op. `(x + 49) / 50` geeft het aantal blokken van 50 W, ook als het laatste niet vol is. Je kent die truc uit de dozen van 01_02.

> [!WARNING]
> `x + 49 / 50 * 50` is gewoon `x`: `49 / 50` is 0, en de deling gaat voor de optelling. Bij afronden met gehele getallen beslissen de haakjes alles.

Test je programma en controleer:

- de laatste rij: 350 × 1,3 = 455, en dat is net meer dan 450;
- <code style="color:#e64980;font-weight:600">125</code> en <code style="color:#e64980;font-weight:600">300</code>: samen met 75 W is dat 500 W, en 500 × 1,3 = 650, precies een veelvoud van 50. Het advies moet <code style="color:#845ef7">Advies: 650 W</code> zijn, niet 700 W;
- <code style="color:#e64980;font-weight:600">110</code> en <code style="color:#e64980;font-weight:600">200</code>: samen met 75 W is dat 385 W, en 385 × 1,3 = 500,5. Dat is net meer dan 500: het advies moet <code style="color:#845ef7">Advies: 550 W</code> zijn. Kap je × 1,3 af in plaats van naar boven af te ronden, dan krijg je hier 500 W.

---

### 7. Alles samen: het buildrapport

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

In dit laatste punt zet je het rekenwerk samen in één rapport. De klant kiest zelf zijn processor, videokaart en voeding; het moederbord (MB-01), het geheugen (RAM-01) en de kast (KAST-02) liggen vast.

Je programma vraagt voor elk van de drie gekozen onderdelen eerst de prijs in euro en dan het vermogen. Daarna toont het enkel nog het buildrapport. De regels van punt 2 tot en met 6 verdwijnen.

```text
Bit & Byte - configurator
Prijs processor: 199,00
Watt processor: 105
Prijs videokaart: 589,00
Watt videokaart: 200
Prijs voeding: 69,00
Watt voeding: 550
==============================
BIT & BYTE - BUILDRAPPORT
==============================
Onderdelen   : 1284,00 euro
  zonder btw : 1061,16 euro
  btw 21 %   : 222,84 euro
Vermogen     : 380 W
Advies       : 500 W
Voeding      : 550 W (volstaat, marge 50 W)
==============================
```

> [!IMPORTANT]
> Elk bedrag in het rapport komt uit een berekening in cent. Er staat geen enkele `double` in je berekeningen, enkel bij het inlezen.

Test je rapport en controleer:

- met <code style="color:#e64980;font-weight:600">389,00</code> <code style="color:#e64980;font-weight:600">120</code>, <code style="color:#e64980;font-weight:600">499,00</code> <code style="color:#e64980;font-weight:600">263</code> en <code style="color:#e64980;font-weight:600">69,00</code> <code style="color:#e64980;font-weight:600">550</code>: <code style="color:#845ef7">Onderdelen   : 1384,00 euro</code>, <code style="color:#845ef7">zonder btw : 1143,80 euro</code>, <code style="color:#845ef7">btw 21 %   : 240,20 euro</code> en <code style="color:#845ef7">Voeding      : 550 W (te zwak, 50 W tekort)</code>;
- met <code style="color:#e64980;font-weight:600">299,00</code> <code style="color:#e64980;font-weight:600">125</code>, <code style="color:#e64980;font-weight:600">499,00</code> <code style="color:#e64980;font-weight:600">263</code> en <code style="color:#e64980;font-weight:600">139,00</code> <code style="color:#e64980;font-weight:600">850</code>: <code style="color:#845ef7">zonder btw : 1127,27 euro</code> en <code style="color:#845ef7">Advies       : 650 W</code>;
- met een processor van <code style="color:#e64980;font-weight:600">199,05</code>: of het rapport <code style="color:#845ef7">Onderdelen   : 1284,05 euro</code> toont, met de nul;
- met <code style="color:#e64980;font-weight:600">189,95</code> <code style="color:#e64980;font-weight:600">105</code>, <code style="color:#e64980;font-weight:600">4,35</code> <code style="color:#e64980;font-weight:600">10</code> en <code style="color:#e64980;font-weight:600">39,99</code> <code style="color:#e64980;font-weight:600">300</code>: <code style="color:#845ef7">Onderdelen   : 661,29 euro</code>. Met een cast in plaats van `Math.round` mis je hier een cent.

> [!NOTE]
> Je schrijft de omzetting van cent naar euro nu drie keer uit, telkens met een andere variabele. Dat voelt verkeerd, en dat is het ook. In hoofdstuk 03 maak je er één methode van.
