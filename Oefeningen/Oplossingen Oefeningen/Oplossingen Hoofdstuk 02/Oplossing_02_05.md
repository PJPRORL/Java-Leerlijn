# Oplossing 02_05

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 02</b> &nbsp;·&nbsp; Eerst controleren, dan veranderen &nbsp;·&nbsp; Fietsverhuur &nbsp;·&nbsp; bij `Oefening_02_05.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21. Een nieuw contractnummer is willekeurig: jouw nummer is anders. Vanaf punt 3 toont je programma na elke keuze opnieuw het menu.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 01_05 staat in de map `Oefening_02_05`. Met type <code style="color:#e64980;font-weight:600">2</code>, van <code style="color:#e64980;font-weight:600">9</code>:<code style="color:#e64980;font-weight:600">45</code> tot <code style="color:#e64980;font-weight:600">13</code>:<code style="color:#e64980;font-weight:600">10</code> toont het contract onder meer <code style="color:#845ef7">Duur          : 3 u 25 min (4 begonnen uren)</code> en <code style="color:#845ef7">Huurprijs     : 24 euro</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_01_05.md` voor de code.

---

## 2. De vloot

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">1. stadsfiets Gazelle - beschikbaar</code> tot en met <code style="color:#845ef7">6. bakfiets Urban Arrow - in herstelling</code>, met <code style="color:#845ef7">4. e-bike Cube - verhuurd</code> |
| <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">9</code> <code style="color:#e64980;font-weight:600">45</code> <code style="color:#e64980;font-weight:600">13</code> <code style="color:#e64980;font-weight:600">10</code> | het contract van punt 1, met <code style="color:#845ef7">Fiets         : e-bike Stromer (6 euro per uur, max 30 euro)</code> |

**Aanpak.** Drie parallelle arrays. Het typenummer haal je uit de array in plaats van uit de invoer; de rest van 01_05 rekent ermee verder zoals vroeger. Het woord voor de status kies je met een `switch` die een waarde teruggeeft (leerboek 1.5).

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int[] typeNummers = {1, 1, 2, 2, 3, 3};
String[] merken = {"Gazelle", "Oxford", "Stromer", "Cube", "Babboe", "Urban Arrow"};
int[] statussen = {0, 0, 0, 1, 0, 2};    // 0 = beschikbaar, 1 = verhuurd, 2 = in herstelling

for (int i = 0; i < merken.length; i++) {
    String type = typeNummers[i] == 1 ? "stadsfiets" : typeNummers[i] == 2 ? "e-bike" : "bakfiets";
    String toestand = switch (statussen[i]) {
        case 0 -> "beschikbaar";
        case 1 -> "verhuurd";
        default -> "in herstelling";
    };
    System.out.println((i + 1) + ". " + type + " " + merken[i] + " - " + toestand);
}
```

</details>

---

## 3. Verhuren en terugbrengen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">9</code> <code style="color:#e64980;font-weight:600">45</code> | <code style="color:#845ef7">Fiets 3 verhuurd om 9:45, contract FV-2026-</code> met vier cijfers |
| daarna <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">3. e-bike Stromer - verhuurd</code> |
| daarna <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">13</code> <code style="color:#e64980;font-weight:600">10</code> | het contract met hetzelfde contractnummer, <code style="color:#845ef7">Vertrek       : 9:45</code> en <code style="color:#845ef7">Huurprijs     : 24 euro</code> |
| daarna <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">3. e-bike Stromer - beschikbaar</code> |

**Aanpak.** Wat bij één fiets hoort en tussen twee keuzes van het menu moet blijven bestaan, krijgt een array: het vertrek in minuten en het contractnummer, naast de status. Verhuren vult die plaatsen; terugbrengen leest ze. Het contract van 01_05 schuift in `case 3`; enkel het vertrek en het contractnummer komen nu uit de arrays, in plaats van uit de invoer.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int[] vertrekken = {0, 0, 0, 8 * 60 + 30, 0, 0};    // minuten na middernacht
String[] contracten = {"", "", "", "FV-2026-4821", "", ""};
// ... in case 2, verhuren:
System.out.print("Fiets (1-6): ");
int i = sc.nextInt() - 1;
System.out.print("Vertrek uur: ");
int uur = sc.nextInt();
System.out.print("Vertrek minuut: ");
int minuut = sc.nextInt();
statussen[i] = 1;
vertrekken[i] = uur * 60 + minuut;
contracten[i] = "FV-" + JAAR + "-" + (random.nextInt(9999 - 1000 + 1) + 1000);
System.out.println("Fiets " + (i + 1) + " verhuurd om " + uur + ":" + (minuut < 10 ? "0" : "") + minuut + ", contract " + contracten[i] + ".");
// ... in case 3, terugbrengen:
int vertrekUur = vertrekken[i] / 60;
int vertrekMinuut = vertrekken[i] % 60;
String contract = contracten[i];
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Een nieuw contractnummer maken bij het terugbrengen. Het contract hoort bij de verhuring: bij vertrek en terugkomst moet het hetzelfde zijn.
> - De beginwaarden van fiets 4 vergeten. Wie fiets 4 terugbrengt, krijgt dan een vertrek om 0:00 en een huurprijs van een volle dag.

---

## 4. Een verhuring weigeren

**Nakijkpunten**

| Invoer na <code style="color:#e64980;font-weight:600">2</code> | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code> | <code style="color:#845ef7">Fiets 9 bestaat niet.</code> |
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Fiets 4 is al verhuurd.</code> |
| <code style="color:#e64980;font-weight:600">6</code> | <code style="color:#845ef7">Fiets 6 is in herstelling.</code> |
| <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">3</code> | <code style="color:#845ef7">Klant 3 bestaat niet.</code> |
| <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">9</code> <code style="color:#e64980;font-weight:600">0</code>, daarna <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">2</code> | eerst <code style="color:#845ef7">Fiets 1 verhuurd aan Tom Wouters om 9:00, contract FV-2026-</code> met vier cijfers en <code style="color:#845ef7">Nu te betalen: 50 euro waarborg.</code>, daarna <code style="color:#845ef7">Tom Wouters heeft al 2 fietsen.</code> |

**Aanpak.** Eerst de fiets: een ladder voor bestaan, verhuurd en herstelling. Pas als de fiets beschikbaar is, vraag je de klant, en tel je met een lus hoeveel fietsen die al heeft: in de array `huurders` staat per fiets de index van de klant, of -1. Pas als ook dat klopt, vraag je het vertrek en verander je de arrays.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String[] klanten = {"Sara Janssens", "Tom Wouters"};
int[] huurders = {-1, -1, -1, 1, -1, -1};    // -1 = niemand
// ... in case 2:
System.out.print("Fiets (1-6): ");
int id = sc.nextInt();
if (id < 1 || id > merken.length) {
    System.out.println("Fiets " + id + " bestaat niet.");
} else if (statussen[id - 1] == 1) {
    System.out.println("Fiets " + id + " is al verhuurd.");
} else if (statussen[id - 1] == 2) {
    System.out.println("Fiets " + id + " is in herstelling.");
} else {
    System.out.print("Klant (1 = " + klanten[0] + ", 2 = " + klanten[1] + "): ");
    int klant = sc.nextInt() - 1;
    int inHuur = 0;
    for (int f = 0; f < huurders.length; f++) {
        if (huurders[f] == klant) {
            inHuur++;
        }
    }
    if (klant < 0 || klant >= klanten.length) {
        System.out.println("Klant " + (klant + 1) + " bestaat niet.");
    } else if (inHuur >= MAX_PER_KLANT) {
        System.out.println(klanten[klant] + " heeft al " + MAX_PER_KLANT + " fietsen.");
    } else {
        // ... het vertrek, en pas nu de arrays aanpassen
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `inHuur > MAX_PER_KLANT` in plaats van `>=`. Tom heeft al 2 fietsen en krijgt er dan toch een derde.
> - De status op verhuurd zetten voor de controle van de klant. Weigert de klant, dan staat de fiets toch als verhuurd in de vloot.

---

## 5. Een terugbrenging weigeren

**Nakijkpunten**

| Invoer na <code style="color:#e64980;font-weight:600">3</code> | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code> | <code style="color:#845ef7">Fiets 9 bestaat niet.</code> |
| <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Fiets 1 is niet verhuurd.</code> |
| <code style="color:#e64980;font-weight:600">4</code> <code style="color:#e64980;font-weight:600">8</code> <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">De terugkomst ligt niet na het vertrek.</code> |
| <code style="color:#e64980;font-weight:600">4</code> <code style="color:#e64980;font-weight:600">17</code> <code style="color:#e64980;font-weight:600">45</code> | het contract met <code style="color:#845ef7">Klant         : Tom Wouters</code>, <code style="color:#845ef7">Vertrek       : 8:30</code>, <code style="color:#845ef7">Duur          : 9 u 15 min (10 begonnen uren)</code> en <code style="color:#845ef7">Huurprijs     : 30 euro</code> |

**Aanpak.** Dezelfde aanpak als bij verhuren: eerst de fiets controleren, dan de tijd. De duur kan je pas uitrekenen als je de terugkomst kent; is ze 0 of negatief, dan stopt de terugbrenging daar. Het contract en de wijziging van de status staan helemaal binnenin.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Fiets (1-6): ");
int id = sc.nextInt();
if (id < 1 || id > merken.length) {
    System.out.println("Fiets " + id + " bestaat niet.");
} else if (statussen[id - 1] != 1) {
    System.out.println("Fiets " + id + " is niet verhuurd.");
} else {
    int i = id - 1;
    // ... type en tarieven, vertrek uit de array, terugkomst inlezen
    int duur = terug - vertrek;
    if (duur <= 0) {
        System.out.println("De terugkomst ligt niet na het vertrek.");
    } else {
        // ... de berekeningen en het contract van 01_05
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Enkel weigeren als `statussen[id - 1] == 0`. Een fiets in herstelling (status 2) kan je dan terugbrengen, en ze wordt daarna beschikbaar.
> - Een negatieve duur niet opvangen. Met een terugkomst om 8:00 rekent je programma dan met -30 minuten, en toont het een huurprijs van 0 euro.

---

## 6. Overzicht per klant

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> bij de start | <code style="color:#845ef7">Sara Janssens:</code><br><code style="color:#845ef7">- geen fietsen</code><br><code style="color:#845ef7">Tom Wouters:</code><br><code style="color:#845ef7">- fiets 4, e-bike Cube, sinds 8:30</code> |
| na een verhuring van fiets 3 aan Sara om 9:45 | onder Sara: <code style="color:#845ef7">- fiets 3, e-bike Stromer, sinds 9:45</code> |

**Aanpak.** Twee lussen in elkaar: per klant overloop je alle fietsen, en toon je die met de index van die klant in `huurders`. De teller staat binnen de buitenste lus, zodat hij voor elke klant opnieuw op 0 begint.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
case 4 -> {
    for (int k = 0; k < klanten.length; k++) {
        System.out.println(klanten[k] + ":");
        int aantal = 0;
        for (int f = 0; f < merken.length; f++) {
            if (huurders[f] == k) {
                String type = typeNummers[f] == 1 ? "stadsfiets" : typeNummers[f] == 2 ? "e-bike" : "bakfiets";
                System.out.println("- fiets " + (f + 1) + ", " + type + " " + merken[f] + ", sinds " + vertrekken[f] / 60 + ":" + (vertrekken[f] % 60 < 10 ? "0" : "") + vertrekken[f] % 60);
                aantal++;
            }
        }
        if (aantal == 0) {
            System.out.println("- geen fietsen");
        }
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De teller voor de buitenste lus declareren. Heeft Sara een fiets en Tom geen, dan toont je programma bij Tom niets: de teller staat nog op 1 van Sara.

---

## 7. Alles samen: een fiets terugbrengen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| fiets <code style="color:#e64980;font-weight:600">4</code> terug om <code style="color:#e64980;font-weight:600">17</code>:<code style="color:#e64980;font-weight:600">45</code>, <code style="color:#e64980;font-weight:600">n</code>, dan <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Niet betaald: fiets 4 blijft verhuurd.</code>, en onder Tom nog altijd <code style="color:#845ef7">- fiets 4, e-bike Cube, sinds 8:30</code> |
| daarna dezelfde terugbrenging met <code style="color:#e64980;font-weight:600">j</code>, dan <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Fiets 4 is terug en weer beschikbaar.</code>, en onder Tom <code style="color:#845ef7">- geen fietsen</code> |
| fiets 5 aan Sara om 14:05, terug om 20:30, <code style="color:#e64980;font-weight:600">j</code> | <code style="color:#845ef7">Huurprijs     : 40 euro</code><br><code style="color:#845ef7">Boete         : 10 euro</code><br><code style="color:#845ef7">Bij terugkomst: 50 euro (waarborg terug)</code> |

**Aanpak.** Het contract toont wat er betaald moet worden; dat verandert nog niets. Pas in de tak van de betaling worden status en huurder teruggezet. Bij *n* staat alles nog zoals het was, want er is nog niets veranderd. Je hoeft dus niets terug te draaien.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
// ... na het contract:
System.out.print("Betaald (j/n): ");
char betaald = sc.next().charAt(0);
if (betaald == 'j') {
    statussen[i] = 0;
    huurders[i] = -1;
    System.out.println("Fiets " + (i + 1) + " is terug en weer beschikbaar.");
} else {
    System.out.println("Niet betaald: fiets " + (i + 1) + " blijft verhuurd.");
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De status al terugzetten voor de vraag, zoals in punt 3 tot 6. Na een *n* is de fiets dan beschikbaar, terwijl de klant nog niet betaald heeft.
> - Enkel de status terugzetten, en de huurder vergeten. Het overzicht toont fiets 4 dan nog altijd onder Tom, en voor zijn maximum van 2 telt ze nog mee.

> [!TIP]
> **Zo kan het beter**
>
> - Het type van een fiets kies je op drie plaatsen met dezelfde ternary operator. In hoofdstuk 03 wordt dat één methode.
> - De status is een getal: 0, 1 of 2. Schrijf je per vergissing `statussen[i] = 3`, dan geeft Java geen fout. In hoofdstuk 06 wordt de status een enum `FietsStatus`, en kan dat niet meer.

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
        String[] klanten = {"Sara Janssens", "Tom Wouters"};
        int[] huurders = {-1, -1, -1, 1, -1, -1};    // -1 = niemand

        int keuze;
        do {
            System.out.println("--- Menu ---");
            System.out.println("1. Vloot");
            System.out.println("2. Verhuren");
            System.out.println("3. Terugbrengen");
            System.out.println("4. Overzicht per klant");
            System.out.println("0. Stoppen");
            System.out.print("Keuze: ");
            keuze = sc.nextInt();
            switch (keuze) {
                case 1 -> {
                    for (int i = 0; i < merken.length; i++) {
                        String type = typeNummers[i] == 1 ? "stadsfiets" : typeNummers[i] == 2 ? "e-bike" : "bakfiets";
                        String toestand = switch (statussen[i]) {
                            case 0 -> "beschikbaar";
                            case 1 -> "verhuurd";
                            default -> "in herstelling";
                        };
                        System.out.println((i + 1) + ". " + type + " " + merken[i] + " - " + toestand);
                    }
                }
                case 2 -> {
                    System.out.print("Fiets (1-6): ");
                    int id = sc.nextInt();
                    if (id < 1 || id > merken.length) {
                        System.out.println("Fiets " + id + " bestaat niet.");
                    } else if (statussen[id - 1] == 1) {
                        System.out.println("Fiets " + id + " is al verhuurd.");
                    } else if (statussen[id - 1] == 2) {
                        System.out.println("Fiets " + id + " is in herstelling.");
                    } else {
                        System.out.print("Klant (1 = " + klanten[0] + ", 2 = " + klanten[1] + "): ");
                        int klant = sc.nextInt() - 1;
                        int inHuur = 0;
                        for (int f = 0; f < huurders.length; f++) {
                            if (huurders[f] == klant) {
                                inHuur++;
                            }
                        }
                        if (klant < 0 || klant >= klanten.length) {
                            System.out.println("Klant " + (klant + 1) + " bestaat niet.");
                        } else if (inHuur >= MAX_PER_KLANT) {
                            System.out.println(klanten[klant] + " heeft al " + MAX_PER_KLANT + " fietsen.");
                        } else {
                            System.out.print("Vertrek uur: ");
                            int uur = sc.nextInt();
                            System.out.print("Vertrek minuut: ");
                            int minuut = sc.nextInt();
                            int i = id - 1;
                            statussen[i] = 1;
                            huurders[i] = klant;
                            vertrekken[i] = uur * 60 + minuut;
                            contracten[i] = "FV-" + JAAR + "-" + (random.nextInt(9999 - 1000 + 1) + 1000);
                            int waarborg = typeNummers[i] == 1 ? 50 : typeNummers[i] == 2 ? 150 : 100;
                            System.out.println("Fiets " + id + " verhuurd aan " + klanten[klant] + " om " + uur + ":" + (minuut < 10 ? "0" : "") + minuut + ", contract " + contracten[i] + ".");
                            System.out.println("Nu te betalen: " + waarborg + " euro waarborg.");
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
                        String type = typeNummer == 1 ? "stadsfiets" : typeNummer == 2 ? "e-bike" : "bakfiets";
                        int uurtarief = typeNummer == 1 ? 3 : typeNummer == 2 ? 6 : 8;
                        int dagmaximum = typeNummer == 1 ? 15 : typeNummer == 2 ? 30 : 40;
                        int waarborg = typeNummer == 1 ? 50 : typeNummer == 2 ? 150 : 100;

                        // de huurduur
                        int vertrekUur = vertrekken[i] / 60;
                        int vertrekMinuut = vertrekken[i] % 60;
                        System.out.print("Terug uur: ");
                        int terugUur = sc.nextInt();
                        System.out.print("Terug minuut: ");
                        int terugMinuut = sc.nextInt();
                        int vertrek = vertrekUur * 60 + vertrekMinuut;
                        int terug = terugUur * 60 + terugMinuut;
                        int duur = terug - vertrek;
                        if (duur <= 0) {
                            System.out.println("De terugkomst ligt niet na het vertrek.");
                        } else {
                            // het contractnummer
                            String contract = contracten[i];

                            // begonnen uren
                            int begonnenUren = (duur + 59) / 60;

                            // de huurprijs
                            int huurprijs = Math.min(begonnenUren * uurtarief, dagmaximum);

                            // te laat terug
                            boolean teLaat = terug > SLUITINGSUUR * 60;
                            int boete = teLaat ? BOETE : 0;
                            int bijTerugkomst = huurprijs + boete;

                            // het huurcontract
                            String vertrekTijd = vertrekUur + ":" + (vertrekMinuut < 10 ? "0" : "") + vertrekMinuut;
                            String terugTijd = terugUur + ":" + (terugMinuut < 10 ? "0" : "") + terugMinuut;
                            System.out.println("==============================");
                            System.out.println("FIETSVERHUUR - HUURCONTRACT");
                            System.out.println("==============================");
                            System.out.println("Contract      : " + contract);
                            System.out.println("Klant         : " + klanten[huurders[i]]);
                            System.out.println("Fiets         : " + type + " " + merken[i] + " (" + uurtarief + " euro per uur, max " + dagmaximum + " euro)");
                            System.out.println("Vertrek       : " + vertrekTijd);
                            System.out.println("Terug         : " + terugTijd);
                            System.out.println("Duur          : " + duur / 60 + " u " + duur % 60 + " min (" + begonnenUren + " begonnen uren)");
                            System.out.println("Huurprijs     : " + huurprijs + " euro");
                            System.out.println("Boete         : " + boete + " euro");
                            System.out.println("------------------------------");
                            System.out.println("Bij vertrek   : " + waarborg + " euro (waarborg)");
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
                        System.out.println(klanten[k] + ":");
                        int aantal = 0;
                        for (int f = 0; f < merken.length; f++) {
                            if (huurders[f] == k) {
                                String type = typeNummers[f] == 1 ? "stadsfiets" : typeNummers[f] == 2 ? "e-bike" : "bakfiets";
                                System.out.println("- fiets " + (f + 1) + ", " + type + " " + merken[f] + ", sinds " + vertrekken[f] / 60 + ":" + (vertrekken[f] % 60 < 10 ? "0" : "") + vertrekken[f] % 60);
                                aantal++;
                            }
                        }
                        if (aantal == 0) {
                            System.out.println("- geen fietsen");
                        }
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
