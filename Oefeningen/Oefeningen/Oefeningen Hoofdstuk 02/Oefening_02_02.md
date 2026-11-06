# 02_02

<b>Hoofdstuk 02</b> &nbsp;·&nbsp; Eerst controleren, dan veranderen &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#f08c00">●●○</span> &nbsp;·&nbsp; ±90 min

## Leerdoel

Na deze oefening kan je met een lus een onbekend aantal gegevens inlezen, en ze bewaren in arrays van een vaste grootte.

Je leert met `break` en `continue` een lus vroeger stoppen of één ronde overslaan, en met `do-while` een programma laten herhalen zolang de gebruiker dat wil.

Daarnaast oefen je op een belangrijke gewoonte: eerst alles controleren, en pas daarna iets veranderen. Een bestelling die geannuleerd wordt, laat geen enkel spoor na.

## Opdracht

Het magazijn van Bit & Byte gebruikt het programma uit 01_02 voor de pakbon. Maar een klant bestelt zelden één artikel: een bestelling bestaat uit meerdere lijnen. Nu maakt de magazijnier voor elke lijn een aparte pakbon, en telt hij de dozen en de verzendkost met de hand op.

Jouw taak is om één pakbon te maken voor een bestelling met meerdere lijnen.

Het programma moet, net als in 01_02:

1. een bestelnummer maken, een staffelkorting per lijn toepassen, en de dozen, de verzendkost, de spaarpunten, de levertermijn en de leverdag bepalen;

en daarnaast:

2. de artikelen uit een catalogus halen;
3. meerdere bestellijnen kunnen inlezen;
4. een verkeerde lijn weigeren, zonder de rest van de bestelling te verliezen;
5. de verzendkost, de spaarpunten en de dozen over de hele bestelling bepalen;
6. de levertermijn bepalen uit de voorraad van alle lijnen;
7. een bestelling laten bevestigen of annuleren, en meerdere bestellingen na elkaar verwerken.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 01_02 naar een nieuwe map `Oefening_02_02`. Deze regels uit 01_02 blijven gelden, en de pakbon komt op het einde van deze oefening terug:

| Regel | Waarde |
|---|---|
| Bestelnummer | `BB-2026-` en vijf cijfers |
| Staffelkorting | per lijn: 5 % vanaf 5 stuks, 10 % vanaf 10 stuks |
| Verzenddoos | maximaal 3 artikelen per doos |
| Verzendkost | 4.99 euro onder 50 euro, anders gratis |
| Spaarpunten | 1 per volle 10 euro, zonder verzendkost |
| Levertermijn | 1 werkdag als alles op voorraad ligt, anders 5 werkdagen |

---

### 2. De catalogus

Bewaar de zes artikelen van de catalogus in drie arrays: namen, prijzen en voorraden. Het Id is de plaats in de arrays plus 1.

| Id | Naam | Prijs | Voorraad |
|---|---|---|---|
| 1 | B650 Tomahawk | 219.00 | 12 |
| 2 | Ryzen 5 7600X | 199.00 | 3 |
| 3 | Ryzen 7 7800X3D | 389.00 | 5 |
| 4 | GeForce RTX 4070 | 589.00 | 0 |
| 5 | Radeon RX 7800 XT | 499.00 | 2 |
| 6 | NH-D15 | 109.00 | 7 |

Toon de catalogus bij de start, en toon daarna het bestelnummer. De magazijnier typt niet langer naam, prijs en voorraad, maar enkel het nummer van het artikel. De pakbon blijft zoals in 01_02:

```text
Bit & Byte - magazijn
1. B650 Tomahawk - 219.0 euro - 12 op voorraad
2. Ryzen 5 7600X - 199.0 euro - 3 op voorraad
3. Ryzen 7 7800X3D - 389.0 euro - 5 op voorraad
4. GeForce RTX 4070 - 589.0 euro - 0 op voorraad
5. Radeon RX 7800 XT - 499.0 euro - 2 op voorraad
6. NH-D15 - 109.0 euro - 7 op voorraad
Bestelnummer: BB-2026-70268
Artikel (1-6): 2
Aantal: 5
Besteldag (1 = maandag, 5 = vrijdag): 4
==============================
BIT & BYTE - PAKBON
==============================
Bestelnummer : BB-2026-70268
Artikel      : Ryzen 5 7600X
Aantal       : 5 x 199.0 euro (korting 5 %)
...
```

---

### 3. Meerdere bestellijnen

Een bestelling heeft hoogstens 5 lijnen. Leg dat getal vast in een constante, en maak twee arrays van die grootte: een voor het artikel van elke lijn, een voor het aantal. Houd in een variabele bij hoeveel lijnen er al gevuld zijn.

Lees lijnen in tot de magazijnier <code style="color:#e64980;font-weight:600">0</code> typt, of tot de bestelling vol is:

```text
Artikel (1-6, 0 = klaar): 2
Aantal: 5
Artikel (1-6, 0 = klaar): 6
Aantal: 2
Artikel (1-6, 0 = klaar): 0
--- Bestelling ---
Ryzen 5 7600X: 5 x 199.0 euro (korting 5 %) = 945.25 euro
NH-D15: 2 x 109.0 euro (korting 0 %) = 218.0 euro
Lijnbedragen: 1163.25 euro
```

Na 5 lijnen stopt de lus vanzelf, met de melding <code style="color:#845ef7">Maximaal 5 lijnen per bestelling.</code>

Elke lijn krijgt haar eigen staffelkorting, zoals in 01_02. *Lijnbedragen* is de som van de lijnen, afgerond op twee cijfers na de komma.

> [!NOTE]
> De pakbon van 01_02 kent maar één lijn. Laat hem in dit punt weg: in punt 7 komt hij terug, met alle lijnen. Ook de dozen, de verzendkost en de levertermijn komen terug, in punt 5 en 6.

> [!TIP]
> `break` stopt de lus meteen, ook in het midden van een ronde (leerboek 1.6). Zo hoef je de stopwaarde 0 niet in de voorwaarde van de `while` te steken.

---

### 4. Eerst controleren

Een lijn met een fout mag de bestelling niet bederven. Controleer elke lijn voor je ze bewaart, in deze volgorde:

| Situatie | Wat je programma toont | Wat er gebeurt |
|---|---|---|
| het nummer ligt niet tussen 1 en 6 | <code style="color:#845ef7">Artikel 9 bestaat niet.</code> | het aantal wordt niet gevraagd |
| het aantal ligt niet tussen 1 en 20 | <code style="color:#845ef7">Het aantal moet tussen 1 en 20 liggen.</code> | de lijn wordt niet bewaard |
| het artikel staat al in de bestelling | <code style="color:#845ef7">Bijgeteld bij lijn 1: nu 7 stuks.</code> | het aantal komt bij de bestaande lijn |
| … en samen zijn het meer dan 20 | <code style="color:#845ef7">Te veel: maximaal 20 stuks per artikel.</code> | er verandert niets |

Na een fout vraagt je programma meteen het volgende artikel. De lijnen die al bewaard waren, blijven staan.

> [!TIP]
> `continue` slaat de rest van de ronde over en begint meteen aan de volgende (leerboek 1.6). Zo hoef je de code die een lijn bewaart, niet in drie lagen `if` te steken.

> [!WARNING]
> Een artikel dat al in de bestelling staat, mag geen tweede lijn krijgen. Anders krijgt een klant die 2 keer 3 stuks bestelt geen staffelkorting, terwijl 6 stuks er wel recht op hebben.

Test je programma en controleer met deze invoer: <code style="color:#e64980;font-weight:600">9</code>, dan <code style="color:#e64980;font-weight:600">2</code> met <code style="color:#e64980;font-weight:600">0</code>, <code style="color:#e64980;font-weight:600">2</code> met <code style="color:#e64980;font-weight:600">25</code>, <code style="color:#e64980;font-weight:600">2</code> met <code style="color:#e64980;font-weight:600">5</code>, <code style="color:#e64980;font-weight:600">2</code> met <code style="color:#e64980;font-weight:600">18</code>, <code style="color:#e64980;font-weight:600">6</code> met <code style="color:#e64980;font-weight:600">2</code>, en <code style="color:#e64980;font-weight:600">0</code>. De bestelling bevat dan twee lijnen: 5 stuks Ryzen 5 7600X en 2 stuks NH-D15.

---

### 5. Verzendkost, spaarpunten en dozen

Bereken onder de lijnbedragen, voor de hele bestelling:

| Gegeven | Regel |
|---|---|
| Verzendkost | 4.99 euro als de lijnbedragen samen onder 50 euro liggen; anders gratis |
| Totaal | lijnbedragen + verzendkost |
| Spaarpunten | 1 per volle 10 euro van de lijnbedragen |
| Dozen | over alle stuks samen, 3 per doos |

| Bestelling | Wat je programma toont |
|---|---|
| 5 × Ryzen 5 7600X, 2 × NH-D15 | <code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Totaal: 1163.25 euro</code><br><code style="color:#845ef7">Spaarpunten: 116</code><br><code style="color:#845ef7">Dozen: 3 (laatste doos: 1 stuk)</code> |
| 1 × NH-D15 | <code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Totaal: 109.0 euro</code><br><code style="color:#845ef7">Spaarpunten: 10</code><br><code style="color:#845ef7">Dozen: 1 (laatste doos: 1 stuk)</code> |

> [!NOTE]
> De dozen tel je over de hele bestelling, niet per lijn: 5 processors en 2 koelers passen samen in 3 dozen.

---

### 6. Levertermijn en leverdag

Vraag daarna de besteldag, zoals in 01_02. De levertermijn is 1 werkdag als *elke* lijn op voorraad ligt. Is er één lijn waarvoor de voorraad te klein is, dan is het 5 werkdagen.

| Bestelling en besteldag | Wat je programma toont |
|---|---|
| 3 × Ryzen 5 7600X, 2 × NH-D15, vrijdag | <code style="color:#845ef7">Levertermijn: 1 werkdag</code><br><code style="color:#845ef7">Leverdag: werkdag 1 (volgende week)</code> |
| 5 × Ryzen 5 7600X, 2 × NH-D15, donderdag | <code style="color:#845ef7">Levertermijn: 5 werkdagen</code><br><code style="color:#845ef7">Leverdag: werkdag 4 (volgende week)</code> |

> [!TIP]
> Zodra je één lijn vindt die niet op voorraad ligt, weet je het antwoord. Stop de lus dan met `break`.

---

### 7. Alles samen: een bestelling afronden

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

In dit laatste punt wordt je programma een echte bestelstraat. Een bestelling verandert de voorraad pas als ze bevestigd is, en het magazijn verwerkt meerdere bestellingen na elkaar.

Na de besteldag toont je programma het totaal en vraagt het <code style="color:#845ef7">Bevestigen (j/n): </code>.

- Bij <code style="color:#e64980;font-weight:600">j</code> verlaagt de voorraad van elk artikel met het bestelde aantal, nooit onder 0, en volgt de pakbon.
- Bij <code style="color:#e64980;font-weight:600">n</code> toont je programma <code style="color:#845ef7">Bestelling geannuleerd: er is niets veranderd.</code>
- Is de bestelling leeg, dan toont je programma <code style="color:#845ef7">Lege bestelling: er is niets besteld.</code>, zonder besteldag en zonder bevestiging.

Daarna vraagt het <code style="color:#845ef7">Nog een bestelling (j/n): </code>. Bij <code style="color:#e64980;font-weight:600">j</code> begint een nieuwe bestelling, met een nieuw bestelnummer en de catalogus met de voorraad van dat moment.

```text
Artikel (1-6, 0 = klaar): 2
Aantal: 5
Artikel (1-6, 0 = klaar): 6
Aantal: 2
Artikel (1-6, 0 = klaar): 0
Besteldag (1 = maandag, 5 = vrijdag): 4
Te betalen: 1163.25 euro
Bevestigen (j/n): j
==============================
BIT & BYTE - PAKBON
==============================
Bestelnummer : BB-2026-70925
Ryzen 5 7600X: 5 x 199.0 euro (korting 5 %) = 945.25 euro
NH-D15: 2 x 109.0 euro (korting 0 %) = 218.0 euro
------------------------------
Lijnbedragen : 1163.25 euro
Verzendkost  : 0.0 euro
Totaal       : 1163.25 euro
Spaarpunten  : 116
Dozen        : 3 (laatste doos: 1 stuk)
Levertermijn : 5 werkdagen
Leverdag     : werkdag 4 (volgende week)
==============================
Nog een bestelling (j/n): 
```

> [!TIP]
> Een `j` of `n` lees je met `sc.next().charAt(0)`: het eerste teken van wat de gebruiker typt (leerboek 1.5).

> [!IMPORTANT]
> Bij een annulering mag geen enkele array veranderd zijn. Controleer dat door dezelfde bestelling daarna opnieuw te plaatsen: de levertermijn moet dezelfde zijn.

Test je bestelstraat en controleer:

- bestel 2 × Radeon RX 7800 XT en annuleer; bestel daarna hetzelfde en bevestig: <code style="color:#845ef7">Levertermijn : 1 werkdag</code>, want de voorraad van 2 was niet aangeraakt;
- bestel 3 × Ryzen 5 7600X en bevestig; bestel daarna 1 × Ryzen 5 7600X en bevestig: de catalogus toont <code style="color:#845ef7">2. Ryzen 5 7600X - 199.0 euro - 0 op voorraad</code>, en de tweede pakbon <code style="color:#845ef7">Levertermijn : 5 werkdagen</code>;
- typ meteen <code style="color:#e64980;font-weight:600">0</code>: <code style="color:#845ef7">Lege bestelling: er is niets besteld.</code>

> [!NOTE]
> Je `main` is nu meer dan honderd regels lang, met lussen in lussen en een pakbon van vijftien regels. Wie het programma leest, moet alles in één keer begrijpen. In hoofdstuk 03 knip je het op in methodes.
