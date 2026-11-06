# Oplossing 02_02

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 02</b> &nbsp;·&nbsp; Eerst controleren, dan veranderen &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_02_02.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21. Het bestelnummer is willekeurig: jouw nummer is anders.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 01_02 staat in de map `Oefening_02_02`. Met <code style="color:#e64980;font-weight:600">Ryzen 5 7600X</code>, <code style="color:#e64980;font-weight:600">199,00</code>, <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">5</code> en <code style="color:#e64980;font-weight:600">4</code> toont de pakbon onder meer <code style="color:#845ef7">Lijnbedrag   : 945.25 euro</code>, <code style="color:#845ef7">Dozen        : 2 (laatste doos: 2 stuks)</code> en <code style="color:#845ef7">Leverdag     : werkdag 4 (volgende week)</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_01_02.md` voor de code.

---

## 2. De catalogus

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">5</code> <code style="color:#e64980;font-weight:600">4</code> | de catalogus, <code style="color:#845ef7">Bestelnummer: BB-2026-</code> met vijf cijfers, en dezelfde pakbon als in punt 1 |

**Aanpak.** Drie parallelle arrays, met de index als sleutel (leerboek 1.7). Het nummer dat de magazijnier typt min 1 geeft de index. Naam, prijs en voorraad haal je in variabelen met de namen van 01_02, zodat de rest van de pakbon niet verandert.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String[] namen = {"B650 Tomahawk", "Ryzen 5 7600X", "Ryzen 7 7800X3D", "GeForce RTX 4070", "Radeon RX 7800 XT", "NH-D15"};
double[] prijzen = {219.00, 199.00, 389.00, 589.00, 499.00, 109.00};
int[] voorraden = {12, 3, 5, 0, 2, 7};
for (int i = 0; i < namen.length; i++) {
    System.out.println((i + 1) + ". " + namen[i] + " - " + prijzen[i] + " euro - " + voorraden[i] + " op voorraad");
}
// ... het bestelnummer, en dan:
System.out.print("Artikel (1-6): ");
int i = sc.nextInt() - 1;
String naam = namen[i];
double prijs = prijzen[i];
int voorraad = voorraden[i];
```

</details>

---

## 3. Meerdere bestellijnen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">5</code> <code style="color:#e64980;font-weight:600">6</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Ryzen 5 7600X: 5 x 199.0 euro (korting 5 %) = 945.25 euro</code><br><code style="color:#845ef7">NH-D15: 2 x 109.0 euro (korting 0 %) = 218.0 euro</code><br><code style="color:#845ef7">Lijnbedragen: 1163.25 euro</code> |
| vijf artikelen van elk 1 stuk | na de vijfde lijn meteen <code style="color:#845ef7">Maximaal 5 lijnen per bestelling.</code>, zonder nog een artikel te vragen |

**Aanpak.** De arrays hebben een vaste grootte, `MAX_LIJNEN`; een teller `aantalLijnen` zegt hoeveel plaatsen er gevuld zijn. De lus loopt zolang er plaats is. Typt de magazijnier 0, dan stopt `break` de lus meteen (leerboek 1.6). Daarna een tweede lus over de gevulde lijnen: per lijn de korting en het bedrag van 01_02. Die bewaar je in twee extra arrays, want de pakbon heeft ze later opnieuw nodig.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final int MAX_LIJNEN = 5;
// ...
int[] lijnArtikel = new int[MAX_LIJNEN];
int[] lijnAantal = new int[MAX_LIJNEN];
int aantalLijnen = 0;
while (aantalLijnen < MAX_LIJNEN) {
    System.out.print("Artikel (1-6, 0 = klaar): ");
    int id = sc.nextInt();
    if (id == 0) {
        break;
    }
    System.out.print("Aantal: ");
    int aantal = sc.nextInt();
    lijnArtikel[aantalLijnen] = id - 1;
    lijnAantal[aantalLijnen] = aantal;
    aantalLijnen++;
}
if (aantalLijnen == MAX_LIJNEN) {
    System.out.println("Maximaal " + MAX_LIJNEN + " lijnen per bestelling.");
}

double lijnbedragen = 0;
int stuksTotaal = 0;
int[] lijnKorting = new int[MAX_LIJNEN];
double[] lijnBedrag = new double[MAX_LIJNEN];
for (int k = 0; k < aantalLijnen; k++) {
    int aantal = lijnAantal[k];
    lijnKorting[k] = aantal >= 10 ? 10 : aantal >= 5 ? 5 : 0;
    lijnBedrag[k] = Math.round(aantal * prijzen[lijnArtikel[k]] * (100 - lijnKorting[k])) / 100.0;
    lijnbedragen += lijnBedrag[k];
    stuksTotaal += aantal;
}
lijnbedragen = Math.round(lijnbedragen * 100) / 100.0;
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De lus laten lopen tot de gebruiker 0 typt, zonder op `MAX_LIJNEN` te letten. Bij de zesde lijn stopt je programma met een `ArrayIndexOutOfBoundsException`.
> - De tweede lus tot `lijnAantal.length` laten lopen in plaats van tot `aantalLijnen`. De lege plaatsen tellen dan mee als artikel 1 met aantal 0: het bedrag klopt toevallig nog, maar elke lijst toont lijnen die niemand besteld heeft.

---

## 4. Eerst controleren

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code> | <code style="color:#845ef7">Artikel 9 bestaat niet.</code>, en meteen weer <code style="color:#845ef7">Artikel (1-6, 0 = klaar): </code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">0</code> en <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">25</code> | twee keer <code style="color:#845ef7">Het aantal moet tussen 1 en 20 liggen.</code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">5</code>, dan <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Bijgeteld bij lijn 1: nu 7 stuks.</code>, en één lijn met <code style="color:#845ef7">Ryzen 5 7600X: 7 x 199.0 euro (korting 5 %) = 1323.35 euro</code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">5</code>, dan <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">18</code> | <code style="color:#845ef7">Te veel: maximaal 20 stuks per artikel.</code>, en de lijn blijft op 5 stuks |

**Aanpak.** Elke controle die mislukt, toont haar melding en springt met `continue` naar de volgende ronde (leerboek 1.6). Zo blijft de code die een lijn bewaart, onderaan en zonder extra inspringing. Om te weten of een artikel al in de bestelling staat, zoek je het met een lus over de gevulde lijnen. Vind je het, dan tel je het aantal erbij op, of weiger je; in beide gevallen maak je geen nieuwe lijn.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final int MAX_PER_ARTIKEL = 20;
// ... in de while-lus, na het inlezen van het artikel:
if (id < 1 || id > namen.length) {
    System.out.println("Artikel " + id + " bestaat niet.");
    continue;
}
System.out.print("Aantal: ");
int aantal = sc.nextInt();
if (aantal < 1 || aantal > MAX_PER_ARTIKEL) {
    System.out.println("Het aantal moet tussen 1 en " + MAX_PER_ARTIKEL + " liggen.");
    continue;
}
int bestaand = -1;
for (int k = 0; k < aantalLijnen; k++) {
    if (lijnArtikel[k] == id - 1) {
        bestaand = k;
    }
}
if (bestaand >= 0) {
    if (lijnAantal[bestaand] + aantal > MAX_PER_ARTIKEL) {
        System.out.println("Te veel: maximaal " + MAX_PER_ARTIKEL + " stuks per artikel.");
    } else {
        lijnAantal[bestaand] += aantal;
        System.out.println("Bijgeteld bij lijn " + (bestaand + 1) + ": nu " + lijnAantal[bestaand] + " stuks.");
    }
    continue;
}
lijnArtikel[aantalLijnen] = id - 1;
lijnAantal[aantalLijnen] = aantal;
aantalLijnen++;
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `break` in plaats van `continue`. Na de eerste fout stopt dan de hele invoer, en is de bestelling af met de lijnen die er toevallig al waren.
> - Het artikel niet zoeken, en voor elke invoer een nieuwe lijn maken. Twee keer 3 stuks worden dan twee lijnen zonder korting, terwijl 6 stuks recht hebben op 5 %.

> [!TIP]
> **Zo kan het beter**
>
> - `-1` als *niet gevonden* is een afspraak die je in Java vaak tegenkomt: een index is nooit negatief. Je ziet ze terug bij `indexOf` in hoofdstuk 03.

---

## 5. Verzendkost, spaarpunten en dozen

**Nakijkpunten**

| Bestelling | Wat je programma toont |
|---|---|
| 5 × Ryzen 5 7600X, 2 × NH-D15 | <code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Totaal: 1163.25 euro</code><br><code style="color:#845ef7">Spaarpunten: 116</code><br><code style="color:#845ef7">Dozen: 3 (laatste doos: 1 stuk)</code> |
| 1 × NH-D15 | <code style="color:#845ef7">Totaal: 109.0 euro</code><br><code style="color:#845ef7">Spaarpunten: 10</code><br><code style="color:#845ef7">Dozen: 1 (laatste doos: 1 stuk)</code> |

**Aanpak.** De regels van 01_02 veranderen niet; enkel wat je erin steekt. In plaats van één lijnbedrag gebruik je de som van de lijnbedragen, in plaats van één aantal het totaal aantal stuks. Beide rekende je in punt 3 al uit.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int dozen = (stuksTotaal + PER_DOOS - 1) / PER_DOOS;
int rest = stuksTotaal % PER_DOOS;
int inLaatsteDoos = rest == 0 ? PER_DOOS : rest;
String stuks = inLaatsteDoos == 1 ? "stuk" : "stuks";
double verzendkost = lijnbedragen < 50 ? 4.99 : 0.0;
double totaal = Math.round((lijnbedragen + verzendkost) * 100) / 100.0;
int spaarpunten = (int) (lijnbedragen / 10);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De dozen per lijn uitrekenen en optellen. Bij 4 processors en 2 koelers geeft dat 2 + 1 = 3 dozen, terwijl 6 stuks in 2 dozen passen.
> - De spaarpunten per lijn afkappen en optellen. Bij twee lijnen van 19 euro geeft dat 1 + 1 = 2 punten, terwijl 38 euro er 3 waard is.

---

## 6. Levertermijn en leverdag

**Nakijkpunten**

| Bestelling en besteldag | Wat je programma toont |
|---|---|
| 3 × Ryzen 5 7600X, 2 × NH-D15, <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Levertermijn: 1 werkdag</code><br><code style="color:#845ef7">Leverdag: werkdag 1 (volgende week)</code> |
| 5 × Ryzen 5 7600X, 2 × NH-D15, <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Levertermijn: 5 werkdagen</code><br><code style="color:#845ef7">Leverdag: werkdag 4 (volgende week)</code> |

**Aanpak.** Begin met de goede afloop, 1 werkdag, en zoek een lijn die het tegendeel bewijst. Vind je er één, dan is het antwoord 5 werkdagen, en stopt `break` de zoektocht. De voorraad van een lijn vind je via het artikel van die lijn: `voorraden[lijnArtikel[k]]`. De leverdag rekent daarna zoals in 01_02.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int levertermijn = 1;
for (int k = 0; k < aantalLijnen; k++) {
    if (lijnAantal[k] > voorraden[lijnArtikel[k]]) {
        levertermijn = 5;
        break;
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `voorraden[k]` in plaats van `voorraden[lijnArtikel[k]]`. Dan vergelijk je lijn 1 met de voorraad van artikel 1, wat er ook besteld is. Een bestelling met als eerste lijn 5 Radeon RX 7800 XT krijgt dan 1 werkdag, terwijl er maar 2 liggen.
> - Het omgekeerde zoeken: beginnen met 5 en op 1 zetten zodra één lijn op voorraad is. Dan bepaalt de eerste lijn die past de levertermijn, in plaats van de eerste die niet past.

---

## 7. Alles samen: een bestelling afronden

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| 5 × Ryzen 5 7600X, 2 × NH-D15, <code style="color:#e64980;font-weight:600">4</code>, <code style="color:#e64980;font-weight:600">j</code> | <code style="color:#845ef7">Te betalen: 1163.25 euro</code>, dan de pakbon met beide lijnen, <code style="color:#845ef7">Lijnbedragen : 1163.25 euro</code> en <code style="color:#845ef7">Levertermijn : 5 werkdagen</code> |
| 2 × Radeon RX 7800 XT, <code style="color:#e64980;font-weight:600">n</code>; daarna hetzelfde met <code style="color:#e64980;font-weight:600">j</code> | eerst <code style="color:#845ef7">Bestelling geannuleerd: er is niets veranderd.</code>, daarna een pakbon met <code style="color:#845ef7">Levertermijn : 1 werkdag</code> |
| 3 × Ryzen 5 7600X, <code style="color:#e64980;font-weight:600">j</code>; daarna 1 × Ryzen 5 7600X, <code style="color:#e64980;font-weight:600">j</code> | de catalogus voor de tweede bestelling toont <code style="color:#845ef7">2. Ryzen 5 7600X - 199.0 euro - 0 op voorraad</code>; de tweede pakbon <code style="color:#845ef7">Levertermijn : 5 werkdagen</code> |
| <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Lege bestelling: er is niets besteld.</code>, zonder besteldag |

**Aanpak.** Alles wat de bestelling verandert, staat in de tak van de bevestiging: de voorraad aanpassen en de pakbon tonen. Daarvoor heb je enkel gerekend en gelezen. Een annulering hoeft dus niets terug te draaien: er is nog niets veranderd. Rond de hele bestelling, vanaf de catalogus, staat een `do-while` die vraagt of er nog een bestelling komt. Een lege bestelling vang je op direct na de invoer, zodat je programma de besteldag niet vraagt.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
char nogEen;
do {
    // ... de catalogus, het bestelnummer en de invoer van punt 3 en 4
    if (aantalLijnen == 0) {
        System.out.println("Lege bestelling: er is niets besteld.");
    } else {
        // ... de berekeningen van punt 3, 5 en 6
        System.out.println("Te betalen: " + totaal + " euro");
        System.out.print("Bevestigen (j/n): ");
        char bevestig = sc.next().charAt(0);
        if (bevestig == 'j') {
            for (int k = 0; k < aantalLijnen; k++) {
                int a = lijnArtikel[k];
                voorraden[a] = Math.max(0, voorraden[a] - lijnAantal[k]);
            }
            // ... de pakbon, met een lus over de lijnen
        } else {
            System.out.println("Bestelling geannuleerd: er is niets veranderd.");
        }
    }
    System.out.print("Nog een bestelling (j/n): ");
    nogEen = sc.next().charAt(0);
} while (nogEen == 'j');
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De voorraad al aanpassen tijdens de invoer van de lijnen. Een annulering laat dan een lagere voorraad achter, en de volgende bestelling krijgt een verkeerde levertermijn.
> - `bevestig == "j"`. Een `char` vergelijk je met een `char` tussen enkele aanhalingstekens; met dubbele compileert het niet: *bad operand types for binary operator '=='*.
> - De voorraad verlagen zonder ondergrens. Bij 5 bestelde processors en 3 op voorraad toont de catalogus daarna <code style="color:#845ef7">-2 op voorraad</code>.

> [!TIP]
> **Zo kan het beter**
>
> - `Math.max(0, ...)` (naslag 10.2) zegt in één regel wat een `if` in drie regels zegt: nooit onder 0.
> - De pakbon herhaalt de regel van een bestellijn die je in punt 3 al toonde. Dat is één van de dingen die in hoofdstuk 03 een methode worden.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Random;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - magazijn");
        final int JAAR = 2026;
        final int PER_DOOS = 3;
        final int MAX_LIJNEN = 5;
        final int MAX_PER_ARTIKEL = 20;
        Random random = new Random();
        Scanner sc = new Scanner(System.in);

        // de catalogus
        String[] namen = {"B650 Tomahawk", "Ryzen 5 7600X", "Ryzen 7 7800X3D", "GeForce RTX 4070", "Radeon RX 7800 XT", "NH-D15"};
        double[] prijzen = {219.00, 199.00, 389.00, 589.00, 499.00, 109.00};
        int[] voorraden = {12, 3, 5, 0, 2, 7};

        char nogEen;
        do {
            for (int i = 0; i < namen.length; i++) {
                System.out.println((i + 1) + ". " + namen[i] + " - " + prijzen[i] + " euro - " + voorraden[i] + " op voorraad");
            }

            // het bestelnummer
            int volgnummer = random.nextInt(99999 - 10000 + 1) + 10000;
            String bestelnummer = "BB-" + JAAR + "-" + volgnummer;

            // de bestellijnen
            int[] lijnArtikel = new int[MAX_LIJNEN];
            int[] lijnAantal = new int[MAX_LIJNEN];
            int aantalLijnen = 0;
            while (aantalLijnen < MAX_LIJNEN) {
                System.out.print("Artikel (1-6, 0 = klaar): ");
                int id = sc.nextInt();
                if (id == 0) {
                    break;
                }
                if (id < 1 || id > namen.length) {
                    System.out.println("Artikel " + id + " bestaat niet.");
                    continue;
                }
                System.out.print("Aantal: ");
                int aantal = sc.nextInt();
                if (aantal < 1 || aantal > MAX_PER_ARTIKEL) {
                    System.out.println("Het aantal moet tussen 1 en " + MAX_PER_ARTIKEL + " liggen.");
                    continue;
                }
                int bestaand = -1;
                for (int k = 0; k < aantalLijnen; k++) {
                    if (lijnArtikel[k] == id - 1) {
                        bestaand = k;
                    }
                }
                if (bestaand >= 0) {
                    if (lijnAantal[bestaand] + aantal > MAX_PER_ARTIKEL) {
                        System.out.println("Te veel: maximaal " + MAX_PER_ARTIKEL + " stuks per artikel.");
                    } else {
                        lijnAantal[bestaand] += aantal;
                        System.out.println("Bijgeteld bij lijn " + (bestaand + 1) + ": nu " + lijnAantal[bestaand] + " stuks.");
                    }
                    continue;
                }
                lijnArtikel[aantalLijnen] = id - 1;
                lijnAantal[aantalLijnen] = aantal;
                aantalLijnen++;
            }
            if (aantalLijnen == MAX_LIJNEN) {
                System.out.println("Maximaal " + MAX_LIJNEN + " lijnen per bestelling.");
            }
            if (aantalLijnen == 0) {
                System.out.println("Lege bestelling: er is niets besteld.");
            } else {
                // de bedragen per lijn
                double lijnbedragen = 0;
                int stuksTotaal = 0;
                int[] lijnKorting = new int[MAX_LIJNEN];
                double[] lijnBedrag = new double[MAX_LIJNEN];
                for (int k = 0; k < aantalLijnen; k++) {
                    int aantal = lijnAantal[k];
                    lijnKorting[k] = aantal >= 10 ? 10 : aantal >= 5 ? 5 : 0;
                    lijnBedrag[k] = Math.round(aantal * prijzen[lijnArtikel[k]] * (100 - lijnKorting[k])) / 100.0;
                    lijnbedragen += lijnBedrag[k];
                    stuksTotaal += aantal;
                }
                lijnbedragen = Math.round(lijnbedragen * 100) / 100.0;

                // de verzenddozen
                int dozen = (stuksTotaal + PER_DOOS - 1) / PER_DOOS;
                int rest = stuksTotaal % PER_DOOS;
                int inLaatsteDoos = rest == 0 ? PER_DOOS : rest;
                String stuks = inLaatsteDoos == 1 ? "stuk" : "stuks";

                // verzendkost, totaal en spaarpunten
                double verzendkost = lijnbedragen < 50 ? 4.99 : 0.0;
                double totaal = Math.round((lijnbedragen + verzendkost) * 100) / 100.0;
                int spaarpunten = (int) (lijnbedragen / 10);

                // levertermijn en leverdag
                System.out.print("Besteldag (1 = maandag, 5 = vrijdag): ");
                int besteldag = sc.nextInt();
                int levertermijn = 1;
                for (int k = 0; k < aantalLijnen; k++) {
                    if (lijnAantal[k] > voorraden[lijnArtikel[k]]) {
                        levertermijn = 5;
                        break;
                    }
                }
                String werkdagen = levertermijn == 1 ? "werkdag" : "werkdagen";
                int dagenVerder = besteldag - 1 + levertermijn;
                int leverdag = dagenVerder % 5 + 1;
                String week = dagenVerder / 5 == 0 ? "deze week" : "volgende week";

                // bevestigen en de pakbon
                System.out.println("Te betalen: " + totaal + " euro");
                System.out.print("Bevestigen (j/n): ");
                char bevestig = sc.next().charAt(0);
                if (bevestig == 'j') {
                    for (int k = 0; k < aantalLijnen; k++) {
                        int a = lijnArtikel[k];
                        voorraden[a] = Math.max(0, voorraden[a] - lijnAantal[k]);
                    }
                    System.out.println("==============================");
                    System.out.println("BIT & BYTE - PAKBON");
                    System.out.println("==============================");
                    System.out.println("Bestelnummer : " + bestelnummer);
                    for (int k = 0; k < aantalLijnen; k++) {
                        System.out.println(namen[lijnArtikel[k]] + ": " + lijnAantal[k] + " x " + prijzen[lijnArtikel[k]] + " euro (korting " + lijnKorting[k] + " %) = " + lijnBedrag[k] + " euro");
                    }
                    System.out.println("------------------------------");
                    System.out.println("Lijnbedragen : " + lijnbedragen + " euro");
                    System.out.println("Verzendkost  : " + verzendkost + " euro");
                    System.out.println("Totaal       : " + totaal + " euro");
                    System.out.println("Spaarpunten  : " + spaarpunten);
                    System.out.println("Dozen        : " + dozen + " (laatste doos: " + inLaatsteDoos + " " + stuks + ")");
                    System.out.println("Levertermijn : " + levertermijn + " " + werkdagen);
                    System.out.println("Leverdag     : werkdag " + leverdag + " (" + week + ")");
                    System.out.println("==============================");
                } else {
                    System.out.println("Bestelling geannuleerd: er is niets veranderd.");
                }
            }
            System.out.print("Nog een bestelling (j/n): ");
            nogEen = sc.next().charAt(0);
        } while (nogEen == 'j');
    }
}
```

</details>
