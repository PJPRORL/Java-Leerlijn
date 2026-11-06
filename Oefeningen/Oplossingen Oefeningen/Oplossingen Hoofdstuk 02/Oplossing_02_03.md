# Oplossing 02_03

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 02</b> &nbsp;·&nbsp; Tabellen en geneste lussen &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_02_03.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21. De zes keuzes <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">2</code> heten hieronder *build A*: Ryzen 5 7600X, B650 Tomahawk, 32 GB DDR5-6000, GeForce RTX 4070, Voeding 550 W en Midi Air.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 01_03 staat in de map `Oefening_02_03`. Met <code style="color:#e64980;font-weight:600">199,00</code> <code style="color:#e64980;font-weight:600">105</code> <code style="color:#e64980;font-weight:600">589,00</code> <code style="color:#e64980;font-weight:600">200</code> <code style="color:#e64980;font-weight:600">69,00</code> <code style="color:#e64980;font-weight:600">550</code> toont het rapport <code style="color:#845ef7">Onderdelen   : 1284,00 euro</code>, <code style="color:#845ef7">zonder btw : 1061,16 euro</code> en <code style="color:#845ef7">Advies       : 500 W</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_01_03.md` voor de code.

---

## 2. De onderdelen in arrays

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | zes lijsten, van <code style="color:#845ef7">1. Ryzen 5 7600X - 199,00 euro - 105 W</code> tot <code style="color:#845ef7">2. Midi Air - 99,00 euro - videokaart tot 360 mm</code> |

- Elke prijs is een `int` in cent; de lijsten tonen ze in euro met een komma.

**Aanpak.** Per soort een groep parallelle arrays, met dezelfde index voor hetzelfde onderdeel. Elke lijst is een eigen lus, met de omzetting van cent naar euro uit 01_03 in de regel die ze toont.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String[] cpuNamen = {"Ryzen 5 7600X", "Ryzen 7 7800X3D", "Core i5-14600K"};
int[] cpuPrijzen = {19900, 38900, 29900};
int[] cpuWatt = {105, 120, 125};
// ... idem voor moederborden, geheugen, videokaarten, voedingen en behuizingen

System.out.println("Processors:");
for (int i = 0; i < cpuNamen.length; i++) {
    System.out.println((i + 1) + ". " + cpuNamen[i] + " - " + cpuPrijzen[i] / 100 + "," + (cpuPrijzen[i] % 100 < 10 ? "0" : "") + cpuPrijzen[i] % 100 + " euro - " + cpuWatt[i] + " W");
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De prijzen als `double` bewaren, omdat de lijst ze in euro toont. Dan ben je de exacte berekening van 01_03 kwijt. Bewaar in cent, toon in euro.

---

## 3. Een build kiezen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| build A | het rapport met bovenaan <code style="color:#845ef7">Processor    : Ryzen 5 7600X</code> tot <code style="color:#845ef7">Behuizing    : Midi Air</code>, en dezelfde bedragen als in punt 1 |
| <code style="color:#e64980;font-weight:600">4</code> <code style="color:#e64980;font-weight:600">0</code> <code style="color:#e64980;font-weight:600">1</code> … met <code style="color:#e64980;font-weight:600">3</code> voor het geheugen | de vraag <code style="color:#845ef7">Processor (1-3): </code> drie keer, de vraag <code style="color:#845ef7">Geheugen (1-2): </code> twee keer |

**Aanpak.** Elke vraag is een `do-while`: stel ze, en stel ze opnieuw zolang het antwoord buiten de lijst valt (leerboek 1.6). Je trekt meteen 1 af, zodat je verder enkel met indexen werkt. De variabelen declareer je voor de lussen, want na de lus heb je ze nog nodig. Daarna haal je prijzen en vermogens uit de arrays, in variabelen met de namen van 01_03: zo blijft de berekening van het rapport ongewijzigd.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int c, m, r, g, p, k;
do {
    System.out.print("Processor (1-" + cpuNamen.length + "): ");
    c = sc.nextInt() - 1;
} while (c < 0 || c >= cpuNamen.length);
// ... vijf keer hetzelfde, voor m, r, g, p en k
int processor = cpuPrijzen[c];
int wattProcessor = cpuWatt[c];
int moederbord = mbPrijzen[m];
int geheugen = ramPrijzen[r];
int videokaart = gpuPrijzen[g];
int wattVideokaart = gpuWatt[g];
int voeding = psuPrijzen[p];
int wattVoeding = psuWatt[p];
int behuizing = kastPrijzen[k];
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Een `if` in plaats van een lus. Dan krijgt de klant één tweede kans; typt hij opnieuw verkeerd, dan stopt je programma met een `ArrayIndexOutOfBoundsException`.
> - `c > cpuNamen.length` in plaats van `>=`. Na het aftrekken van 1 is index 3 niet meer geldig, maar je lus laat hem door.

> [!TIP]
> **Zo kan het beter**
>
> - Zes keer bijna dezelfde `do-while`, enkel met een andere tekst en een andere array. Dat is precies wat een methode `kies(String vraag, int aantal)` in hoofdstuk 03 oplost.

---

## 4. De compatibiliteitstabel

**Nakijkpunten**

| Keuzes | Wat het rapport toont |
|---|---|
| build A | vier keer <code style="color:#845ef7">ok</code>, <code style="color:#845ef7">5. Voeding      : 550 W (volstaat, marge 50 W)</code> en <code style="color:#845ef7">Compatibel      : ja</code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">5. Voeding      : 550 W (te zwak, 50 W tekort)</code><br><code style="color:#845ef7">Compatibel      : nee (1 probleem)</code> |
| <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">1. Socket       : past niet</code><br><code style="color:#845ef7">3. Formfactor   : past niet</code><br><code style="color:#845ef7">4. Lengte       : te lang (320 mm, kast tot 300 mm)</code><br><code style="color:#845ef7">5. Voeding      : 550 W (te zwak, 100 W tekort)</code><br><code style="color:#845ef7">Compatibel      : nee (4 problemen)</code> |
| <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">5. Voeding      : 550 W (volstaat, marge 0 W)</code><br><code style="color:#845ef7">Compatibel      : ja</code> |

**Aanpak.** Een tweedimensionale array met beginwaarden schrijf je als een array van rijen: `{{true, false}, {true, false}, {false, true}}` (leerboek 1.7). De eerste index is de rij, hier de processor; de tweede de kolom, het moederbord. Elke regel krijgt een `String` met beginwaarde `"ok"`, die je overschrijft als de regel faalt, en een teller voor de problemen. De lengte vergelijk je met gewone getallen; de voeding met het advies dat je al uitrekende.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
boolean[][] socketPast = {{true, false}, {true, false}, {false, true}};
boolean[][] geheugenPast = {{true, false}, {false, true}};
boolean[][] kastPast = {{false, true}, {true, true}};
// ...
int problemen = 0;
String socket = "ok";
if (!socketPast[c][m]) {
    socket = "past niet";
    problemen++;
}
String geheugentype = "ok";
if (!geheugenPast[r][m]) {
    geheugentype = "past niet";
    problemen++;
}
String formfactor = "ok";
if (!kastPast[m][k]) {
    formfactor = "past niet";
    problemen++;
}
String lengte = "ok";
if (gpuLengte[g] > kastMaxLengte[k]) {
    lengte = "te lang (" + gpuLengte[g] + " mm, kast tot " + kastMaxLengte[k] + " mm)";
    problemen++;
}
if (wattVoeding < advies) {
    problemen++;
}
// ... in het rapport:
System.out.println("Compatibel      : " + (problemen == 0 ? "ja" : "nee (" + problemen + " " + (problemen == 1 ? "probleem" : "problemen") + ")"));
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `socketPast[m][c]`: de indexen omgedraaid. Bij de Core i5-14600K op de B650 Tomahawk vraag je dan naar `socketPast[0][2]`, en die rij heeft maar 2 plaatsen: *ArrayIndexOutOfBoundsException*.
> - Stoppen bij het eerste probleem met een `else if`-ladder. Het rapport toont dan maar één fout, terwijl de klant ze alle vier moet oplossen.

---

## 5. Alle combinaties

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| build A | na het rapport <code style="color:#845ef7">Geldige builds: 18 van 144</code> |

**Aanpak.** Zes geneste `for`-lussen, elk met een eigen teller. Bovenaan de binnenste lus tel je de combinatie. Dan de regels: faalt er een, dan gaat `continue` meteen naar de volgende combinatie (leerboek 1.6). Enkel wat alle regels doorstaat, bereikt `geldig++`. Het advies reken je per combinatie opnieuw uit, met dezelfde formule als voor het rapport.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int geldig = 0;
int combinaties = 0;
for (int ci = 0; ci < cpuNamen.length; ci++) {
    for (int mi = 0; mi < mbNamen.length; mi++) {
        for (int ri = 0; ri < ramNamen.length; ri++) {
            for (int gi = 0; gi < gpuNamen.length; gi++) {
                for (int pi = 0; pi < psuNamen.length; pi++) {
                    for (int ki = 0; ki < kastNamen.length; ki++) {
                        combinaties++;
                        if (!socketPast[ci][mi] || !geheugenPast[ri][mi] || !kastPast[mi][ki]) {
                            continue;
                        }
                        if (gpuLengte[gi] > kastMaxLengte[ki]) {
                            continue;
                        }
                        int v = cpuWatt[ci] + gpuWatt[gi] + REST;
                        int a = ((v * 13 + 9) / 10 + 49) / 50 * 50;
                        if (psuWatt[pi] < a) {
                            continue;
                        }
                        geldig++;
                    }
                }
            }
        }
    }
}
System.out.println("Geldige builds: " + geldig + " van " + combinaties);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `break` in plaats van `continue`. `break` stopt de hele binnenste lus: de andere behuizingen komen dan niet meer aan bod. Je programma toont <code style="color:#845ef7">Geldige builds: 6 van 75</code>.
> - `combinaties++` pas na de controles zetten. Dan tel je enkel de geldige, en toont je programma <code style="color:#845ef7">Geldige builds: 18 van 18</code>.
> - De tellers van een buitenste lus hergebruiken: `c` uit punt 3 in plaats van een nieuwe `ci`. Dan overschrijf je de keuze van de klant.

> [!TIP]
> **Zo kan het beter**
>
> - Een controle die enkel van de buitenste lussen afhangt, kan je hoger zetten. De socket hangt enkel van `ci` en `mi` af: met een `continue` in de tweede lus sla je in één keer 24 combinaties over. Bij 144 combinaties maakt dat niets uit; bij duizenden wel.

---

## 6. De goedkoopste geldige build

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| build A | <code style="color:#845ef7">Goedkoopste geldige build: 1170,00 euro</code><br><code style="color:#845ef7">Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, Radeon RX 7800 XT, Voeding 750 W, Midi Air</code> |

**Aanpak.** Je bewaart enkel de beste tot nu toe: haar prijs, en haar zes indexen in een kleine array. Elke geldige combinatie vergelijk je daarmee. Met `Integer.MAX_VALUE` als beginwaarde is de eerste geldige build altijd goedkoper (naslag 10.2).

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int goedkoopstePrijs = Integer.MAX_VALUE;
int[] goedkoopste = new int[6];
// ... in de binnenste lus, na geldig++:
int prijs = cpuPrijzen[ci] + mbPrijzen[mi] + ramPrijzen[ri] + gpuPrijzen[gi] + psuPrijzen[pi] + kastPrijzen[ki];
if (prijs < goedkoopstePrijs) {
    goedkoopstePrijs = prijs;
    goedkoopste[0] = ci;
    goedkoopste[1] = mi;
    goedkoopste[2] = ri;
    goedkoopste[3] = gi;
    goedkoopste[4] = pi;
    goedkoopste[5] = ki;
}
// ... na de lussen:
System.out.println("Goedkoopste geldige build: " + goedkoopstePrijs / 100 + "," + (goedkoopstePrijs % 100 < 10 ? "0" : "") + goedkoopstePrijs % 100 + " euro");
System.out.println("  " + cpuNamen[goedkoopste[0]] + ", " + mbNamen[goedkoopste[1]] + ", " + ramNamen[goedkoopste[2]] + ", " + gpuNamen[goedkoopste[3]] + ", " + psuNamen[goedkoopste[4]] + ", " + kastNamen[goedkoopste[5]]);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `int goedkoopstePrijs = 0;`. Geen build is goedkoper dan 0: je programma toont <code style="color:#845ef7">Goedkoopste geldige build: 0,00 euro</code> met de onderdelen op index 0.
> - De prijs vergelijken voor de controles. Dan vind je de goedkoopste combinatie, maar die is niet geldig.

---

## 7. Alles samen: alle geldige builds onder een budget

**Nakijkpunten**

| Budget | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">1250</code> | zes regels, van <code style="color:#845ef7">1234,00 euro: Ryzen 5 7600X, B650 Tomahawk, 32 GB DDR5-6000, Radeon RX 7800 XT, Voeding 750 W, Midi Air</code> tot <code style="color:#845ef7">1200,00 euro: Core i5-14600K, B760M DS3H DDR4, 16 GB DDR4-3200, Radeon RX 7800 XT, Voeding 850 W, Midi Air</code>, en <code style="color:#845ef7">6 geldige build(s) binnen 1250,00 euro.</code> |
| <code style="color:#e64980;font-weight:600">1300</code> | <code style="color:#845ef7">11 geldige build(s) binnen 1300,00 euro.</code> |
| <code style="color:#e64980;font-weight:600">1170</code> | één regel, <code style="color:#845ef7">1170,00 euro: Core i5-14600K, ...</code>, en <code style="color:#845ef7">1 geldige build(s) binnen 1170,00 euro.</code> |
| <code style="color:#e64980;font-weight:600">1100</code> | <code style="color:#845ef7">Geen geldige build binnen 1100,00 euro.</code> |

- Na de lijst volgen, zoals in punt 5 en 6, <code style="color:#845ef7">Geldige builds: 18 van 144</code> en de goedkoopste build.

**Aanpak.** Je vraagt het budget voor de lussen, en zet het met `Math.round` om naar cent. In de binnenste lus, na de controles en na de prijs van punt 6, komt nog één `if`: kost de build niet meer dan het budget, dan toon je haar en tel je haar. Zo doen dezelfde lussen drie dingen tegelijk. Of er niets binnen het budget viel, weet je pas na de lussen.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Budget in euro: ");
int budget = (int) Math.round(sc.nextDouble() * 100);
int binnenBudget = 0;
// ... de zes lussen van punt 5, en in de binnenste, na de goedkoopste:
if (prijs <= budget) {
    binnenBudget++;
    System.out.println(prijs / 100 + "," + (prijs % 100 < 10 ? "0" : "") + prijs % 100 + " euro: " + cpuNamen[ci] + ", " + mbNamen[mi] + ", " + ramNamen[ri] + ", " + gpuNamen[gi] + ", " + psuNamen[pi] + ", " + kastNamen[ki]);
}
// ... na de lussen:
if (binnenBudget == 0) {
    System.out.println("Geen geldige build binnen " + budget / 100 + "," + (budget % 100 < 10 ? "0" : "") + budget % 100 + " euro.");
} else {
    System.out.println(binnenBudget + " geldige build(s) binnen " + budget / 100 + "," + (budget % 100 < 10 ? "0" : "") + budget % 100 + " euro.");
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `prijs < budget`. Met een budget van 1170 euro vindt je programma dan niets, terwijl de goedkoopste build precies 1170 euro kost.
> - Het budget als `double` vergelijken met een prijs in cent. Een budget van 1250 is dan 1250 cent, en past er niets in.

> [!TIP]
> **Zo kan het beter**
>
> - De lijst staat in de volgorde van je lussen, niet van goedkoop naar duur. Sorteren kan pas als je de builds bewaart; met de lijsten uit hoofdstuk 07 wordt dat één regel.
> - De omzetting van cent naar euro staat dertien keer in je code. In hoofdstuk 03 wordt het één methode `euro(int cent)`.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - configurator");
        final int REST = 75;
        Scanner sc = new Scanner(System.in);

        // de onderdelen, prijzen in cent
        String[] cpuNamen = {"Ryzen 5 7600X", "Ryzen 7 7800X3D", "Core i5-14600K"};
        int[] cpuPrijzen = {19900, 38900, 29900};
        int[] cpuWatt = {105, 120, 125};
        String[] mbNamen = {"B650 Tomahawk", "B760M DS3H DDR4"};
        int[] mbPrijzen = {21900, 11900};
        String[] ramNamen = {"32 GB DDR5-6000", "16 GB DDR4-3200"};
        int[] ramPrijzen = {10900, 4500};
        String[] gpuNamen = {"GeForce RTX 4070", "Radeon RX 7800 XT"};
        int[] gpuPrijzen = {58900, 49900};
        int[] gpuWatt = {200, 263};
        int[] gpuLengte = {267, 320};
        String[] psuNamen = {"Voeding 550 W", "Voeding 750 W", "Voeding 850 W"};
        int[] psuPrijzen = {6900, 10900, 13900};
        int[] psuWatt = {550, 750, 850};
        String[] kastNamen = {"Compact M", "Midi Air"};
        int[] kastPrijzen = {7900, 9900};
        int[] kastMaxLengte = {300, 360};

        // de compatibiliteitstabellen
        boolean[][] socketPast = {{true, false}, {true, false}, {false, true}};
        boolean[][] geheugenPast = {{true, false}, {false, true}};
        boolean[][] kastPast = {{false, true}, {true, true}};

        // de lijsten
        System.out.println("Processors:");
        for (int i = 0; i < cpuNamen.length; i++) {
            System.out.println((i + 1) + ". " + cpuNamen[i] + " - " + cpuPrijzen[i] / 100 + "," + (cpuPrijzen[i] % 100 < 10 ? "0" : "") + cpuPrijzen[i] % 100 + " euro - " + cpuWatt[i] + " W");
        }
        System.out.println("Moederborden:");
        for (int i = 0; i < mbNamen.length; i++) {
            System.out.println((i + 1) + ". " + mbNamen[i] + " - " + mbPrijzen[i] / 100 + "," + (mbPrijzen[i] % 100 < 10 ? "0" : "") + mbPrijzen[i] % 100 + " euro");
        }
        System.out.println("Geheugen:");
        for (int i = 0; i < ramNamen.length; i++) {
            System.out.println((i + 1) + ". " + ramNamen[i] + " - " + ramPrijzen[i] / 100 + "," + (ramPrijzen[i] % 100 < 10 ? "0" : "") + ramPrijzen[i] % 100 + " euro");
        }
        System.out.println("Videokaarten:");
        for (int i = 0; i < gpuNamen.length; i++) {
            System.out.println((i + 1) + ". " + gpuNamen[i] + " - " + gpuPrijzen[i] / 100 + "," + (gpuPrijzen[i] % 100 < 10 ? "0" : "") + gpuPrijzen[i] % 100 + " euro - " + gpuWatt[i] + " W - " + gpuLengte[i] + " mm");
        }
        System.out.println("Voedingen:");
        for (int i = 0; i < psuNamen.length; i++) {
            System.out.println((i + 1) + ". " + psuNamen[i] + " - " + psuPrijzen[i] / 100 + "," + (psuPrijzen[i] % 100 < 10 ? "0" : "") + psuPrijzen[i] % 100 + " euro");
        }
        System.out.println("Behuizingen:");
        for (int i = 0; i < kastNamen.length; i++) {
            System.out.println((i + 1) + ". " + kastNamen[i] + " - " + kastPrijzen[i] / 100 + "," + (kastPrijzen[i] % 100 < 10 ? "0" : "") + kastPrijzen[i] % 100 + " euro - videokaart tot " + kastMaxLengte[i] + " mm");
        }

        // een build kiezen
        int c, m, r, g, p, k;
        do {
            System.out.print("Processor (1-" + cpuNamen.length + "): ");
            c = sc.nextInt() - 1;
        } while (c < 0 || c >= cpuNamen.length);
        do {
            System.out.print("Moederbord (1-" + mbNamen.length + "): ");
            m = sc.nextInt() - 1;
        } while (m < 0 || m >= mbNamen.length);
        do {
            System.out.print("Geheugen (1-" + ramNamen.length + "): ");
            r = sc.nextInt() - 1;
        } while (r < 0 || r >= ramNamen.length);
        do {
            System.out.print("Videokaart (1-" + gpuNamen.length + "): ");
            g = sc.nextInt() - 1;
        } while (g < 0 || g >= gpuNamen.length);
        do {
            System.out.print("Voeding (1-" + psuNamen.length + "): ");
            p = sc.nextInt() - 1;
        } while (p < 0 || p >= psuNamen.length);
        do {
            System.out.print("Behuizing (1-" + kastNamen.length + "): ");
            k = sc.nextInt() - 1;
        } while (k < 0 || k >= kastNamen.length);
        int processor = cpuPrijzen[c];
        int wattProcessor = cpuWatt[c];
        int moederbord = mbPrijzen[m];
        int geheugen = ramPrijzen[r];
        int videokaart = gpuPrijzen[g];
        int wattVideokaart = gpuWatt[g];
        int voeding = psuPrijzen[p];
        int wattVoeding = psuWatt[p];
        int behuizing = kastPrijzen[k];

        // prijs, btw en voedingsadvies
        int onderdelen = processor + moederbord + geheugen + videokaart + voeding + behuizing;
        int zonderBtw = (onderdelen * 100 + 60) / 121;
        int btw = onderdelen - zonderBtw;
        int vermogen = wattProcessor + wattVideokaart + REST;
        int metMarge = (vermogen * 13 + 9) / 10;
        int advies = (metMarge + 49) / 50 * 50;
        String oordeel = wattVoeding >= advies
                ? "volstaat, marge " + (wattVoeding - advies) + " W"
                : "te zwak, " + (advies - wattVoeding) + " W tekort";

        // de compatibiliteitsregels
        int problemen = 0;
        String socket = "ok";
        if (!socketPast[c][m]) {
            socket = "past niet";
            problemen++;
        }
        String geheugentype = "ok";
        if (!geheugenPast[r][m]) {
            geheugentype = "past niet";
            problemen++;
        }
        String formfactor = "ok";
        if (!kastPast[m][k]) {
            formfactor = "past niet";
            problemen++;
        }
        String lengte = "ok";
        if (gpuLengte[g] > kastMaxLengte[k]) {
            lengte = "te lang (" + gpuLengte[g] + " mm, kast tot " + kastMaxLengte[k] + " mm)";
            problemen++;
        }
        if (wattVoeding < advies) {
            problemen++;
        }

        // het buildrapport
        System.out.println("==============================");
        System.out.println("BIT & BYTE - BUILDRAPPORT");
        System.out.println("==============================");
        System.out.println("Processor    : " + cpuNamen[c]);
        System.out.println("Moederbord   : " + mbNamen[m]);
        System.out.println("Geheugen     : " + ramNamen[r]);
        System.out.println("Videokaart   : " + gpuNamen[g]);
        System.out.println("Voeding      : " + psuNamen[p]);
        System.out.println("Behuizing    : " + kastNamen[k]);
        System.out.println("------------------------------");
        System.out.println("Onderdelen   : " + onderdelen / 100 + "," + (onderdelen % 100 < 10 ? "0" : "") + onderdelen % 100 + " euro");
        System.out.println("  zonder btw : " + zonderBtw / 100 + "," + (zonderBtw % 100 < 10 ? "0" : "") + zonderBtw % 100 + " euro");
        System.out.println("  btw 21 %   : " + btw / 100 + "," + (btw % 100 < 10 ? "0" : "") + btw % 100 + " euro");
        System.out.println("Vermogen     : " + vermogen + " W");
        System.out.println("Advies       : " + advies + " W");
        System.out.println("------------------------------");
        System.out.println("1. Socket       : " + socket);
        System.out.println("2. Geheugentype : " + geheugentype);
        System.out.println("3. Formfactor   : " + formfactor);
        System.out.println("4. Lengte       : " + lengte);
        System.out.println("5. Voeding      : " + wattVoeding + " W (" + oordeel + ")");
        System.out.println("Compatibel      : " + (problemen == 0 ? "ja" : "nee (" + problemen + " " + (problemen == 1 ? "probleem" : "problemen") + ")"));
        System.out.println("==============================");

        // alle combinaties
        int geldig = 0;
        int combinaties = 0;
        int goedkoopstePrijs = Integer.MAX_VALUE;
        int[] goedkoopste = new int[6];
        System.out.print("Budget in euro: ");
        int budget = (int) Math.round(sc.nextDouble() * 100);
        int binnenBudget = 0;
        for (int ci = 0; ci < cpuNamen.length; ci++) {
            for (int mi = 0; mi < mbNamen.length; mi++) {
                for (int ri = 0; ri < ramNamen.length; ri++) {
                    for (int gi = 0; gi < gpuNamen.length; gi++) {
                        for (int pi = 0; pi < psuNamen.length; pi++) {
                            for (int ki = 0; ki < kastNamen.length; ki++) {
                                combinaties++;
                                if (!socketPast[ci][mi] || !geheugenPast[ri][mi] || !kastPast[mi][ki]) {
                                    continue;
                                }
                                if (gpuLengte[gi] > kastMaxLengte[ki]) {
                                    continue;
                                }
                                int v = cpuWatt[ci] + gpuWatt[gi] + REST;
                                int a = ((v * 13 + 9) / 10 + 49) / 50 * 50;
                                if (psuWatt[pi] < a) {
                                    continue;
                                }
                                geldig++;
                                int prijs = cpuPrijzen[ci] + mbPrijzen[mi] + ramPrijzen[ri] + gpuPrijzen[gi] + psuPrijzen[pi] + kastPrijzen[ki];
                                if (prijs < goedkoopstePrijs) {
                                    goedkoopstePrijs = prijs;
                                    goedkoopste[0] = ci;
                                    goedkoopste[1] = mi;
                                    goedkoopste[2] = ri;
                                    goedkoopste[3] = gi;
                                    goedkoopste[4] = pi;
                                    goedkoopste[5] = ki;
                                }
                                if (prijs <= budget) {
                                    binnenBudget++;
                                    System.out.println(prijs / 100 + "," + (prijs % 100 < 10 ? "0" : "") + prijs % 100 + " euro: " + cpuNamen[ci] + ", " + mbNamen[mi] + ", " + ramNamen[ri] + ", " + gpuNamen[gi] + ", " + psuNamen[pi] + ", " + kastNamen[ki]);
                                }
                            }
                        }
                    }
                }
            }
        }
        if (binnenBudget == 0) {
            System.out.println("Geen geldige build binnen " + budget / 100 + "," + (budget % 100 < 10 ? "0" : "") + budget % 100 + " euro.");
        } else {
            System.out.println(binnenBudget + " geldige build(s) binnen " + budget / 100 + "," + (budget % 100 < 10 ? "0" : "") + budget % 100 + " euro.");
        }
        System.out.println("Geldige builds: " + geldig + " van " + combinaties);
        System.out.println("Goedkoopste geldige build: " + goedkoopstePrijs / 100 + "," + (goedkoopstePrijs % 100 < 10 ? "0" : "") + goedkoopstePrijs % 100 + " euro");
        System.out.println("  " + cpuNamen[goedkoopste[0]] + ", " + mbNamen[goedkoopste[1]] + ", " + ramNamen[goedkoopste[2]] + ", " + gpuNamen[goedkoopste[3]] + ", " + psuNamen[goedkoopste[4]] + ", " + kastNamen[goedkoopste[5]]);
    }
}
```

</details>
