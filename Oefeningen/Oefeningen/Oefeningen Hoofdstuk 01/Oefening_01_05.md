# 01_05

<b>Hoofdstuk 01</b> &nbsp;·&nbsp; Gehele deling en rest &nbsp;·&nbsp; Fietsverhuur &nbsp;·&nbsp; niveau <span style="color:#f08c00">●●○</span> &nbsp;·&nbsp; ±75 min

## Leerdoel

Na deze oefening kan je rekenen met de gehele deling `/` en de rest `%`, en weet je wanneer Java een deling afkapt.

Je leert getallen omzetten van het ene datatype naar het andere met typecasting, en een willekeurig getal maken met `Random`.

Daarnaast oefen je op regels die op elkaar inwerken: de duur bepaalt de begonnen uren, de uren en het dagmaximum bepalen de prijs, het uur van terugkomst bepaalt de boete.

## Opdracht

Fietsverhuur Trapper verhuurt stadsfietsen, e-bikes en bakfietsen per uur. Aan de balie rekent de medewerker de huurprijs nog uit op papier. Daarbij gaat het vaak mis met de begonnen uren en het dagmaximum.

Jouw taak is om het programma te schrijven dat het huurcontract opstelt.

Het programma moet:

1. een contractnummer kunnen maken;
2. de huurduur kunnen uitrekenen;
3. het aantal begonnen uren kunnen bepalen;
4. de huurprijs kunnen uitrekenen, met het dagmaximum;
5. een boete kunnen aanrekenen als de fiets te laat terug is;
6. alles samen kunnen tonen op één huurcontract.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Maak een nieuwe map `Oefening_01_05` met een bestand `src/Main.java`, zoals in 01_04. Het programma toont één regel:

<code style="color:#845ef7">Fietsverhuur - balie</code>

---

### 2. Het contractnummer

Elk contract krijgt een nummer van de vorm `FV-` + jaar + `-` + vier cijfers, bijvoorbeeld <code style="color:#845ef7">FV-2026-4821</code>.

- Leg het jaar `2026` vast in een constante.
- Maak de vier cijfers met `Random`: een getal van 1000 tot en met 9999 (leerboek 1.4).
- Bewaar het hele nummer in één `String`, en toon het:

<code style="color:#845ef7">Contract: FV-2026-4821</code>

Je programma toont bij elke start een ander nummer.

> [!TIP]
> Waarom van 1000 en niet van 0? Een getal onder 1000 heeft geen vier cijfers. Leerboek 1.4 geeft de formule voor een getal tussen een minimum en een maximum.

---

### 3. De huurduur

Het contract gaat over een e-bike. Bewaar de gegevens van dat type in variabelen:

| Gegeven | Waarde |
|---|---|
| type | e-bike |
| uurtarief | 6 euro |
| dagmaximum | 30 euro |
| waarborg | 150 euro |

Vraag het uur en de minuut van vertrek en van terugkomst, en reken de huurduur uit in minuten. Toon het type en de duur in uren en minuten:

```text
Fietsverhuur - balie
Contract: FV-2026-6987
Vertrek uur: 9
Vertrek minuut: 45
Terug uur: 13
Terug minuut: 10
Fiets: e-bike
Duur: 3 u 25 min
```

> [!TIP]
> Zet beide tijdstippen eerst om naar minuten na middernacht. Dan is de duur een gewone aftrekking.

> [!NOTE]
> De fiets komt dezelfde dag terug. Een verhuring over middernacht kan je programma nog niet aan.

---

### 4. Begonnen uren

Fietsverhuur Trapper rekent per begonnen uur: 61 minuten zijn 2 begonnen uren. Toon onder de duur:

| Vertrek en terug | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code>:<code style="color:#e64980;font-weight:600">45</code> tot <code style="color:#e64980;font-weight:600">13</code>:<code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Duur: 3 u 25 min</code><br><code style="color:#845ef7">Begonnen uren: 4</code> |
| <code style="color:#e64980;font-weight:600">10</code>:<code style="color:#e64980;font-weight:600">0</code> tot <code style="color:#e64980;font-weight:600">11</code>:<code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Duur: 1 u 0 min</code><br><code style="color:#845ef7">Begonnen uren: 1</code> |
| <code style="color:#e64980;font-weight:600">10</code>:<code style="color:#e64980;font-weight:600">0</code> tot <code style="color:#e64980;font-weight:600">11</code>:<code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Duur: 1 u 1 min</code><br><code style="color:#845ef7">Begonnen uren: 2</code> |

Je hebt hier geen `if` voor nodig. De gehele deling volstaat.

> [!WARNING]
> `duur / 60 + 1` lijkt te werken, tot je precies 60 minuten test: dat zijn 2 uren in plaats van 1.

---

### 5. De huurprijs

De huurprijs is het aantal begonnen uren × het uurtarief, maar nooit meer dan het dagmaximum. Toon daarnaast wat de klant gemiddeld per minuut betaalt, afgerond op drie cijfers na de komma:

| Vertrek en terug | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code>:<code style="color:#e64980;font-weight:600">45</code> tot <code style="color:#e64980;font-weight:600">13</code>:<code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Huurprijs: 24 euro</code><br><code style="color:#845ef7">Per minuut: 0.117 euro</code> |
| <code style="color:#e64980;font-weight:600">8</code>:<code style="color:#e64980;font-weight:600">30</code> tot <code style="color:#e64980;font-weight:600">17</code>:<code style="color:#e64980;font-weight:600">15</code> | <code style="color:#845ef7">Huurprijs: 30 euro</code><br><code style="color:#845ef7">Per minuut: 0.057 euro</code> |
| <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">0</code> tot <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">20</code> | <code style="color:#845ef7">Huurprijs: 6 euro</code><br><code style="color:#845ef7">Per minuut: 0.3 euro</code> |

> [!TIP]
> Het kleinste van twee getallen kies je met een ternary operator, of met `Math.min` (naslag 10.2).

> [!WARNING]
> De huurprijs en de duur zijn allebei een `int`. `huurprijs / duur` geeft dus 0. Zet één van beide eerst om naar `double` (leerboek 1.3, *Typecasting*). Maar let op waar je de cast zet: `(double) (huurprijs / duur)` is nog altijd 0.

---

### 6. Te laat terug

De verhuur sluit om 20:00 uur. Is de fiets later terug, dan betaalt de klant 10 euro boete. Leg het sluitingsuur en de boete vast in constanten, en bewaar of de fiets te laat is in een `boolean`.

Toon onder de prijs:

| Vertrek en terug | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code>:<code style="color:#e64980;font-weight:600">45</code> tot <code style="color:#e64980;font-weight:600">13</code>:<code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Te laat: false</code><br><code style="color:#845ef7">Boete: 0 euro</code><br><code style="color:#845ef7">Te betalen bij terugkomst: 24 euro</code> |
| <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">5</code> tot <code style="color:#e64980;font-weight:600">20</code>:<code style="color:#e64980;font-weight:600">30</code> | <code style="color:#845ef7">Te laat: true</code><br><code style="color:#845ef7">Boete: 10 euro</code><br><code style="color:#845ef7">Te betalen bij terugkomst: 40 euro</code> |
| <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">5</code> tot <code style="color:#e64980;font-weight:600">20</code>:<code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Te laat: false</code><br><code style="color:#845ef7">Boete: 0 euro</code><br><code style="color:#845ef7">Te betalen bij terugkomst: 30 euro</code> |

> [!NOTE]
> Om precies 20:00 uur is de fiets op tijd. Kijk goed of je `>` of `>=` nodig hebt.

---

### 7. Alles samen: het huurcontract

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

In dit laatste punt maak je van je programma een echt huurcontract. Het type fiets ligt niet langer vast: de medewerker typt een nummer, 1 voor een stadsfiets, 2 voor een e-bike, 3 voor een bakfiets. Je programma kiest daarmee het type, het uurtarief, het dagmaximum en de waarborg:

| Type | Per begonnen uur | Maximum per dag | Waarborg |
|---|---|---|---|
| stadsfiets | 3 euro | 15 euro | 50 euro |
| e-bike | 6 euro | 30 euro | 150 euro |
| bakfiets | 8 euro | 40 euro | 100 euro |

Daarna toont je programma enkel nog het contract. De regels van punt 2 tot en met 6 verdwijnen; wat ze uitrekenden, staat op het contract.

```text
Fietsverhuur - balie
Type (1 = stadsfiets, 2 = e-bike, 3 = bakfiets): 2
Vertrek uur: 9
Vertrek minuut: 45
Terug uur: 13
Terug minuut: 10
==============================
FIETSVERHUUR - HUURCONTRACT
==============================
Contract      : FV-2026-3650
Fiets         : e-bike (6 euro per uur, max 30 euro)
Vertrek       : 9:45
Terug         : 13:10
Duur          : 3 u 25 min (4 begonnen uren)
Huurprijs     : 24 euro
Boete         : 0 euro
------------------------------
Bij vertrek   : 150 euro (waarborg)
Bij terugkomst: 24 euro (waarborg terug)
==============================
```

> [!WARNING]
> Vertrekt de klant om 8:05, dan moet het contract <code style="color:#845ef7">8:05</code> tonen, niet <code style="color:#845ef7">8:5</code>. Een minuut onder 10 krijgt een extra nul, en die kies je met de ternary operator.

> [!IMPORTANT]
> De labels en de bedragen moeten kloppen zoals hierboven. Het aantal spaties voor een dubbelpunt telt niet mee. Het contractnummer verschilt bij elke start.

Test je contract en controleer:

- met type <code style="color:#e64980;font-weight:600">1</code>, van <code style="color:#e64980;font-weight:600">8</code>:<code style="color:#e64980;font-weight:600">5</code> tot <code style="color:#e64980;font-weight:600">19</code>:<code style="color:#e64980;font-weight:600">40</code>: <code style="color:#845ef7">Vertrek       : 8:05</code>, <code style="color:#845ef7">Duur          : 11 u 35 min (12 begonnen uren)</code> en <code style="color:#845ef7">Huurprijs     : 15 euro</code>;
- met type <code style="color:#e64980;font-weight:600">3</code>, van <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">5</code> tot <code style="color:#e64980;font-weight:600">20</code>:<code style="color:#e64980;font-weight:600">30</code>: <code style="color:#845ef7">Huurprijs     : 40 euro</code>, <code style="color:#845ef7">Boete         : 10 euro</code> en <code style="color:#845ef7">Bij terugkomst: 50 euro (waarborg terug)</code>.

> [!NOTE]
> Kijk welke regels elkaar beïnvloeden: de duur bepaalt de begonnen uren, de uren en het dagmaximum samen de prijs, en het uur van terugkomst de boete. Bij de bakfiets zie je ze allemaal tegelijk.
