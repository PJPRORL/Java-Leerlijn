# Oplossing 02_04

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 02</b> &nbsp;·&nbsp; Arrays en een menu &nbsp;·&nbsp; Dierenasiel &nbsp;·&nbsp; bij `Oefening_02_04.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21. Vanaf punt 4 toont je programma na elke keuze opnieuw het menu; dat staat niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 01_04 staat in de map `Oefening_02_04`. Met <code style="color:#e64980;font-weight:600">Luna</code>, <code style="color:#e64980;font-weight:600">2</code>, <code style="color:#e64980;font-weight:600">7</code>, <code style="color:#e64980;font-weight:600">3,1</code> en <code style="color:#e64980;font-weight:600">Ines Maes</code> toont de fiche onder meer <code style="color:#845ef7">Leeftijd     : 0 jaar en 7 maanden (jong)</code> en <code style="color:#845ef7">Adoptieprijs : 90 euro</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_01_04.md` voor de code.

---

## 2. Het asiel in arrays

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">1. Rex (hond) - 30 maanden - 28.5 kg - beschikbaar</code> tot en met <code style="color:#845ef7">6. Bobby (hond) - 4 maanden - 6.0 kg - beschikbaar</code>, met <code style="color:#845ef7">4. Pluis (konijn) - 14 maanden - 2.2 kg - gereserveerd</code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">Lotte Peeters</code> | de fiche van Luna, met als laatste regel <code style="color:#845ef7">Adoptant     : Lotte Peeters</code> |

**Aanpak.** Vijf parallelle arrays. De soort blijft een nummer, zoals in 01_04, en de status ook: teksten vergelijken komt pas in hoofdstuk 03. Het woord voor de status kies je met een `switch` die een waarde teruggeeft (leerboek 1.5, *Moderne switch*). Na het nummer van het dier lees je de adoptant met `nextLine()`; daarvoor eerst één `nextLine()` om de Enter na het nummer op te ruimen.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String[] namen = {"Rex", "Luna", "Max", "Pluis", "Mimi", "Bobby"};
int[] soortNummers = {1, 2, 1, 3, 2, 1};
int[] leeftijden = {30, 7, 110, 14, 50, 4};
double[] gewichten = {28.5, 3.1, 31.0, 2.2, 4.4, 6.0};
int[] statussen = {0, 0, 0, 1, 0, 0};    // 0 = beschikbaar, 1 = gereserveerd, 2 = geadopteerd

for (int i = 0; i < namen.length; i++) {
    String soort = soortNummers[i] == 1 ? "hond" : soortNummers[i] == 2 ? "kat" : "konijn";
    String toestand = switch (statussen[i]) {
        case 0 -> "beschikbaar";
        case 1 -> "gereserveerd";
        default -> "geadopteerd";
    };
    System.out.println((i + 1) + ". " + namen[i] + " (" + soort + ") - " + leeftijden[i] + " maanden - " + gewichten[i] + " kg - " + toestand);
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Een `switch` met pijlen zonder `default`, die een waarde moet teruggeven. Java weigert dat: een `int` kan ook 7 zijn, en dan zou er geen waarde zijn. De foutmelding zegt dat de `switch` niet alle mogelijke waarden dekt.
> - De Enter na het nummer vergeten. Dan krijgt Luna een lege adoptant.

---

## 3. Een adoptie weigeren

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code> | <code style="color:#845ef7">Dier 9 bestaat niet.</code> |
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Pluis is gereserveerd.</code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">Lotte Peeters</code>, daarna nog eens <code style="color:#e64980;font-weight:600">2</code> | eerst de fiche, daarna <code style="color:#845ef7">Luna is al geadopteerd.</code> |

**Aanpak.** Eén ladder, in de volgorde van de opgave: eerst het nummer, dan de status. Omdat Java een `||` en de takken van een ladder van links naar rechts en van boven naar onder afloopt, kijkt `statussen[id - 1]` nooit naar een index die niet bestaat: een verkeerd nummer is dan al opgevangen. Lukt de adoptie, dan zet je de status op 2.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Dier (1-6): ");
int id = sc.nextInt();
if (id < 1 || id > namen.length) {
    System.out.println("Dier " + id + " bestaat niet.");
} else if (statussen[id - 1] == 1) {
    System.out.println(namen[id - 1] + " is gereserveerd.");
} else if (statussen[id - 1] == 2) {
    System.out.println(namen[id - 1] + " is al geadopteerd.");
} else {
    int i = id - 1;
    // ... de gegevens uit de arrays, en de berekeningen van 01_04
    statussen[i] = 2;
    // ... de adoptant en de fiche
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De status controleren voor het nummer. Bij <code style="color:#e64980;font-weight:600">9</code> kijkt je programma dan naar `statussen[8]`, en stopt het met een `ArrayIndexOutOfBoundsException`.

---

## 4. Een menu in een lus

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> | de dieren |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">Lotte Peeters</code>, dan <code style="color:#e64980;font-weight:600">1</code> | de fiche, daarna <code style="color:#845ef7">2. Luna (kat) - 7 maanden - 3.1 kg - geadopteerd</code> |
| <code style="color:#e64980;font-weight:600">8</code> | <code style="color:#845ef7">Ongeldige keuze.</code> |
| <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Tot ziens!</code> |

**Aanpak.** Zoals in 02_01: een `do-while` met de keuze buiten de lus gedeclareerd, en een `switch` met pijlen. De code van punt 2 en 3 schuift in `case 1` en `case 2`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int keuze;
do {
    System.out.println("--- Menu ---");
    System.out.println("1. Overzicht");
    System.out.println("2. Adopteren");
    System.out.println("0. Stoppen");
    System.out.print("Keuze: ");
    keuze = sc.nextInt();
    switch (keuze) {
        case 1 -> {
            // ... de lus van punt 2
        }
        case 2 -> {
            // ... de adoptie van punt 3
        }
        case 0 -> System.out.println("Tot ziens!");
        default -> System.out.println("Ongeldige keuze.");
    }
} while (keuze != 0);
```

</details>

---

## 5. Zoeken tot een maximumleeftijd

**Nakijkpunten**

| Maximumleeftijd | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">12</code> | <code style="color:#845ef7">2. Luna - 7 maanden</code><br><code style="color:#845ef7">6. Bobby - 4 maanden</code><br><code style="color:#845ef7">2 dier(en) tot 12 maanden.</code> |
| <code style="color:#e64980;font-weight:600">14</code> | drie dieren, met <code style="color:#845ef7">4. Pluis - 14 maanden</code> |
| <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Geen dieren tot 2 maanden.</code> |

**Aanpak.** Eén lus met een `if` en een teller; of er niets gevonden is, weet je pas na de lus.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
case 3 -> {
    System.out.print("Maximumleeftijd in maanden: ");
    int maximum = sc.nextInt();
    int gevonden = 0;
    for (int i = 0; i < namen.length; i++) {
        if (leeftijden[i] <= maximum) {
            System.out.println((i + 1) + ". " + namen[i] + " - " + leeftijden[i] + " maanden");
            gevonden++;
        }
    }
    if (gevonden == 0) {
        System.out.println("Geen dieren tot " + maximum + " maanden.");
    } else {
        System.out.println(gevonden + " dier(en) tot " + maximum + " maanden.");
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `leeftijden[i] < maximum`. Dan valt Pluis weg bij een maximum van 14 maanden.

---

## 6. Statistiek

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> bij de start | <code style="color:#845ef7">Gemiddeld gewicht: 12.53 kg</code><br><code style="color:#845ef7">Jongste: Bobby (4 maanden)</code><br><code style="color:#845ef7">Oudste: Max (110 maanden)</code><br><code style="color:#845ef7">Beschikbaar: 5</code> |

**Aanpak.** Eén lus: de gewichten optellen, de beschikbare dieren tellen, en de index van het jongste en het oudste dier bijwerken, met index 0 als beginwaarde.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
case 4 -> {
    double som = 0;
    int jongste = 0;
    int oudste = 0;
    int beschikbaar = 0;
    for (int i = 0; i < namen.length; i++) {
        som += gewichten[i];
        if (statussen[i] == 0) {
            beschikbaar++;
        }
        if (leeftijden[i] < leeftijden[jongste]) {
            jongste = i;
        }
        if (leeftijden[i] > leeftijden[oudste]) {
            oudste = i;
        }
    }
    double gemiddelde = Math.round(som / namen.length * 100) / 100.0;
    System.out.println("Gemiddeld gewicht: " + gemiddelde + " kg");
    System.out.println("Jongste: " + namen[jongste] + " (" + leeftijden[jongste] + " maanden)");
    System.out.println("Oudste: " + namen[oudste] + " (" + leeftijden[oudste] + " maanden)");
    System.out.println("Beschikbaar: " + beschikbaar);
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Het gemiddelde niet afronden: <code style="color:#845ef7">Gemiddeld gewicht: 12.533333333333333 kg</code>.

---

## 7. Alles samen: het asieloverzicht

**Nakijkpunten**

| Invoer | Wat het overzicht toont |
|---|---|
| <code style="color:#e64980;font-weight:600">5</code> bij de start | <code style="color:#845ef7">Beschikbaar: 5, gereserveerd: 1, geadopteerd: 0</code><br><code style="color:#845ef7">Opbrengst adopties: 0 euro</code><br><code style="color:#845ef7">- Max</code> onder *Moeilijk te plaatsen* |
| na de adoptie van Max en Luna | <code style="color:#845ef7">2. Luna: geadopteerd</code><br><code style="color:#845ef7">3. Max: geadopteerd</code><br><code style="color:#845ef7">Beschikbaar: 3, gereserveerd: 1, geadopteerd: 2</code><br><code style="color:#845ef7">Opbrengst adopties: 165 euro</code><br><code style="color:#845ef7">- niemand</code> |

**Aanpak.** De eerste lus doet het werk per dier: de status met een `if`-ladder, drie tellers, en voor een geadopteerd dier de adoptieprijs, met dezelfde regels als op de fiche. De tweede lus zoekt de beschikbare seniors; of er geen zijn, weet je pas na die lus.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
case 5 -> {
    System.out.println("--- Asieloverzicht ---");
    int beschikbaar = 0;
    int gereserveerd = 0;
    int geadopteerd = 0;
    int opbrengst = 0;
    for (int i = 0; i < namen.length; i++) {
        String toestand;
        if (statussen[i] == 0) {
            toestand = "beschikbaar";
            beschikbaar++;
        } else if (statussen[i] == 1) {
            toestand = "gereserveerd";
            gereserveerd++;
        } else {
            toestand = "geadopteerd";
            geadopteerd++;
            int basis = soortNummers[i] == 1 ? 150 : soortNummers[i] == 2 ? 90 : 40;
            opbrengst += leeftijden[i] >= 96 ? basis / 2 : basis;
        }
        System.out.println((i + 1) + ". " + namen[i] + ": " + toestand);
    }
    System.out.println("Beschikbaar: " + beschikbaar + ", gereserveerd: " + gereserveerd + ", geadopteerd: " + geadopteerd);
    System.out.println("Opbrengst adopties: " + opbrengst + " euro");
    System.out.println("Moeilijk te plaatsen:");
    int moeilijk = 0;
    for (int i = 0; i < namen.length; i++) {
        if (statussen[i] == 0 && leeftijden[i] >= 96) {
            System.out.println("- " + namen[i]);
            moeilijk++;
        }
    }
    if (moeilijk == 0) {
        System.out.println("- niemand");
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De seniorkorting vergeten in de opbrengst. Na de adoptie van Max en Luna toont je programma dan 240 euro in plaats van 165.
> - Alle seniors tonen als *moeilijk te plaatsen*, ook een geadopteerde. Max staat er dan nog, terwijl hij al een thuis heeft.

> [!TIP]
> **Zo kan het beter**
>
> - De adoptieprijs staat op twee plaatsen in je code, met dezelfde getallen. In hoofdstuk 03 wordt het één methode `adoptieprijs(int soortNummer, int leeftijd)`, die de fiche en het overzicht allebei oproepen.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Dierenasiel - onthaal");
        Scanner sc = new Scanner(System.in);

        // het asiel
        String[] namen = {"Rex", "Luna", "Max", "Pluis", "Mimi", "Bobby"};
        int[] soortNummers = {1, 2, 1, 3, 2, 1};
        int[] leeftijden = {30, 7, 110, 14, 50, 4};
        double[] gewichten = {28.5, 3.1, 31.0, 2.2, 4.4, 6.0};
        int[] statussen = {0, 0, 0, 1, 0, 0};    // 0 = beschikbaar, 1 = gereserveerd, 2 = geadopteerd

        int keuze;
        do {
            System.out.println("--- Menu ---");
            System.out.println("1. Overzicht");
            System.out.println("2. Adopteren");
            System.out.println("3. Zoeken tot een maximumleeftijd");
            System.out.println("4. Statistiek");
            System.out.println("5. Asieloverzicht");
            System.out.println("0. Stoppen");
            System.out.print("Keuze: ");
            keuze = sc.nextInt();
            switch (keuze) {
                case 1 -> {
                    for (int i = 0; i < namen.length; i++) {
                        String soort = soortNummers[i] == 1 ? "hond" : soortNummers[i] == 2 ? "kat" : "konijn";
                        String toestand = switch (statussen[i]) {
                            case 0 -> "beschikbaar";
                            case 1 -> "gereserveerd";
                            default -> "geadopteerd";
                        };
                        System.out.println((i + 1) + ". " + namen[i] + " (" + soort + ") - " + leeftijden[i] + " maanden - " + gewichten[i] + " kg - " + toestand);
                    }
                }
                case 2 -> {
                    // het dier
                    System.out.print("Dier (1-6): ");
                    int id = sc.nextInt();
                    if (id < 1 || id > namen.length) {
                        System.out.println("Dier " + id + " bestaat niet.");
                    } else if (statussen[id - 1] == 1) {
                        System.out.println(namen[id - 1] + " is gereserveerd.");
                    } else if (statussen[id - 1] == 2) {
                        System.out.println(namen[id - 1] + " is al geadopteerd.");
                    } else {
                        int i = id - 1;
                        String naam = namen[i];
                        int soortNummer = soortNummers[i];
                        String soort = soortNummer == 1 ? "hond" : soortNummer == 2 ? "kat" : "konijn";
                        int leeftijd = leeftijden[i];
                        double gewicht = gewichten[i];

                        // leeftijd in jaren en maanden
                        int jaren = leeftijd / 12;
                        int maanden = leeftijd % 12;

                        // de adoptieprijs
                        int basisprijs = soortNummer == 1 ? 150 : soortNummer == 2 ? 90 : 40;
                        boolean senior = leeftijd >= 96;
                        int adoptieprijs = senior ? basisprijs / 2 : basisprijs;

                        // levensfase en voer
                        String levensfase = leeftijd < 12 ? "jong" : senior ? "senior" : "volwassen";
                        int voer = (int) Math.ceil(gewicht * 20 / 10) * 10;
                        statussen[i] = 2;

                        // de dierfiche
                        sc.nextLine();                         // de Enter na het nummer
                        System.out.print("Adoptant: ");
                        String adoptant = sc.nextLine();
                        System.out.println("==============================");
                        System.out.println("DIERENASIEL - DIERFICHE");
                        System.out.println("==============================");
                        System.out.println("Naam         : " + naam);
                        System.out.println("Soort        : " + soort);
                        System.out.println("Leeftijd     : " + jaren + " jaar en " + maanden + " maanden (" + levensfase + ")");
                        System.out.println("Gewicht      : " + gewicht + " kg");
                        System.out.println("Voer per dag : " + voer + " gram");
                        System.out.println("Adoptieprijs : " + adoptieprijs + " euro");
                        System.out.println("Adoptant     : " + adoptant);
                        System.out.println("==============================");
                    }
                }
                case 3 -> {
                    System.out.print("Maximumleeftijd in maanden: ");
                    int maximum = sc.nextInt();
                    int gevonden = 0;
                    for (int i = 0; i < namen.length; i++) {
                        if (leeftijden[i] <= maximum) {
                            System.out.println((i + 1) + ". " + namen[i] + " - " + leeftijden[i] + " maanden");
                            gevonden++;
                        }
                    }
                    if (gevonden == 0) {
                        System.out.println("Geen dieren tot " + maximum + " maanden.");
                    } else {
                        System.out.println(gevonden + " dier(en) tot " + maximum + " maanden.");
                    }
                }
                case 4 -> {
                    double som = 0;
                    int jongste = 0;
                    int oudste = 0;
                    int beschikbaar = 0;
                    for (int i = 0; i < namen.length; i++) {
                        som += gewichten[i];
                        if (statussen[i] == 0) {
                            beschikbaar++;
                        }
                        if (leeftijden[i] < leeftijden[jongste]) {
                            jongste = i;
                        }
                        if (leeftijden[i] > leeftijden[oudste]) {
                            oudste = i;
                        }
                    }
                    double gemiddelde = Math.round(som / namen.length * 100) / 100.0;
                    System.out.println("Gemiddeld gewicht: " + gemiddelde + " kg");
                    System.out.println("Jongste: " + namen[jongste] + " (" + leeftijden[jongste] + " maanden)");
                    System.out.println("Oudste: " + namen[oudste] + " (" + leeftijden[oudste] + " maanden)");
                    System.out.println("Beschikbaar: " + beschikbaar);
                }
                case 5 -> {
                    System.out.println("--- Asieloverzicht ---");
                    int beschikbaar = 0;
                    int gereserveerd = 0;
                    int geadopteerd = 0;
                    int opbrengst = 0;
                    for (int i = 0; i < namen.length; i++) {
                        String toestand;
                        if (statussen[i] == 0) {
                            toestand = "beschikbaar";
                            beschikbaar++;
                        } else if (statussen[i] == 1) {
                            toestand = "gereserveerd";
                            gereserveerd++;
                        } else {
                            toestand = "geadopteerd";
                            geadopteerd++;
                            int basis = soortNummers[i] == 1 ? 150 : soortNummers[i] == 2 ? 90 : 40;
                            opbrengst += leeftijden[i] >= 96 ? basis / 2 : basis;
                        }
                        System.out.println((i + 1) + ". " + namen[i] + ": " + toestand);
                    }
                    System.out.println("Beschikbaar: " + beschikbaar + ", gereserveerd: " + gereserveerd + ", geadopteerd: " + geadopteerd);
                    System.out.println("Opbrengst adopties: " + opbrengst + " euro");
                    System.out.println("Moeilijk te plaatsen:");
                    int moeilijk = 0;
                    for (int i = 0; i < namen.length; i++) {
                        if (statussen[i] == 0 && leeftijden[i] >= 96) {
                            System.out.println("- " + namen[i]);
                            moeilijk++;
                        }
                    }
                    if (moeilijk == 0) {
                        System.out.println("- niemand");
                    }
                }
                case 0 -> System.out.println("Tot ziens!");
                default -> System.out.println("Ongeldige keuze.");
            }
        } while (keuze != 0);
    }
}
```

</details>
