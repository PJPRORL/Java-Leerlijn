# Oplossing 03_05

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 03</b> &nbsp;·&nbsp; Tekst ontleden &nbsp;·&nbsp; Fietsverhuur &nbsp;·&nbsp; bij `Oefening_03_05.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21. Een nieuw contractnummer is willekeurig: jouw nummer is anders. Na elke keuze toont je programma opnieuw het menu; dat staat niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 02_05 staat in de map `Oefening_03_05`. Fiets 3 aan klant 1 om 9:45, terug om 13:10, toont onder meer <code style="color:#845ef7">Vertrek       : 9:45</code> en <code style="color:#845ef7">Huurprijs     : 24 euro</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_02_05.md` voor de code.

---

## 2. De tarieven in methodes

**Nakijkpunten**

- Je programma toont bij dezelfde invoer exact hetzelfde als in punt 1, op het contractnummer na: <code style="color:#845ef7">Fiets         : e-bike Stromer (6 euro per uur, max 30 euro)</code> en <code style="color:#845ef7">Huurprijs     : 24 euro</code>.
- De vloot: <code style="color:#845ef7">5. bakfiets Babboe - beschikbaar</code> en <code style="color:#845ef7">6. bakfiets Urban Arrow - in herstelling</code>.

**Aanpak.** Elke tabel van het verhuurbedrijf wordt een methode met het type als parameter. `huurprijs` gebruikt de andere methodes: `begonnenUren`, `uurtarief` en `dagmaximum`. Daarna staat de regel voor de prijs op één plaats.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String typeNaam(int type) {
    return switch (type) {
        case 1 -> "stadsfiets";
        case 2 -> "e-bike";
        default -> "bakfiets";
    };
}

static int uurtarief(int type) {
    return type == 1 ? 3 : type == 2 ? 6 : 8;
}

static int begonnenUren(int duur) {
    return (duur + 59) / 60;
}

static int huurprijs(int type, int duur) {
    return Math.min(begonnenUren(duur) * uurtarief(type), dagmaximum(type));
}

// in main, bij het terugbrengen:
int huurprijs = huurprijs(typeNummer, duur);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `huurprijs(duur, typeNummer)`: de argumenten in de omgekeerde volgorde. Allebei `int`, dus de compiler zegt niets. Je programma rekent met type 205, een bakfiets, en een duur van 2 minuten: <code style="color:#845ef7">Huurprijs     : 8 euro</code> in plaats van 24.

---

## 3. Tijden als 09:45

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| fiets <code style="color:#e64980;font-weight:600">3</code> aan <code style="color:#e64980;font-weight:600">1</code> om <code style="color:#e64980;font-weight:600">9</code> uur <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Fiets 3 verhuurd aan Sara Janssens om 09:05, contract FV-2026-</code> met vier cijfers |
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">- fiets 3, e-bike Stromer, sinds 09:05</code> en <code style="color:#845ef7">- fiets 4, e-bike Cube, sinds 08:30</code> |
| fiets <code style="color:#e64980;font-weight:600">3</code> terug om <code style="color:#e64980;font-weight:600">13</code> uur <code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Vertrek       : 09:05</code>, <code style="color:#845ef7">Terug         : 13:10</code> en <code style="color:#845ef7">Huurprijs     : 30 euro</code> |

**Aanpak.** `tijd` krijgt minuten na middernacht en maakt er uren en minuten van met `/ 60` en `% 60`. `%02d` zet die met minstens twee cijfers, en vult aan met nullen. Op het contract vervang je de twee hulpvariabelen voor vertrek en terug door een oproep van `tijd`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String tijd(int minuten) {
    return String.format("%02d:%02d", minuten / 60, minuten % 60);
}

// in main:
System.out.println("Fiets " + id + " verhuurd aan " + klanten[klant] + " om " + tijd(vertrek) + ", contract " + contracten[i] + ".");
System.out.println("Vertrek       : " + tijd(vertrek));
System.out.println("Terug         : " + tijd(terug));
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `%2d` in plaats van `%02d`. Zonder de 0 vult Java aan met spaties: `om  9: 5` en `sinds  8:30`.

---

## 4. Een tijd als tekst

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| fiets <code style="color:#e64980;font-weight:600">3</code> aan <code style="color:#e64980;font-weight:600">1</code>, dan <code style="color:#e64980;font-weight:600">9u45</code>, <code style="color:#e64980;font-weight:600">25:00</code>, <code style="color:#e64980;font-weight:600">9:5</code> | telkens <code style="color:#845ef7">Geen geldige tijd: 9u45.</code> enzovoort, en de vraag opnieuw |
| daarna <code style="color:#e64980;font-weight:600">9:45</code> | <code style="color:#845ef7">Fiets 3 verhuurd aan Sara Janssens om 09:45, contract FV-2026-</code> met vier cijfers |
| fiets <code style="color:#e64980;font-weight:600">3</code> terug om <code style="color:#e64980;font-weight:600">13:10</code> | <code style="color:#845ef7">Huurprijs     : 24 euro</code> |
| fiets <code style="color:#e64980;font-weight:600">4</code> terug om <code style="color:#e64980;font-weight:600">8:00</code> | <code style="color:#845ef7">De terugkomst ligt niet na het vertrek.</code> |

**Aanpak.** `minuten` geeft -1 bij elke fout, zodat de vraag één test nodig heeft. Eerst de vorm: twee stukken, allebei cijfers, en de minuten met twee cijfers. Pas dan `parseInt`, en daarna de grenzen. `vraagTijd` herhaalt de vraag met een `do-while`, en krijgt de `Scanner` als parameter: een methode ziet de variabele `sc` van `main` niet.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int minuten(String tijd) {
    String[] delen = tijd.split(":");
    if (delen.length != 2 || !isGetal(delen[0]) || !isGetal(delen[1]) || delen[1].length() != 2) {
        return -1;
    }
    int uur = Integer.parseInt(delen[0]);
    int minuut = Integer.parseInt(delen[1]);
    if (uur > 23 || minuut > 59) {
        return -1;
    }
    return uur * 60 + minuut;
}

static int vraagTijd(Scanner sc, String wat) {
    int minuten;
    do {
        System.out.print(wat + " (uu:mm): ");
        String tekst = sc.next();
        minuten = minuten(tekst);
        if (minuten == -1) {
            System.out.println("Geen geldige tijd: " + tekst + ".");
        }
    } while (minuten == -1);
    return minuten;
}

// in main:
int vertrek = vraagTijd(sc, "Vertrek");
```

`isGetal` is dezelfde als in `Oplossing_03_02.md`.

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `parseInt` zonder `isGetal` ervoor. Bij <code style="color:#e64980;font-weight:600">9:4a</code> stopt je programma met `NumberFormatException: For input string: "4a"`.
> - De tijd lezen met `nextInt`. `09:45` is geen geheel getal: `InputMismatchException`.

---

## 5. Een klant op nummer of naam

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| fiets <code style="color:#e64980;font-weight:600">3</code> aan <code style="color:#e64980;font-weight:600">f001</code>, om <code style="color:#e64980;font-weight:600">09:45</code> | <code style="color:#845ef7">Fiets 3 verhuurd aan Sara Janssens om 09:45, contract FV-2026-</code> met vier cijfers |
| fiets <code style="color:#e64980;font-weight:600">1</code> aan <code style="color:#e64980;font-weight:600">Tom Wouters</code>, dan fiets <code style="color:#e64980;font-weight:600">2</code> aan <code style="color:#e64980;font-weight:600">tom wouters</code> | <code style="color:#845ef7">Tom Wouters heeft al 2 fietsen.</code> |
| fiets <code style="color:#e64980;font-weight:600">2</code> aan <code style="color:#e64980;font-weight:600">Jan</code> | <code style="color:#845ef7">Onbekende klant: Jan.</code> |
| fiets <code style="color:#e64980;font-weight:600">4</code> terug om <code style="color:#e64980;font-weight:600">17:45</code> | <code style="color:#845ef7">Klant         : F002 Tom Wouters</code> |

**Aanpak.** `zoekKlant` vergelijkt de tekst met het nummer én met de naam, telkens met `equalsIgnoreCase`. In `main` vervangt de test `klant == -1` de vroegere test op een nummer buiten 1 en 2. Lees de klant met `nextLine`, na een extra `sc.nextLine()` voor de Enter na het fietsnummer.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int zoekKlant(String[] nummers, String[] namen, String tekst) {
    for (int k = 0; k < namen.length; k++) {
        if (nummers[k].equalsIgnoreCase(tekst) || namen[k].equalsIgnoreCase(tekst)) {
            return k;
        }
    }
    return -1;
}

// in main, bij het verhuren:
sc.nextLine();                         // de Enter na het nummer
System.out.print("Klant (nummer of naam): ");
String tekst = sc.nextLine().trim();
int klant = zoekKlant(klantNummers, klanten, tekst);
// ... inHuur tellen zoals in 02_05
if (klant == -1) {
    System.out.println("Onbekende klant: " + tekst + ".");
} else if (inHuur >= MAX_PER_KLANT) {
    // ...
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `nummers[k] == tekst`. De tekst uit de `Scanner` is een ander object dan `"F001"` in je array: <code style="color:#e64980;font-weight:600">F001</code> geeft <code style="color:#845ef7">Onbekende klant: F001.</code>

---

## 6. Het overzicht op één regel per klant

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> bij de start | <code style="color:#845ef7">F001 Sara Janssens: geen fietsen</code><br><code style="color:#845ef7">F002 Tom Wouters: fiets 4 (e-bike Cube, sinds 08:30)</code> |
| na fiets 3 aan <code style="color:#e64980;font-weight:600">F001</code> om <code style="color:#e64980;font-weight:600">09:45</code> en fiets 1 aan <code style="color:#e64980;font-weight:600">F002</code> om <code style="color:#e64980;font-weight:600">09:00</code> | <code style="color:#845ef7">F001 Sara Janssens: fiets 3 (e-bike Stromer, sinds 09:45)</code><br><code style="color:#845ef7">F002 Tom Wouters: fiets 1 (stadsfiets Gazelle, sinds 09:00), fiets 4 (e-bike Cube, sinds 08:30)</code> |

**Aanpak.** Eén `StringBuilder` per klant: `fietsenVan` maakt hem zelf aan bij elke oproep. Een lege `StringBuilder` op het einde betekent dat de klant geen fietsen heeft. De ternary operator op de laatste regel kiest dan *geen fietsen*.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String fietsenVan(int klant, int[] huurders, int[] typeNummers, String[] merken, int[] vertrekken) {
    StringBuilder sb = new StringBuilder();
    for (int f = 0; f < huurders.length; f++) {
        if (huurders[f] == klant) {
            if (sb.length() > 0) {
                sb.append(", ");
            }
            sb.append("fiets ").append(f + 1).append(" (").append(typeNaam(typeNummers[f]))
              .append(" ").append(merken[f]).append(", sinds ").append(tijd(vertrekken[f])).append(")");
        }
    }
    return sb.length() == 0 ? "geen fietsen" : sb.toString();
}

// in main:
System.out.println(klantNummers[k] + " " + klanten[k] + ": " + fietsenVan(k, huurders, typeNummers, merken, vertrekken));
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `if (f > 0)` om te beslissen over de komma. De eerste fiets van Tom is fiets 4, met index 3: je krijgt `F002 Tom Wouters: , fiets 4 (e-bike Cube, sinds 08:30)`.

---

## 7. Alles samen: de huurbon uit één regel invoer

**Nakijkpunten**

| Invoer na <code style="color:#e64980;font-weight:600">2</code> | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">3 F001 09:45</code> | de huurbon uit de opgave |
| <code style="color:#e64980;font-weight:600">3 F002 10:00</code> | <code style="color:#845ef7">Fiets 3 is al verhuurd.</code> |
| <code style="color:#e64980;font-weight:600">6 F001 10:00</code> | <code style="color:#845ef7">Fiets 6 is in herstelling.</code> |
| <code style="color:#e64980;font-weight:600">9 F001 10:00</code> | <code style="color:#845ef7">Fiets 9 bestaat niet.</code> |
| <code style="color:#e64980;font-weight:600">1 F003 10:00</code> | <code style="color:#845ef7">Onbekende klant: F003.</code> |
| <code style="color:#e64980;font-weight:600">1 F002 25:00</code> | <code style="color:#845ef7">Geen geldige tijd: 25:00.</code> |
| <code style="color:#e64980;font-weight:600">1 f002 9:00</code> | een huurbon met <code style="color:#845ef7">Klant         : F002 Tom Wouters</code>, <code style="color:#845ef7">Fiets         : 1 stadsfiets Gazelle</code> en <code style="color:#845ef7">Tarief        : 3 euro per begonnen uur, max 15 euro</code> |
| <code style="color:#e64980;font-weight:600">2 F002 11:00</code> | <code style="color:#845ef7">Tom Wouters heeft al 2 fietsen.</code> |
| <code style="color:#e64980;font-weight:600">1 Tom Wouters 9:00</code> of een lege regel | <code style="color:#845ef7">Gebruik de vorm fiets klant uu:mm, zoals 3 F001 09:45.</code> |

**Aanpak.** Split de regel op de spatie, en controleer eerst het aantal delen. Het fietsnummer controleer je met `isGetal` vóór je het omzet. Daarna een ladder: de fiets, de klant, de tijd. De methodes `zoekKlant` en `minuten` uit punt 4 en 5 doen het eigenlijke werk. Pas in de laatste `else` verander je de arrays en toon je de huurbon.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
case 2 -> {
    sc.nextLine();                         // de Enter na de keuze
    System.out.print("Verhuring (fiets klant uu:mm): ");
    String regel = sc.nextLine().trim();
    String[] delen = regel.split(" ");
    if (delen.length != 3) {
        System.out.println("Gebruik de vorm fiets klant uu:mm, zoals 3 F001 09:45.");
    } else if (!isGetal(delen[0]) || Integer.parseInt(delen[0]) < 1 || Integer.parseInt(delen[0]) > merken.length) {
        System.out.println("Fiets " + delen[0] + " bestaat niet.");
    } else {
        int i = Integer.parseInt(delen[0]) - 1;
        int klant = zoekKlant(klantNummers, klanten, delen[1]);
        int vertrek = minuten(delen[2]);
        if (statussen[i] == 1) {
            System.out.println("Fiets " + (i + 1) + " is al verhuurd.");
        } else if (statussen[i] == 2) {
            System.out.println("Fiets " + (i + 1) + " is in herstelling.");
        } else if (klant == -1) {
            System.out.println("Onbekende klant: " + delen[1] + ".");
        } else if (aantalFietsen(klant, huurders) >= MAX_PER_KLANT) {
            System.out.println(klanten[klant] + " heeft al " + MAX_PER_KLANT + " fietsen.");
        } else if (vertrek == -1) {
            System.out.println("Geen geldige tijd: " + delen[2] + ".");
        } else {
            statussen[i] = 1;
            huurders[i] = klant;
            vertrekken[i] = vertrek;
            contracten[i] = "FV-" + JAAR + "-" + (random.nextInt(9999 - 1000 + 1) + 1000);
            System.out.print(huurbon(contracten[i], klantNummers[klant] + " " + klanten[klant], i + 1, typeNummers[i], merken[i], vertrek));
        }
    }
}

// onder main:
static String huurbon(String contract, String klant, int fiets, int type, String merk, int vertrek) {
    StringBuilder sb = new StringBuilder();
    sb.append("==============================\n");
    sb.append("FIETSVERHUUR - HUURBON\n");
    sb.append("==============================\n");
    sb.append("Contract      : ").append(contract).append("\n");
    sb.append("Klant         : ").append(klant).append("\n");
    sb.append("Fiets         : ").append(fiets).append(" ").append(typeNaam(type)).append(" ").append(merk).append("\n");
    sb.append("Vertrek       : ").append(tijd(vertrek)).append("\n");
    sb.append(String.format("Tarief        : %d euro per begonnen uur, max %d euro\n", uurtarief(type), dagmaximum(type)));
    sb.append("Nu te betalen : ").append(waarborg(type)).append(" euro (waarborg)\n");
    sb.append("==============================\n");
    return sb.toString();
}
```

`aantalFietsen(int klant, int[] huurders)` telt hoeveel elementen van `huurders` gelijk zijn aan `klant`: de lus van 02_05, in een methode.

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De status al op verhuurd zetten voor de ladder. Bij <code style="color:#e64980;font-weight:600">3 F003 09:45</code> meldt je programma dan <code style="color:#845ef7">Fiets 3 is al verhuurd.</code> in plaats van de onbekende klant, en de vloot toont <code style="color:#845ef7">3. e-bike Stromer - verhuurd</code>, aan niemand.
> - `Integer.parseInt(delen[0])` zonder `isGetal`. Bij <code style="color:#e64980;font-weight:600">x F001 09:45</code> stopt je programma met een `NumberFormatException`.

> [!TIP]
> **Zo kan het beter**
>
> - In de `else if` voor het fietsnummer roep je `Integer.parseInt(delen[0])` drie keer op. Een methode `fietsVan(String tekst, int aantal)` die het nummer of -1 teruggeeft, zoals `minuten`, maakt die regel korter.
> - De medewerker moet precies één spatie typen. Twee spaties geven vier delen. In hoofdstuk 08 leer je met reguliere expressies splitsen op *één of meer* spaties: `split(" +")`.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Random;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Fietsverhuur - balie");
        final int JAAR = 2026;
        final int SLUITINGSUUR = 20;
        final int BOETE = 10;
        final int MAX_PER_KLANT = 2;
        Random random = new Random();
        Scanner sc = new Scanner(System.in);

        // de vloot
        int[] typeNummers = {1, 1, 2, 2, 3, 3};
        String[] merken = {"Gazelle", "Oxford", "Stromer", "Cube", "Babboe", "Urban Arrow"};
        int[] statussen = {0, 0, 0, 1, 0, 2};    // 0 = beschikbaar, 1 = verhuurd, 2 = in herstelling
        int[] vertrekken = {0, 0, 0, 8 * 60 + 30, 0, 0};    // minuten na middernacht
        String[] contracten = {"", "", "", "FV-2026-4821", "", ""};
        String[] klantNummers = {"F001", "F002"};
        String[] klanten = {"Sara Janssens", "Tom Wouters"};
        int[] huurders = {-1, -1, -1, 1, -1, -1};    // -1 = niemand

        int keuze;
        do {
            toonMenu();
            keuze = sc.nextInt();
            switch (keuze) {
                case 1 -> {
                    for (int i = 0; i < merken.length; i++) {
                        System.out.println((i + 1) + ". " + typeNaam(typeNummers[i]) + " " + merken[i] + " - " + statusNaam(statussen[i]));
                    }
                }
                case 2 -> {
                    sc.nextLine();                         // de Enter na de keuze
                    System.out.print("Verhuring (fiets klant uu:mm): ");
                    String regel = sc.nextLine().trim();
                    String[] delen = regel.split(" ");
                    if (delen.length != 3) {
                        System.out.println("Gebruik de vorm fiets klant uu:mm, zoals 3 F001 09:45.");
                    } else if (!isGetal(delen[0]) || Integer.parseInt(delen[0]) < 1 || Integer.parseInt(delen[0]) > merken.length) {
                        System.out.println("Fiets " + delen[0] + " bestaat niet.");
                    } else {
                        int i = Integer.parseInt(delen[0]) - 1;
                        int klant = zoekKlant(klantNummers, klanten, delen[1]);
                        int vertrek = minuten(delen[2]);
                        if (statussen[i] == 1) {
                            System.out.println("Fiets " + (i + 1) + " is al verhuurd.");
                        } else if (statussen[i] == 2) {
                            System.out.println("Fiets " + (i + 1) + " is in herstelling.");
                        } else if (klant == -1) {
                            System.out.println("Onbekende klant: " + delen[1] + ".");
                        } else if (aantalFietsen(klant, huurders) >= MAX_PER_KLANT) {
                            System.out.println(klanten[klant] + " heeft al " + MAX_PER_KLANT + " fietsen.");
                        } else if (vertrek == -1) {
                            System.out.println("Geen geldige tijd: " + delen[2] + ".");
                        } else {
                            statussen[i] = 1;
                            huurders[i] = klant;
                            vertrekken[i] = vertrek;
                            contracten[i] = "FV-" + JAAR + "-" + (random.nextInt(9999 - 1000 + 1) + 1000);
                            System.out.print(huurbon(contracten[i], klantNummers[klant] + " " + klanten[klant], i + 1, typeNummers[i], merken[i], vertrek));
                        }
                    }
                }
                case 3 -> {
                    // de fiets
                    System.out.print("Fiets (1-6): ");
                    int id = sc.nextInt();
                    if (id < 1 || id > merken.length) {
                        System.out.println("Fiets " + id + " bestaat niet.");
                    } else if (statussen[id - 1] != 1) {
                        System.out.println("Fiets " + id + " is niet verhuurd.");
                    } else {
                        int i = id - 1;
                        int typeNummer = typeNummers[i];

                        // de huurduur
                        int vertrek = vertrekken[i];
                        int terug = vraagTijd(sc, "Terug");
                        int duur = terug - vertrek;
                        if (duur <= 0) {
                            System.out.println("De terugkomst ligt niet na het vertrek.");
                        } else {
                            int huurprijs = huurprijs(typeNummer, duur);
                            boolean teLaat = terug > SLUITINGSUUR * 60;
                            int boete = teLaat ? BOETE : 0;
                            int bijTerugkomst = huurprijs + boete;

                            // het huurcontract
                            System.out.println("==============================");
                            System.out.println("FIETSVERHUUR - HUURCONTRACT");
                            System.out.println("==============================");
                            System.out.println("Contract      : " + contracten[i]);
                            System.out.println("Klant         : " + klantNummers[huurders[i]] + " " + klanten[huurders[i]]);
                            System.out.println("Fiets         : " + typeNaam(typeNummer) + " " + merken[i] + " (" + uurtarief(typeNummer) + " euro per uur, max " + dagmaximum(typeNummer) + " euro)");
                            System.out.println("Vertrek       : " + tijd(vertrek));
                            System.out.println("Terug         : " + tijd(terug));
                            System.out.println("Duur          : " + duur / 60 + " u " + duur % 60 + " min (" + begonnenUren(duur) + " begonnen uren)");
                            System.out.println("Huurprijs     : " + huurprijs + " euro");
                            System.out.println("Boete         : " + boete + " euro");
                            System.out.println("------------------------------");
                            System.out.println("Bij vertrek   : " + waarborg(typeNummer) + " euro (waarborg)");
                            System.out.println("Bij terugkomst: " + bijTerugkomst + " euro (waarborg terug)");
                            System.out.println("==============================");
                            System.out.print("Betaald (j/n): ");
                            char betaald = sc.next().charAt(0);
                            if (betaald == 'j') {
                                statussen[i] = 0;
                                huurders[i] = -1;
                                System.out.println("Fiets " + (i + 1) + " is terug en weer beschikbaar.");
                            } else {
                                System.out.println("Niet betaald: fiets " + (i + 1) + " blijft verhuurd.");
                            }
                        }
                    }
                }
                case 4 -> {
                    for (int k = 0; k < klanten.length; k++) {
                        System.out.println(klantNummers[k] + " " + klanten[k] + ": " + fietsenVan(k, huurders, typeNummers, merken, vertrekken));
                    }
                }
                case 0 -> System.out.println("Tot ziens!");
                default -> System.out.println("Ongeldige keuze.");
            }
        } while (keuze != 0);
    }

    // het menu
    static void toonMenu() {
        System.out.println("--- Menu ---");
        System.out.println("1. Vloot");
        System.out.println("2. Verhuren");
        System.out.println("3. Terugbrengen");
        System.out.println("4. Overzicht per klant");
        System.out.println("0. Stoppen");
        System.out.print("Keuze: ");
    }

    // van nummer naar woord
    static String typeNaam(int type) {
        return switch (type) {
            case 1 -> "stadsfiets";
            case 2 -> "e-bike";
            default -> "bakfiets";
        };
    }

    static String statusNaam(int status) {
        return switch (status) {
            case 0 -> "beschikbaar";
            case 1 -> "verhuurd";
            default -> "in herstelling";
        };
    }

    // de tarieven per type
    static int uurtarief(int type) {
        return type == 1 ? 3 : type == 2 ? 6 : 8;
    }

    static int dagmaximum(int type) {
        return type == 1 ? 15 : type == 2 ? 30 : 40;
    }

    static int waarborg(int type) {
        return type == 1 ? 50 : type == 2 ? 150 : 100;
    }

    // de huurprijs voor een duur in minuten
    static int begonnenUren(int duur) {
        return (duur + 59) / 60;
    }

    static int huurprijs(int type, int duur) {
        return Math.min(begonnenUren(duur) * uurtarief(type), dagmaximum(type));
    }

    // een tijd als tekst, zoals 09:45
    static String tijd(int minuten) {
        return String.format("%02d:%02d", minuten / 60, minuten % 60);
    }

    // een tijd uit tekst, of -1
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

    static int minuten(String tijd) {
        String[] delen = tijd.split(":");
        if (delen.length != 2 || !isGetal(delen[0]) || !isGetal(delen[1]) || delen[1].length() != 2) {
            return -1;
        }
        int uur = Integer.parseInt(delen[0]);
        int minuut = Integer.parseInt(delen[1]);
        if (uur > 23 || minuut > 59) {
            return -1;
        }
        return uur * 60 + minuut;
    }

    static int vraagTijd(Scanner sc, String wat) {
        int minuten;
        do {
            System.out.print(wat + " (uu:mm): ");
            String tekst = sc.next();
            minuten = minuten(tekst);
            if (minuten == -1) {
                System.out.println("Geen geldige tijd: " + tekst + ".");
            }
        } while (minuten == -1);
        return minuten;
    }

    // een klant op nummer of naam, of -1
    static int zoekKlant(String[] nummers, String[] namen, String tekst) {
        for (int k = 0; k < namen.length; k++) {
            if (nummers[k].equalsIgnoreCase(tekst) || namen[k].equalsIgnoreCase(tekst)) {
                return k;
            }
        }
        return -1;
    }

    // de fietsen van een klant op een regel
    static String fietsenVan(int klant, int[] huurders, int[] typeNummers, String[] merken, int[] vertrekken) {
        StringBuilder sb = new StringBuilder();
        for (int f = 0; f < huurders.length; f++) {
            if (huurders[f] == klant) {
                if (sb.length() > 0) {
                    sb.append(", ");
                }
                sb.append("fiets ").append(f + 1).append(" (").append(typeNaam(typeNummers[f]))
                  .append(" ").append(merken[f]).append(", sinds ").append(tijd(vertrekken[f])).append(")");
            }
        }
        return sb.length() == 0 ? "geen fietsen" : sb.toString();
    }

    // de huurbon, uit een regel invoer
    static int aantalFietsen(int klant, int[] huurders) {
        int aantal = 0;
        for (int h : huurders) {
            if (h == klant) {
                aantal++;
            }
        }
        return aantal;
    }

    static String huurbon(String contract, String klant, int fiets, int type, String merk, int vertrek) {
        StringBuilder sb = new StringBuilder();
        sb.append("==============================\n");
        sb.append("FIETSVERHUUR - HUURBON\n");
        sb.append("==============================\n");
        sb.append("Contract      : ").append(contract).append("\n");
        sb.append("Klant         : ").append(klant).append("\n");
        sb.append("Fiets         : ").append(fiets).append(" ").append(typeNaam(type)).append(" ").append(merk).append("\n");
        sb.append("Vertrek       : ").append(tijd(vertrek)).append("\n");
        sb.append(String.format("Tarief        : %d euro per begonnen uur, max %d euro\n", uurtarief(type), dagmaximum(type)));
        sb.append("Nu te betalen : ").append(waarborg(type)).append(" euro (waarborg)\n");
        sb.append("==============================\n");
        return sb.toString();
    }
}
```

</details>
