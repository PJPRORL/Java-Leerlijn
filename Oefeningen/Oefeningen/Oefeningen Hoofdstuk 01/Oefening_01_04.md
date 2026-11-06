# 01_04

<b>Hoofdstuk 01</b> &nbsp;·&nbsp; Variabelen en rekenen &nbsp;·&nbsp; Dierenasiel &nbsp;·&nbsp; niveau <span style="color:#37b24d">●○○</span> &nbsp;·&nbsp; ±60 min

## Leerdoel

Na deze oefening kan je gegevens bewaren in variabelen van het juiste datatype, en ermee rekenen.

Je leert invoer van de gebruiker lezen met `Scanner`, en met de ternary operator een tekst of een bedrag kiezen zonder `if`.

Daarnaast oefen je op de gehele deling en de rest, om een leeftijd in maanden om te zetten naar jaren en maanden.

## Opdracht

Dierenasiel Pootjes vangt honden, katten en konijnen op tot ze een nieuwe thuis vinden. Bij het onthaal schrijft een vrijwilliger voor elk dier een fiche met de hand. Het asiel wil een programma dat die fiche opmaakt, met de adoptieprijs en de hoeveelheid voer erbij.

Jouw taak is om dat programma te schrijven.

Het programma moet:

1. een dier kunnen tonen met zijn leeftijd en gewicht;
2. de leeftijd kunnen tonen in jaren en maanden;
3. de gegevens van een dier kunnen inlezen;
4. de adoptieprijs kunnen uitrekenen;
5. de levensfase en het voer per dag kunnen tonen;
6. alles samen kunnen tonen op één dierfiche.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Maak een map `Oefening_01_04`, met daarin een map `src`. Zet in `src` een bestand `Main.java` met een klasse `Main` en een methode `main` (leerboek 1.1).

Het programma toont één regel:

<code style="color:#845ef7">Dierenasiel - onthaal</code>

Start het in de terminal van VS Code, vanuit de map `Oefening_01_04`:

```powershell
java src/Main.java
```

---

### 2. Het dier in variabelen

Bewaar de gegevens van dier 1 uit het asiel in variabelen:

| Gegeven | Waarde |
|---|---|
| naam | Rex |
| soort | hond |
| leeftijd in maanden | 30 |
| gewicht in kg | 28.5 |

Kies voor elke variabele het datatype dat past (leerboek 1.2). Toon daarna het dier:

```text
Dierenasiel - onthaal
Rex (hond)
Leeftijd: 30 maanden
Gewicht: 28.5 kg
```

---

### 3. Leeftijd in jaren en maanden

Niemand zegt dat zijn hond 30 maanden oud is. Vervang de regel met de leeftijd door de leeftijd in jaren en maanden:

<code style="color:#845ef7">Leeftijd: 2 jaar en 6 maanden</code>

Bewaar het aantal jaren en het aantal maanden elk in een eigen variabele, zodat je ze later opnieuw kan gebruiken.

> [!TIP]
> Een jaar heeft 12 maanden. Wat geeft `30 / 12`, en wat geeft `30 % 12` (leerboek 1.3)?

Test je programma en controleer:

- met een leeftijd van 7, 12, 96 en 110 maanden. Verander daarvoor even de waarde van je variabele.

---

### 4. Het dier inlezen

Laat de vrijwilliger de gegevens van het dier intypen, met een `Scanner` (leerboek 1.4). Je variabelen blijven dezelfde; enkel hun waarde komt nu van de vrijwilliger.

De soort typt de vrijwilliger als een nummer: 1 voor een hond, 2 voor een kat, 3 voor een konijn. Je programma zet dat nummer om naar het woord, met de ternary operator.

```text
Dierenasiel - onthaal
Naam: Luna
Soort (1 = hond, 2 = kat, 3 = konijn): 2
Leeftijd in maanden: 7
Gewicht in kg: 3,1
Luna (kat)
Leeftijd: 0 jaar en 7 maanden
Gewicht: 3.1 kg
```

> [!WARNING]
> `Scanner` leest een kommagetal in de taal van je computer. Op een Belgische of Nederlandse Windows typ je `3,1`; `3.1` geeft dan een `InputMismatchException`. Je programma toont het getal daarna wel met een punt: dat doet Java altijd bij `+`.

> [!TIP]
> Een ternary operator mag in een andere staan: `a ? x : b ? y : z`. Lees hem van links naar rechts: eerst `a`, en enkel als `a` niet waar is, `b`.

---

### 5. De adoptieprijs

Een senior is een dier van 96 maanden (8 jaar) of ouder. Bewaar dat in een variabele van het type `boolean`.

De adoptieprijs hangt af van de soort, en een senior betaalt de helft:

| Soort | Adoptieprijs |
|---|---|
| hond | 150 euro |
| kat | 90 euro |
| konijn | 40 euro |

Toon onder het gewicht:

| Invoer: soort en leeftijd | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">7</code> | <code style="color:#845ef7">Senior: false</code><br><code style="color:#845ef7">Adoptieprijs: 90 euro</code> |
| <code style="color:#e64980;font-weight:600">1</code> en <code style="color:#e64980;font-weight:600">110</code> | <code style="color:#845ef7">Senior: true</code><br><code style="color:#845ef7">Adoptieprijs: 75 euro</code> |
| <code style="color:#e64980;font-weight:600">3</code> en <code style="color:#e64980;font-weight:600">14</code> | <code style="color:#845ef7">Senior: false</code><br><code style="color:#845ef7">Adoptieprijs: 40 euro</code> |

> [!NOTE]
> De grens zelf telt mee: een kat van precies 96 maanden is een senior. Kijk goed of je `>` of `>=` nodig hebt.

---

### 6. Levensfase en voer

Toon onder de adoptieprijs nog twee regels: de levensfase van het dier, en hoeveel voer het per dag krijgt.

| Leeftijd | Levensfase |
|---|---|
| jonger dan 12 maanden | <code style="color:#845ef7">jong</code> |
| 12 tot 96 maanden | <code style="color:#845ef7">volwassen</code> |
| 96 maanden of ouder | <code style="color:#845ef7">senior</code> |

Een dier krijgt 20 gram voer per kg lichaamsgewicht, naar boven afgerond op 10 gram.

| Invoer: leeftijd en gewicht | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">7</code> en <code style="color:#e64980;font-weight:600">3,1</code> | <code style="color:#845ef7">Levensfase: jong</code><br><code style="color:#845ef7">Voer per dag: 70 gram</code> |
| <code style="color:#e64980;font-weight:600">30</code> en <code style="color:#e64980;font-weight:600">28,5</code> | <code style="color:#845ef7">Levensfase: volwassen</code><br><code style="color:#845ef7">Voer per dag: 570 gram</code> |
| <code style="color:#e64980;font-weight:600">110</code> en <code style="color:#e64980;font-weight:600">31,0</code> | <code style="color:#845ef7">Levensfase: senior</code><br><code style="color:#845ef7">Voer per dag: 620 gram</code> |
| <code style="color:#e64980;font-weight:600">12</code> en <code style="color:#e64980;font-weight:600">6,0</code> | <code style="color:#845ef7">Levensfase: volwassen</code><br><code style="color:#845ef7">Voer per dag: 120 gram</code> |

> [!TIP]
> Naar boven afronden doet `Math.ceil` (naslag 10.2). Wil je op 10 gram afronden, deel dan eerst door 10, rond af, en vermenigvuldig opnieuw met 10. `Math.ceil` geeft een `double`: een cast maakt er een `int` van.

---

### 7. Alles samen: de dierfiche

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

De losse regels van de vorige punten zijn handig om te testen, maar het asiel wil één fiche per dier. In dit laatste punt zet je alles samen.

Je programma vraagt na het gewicht ook de naam van de verzorger, en toont daarna enkel nog de dierfiche. De regels van punt 2 tot en met 6 verdwijnen; wat ze uitrekenden, staat op de fiche.

```text
Dierenasiel - onthaal
Naam: Luna
Soort (1 = hond, 2 = kat, 3 = konijn): 2
Leeftijd in maanden: 7
Gewicht in kg: 3,1
Verzorger: Ines Maes
==============================
DIERENASIEL - DIERFICHE
==============================
Naam         : Luna
Soort        : kat
Leeftijd     : 0 jaar en 7 maanden (jong)
Gewicht      : 3.1 kg
Voer per dag : 70 gram
Adoptieprijs : 90 euro
Verzorger    : Ines Maes
==============================
```

> [!WARNING]
> Lees je de naam van de verzorger met `nextLine()` direct na `nextDouble()`, dan krijg je een lege naam en wacht je programma niet op de vrijwilliger. Lees in leerboek 1.4 *Het Enter-probleem*.

> [!IMPORTANT]
> De labels en de waarden moeten kloppen zoals hierboven. Het aantal spaties voor een dubbelpunt telt niet mee.

Test je dierfiche en controleer:

- met <code style="color:#e64980;font-weight:600">Max</code>, <code style="color:#e64980;font-weight:600">1</code>, <code style="color:#e64980;font-weight:600">110</code>, <code style="color:#e64980;font-weight:600">31,0</code>: of de fiche <code style="color:#845ef7">Leeftijd     : 9 jaar en 2 maanden (senior)</code> en <code style="color:#845ef7">Adoptieprijs : 75 euro</code> toont;
- met <code style="color:#e64980;font-weight:600">Pluis</code>, <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">14</code>, <code style="color:#e64980;font-weight:600">2,2</code>: of de fiche <code style="color:#845ef7">Voer per dag : 50 gram</code> en <code style="color:#845ef7">Adoptieprijs : 40 euro</code> toont;
- of een verzorger met een naam met spaties, zoals <code style="color:#e64980;font-weight:600">Wout Smets</code>, volledig op de fiche staat.

> [!NOTE]
> Kijk wat je programma nog niet kan: een soortnummer 7 wordt stilletjes een konijn, en het asiel kent maar één dier tegelijk. In hoofdstuk 02 los je allebei op, met `if` en met arrays.
