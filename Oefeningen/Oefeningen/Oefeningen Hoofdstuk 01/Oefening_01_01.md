# 01_01

<b>Hoofdstuk 01</b> &nbsp;·&nbsp; Variabelen en rekenen &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#37b24d">●○○</span> &nbsp;·&nbsp; ±60 min

## Leerdoel

Na deze oefening kan je gegevens bewaren in variabelen van het juiste datatype, en ermee rekenen.

Je leert invoer van de gebruiker lezen met `Scanner`, en met de ternary operator een tekst of een bedrag kiezen zonder `if`.

Daarnaast oefen je op rekenen met kommagetallen: afronden op twee cijfers na de komma, en het verschil tussen een prijs met en zonder btw.

## Opdracht

Bit & Byte verkoopt pc-onderdelen in een webwinkel en aan de toonbank. Aan de toonbank rekent de verkoper nog af met een rekenmachine en een blocnote. Hij wil een klein kassaprogramma dat de prijs van een verkoop uitrekent en een kassabon toont.

Jouw taak is om dat kassaprogramma te schrijven.

Het programma moet:

1. een artikel kunnen tonen met zijn prijs en voorraad;
2. de prijs zonder btw en de btw kunnen uitrekenen;
3. de gegevens van een artikel kunnen inlezen;
4. het bedrag van een verkoop kunnen uitrekenen, met verzendkost;
5. de voorraad na de verkoop kunnen tonen;
6. alles samen kunnen tonen op één kassabon.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Maak een map `Oefening_01_01`, met daarin een map `src`. Zet in `src` een bestand `Main.java` met een klasse `Main` en een methode `main` (leerboek 1.1).

Het programma toont één regel:

<code style="color:#845ef7">Bit & Byte - kassa</code>

Start het in de terminal van VS Code, vanuit de map `Oefening_01_01`:

```powershell
java src/Main.java
```

> [!TIP]
> Sinds Java 11 compileert `java` een programma van één bestand en voert het meteen uit (leerboek 1.1). Je hebt `javac` in dit hoofdstuk niet nodig.

---

### 2. Het artikel in variabelen

Bewaar de gegevens van artikel 1 uit de catalogus in variabelen:

| Gegeven | Waarde |
|---|---|
| naam | B650 Tomahawk |
| merk | MSI |
| prijs | 219.00 |
| voorraad | 12 |

Kies voor elke variabele het datatype dat past (leerboek 1.2). Toon daarna het artikel:

```text
Bit & Byte - kassa
B650 Tomahawk van MSI
Prijs met btw: 219.0 euro
Op voorraad: 12 stuks
```

> [!NOTE]
> Java toont `219.00` als `219.0`: een `double` heeft geen vast aantal cijfers na de komma. Vanaf hoofdstuk 03 maak je bedragen netjes op met `printf`.

---

### 3. De prijs zonder btw

De prijzen van Bit & Byte zijn inclusief 21 % btw. Reken voor het artikel uit:

- de prijs zonder btw: de prijs gedeeld door 1,21;
- de btw: het verschil tussen de prijs en de prijs zonder btw.

Rond beide af op twee cijfers na de komma, en toon ze onder het artikel:

<code style="color:#845ef7">Prijs zonder btw: 180.99 euro</code><br>
<code style="color:#845ef7">Btw: 38.01 euro</code>

Leg het btw-tarief vast in een constante, zodat je het maar op één plaats moet aanpassen (leerboek 1.2).

> [!TIP]
> `Math.round(x * 100) / 100.0` rondt `x` af op twee cijfers na de komma. Waarom `100.0` en niet `100`? Kijk in naslag 10.2 wat `Math.round` teruggeeft.

> [!WARNING]
> Trek je de afgeronde prijs zonder btw af van de prijs, en rond je het resultaat niet opnieuw af, dan toont je programma `38.00999999999999`. Een `double` kan de meeste kommagetallen niet exact bewaren. In 01_03 zie je hoe het wel exact kan.

Test je programma en controleer:

- of de prijs zonder btw en de btw samen opnieuw 219.00 geven;
- wat je ziet als je het afronden weglaat.

---

### 4. Het artikel inlezen

Een kassa die maar één artikel kent, heeft weinig zin. Laat de verkoper de gegevens van het artikel intypen, met een `Scanner` (leerboek 1.4). Je variabelen blijven dezelfde; enkel hun waarde komt nu van de verkoper.

In de voorbeeldrun staat wat de verkoper intypt op dezelfde regel als de vraag, zoals in je terminal:

```text
Bit & Byte - kassa
Artikel: Ryzen 5 7600X
Merk: AMD
Prijs: 199,00
Voorraad: 3
Ryzen 5 7600X van AMD
Prijs met btw: 199.0 euro
Op voorraad: 3 stuks
Prijs zonder btw: 164.46 euro
Btw: 34.54 euro
```

> [!WARNING]
> `Scanner` leest een kommagetal in de taal van je computer. Op een Belgische of Nederlandse Windows typ je `199,00`; `199.00` geeft dan een `InputMismatchException`. Op een Engelstalige computer is het omgekeerd. Je programma toont het getal daarna wel met een punt: dat doet Java altijd bij `+`.

Test je programma en controleer:

- of <code style="color:#e64980;font-weight:600">199,00</code> werkt, en wat er gebeurt met <code style="color:#e64980;font-weight:600">199.00</code>;
- of een naam met spaties, zoals <code style="color:#e64980;font-weight:600">Ryzen 5 7600X</code>, volledig in je variabele komt.

---

### 5. Een verkoop

Vraag na het artikel ook het aantal dat de klant koopt: <code style="color:#845ef7">Aantal: </code>. Reken daarna uit:

| Bedrag | Regel |
|---|---|
| Subtotaal | prijs × aantal, afgerond op twee cijfers na de komma |
| Verzendkost | 4.99 euro als het subtotaal onder 50 euro ligt; vanaf 50 euro gratis |
| Te betalen | subtotaal + verzendkost, afgerond op twee cijfers na de komma |

Kies de verzendkost met de ternary operator (leerboek 1.3), en toon de drie bedragen onder de btw:

| Invoer: prijs en aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">199,00</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Subtotaal: 398.0 euro</code><br><code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Te betalen: 398.0 euro</code> |
| <code style="color:#e64980;font-weight:600">1,99</code> en <code style="color:#e64980;font-weight:600">3</code> | <code style="color:#845ef7">Subtotaal: 5.97 euro</code><br><code style="color:#845ef7">Verzendkost: 4.99 euro</code><br><code style="color:#845ef7">Te betalen: 10.96 euro</code> |
| <code style="color:#e64980;font-weight:600">25,00</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Subtotaal: 50.0 euro</code><br><code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Te betalen: 50.0 euro</code> |

> [!NOTE]
> De grens zelf telt mee: bij precies 50 euro is de verzending gratis. Kijk goed of je `<` of `<=` nodig hebt.

---

### 6. De voorraad na de verkoop

Toon onder de bedragen nog twee regels:

- of er genoeg op voorraad is, als `true` of `false`. Bewaar dat eerst in een variabele van het type `boolean`;
- de voorraad na de verkoop, met haar status.

De status hangt af van de voorraad na de verkoop:

| Voorraad na de verkoop | Status |
|---|---|
| 0 | <code style="color:#845ef7">uitverkocht</code> |
| 1 of 2 | <code style="color:#845ef7">bijna uitverkocht</code> |
| 3 of meer | <code style="color:#845ef7">op voorraad</code> |

| Invoer: voorraad en aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">3</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Genoeg op voorraad: true</code><br><code style="color:#845ef7">Voorraad na verkoop: 1 (bijna uitverkocht)</code> |
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Genoeg op voorraad: true</code><br><code style="color:#845ef7">Voorraad na verkoop: 0 (uitverkocht)</code> |
| <code style="color:#e64980;font-weight:600">7</code> en <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Genoeg op voorraad: true</code><br><code style="color:#845ef7">Voorraad na verkoop: 6 (op voorraad)</code> |

> [!TIP]
> Een ternary operator mag in een andere staan: `a ? x : b ? y : z`. Lees hem van links naar rechts: eerst `a`, en enkel als `a` niet waar is, `b`.

Test je programma en controleer:

- wat er gebeurt als de klant er meer wil dan er op voorraad zijn. Je programma kan dat nog niet weigeren: daarvoor heb je `if` nodig, uit hoofdstuk 02.

---

### 7. Alles samen: de kassabon

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

De losse regels van de vorige punten zijn handig om te testen, maar een klant wil één bon. In dit laatste punt zet je alles samen.

Je programma vraagt na het aantal ook de naam van de klant, en toont daarna enkel nog de kassabon. De regels van punt 2 tot en met 6 verdwijnen; wat ze uitrekenden, staat op de bon.

```text
Bit & Byte - kassa
Artikel: Ryzen 5 7600X
Merk: AMD
Prijs: 199,00
Voorraad: 3
Aantal: 2
Klant: Lotte Peeters
==============================
BIT & BYTE - KASSABON
==============================
Klant       : Lotte Peeters
Artikel     : Ryzen 5 7600X (AMD)
Aantal      : 2 x 199.0 euro
Subtotaal   : 398.0 euro
Verzendkost : 0.0 euro
Te betalen  : 398.0 euro
Waarvan btw : 69.07 euro
------------------------------
Voorraad na verkoop: 1 (bijna uitverkocht)
==============================
```

`Waarvan btw` is de btw die in het bedrag *Te betalen* zit, op dezelfde manier berekend als in punt 3.

> [!WARNING]
> Lees je de naam van de klant met `nextLine()` direct na `nextInt()`, dan krijg je een lege naam en wacht je programma niet op de klant. Lees in leerboek 1.4 *Het Enter-probleem*.

> [!IMPORTANT]
> De labels en de bedragen moeten kloppen zoals hierboven. Het aantal spaties voor een dubbelpunt telt niet mee.

Test je kassabon en controleer:

- met <code style="color:#e64980;font-weight:600">Kabelbinders</code>, <code style="color:#e64980;font-weight:600">Velcro</code>, <code style="color:#e64980;font-weight:600">1,99</code>, voorraad <code style="color:#e64980;font-weight:600">40</code> en aantal <code style="color:#e64980;font-weight:600">3</code>: of de bon <code style="color:#845ef7">Te betalen  : 10.96 euro</code> en <code style="color:#845ef7">Waarvan btw : 1.9 euro</code> toont;
- met <code style="color:#e64980;font-weight:600">Radeon RX 7800 XT</code>, <code style="color:#e64980;font-weight:600">Sapphire</code>, <code style="color:#e64980;font-weight:600">499,00</code>, voorraad <code style="color:#e64980;font-weight:600">2</code> en aantal <code style="color:#e64980;font-weight:600">2</code>: of de bon <code style="color:#845ef7">Waarvan btw : 173.21 euro</code> en <code style="color:#845ef7">Voorraad na verkoop: 0 (uitverkocht)</code> toont;
- of een klantnaam met spaties, zoals <code style="color:#e64980;font-weight:600">Bureau Claes bv</code>, volledig op de bon staat.

> [!NOTE]
> Kijk wat je programma nog niet kan: het verkoopt ook als er te weinig voorraad is, en het kent maar één artikel tegelijk. In hoofdstuk 02 los je allebei op, met `if` en met arrays.
