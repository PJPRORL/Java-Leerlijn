# Oplossing 03_03

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 03</b> &nbsp;·&nbsp; Codes, overloading en recursie &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_03_03.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. De lijsten van onderdelen aan het begin staan niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 02_03 staat in de map `Oefening_03_03`. De build 1, 1, 1, 1, 1, 2 met een budget van <code style="color:#e64980;font-weight:600">1250</code> toont onder meer <code style="color:#845ef7">Compatibel      : ja</code>, <code style="color:#845ef7">6 geldige build(s) binnen 1250,00 euro.</code> en <code style="color:#845ef7">Geldige builds: 18 van 144</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_02_03.md` voor de code.

---

## 2. Bedragen en advies in een methode

**Nakijkpunten**

- Je programma toont bij dezelfde invoer exact hetzelfde als in punt 1: <code style="color:#845ef7">Onderdelen   : 1284,00 euro</code>, <code style="color:#845ef7">  zonder btw : 1061,16 euro</code>, <code style="color:#845ef7">Advies       : 500 W</code>.
- In je code staat `% 100 < 10` nergens meer, en de berekening van het advies één keer.

**Aanpak.** `euro` deelt het bedrag in cent in euro en cent, en `%02d` zet de cent altijd met twee cijfers. `advies` krijgt de watt van de processor en de videokaart: zo werkt ze voor de build van de klant én voor elke combinatie in de lussen.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    static final int REST = 75;

    // ... main

    static String euro(int cent) {
        return String.format("%d,%02d euro", cent / 100, cent % 100);
    }

    static int advies(int wattProcessor, int wattVideokaart) {
        int vermogen = wattProcessor + wattVideokaart + REST;
        int metMarge = (vermogen * 13 + 9) / 10;
        return (metMarge + 49) / 50 * 50;
    }
}

// in main:
System.out.println("Onderdelen   : " + euro(onderdelen));
int advies = advies(wattProcessor, wattVideokaart);
// en in de zes lussen:
if (psuWatt[pi] < advies(cpuWatt[ci], gpuWatt[gi])) {
    continue;
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `%d,%d` in plaats van `%d,%02d`. Bij een rond bedrag krijg je `1284,0 euro`, en bij 5 cent `12,5` in plaats van `12,05`.

---

## 3. Codes en een tabel

**Nakijkpunten**

- Bij de start: de lijsten uit de opgave, met onder meer <code style="color:#845ef7">CPU-03   Core i5-14600K       299,00 euro   125 W</code>, <code style="color:#845ef7">RAM-02   16 GB DDR4-3200       45,00 euro</code> en <code style="color:#845ef7">KAST-01  Compact M             79,00 euro   videokaart tot 300 mm</code>.
- Geen enkele regel eindigt op een spatie.
- De rest van het programma werkt zoals in punt 2.

**Aanpak.** De zes losse arrays verhuizen naar boven in de klasse, met `static`. Daaronder maak je de tweedimensionale arrays: `namen[3]` is dan dezelfde array als `gpuNamen`. Eén lus over de soorten, met daarin een lus over de onderdelen, vervangt de zes lussen. `info` kiest met een `switch` wat er per soort na de prijs komt.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String[] codes = {"CPU", "MB", "RAM", "GPU", "PSU", "KAST"};
static String[] koppen = {"Processors", "Moederborden", "Geheugen", "Videokaarten", "Voedingen", "Behuizingen"};
static String[][] namen = {cpuNamen, mbNamen, ramNamen, gpuNamen, psuNamen, kastNamen};
static int[][] prijzen = {cpuPrijzen, mbPrijzen, ramPrijzen, gpuPrijzen, psuPrijzen, kastPrijzen};

// in main:
for (int s = 0; s < namen.length; s++) {
    System.out.println(koppen[s] + ":");
    for (int i = 0; i < namen[s].length; i++) {
        String info = info(s, i);
        System.out.printf("%-8s %-18s %8.2f euro%s%n", code(s, i), namen[s][i], prijzen[s][i] / 100.0, info.isEmpty() ? "" : "   " + info);
    }
}

// onder main:
static String code(int soort, int i) {
    return String.format("%s-%02d", codes[soort], i + 1);
}

static String info(int soort, int i) {
    return switch (soort) {
        case 0 -> cpuWatt[i] + " W";
        case 3 -> gpuWatt[i] + " W, " + gpuLengte[i] + " mm";
        case 5 -> "videokaart tot " + kastMaxLengte[i] + " mm";
        default -> "";
    };
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `namen` bovenaan zetten, voor de zes losse arrays. De compiler weigert: `illegal forward reference`.
> - `prijzen[s][i] / 100` zonder `.0`. Dat is een gehele deling: je verliest de cent, en `%8.2f` krijgt een `int`, wat stopt met `IllegalFormatConversionException`.

---

## 4. Een build uit één regel codes

**Nakijkpunten**

| Regel | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">CPU-01 MB-01 RAM-01 GPU-01 PSU-01</code> | <code style="color:#845ef7">Geen behuizing gekozen.</code>, en de vraag opnieuw |
| <code style="color:#e64980;font-weight:600">cpu-01 mb-01 ram-01 gpu-01 psu-01 kast-02</code> | het rapport van punt 1 |
| <code style="color:#e64980;font-weight:600">CPU-01 CPU-02 MB-01 RAM-03 HDD-01 GPU-x PSU-01</code> | de zeven meldingen uit de opgave, in die volgorde |
| <code style="color:#e64980;font-weight:600">CPU-03  MB-02 RAM-02 GPU-02 PSU-02 KAST-01</code>, met twee spaties | het rapport, met <code style="color:#845ef7">4. Lengte       : te lang (320 mm, kast tot 300 mm)</code> |

**Aanpak.** `leesBuild` begint met zes keer -1. Elke code gaat door twee methodes: `soortVan` zoekt de tekst voor het streepje in `codes`, `nummerVan` zet de tekst erna om. Elke fout toont een melding en zet `fout` op `true`, maar de lus gaat verder. Na de lus vind je de ontbrekende soorten aan een -1 die bleef staan. Bij een fout geeft de methode `null` terug, en vraagt `main` de regel opnieuw.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int soortVan(String code) {
    int streep = code.indexOf('-');
    if (streep == -1) {
        return -1;
    }
    String voor = code.substring(0, streep).toUpperCase();
    for (int s = 0; s < codes.length; s++) {
        if (codes[s].equals(voor)) {
            return s;
        }
    }
    return -1;
}

static int nummerVan(String code) {
    String na = code.substring(code.indexOf('-') + 1);
    return isGetal(na) ? Integer.parseInt(na) : -1;
}

static int[] leesBuild(String regel) {
    int[] build = {-1, -1, -1, -1, -1, -1};
    boolean fout = false;
    for (String code : regel.trim().split(" ")) {
        if (code.isEmpty()) {
            continue;                          // twee spaties na elkaar
        }
        int s = soortVan(code);
        if (s == -1) {
            System.out.println("Onbekende code: " + code + ".");
            fout = true;
            continue;
        }
        int nr = nummerVan(code);
        if (nr < 1 || nr > namen[s].length) {
            System.out.println("Bestaat niet: " + code + ".");
            fout = true;
        } else if (build[s] != -1) {
            System.out.println("Twee keer een " + soorten[s] + ": " + code(s, build[s]) + " en " + code + ".");
            fout = true;
        } else {
            build[s] = nr - 1;
        }
    }
    for (int s = 0; s < build.length; s++) {
        if (build[s] == -1) {
            System.out.println("Geen " + soorten[s] + " gekozen.");
            fout = true;
        }
    }
    return fout ? null : build;
}

// in main:
int[] build;
do {
    System.out.print("Build (6 codes): ");
    build = leesBuild(sc.nextLine());
} while (build == null);
int c = build[0], m = build[1], r = build[2], g = build[3], p = build[4], k = build[5];
```

`soorten` is een klassevariabele met `"processor"` tot `"behuizing"`; `isGetal` is dezelfde als in `Oplossing_03_02.md`.

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `codes[s] == voor`. `voor` is een nieuwe `String`, uit `substring`; `==` vergelijkt objecten, geen inhoud. Elke code is onbekend, ook <code style="color:#e64980;font-weight:600">CPU-01</code>.
> - De lege stukken niet overslaan. Bij twee spaties toont je programma `Onbekende code: .`

---

## 5. Een build als één regel codes

**Nakijkpunten**

| Waar | Wat je programma toont |
|---|---|
| het rapport | <code style="color:#845ef7">Build        : CPU-01 MB-01 RAM-01 GPU-01 PSU-01 KAST-02</code> |
| binnen het budget van 1250 euro | <code style="color:#845ef7">1234,00 euro: CPU-01 MB-01 RAM-01 GPU-02 PSU-02 KAST-02</code> en vijf andere |
| de goedkoopste | <code style="color:#845ef7">  CPU-03 MB-02 RAM-02 GPU-02 PSU-02 KAST-02</code> |

**Aanpak.** De nieuwe `code` loopt over de soorten en roept voor elke soort de oude `code` op. Java weet welke je bedoelt: met twee `int`s de ene, met een `int[]` de andere. In de zes lussen maak je de build met `new int[] {ci, mi, ri, gi, pi, ki}`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String code(int[] build) {
    StringBuilder sb = new StringBuilder();
    for (int s = 0; s < build.length; s++) {
        if (s > 0) {
            sb.append(" ");
        }
        sb.append(code(s, build[s]));
    }
    return sb.toString();
}

static int prijs(int[] build) {
    int som = 0;
    for (int s = 0; s < build.length; s++) {
        som += prijzen[s][build[s]];
    }
    return som;
}

// in main:
System.out.println("Build        : " + code(build));
// in de binnenste lus:
System.out.println(euro(prijs) + ": " + code(new int[] {ci, mi, ri, gi, pi, ki}));
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Twee methodes met dezelfde parameterlijst, ook als de namen van de parameters verschillen: `method code(int,int) is already defined in class Main`. Voor de compiler telt enkel het aantal, de volgorde en het type.

---

## 6. Alle builds met recursie

**Nakijkpunten**

- Budget <code style="color:#e64980;font-weight:600">1250</code>: dezelfde zes builds als in punt 5, in dezelfde volgorde, en <code style="color:#845ef7">Geldige builds: 18 van 144</code>.
- Budget <code style="color:#e64980;font-weight:600">1100</code>: <code style="color:#845ef7">Geen geldige build binnen 1100,00 euro.</code>, en toch <code style="color:#845ef7">Goedkoopste geldige build: 1170,00 euro</code>.

**Aanpak.** `zoek` vult de build soort per soort. Bij soort 6 zijn alle zes gekozen: dat is het basisgeval, waar je telt, controleert en toont. Daarvoor probeert de lus elk onderdeel van de huidige soort, en laat de rest over aan `zoek(soort + 1, ...)`. De methode telt op wat de oproepen eronder teruggeven. De tellers en de goedkoopste staan bovenaan de klasse, want elke oproep moet ze kunnen bijwerken.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int combinaties = 0;
static int geldig = 0;
static int goedkoopstePrijs = Integer.MAX_VALUE;
static int[] goedkoopste = new int[6];

static boolean isGeldig(int[] b) {
    if (!socketPast[b[0]][b[1]] || !geheugenPast[b[2]][b[1]] || !kastPast[b[1]][b[5]]) {
        return false;
    }
    if (gpuLengte[b[3]] > kastMaxLengte[b[5]]) {
        return false;
    }
    return psuWatt[b[4]] >= advies(cpuWatt[b[0]], gpuWatt[b[3]]);
}

static int zoek(int soort, int[] build, int budget) {
    if (soort == build.length) {                // basisgeval: alle zes gekozen
        combinaties++;
        if (!isGeldig(build)) {
            return 0;
        }
        geldig++;
        int prijs = prijs(build);
        if (prijs < goedkoopstePrijs) {
            goedkoopstePrijs = prijs;
            for (int s = 0; s < build.length; s++) {
                goedkoopste[s] = build[s];
            }
        }
        if (prijs <= budget) {
            System.out.println(euro(prijs) + ": " + code(build));
            return 1;
        }
        return 0;
    }
    int binnenBudget = 0;
    for (int i = 0; i < namen[soort].length; i++) {
        build[soort] = i;
        binnenBudget += zoek(soort + 1, build, budget);
    }
    return binnenBudget;
}

// in main:
combinaties = 0;
geldig = 0;
goedkoopstePrijs = Integer.MAX_VALUE;
int binnenBudget = zoek(0, new int[6], budget);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `zoek(soort, build, budget)` in de lus. De soort wordt nooit groter, en de methode roept zichzelf op tot de stack vol is: `StackOverflowError`.
> - Het basisgeval een stap te laat: `soort == build.length + 1`. Bij soort 6 zoekt de lus dan in `namen[6]`: `ArrayIndexOutOfBoundsException: Index 6 out of bounds for length 6`.
> - `goedkoopste = build;` in plaats van de zes waarden over te nemen. Dan wijzen ze naar dezelfde array, en verandert *de goedkoopste* mee met elke volgende combinatie.

---

## 7. Alles samen: het buildrapport

**Nakijkpunten**

| Build | Wat het rapport toont |
|---|---|
| <code style="color:#e64980;font-weight:600">CPU-02 MB-02 RAM-01 GPU-02 PSU-01 KAST-01</code> | <code style="color:#845ef7">Compatibel      : nee (4 problemen)</code> en het voorstel uit de opgave |
| <code style="color:#e64980;font-weight:600">CPU-03 MB-01 RAM-01 GPU-01 PSU-01 KAST-01</code> | <code style="color:#845ef7">Voorstel        : CPU-03 MB-02 RAM-02 GPU-01 PSU-01 KAST-01</code> en <code style="color:#845ef7">1200,00 euro (164,00 euro minder)</code> |
| <code style="color:#e64980;font-weight:600">CPU-01 MB-01 RAM-01 GPU-01 PSU-01 KAST-02</code> | <code style="color:#845ef7">Voorstel        : geen, deze build is compatibel</code> |

- Na het voorstel, voor elk budget: nog altijd <code style="color:#845ef7">Geldige builds: 18 van 144</code>.

**Aanpak.** `zoek` krijgt een extra parameter `vast`. In de lus sla je elk onderdeel over dat niet gelijk is aan het vaste, als die soort vastligt. Voor het voorstel liggen de processor en de videokaart vast, en is het budget -1: zo toont `zoek` niets, maar houdt ze wel de goedkoopste bij. `verschillen` vergelijkt de twee builds soort per soort.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int zoek(int soort, int[] build, int[] vast, int budget) {
    // ... het basisgeval zoals in punt 6
    int binnenBudget = 0;
    for (int i = 0; i < namen[soort].length; i++) {
        if (vast[soort] != -1 && i != vast[soort]) {
            continue;                          // deze soort ligt vast
        }
        build[soort] = i;
        binnenBudget += zoek(soort + 1, build, vast, budget);
    }
    return binnenBudget;
}

static String verschillen(int[] build, int[] voorstel) {
    StringBuilder sb = new StringBuilder();
    for (int s = 0; s < build.length; s++) {
        if (build[s] != voorstel[s]) {
            if (sb.length() > 0) {
                sb.append(", ");
            }
            sb.append(code(s, voorstel[s])).append(" in plaats van ").append(code(s, build[s]));
        }
    }
    return sb.toString();
}

// in main, onder de regels van het rapport:
if (problemen == 0) {
    System.out.println("Voorstel        : geen, deze build is compatibel");
} else {
    int[] vast = {c, -1, -1, g, -1, -1};      // processor en videokaart blijven
    goedkoopstePrijs = Integer.MAX_VALUE;
    zoek(0, new int[6], vast, -1);
    int verschil = goedkoopstePrijs - onderdelen;
    System.out.println("Voorstel        : " + code(goedkoopste));
    System.out.println("                  " + euro(goedkoopstePrijs) + " (" + euro(Math.abs(verschil)) + (verschil >= 0 ? " meer)" : " minder)"));
    System.out.println("Anders          : " + verschillen(build, goedkoopste));
}
// en voor de zoektocht binnen het budget:
int[] vrij = {-1, -1, -1, -1, -1, -1};
int binnenBudget = zoek(0, new int[6], vrij, budget);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `int[] vast = build;`: alle zes soorten liggen vast. Een build die niet compatibel is, heeft dan geen enkel geldig voorstel, en `goedkoopstePrijs` blijft `Integer.MAX_VALUE`: `21474836,47 euro`.
> - De tellers niet terugzetten na het voorstel. De zoektocht binnen het budget telt dan de 24 combinaties van het voorstel mee: `Geldige builds: 20 van 168`.

> [!TIP]
> **Zo kan het beter**
>
> - `isGeldig` en de regels in het rapport controleren dezelfde vijf regels, maar elk op hun manier. In hoofdstuk 06 wordt elke regel een klasse achter de interface `CompatibiliteitsRegel`, die zowel *ok of niet* als *waarom* kan geven.
> - Het voorstel houdt de processor en de videokaart vast. Met een array `vast` die de klant zelf kiest, of een voorstel per regel die faalt, wordt het rapport nog bruikbaarder, zonder iets aan `zoek` te veranderen.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Scanner;

public class Main {
    static final int REST = 75;

    // de onderdelen, prijzen in cent
    static String[] cpuNamen = {"Ryzen 5 7600X", "Ryzen 7 7800X3D", "Core i5-14600K"};
    static int[] cpuPrijzen = {19900, 38900, 29900};
    static int[] cpuWatt = {105, 120, 125};
    static String[] mbNamen = {"B650 Tomahawk", "B760M DS3H DDR4"};
    static int[] mbPrijzen = {21900, 11900};
    static String[] ramNamen = {"32 GB DDR5-6000", "16 GB DDR4-3200"};
    static int[] ramPrijzen = {10900, 4500};
    static String[] gpuNamen = {"GeForce RTX 4070", "Radeon RX 7800 XT"};
    static int[] gpuPrijzen = {58900, 49900};
    static int[] gpuWatt = {200, 263};
    static int[] gpuLengte = {267, 320};
    static String[] psuNamen = {"Voeding 550 W", "Voeding 750 W", "Voeding 850 W"};
    static int[] psuPrijzen = {6900, 10900, 13900};
    static int[] psuWatt = {550, 750, 850};
    static String[] kastNamen = {"Compact M", "Midi Air"};
    static int[] kastPrijzen = {7900, 9900};
    static int[] kastMaxLengte = {300, 360};

    // de compatibiliteitstabellen
    static boolean[][] socketPast = {{true, false}, {true, false}, {false, true}};
    static boolean[][] geheugenPast = {{true, false}, {false, true}};
    static boolean[][] kastPast = {{false, true}, {true, true}};

    // per soort: 0 processor, 1 moederbord, 2 geheugen, 3 videokaart, 4 voeding, 5 behuizing
    static String[] codes = {"CPU", "MB", "RAM", "GPU", "PSU", "KAST"};
    static String[] soorten = {"processor", "moederbord", "geheugen", "videokaart", "voeding", "behuizing"};
    static String[] koppen = {"Processors", "Moederborden", "Geheugen", "Videokaarten", "Voedingen", "Behuizingen"};
    static String[][] namen = {cpuNamen, mbNamen, ramNamen, gpuNamen, psuNamen, kastNamen};
    static int[][] prijzen = {cpuPrijzen, mbPrijzen, ramPrijzen, gpuPrijzen, psuPrijzen, kastPrijzen};

    // de zoektocht
    static int combinaties = 0;
    static int geldig = 0;
    static int goedkoopstePrijs = Integer.MAX_VALUE;
    static int[] goedkoopste = new int[6];

    public static void main(String[] args) {
        System.out.println("Bit & Byte - configurator");
        Scanner sc = new Scanner(System.in);

        // de lijsten
        for (int s = 0; s < namen.length; s++) {
            System.out.println(koppen[s] + ":");
            for (int i = 0; i < namen[s].length; i++) {
                String info = info(s, i);
                System.out.printf("%-8s %-18s %8.2f euro%s%n", code(s, i), namen[s][i], prijzen[s][i] / 100.0, info.isEmpty() ? "" : "   " + info);
            }
        }

        // een build kiezen
        int[] build;
        do {
            System.out.print("Build (6 codes): ");
            build = leesBuild(sc.nextLine());
        } while (build == null);
        int c = build[0], m = build[1], r = build[2], g = build[3], p = build[4], k = build[5];
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
        int advies = advies(wattProcessor, wattVideokaart);
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
        System.out.println("Build        : " + code(build));
        System.out.println("Processor    : " + cpuNamen[c]);
        System.out.println("Moederbord   : " + mbNamen[m]);
        System.out.println("Geheugen     : " + ramNamen[r]);
        System.out.println("Videokaart   : " + gpuNamen[g]);
        System.out.println("Voeding      : " + psuNamen[p]);
        System.out.println("Behuizing    : " + kastNamen[k]);
        System.out.println("------------------------------");
        System.out.println("Onderdelen   : " + euro(onderdelen));
        System.out.println("  zonder btw : " + euro(zonderBtw));
        System.out.println("  btw 21 %   : " + euro(btw));
        System.out.println("Vermogen     : " + vermogen + " W");
        System.out.println("Advies       : " + advies + " W");
        System.out.println("------------------------------");
        System.out.println("1. Socket       : " + socket);
        System.out.println("2. Geheugentype : " + geheugentype);
        System.out.println("3. Formfactor   : " + formfactor);
        System.out.println("4. Lengte       : " + lengte);
        System.out.println("5. Voeding      : " + wattVoeding + " W (" + oordeel + ")");
        System.out.println("Compatibel      : " + (problemen == 0 ? "ja" : "nee (" + problemen + " " + (problemen == 1 ? "probleem" : "problemen") + ")"));
        if (problemen == 0) {
            System.out.println("Voorstel        : geen, deze build is compatibel");
        } else {
            int[] vast = {c, -1, -1, g, -1, -1};      // processor en videokaart blijven
            goedkoopstePrijs = Integer.MAX_VALUE;
            zoek(0, new int[6], vast, -1);
            int verschil = goedkoopstePrijs - onderdelen;
            System.out.println("Voorstel        : " + code(goedkoopste));
            System.out.println("                  " + euro(goedkoopstePrijs) + " (" + euro(Math.abs(verschil)) + (verschil >= 0 ? " meer)" : " minder)"));
            System.out.println("Anders          : " + verschillen(build, goedkoopste));
        }
        System.out.println("==============================");

        // alle combinaties
        System.out.print("Budget in euro: ");
        int budget = (int) Math.round(sc.nextDouble() * 100);
        combinaties = 0;
        geldig = 0;
        goedkoopstePrijs = Integer.MAX_VALUE;
        int[] vrij = {-1, -1, -1, -1, -1, -1};
        int binnenBudget = zoek(0, new int[6], vrij, budget);
        if (binnenBudget == 0) {
            System.out.println("Geen geldige build binnen " + euro(budget) + ".");
        } else {
            System.out.println(binnenBudget + " geldige build(s) binnen " + euro(budget) + ".");
        }
        System.out.println("Geldige builds: " + geldig + " van " + combinaties);
        System.out.println("Goedkoopste geldige build: " + euro(goedkoopstePrijs));
        System.out.println("  " + code(goedkoopste));
    }

    // een bedrag in cent als tekst
    static String euro(int cent) {
        return String.format("%d,%02d euro", cent / 100, cent % 100);
    }

    // het voedingsadvies: 30 % marge, naar boven afgerond op 50 W
    static int advies(int wattProcessor, int wattVideokaart) {
        int vermogen = wattProcessor + wattVideokaart + REST;
        int metMarge = (vermogen * 13 + 9) / 10;
        return (metMarge + 49) / 50 * 50;
    }

    // codes zoals CPU-01
    static String code(int soort, int i) {
        return String.format("%s-%02d", codes[soort], i + 1);
    }

    static String info(int soort, int i) {
        return switch (soort) {
            case 0 -> cpuWatt[i] + " W";
            case 3 -> gpuWatt[i] + " W, " + gpuLengte[i] + " mm";
            case 5 -> "videokaart tot " + kastMaxLengte[i] + " mm";
            default -> "";
        };
    }

    // een build uit een regel codes, of null bij een fout
    static boolean isGetal(String tekst) {
        if (tekst.isEmpty()) {
            return false;
        }
        for (int i = 0; i < tekst.length(); i++) {
            if (!Character.isDigit(tekst.charAt(i))) {
                return false;
            }
        }
        return true;
    }

    static int soortVan(String code) {
        int streep = code.indexOf('-');
        if (streep == -1) {
            return -1;
        }
        String voor = code.substring(0, streep).toUpperCase();
        for (int s = 0; s < codes.length; s++) {
            if (codes[s].equals(voor)) {
                return s;
            }
        }
        return -1;
    }

    static int nummerVan(String code) {
        String na = code.substring(code.indexOf('-') + 1);
        return isGetal(na) ? Integer.parseInt(na) : -1;
    }

    static int[] leesBuild(String regel) {
        int[] build = {-1, -1, -1, -1, -1, -1};
        boolean fout = false;
        for (String code : regel.trim().split(" ")) {
            if (code.isEmpty()) {
                continue;                          // twee spaties na elkaar
            }
            int s = soortVan(code);
            if (s == -1) {
                System.out.println("Onbekende code: " + code + ".");
                fout = true;
                continue;
            }
            int nr = nummerVan(code);
            if (nr < 1 || nr > namen[s].length) {
                System.out.println("Bestaat niet: " + code + ".");
                fout = true;
            } else if (build[s] != -1) {
                System.out.println("Twee keer een " + soorten[s] + ": " + code(s, build[s]) + " en " + code + ".");
                fout = true;
            } else {
                build[s] = nr - 1;
            }
        }
        for (int s = 0; s < build.length; s++) {
            if (build[s] == -1) {
                System.out.println("Geen " + soorten[s] + " gekozen.");
                fout = true;
            }
        }
        return fout ? null : build;
    }

    // een volledige build als codes: dezelfde naam, andere parameters
    static String code(int[] build) {
        StringBuilder sb = new StringBuilder();
        for (int s = 0; s < build.length; s++) {
            if (s > 0) {
                sb.append(" ");
            }
            sb.append(code(s, build[s]));
        }
        return sb.toString();
    }

    static int prijs(int[] build) {
        int som = 0;
        for (int s = 0; s < build.length; s++) {
            som += prijzen[s][build[s]];
        }
        return som;
    }

    // alle builds, soort per soort: recursie in plaats van zes lussen
    static boolean isGeldig(int[] b) {
        if (!socketPast[b[0]][b[1]] || !geheugenPast[b[2]][b[1]] || !kastPast[b[1]][b[5]]) {
            return false;
        }
        if (gpuLengte[b[3]] > kastMaxLengte[b[5]]) {
            return false;
        }
        return psuWatt[b[4]] >= advies(cpuWatt[b[0]], gpuWatt[b[3]]);
    }

    static int zoek(int soort, int[] build, int[] vast, int budget) {
        if (soort == build.length) {                // basisgeval: alle zes gekozen
            combinaties++;
            if (!isGeldig(build)) {
                return 0;
            }
            geldig++;
            int prijs = prijs(build);
            if (prijs < goedkoopstePrijs) {
                goedkoopstePrijs = prijs;
                for (int s = 0; s < build.length; s++) {
                    goedkoopste[s] = build[s];
                }
            }
            if (prijs <= budget) {
                System.out.println(euro(prijs) + ": " + code(build));
                return 1;
            }
            return 0;
        }
        int binnenBudget = 0;
        for (int i = 0; i < namen[soort].length; i++) {
            if (vast[soort] != -1 && i != vast[soort]) {
                continue;                          // deze soort ligt vast
            }
            build[soort] = i;
            binnenBudget += zoek(soort + 1, build, vast, budget);
        }
        return binnenBudget;
    }

    // de verschillen tussen twee builds
    static String verschillen(int[] build, int[] voorstel) {
        StringBuilder sb = new StringBuilder();
        for (int s = 0; s < build.length; s++) {
            if (build[s] != voorstel[s]) {
                if (sb.length() > 0) {
                    sb.append(", ");
                }
                sb.append(code(s, voorstel[s])).append(" in plaats van ").append(code(s, build[s]));
            }
        }
        return sb.toString();
    }
}
```

</details>
