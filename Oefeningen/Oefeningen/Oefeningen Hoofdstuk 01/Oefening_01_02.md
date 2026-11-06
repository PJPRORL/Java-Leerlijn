# 01_02

<b>Hoofdstuk 01</b> &nbsp;·&nbsp; Gehele deling en rest &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#f08c00">●●○</span> &nbsp;·&nbsp; ±75 min

## Leerdoel

Na deze oefening kan je rekenen met de gehele deling `/` en de rest `%`, en weet je wanneer Java een deling afkapt.

Je leert getallen omzetten van het ene datatype naar het andere met typecasting, en een willekeurig getal maken met `Random`.

Daarnaast oefen je op regels die op elkaar inwerken: de korting bepaalt het bedrag, het bedrag bepaalt de verzendkost, de voorraad bepaalt de leverdag.

## Opdracht

Het magazijn van Bit & Byte pakt elke dag de bestellingen van de webwinkel in. Voor elke bestellijn moet een magazijnier weten hoeveel dozen hij nodig heeft, wat de klant betaalt en wanneer het pakket aankomt. Nu rekent hij dat uit met de hand, en dat loopt regelmatig mis.

Jouw taak is om het programma te schrijven dat de pakbon van een bestellijn opstelt.

Het programma moet:

1. een bestelnummer kunnen maken;
2. het bedrag van een bestellijn kunnen uitrekenen, met staffelkorting;
3. kunnen zeggen hoeveel verzenddozen er nodig zijn;
4. de verzendkost, het totaal en de spaarpunten kunnen uitrekenen;
5. de levertermijn en de leverdag kunnen bepalen;
6. alles samen kunnen tonen op één pakbon.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Maak een nieuwe map `Oefening_01_02` met een bestand `src/Main.java`, zoals in 01_01. Het programma toont één regel:

<code style="color:#845ef7">Bit & Byte - magazijn</code>

---

### 2. Het bestelnummer

Elke bestelling krijgt een bestelnummer van de vorm `BB-` + jaar + `-` + vijf cijfers, bijvoorbeeld <code style="color:#845ef7">BB-2026-48213</code>.

- Leg het jaar `2026` vast in een constante.
- Maak de vijf cijfers met `Random`: een getal van 10000 tot en met 99999 (leerboek 1.4).
- Bewaar het hele bestelnummer in één `String`, en toon het:

<code style="color:#845ef7">Bestelnummer: BB-2026-48213</code>

Je programma toont bij elke start een ander nummer.

> [!TIP]
> Waarom van 10000 en niet van 0? Een getal onder 10000 heeft geen vijf cijfers: `BB-2026-482` is geen geldig bestelnummer. Leerboek 1.4 geeft de formule voor een getal tussen een minimum en een maximum.

Test je programma en controleer:

- of het nummer na tien keer starten altijd vijf cijfers heeft.

---

### 3. De bestellijn

De bestellijn gaat over artikel 2 uit de catalogus. Bewaar het in variabelen:

| Gegeven | Waarde |
|---|---|
| naam | Ryzen 5 7600X |
| prijs | 199.00 |
| voorraad | 3 |

Vraag het aantal: <code style="color:#845ef7">Aantal: </code>. Op een bestellijn geldt een staffelkorting:

| Aantal | Korting |
|---|---|
| 1 tot en met 4 | 0 % |
| 5 tot en met 9 | 5 % |
| 10 of meer | 10 % |

Bewaar de korting als een geheel getal, het percentage. Reken het lijnbedrag uit: aantal × prijs, min de korting, afgerond op twee cijfers na de komma. Toon:

| Aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Artikel: Ryzen 5 7600X (199.0 euro)</code><br><code style="color:#845ef7">Korting: 0 %</code><br><code style="color:#845ef7">Lijnbedrag: 796.0 euro</code> |
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Artikel: Ryzen 5 7600X (199.0 euro)</code><br><code style="color:#845ef7">Korting: 5 %</code><br><code style="color:#845ef7">Lijnbedrag: 945.25 euro</code> |
| <code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Artikel: Ryzen 5 7600X (199.0 euro)</code><br><code style="color:#845ef7">Korting: 10 %</code><br><code style="color:#845ef7">Lijnbedrag: 1791.0 euro</code> |

> [!WARNING]
> `(100 - korting) / 100` is altijd 0, hoe groot de korting ook is: twee gehele getallen geven een gehele deling (leerboek 1.3). Kijk naar de volgorde waarin je vermenigvuldigt en deelt, of zorg dat er een `double` in de deling zit.

> [!NOTE]
> Een bestellijn mag 1 tot en met 20 stuks bevatten. Dat kan je programma nog niet controleren; test enkel met aantallen in dat bereik.

---

### 4. De verzenddozen

In één verzenddoos passen maximaal 3 artikelen. Leg dat getal vast in een constante, en toon onder het lijnbedrag:

- hoeveel dozen er nodig zijn;
- hoeveel stuks er in de laatste doos zitten.

| Aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Dozen: 2</code><br><code style="color:#845ef7">In de laatste doos: 2 stuks</code> |
| <code style="color:#e64980;font-weight:600">6</code> | <code style="color:#845ef7">Dozen: 2</code><br><code style="color:#845ef7">In de laatste doos: 3 stuks</code> |
| <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Dozen: 1</code><br><code style="color:#845ef7">In de laatste doos: 1 stuk</code> |

Je hebt hier geen `if` voor nodig. De gehele deling, de rest en de ternary operator volstaan.

> [!TIP]
> `aantal / 3` geeft het aantal vólle dozen. Hoe maak je daar het aantal dozen van, ook als de laatste niet vol is? Probeer wat `(aantal + 2) / 3` geeft voor 4, 5 en 6.

> [!WARNING]
> Bij 6 stuks is de rest 0, maar de laatste doos is niet leeg: ze bevat er 3.

---

### 5. Verzendkost, totaal en spaarpunten

Reken onder de dozen uit:

| Bedrag | Regel |
|---|---|
| Verzendkost | 4.99 euro als het lijnbedrag onder 50 euro ligt; vanaf 50 euro gratis |
| Totaal | lijnbedrag + verzendkost, afgerond op twee cijfers na de komma |
| Spaarpunten | 1 punt per volle 10 euro van het lijnbedrag |

| Aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Totaal: 945.25 euro</code><br><code style="color:#845ef7">Spaarpunten: 94</code> |
| <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Totaal: 199.0 euro</code><br><code style="color:#845ef7">Spaarpunten: 19</code> |

De spaarpunten zijn een `int`. Een `double` past niet zomaar in een `int`: je moet zelf zeggen dat Java het deel na de komma mag weggooien (leerboek 1.3, *Typecasting*).

> [!WARNING]
> *Volle* 10 euro betekent afkappen, niet afronden: 945.25 euro geeft 94 punten, geen 95. `Math.round` is hier dus de verkeerde keuze.

---

### 6. Levertermijn en leverdag

Vraag na het totaal op welke werkdag de klant bestelt: <code style="color:#845ef7">Besteldag (1 = maandag, 5 = vrijdag): </code>.

De levertermijn hangt af van de voorraad:

| Situatie | Levertermijn |
|---|---|
| het aantal ligt op voorraad | 1 werkdag |
| er ligt te weinig op voorraad | 5 werkdagen |

Reken uit op welke werkdag het pakket aankomt, en of dat deze week is of volgende week. Een werkweek telt 5 dagen; zaterdag en zondag tellen niet mee.

| Aantal en besteldag | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">5</code> en <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Levertermijn: 5 werkdagen</code><br><code style="color:#845ef7">Leverdag: werkdag 4 (volgende week)</code> |
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Levertermijn: 1 werkdag</code><br><code style="color:#845ef7">Leverdag: werkdag 1 (volgende week)</code> |
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Levertermijn: 1 werkdag</code><br><code style="color:#845ef7">Leverdag: werkdag 3 (deze week)</code> |

> [!TIP]
> Nummer de werkdagen even van 0 tot en met 4 in plaats van 1 tot en met 5. Tel er de levertermijn bij op. De rest na deling door 5 is dan de dag, en de gehele deling door 5 zegt hoeveel weken verder dat is.

> [!NOTE]
> Merk op dat `1 werkdag` geen `s` krijgt. Dat kies je, net als bij `1 stuk` in punt 4, met een ternary operator.

---

### 7. Alles samen: de pakbon

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

In dit laatste punt maak je van je programma een echte pakbon. Het artikel ligt niet langer vast: de magazijnier typt naam, prijs en voorraad zelf in, net zoals in 01_01. Daarna toont je programma enkel nog de pakbon. De regels van punt 2 tot en met 6 verdwijnen; wat ze uitrekenden, staat op de pakbon.

```text
Bit & Byte - magazijn
Artikel: Ryzen 5 7600X
Prijs: 199,00
Voorraad: 3
Aantal: 5
Besteldag (1 = maandag, 5 = vrijdag): 4
==============================
BIT & BYTE - PAKBON
==============================
Bestelnummer : BB-2026-67362
Artikel      : Ryzen 5 7600X
Aantal       : 5 x 199.0 euro (korting 5 %)
Lijnbedrag   : 945.25 euro
Verzendkost  : 0.0 euro
Totaal       : 945.25 euro
Spaarpunten  : 94
Dozen        : 2 (laatste doos: 2 stuks)
Levertermijn : 5 werkdagen
Leverdag     : werkdag 4 (volgende week)
==============================
```

> [!IMPORTANT]
> De labels en de bedragen moeten kloppen zoals hierboven. Het aantal spaties voor een dubbelpunt telt niet mee. Het bestelnummer verschilt bij elke start.

Test je pakbon en controleer:

- met <code style="color:#e64980;font-weight:600">Kabelbinders</code>, <code style="color:#e64980;font-weight:600">1,99</code>, voorraad <code style="color:#e64980;font-weight:600">40</code>, aantal <code style="color:#e64980;font-weight:600">3</code> en besteldag <code style="color:#e64980;font-weight:600">1</code>: of de pakbon <code style="color:#845ef7">Verzendkost  : 4.99 euro</code>, <code style="color:#845ef7">Totaal       : 10.96 euro</code>, <code style="color:#845ef7">Spaarpunten  : 0</code> en <code style="color:#845ef7">Leverdag     : werkdag 2 (deze week)</code> toont;
- met dezelfde kabelbinders, aantal <code style="color:#e64980;font-weight:600">10</code> en besteldag <code style="color:#e64980;font-weight:600">5</code>: of de pakbon <code style="color:#845ef7">Lijnbedrag   : 17.91 euro</code>, <code style="color:#845ef7">Totaal       : 22.9 euro</code> en <code style="color:#845ef7">Dozen        : 4 (laatste doos: 1 stuk)</code> toont;
- met <code style="color:#e64980;font-weight:600">GeForce RTX 4070</code>, <code style="color:#e64980;font-weight:600">589,00</code>, voorraad <code style="color:#e64980;font-weight:600">0</code>, aantal <code style="color:#e64980;font-weight:600">1</code> en besteldag <code style="color:#e64980;font-weight:600">3</code>: of de levertermijn 5 werkdagen is.

> [!NOTE]
> Kijk welke regels elkaar beïnvloeden: de korting verandert het lijnbedrag, het lijnbedrag beslist over de verzendkost en de spaarpunten, en de voorraad over de leverdag. Bij de kabelbinders zie je ze allemaal tegelijk.
