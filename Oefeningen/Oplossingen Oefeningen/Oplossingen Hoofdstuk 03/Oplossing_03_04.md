# Oplossing 03_04

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 03</b> &nbsp;·&nbsp; Opsplitsen in methodes &nbsp;·&nbsp; Dierenasiel &nbsp;·&nbsp; bij `Oefening_03_04.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Na elke keuze toont je programma opnieuw het menu; dat staat niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 02_04 staat in de map `Oefening_03_04`. Een adoptie van Luna door Ines Maes toont onder meer <code style="color:#845ef7">Leeftijd     : 0 jaar en 7 maanden (jong)</code> en <code style="color:#845ef7">Adoptieprijs : 90 euro</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_02_04.md` voor de code.

---

## 2. Wat dubbel staat, in een methode

**Nakijkpunten**

- Je programma toont bij dezelfde invoer exact hetzelfde als in punt 1, ook bij <code style="color:#e64980;font-weight:600">9</code>: <code style="color:#845ef7">Ongeldige keuze.</code>
- Na de adoptie van Luna: <code style="color:#845ef7">Opbrengst adopties: 90 euro</code>.
- `main` bevat geen ternary operator meer voor de soort, en geen `switch` meer voor de status.

**Aanpak.** Een `switch` die een waarde geeft, kan je meteen teruggeven met `return switch (...)`. De adoptieprijs heeft twee gegevens nodig, de soort en de leeftijd: dus twee parameters. Waar `main` vroeger zelf rekende, roept het nu de methode op met de waarden van één dier.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String soortNaam(int soortNummer) {
    return switch (soortNummer) {
        case 1 -> "hond";
        case 2 -> "kat";
        default -> "konijn";
    };
}

static int adoptieprijs(int soortNummer, int leeftijd) {
    int basisprijs = soortNummer == 1 ? 150 : soortNummer == 2 ? 90 : 40;
    return leeftijd >= 96 ? basisprijs / 2 : basisprijs;
}

static String levensfase(int leeftijd) {
    if (leeftijd < 12) {
        return "jong";
    } else if (leeftijd >= 96) {
        return "senior";
    }
    return "volwassen";
}

static int voer(double gewicht) {
    return (int) Math.ceil(gewicht * 20 / 10) * 10;
}

// in main, in het asieloverzicht:
opbrengst += adoptieprijs(soortNummers[i], leeftijden[i]);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `case 3 -> "konijn";` in plaats van `default`. De compiler weet niet dat er enkel 1, 2 en 3 bestaan: `the switch expression does not cover all possible input values`.
> - `static` vergeten. De compiler meldt `non-static method ... cannot be referenced from a static context`.

---

## 3. Leeftijd en gewicht als tekst

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> bij de start | <code style="color:#845ef7">1. Rex (hond) - 2 jaar en 6 maanden - 28,5 kg - beschikbaar</code> en <code style="color:#845ef7">6. Bobby (hond) - 4 maanden - 6,0 kg - beschikbaar</code> |
| adoptie van Luna | <code style="color:#845ef7">Leeftijd     : 7 maanden (jong)</code> en <code style="color:#845ef7">Gewicht      : 3,1 kg</code> |
| <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">24</code> | <code style="color:#845ef7">4. Pluis - 1 jaar en 2 maanden</code> |
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Gemiddeld gewicht: 12,5 kg</code> en <code style="color:#845ef7">Oudste: Max (9 jaar en 2 maanden)</code> |

Met een tijdelijke regel in `main` geeft `leeftijdTekst` voor 1, 12, 13 en 24 maanden: `1 maand`, `1 jaar`, `1 jaar en 1 maand` en `2 jaar`.

**Aanpak.** Reken eerst de jaren en de resterende maanden uit. Het woord *maand* of *maanden* hangt af van het getal: dat beslis je één keer, in een variabele. Daarna zijn er drie gevallen: geen jaren, geen resterende maanden, of allebei.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String leeftijdTekst(int maanden) {
    int jaren = maanden / 12;
    int rest = maanden % 12;
    String tekstMaanden = rest + (rest == 1 ? " maand" : " maanden");
    if (jaren == 0) {
        return tekstMaanden;
    } else if (rest == 0) {
        return jaren + " jaar";
    }
    return jaren + " jaar en " + tekstMaanden;
}

static String kg(double gewicht) {
    return String.format("%.1f kg", gewicht);
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `%f` in plaats van `%.1f`. Zonder precisie krijg je zes cijfers na de komma: `12,530000 kg`.
> - `kg(gewicht) + " kg"`. Het woord staat dan twee keer op de fiche.

---

## 4. Het overzicht als tabel

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> | de tabel uit de opgave, met <code style="color:#845ef7"> 4  Pluis  konijn  1 jaar en 2 maanden    2,2 kg  gereserveerd</code> |
| <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">24</code> | de kop, Luna, Pluis en Bobby in de tabel, en <code style="color:#845ef7">3 dier(en) tot 24 maanden.</code> |
| <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">2</code> | geen kop, enkel <code style="color:#845ef7">Geen dieren tot 2 maanden.</code> |

**Aanpak.** `toonRij` krijgt de waarden van één dier, en maakt er zelf tekst van met `soortNaam`, `leeftijdTekst`, `kg` en `statusNaam`. In de opmaak staan daarom enkel `%s`, op de nummer na. Bij het zoeken toon je de kop pas bij het eerste gevonden dier.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static void toonKop() {
    System.out.printf("%2s  %-6s %-7s %-20s %8s  %s%n", "Nr", "Naam", "Soort", "Leeftijd", "Gewicht", "Status");
}

static void toonRij(int nr, String naam, int soortNummer, int leeftijd, double gewicht, int status) {
    System.out.printf("%2d  %-6s %-7s %-20s %8s  %s%n", nr, naam, soortNaam(soortNummer), leeftijdTekst(leeftijd), kg(gewicht), statusNaam(status));
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `%8.1f` voor het gewicht, terwijl je `kg(gewicht)` meegeeft. Een `String` past niet in `%f`: `IllegalFormatConversionException`. Kies één van de twee: tekst met `%s`, of het getal met `%.1f`.

---

## 5. Zoeken op een stukje van de naam

**Nakijkpunten**

| Zoek | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">u</code> | de kop, Luna en Pluis, en <code style="color:#845ef7">2 dier(en) gevonden.</code> |
| <code style="color:#e64980;font-weight:600">BO</code> | <code style="color:#845ef7"> 6  Bobby  hond    4 maanden              6,0 kg  beschikbaar</code> |
| <code style="color:#e64980;font-weight:600">  max </code> | de rij van Max |
| <code style="color:#e64980;font-weight:600">zz</code> | <code style="color:#845ef7">Geen dier met zz in de naam.</code> |
| enkel Enter | <code style="color:#845ef7">Geef minstens een letter in.</code> |

**Aanpak.** Maak beide kanten klein: de naam en de zoektekst. `trim()` haalt de spaties voor en na de zoektekst weg. De extra `sc.nextLine()` haalt de Enter weg die na de keuze bleef staan.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static boolean bevat(String tekst, String stuk) {
    return tekst.toLowerCase().contains(stuk.toLowerCase());
}

// in main:
case 6 -> {
    sc.nextLine();                         // de Enter na de keuze
    System.out.print("Zoek: ");
    String zoek = sc.nextLine().trim();
    if (zoek.isEmpty()) {
        System.out.println("Geef minstens een letter in.");
    } else {
        int gevonden = 0;
        for (int i = 0; i < namen.length; i++) {
            if (bevat(namen[i], zoek)) {
                if (gevonden == 0) {
                    toonKop();
                }
                toonRij(i + 1, namen[i], soortNummers[i], leeftijden[i], gewichten[i], statussen[i]);
                gevonden++;
            }
        }
        // ... de melding of het aantal
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Enkel de zoektekst klein maken. <code style="color:#e64980;font-weight:600">u</code> werkt dan nog, maar bij <code style="color:#e64980;font-weight:600">BO</code> zoek je `bo` in `Bobby`, en dat staat er niet: <code style="color:#845ef7">Geen dier met BO in de naam.</code>

---

## 6. Rekenen in methodes

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> bij de start | <code style="color:#845ef7">Jongste: Bobby (4 maanden)</code>, <code style="color:#845ef7">Oudste: Max (9 jaar en 2 maanden)</code> en <code style="color:#845ef7">Beschikbaar: 5</code> |
| <code style="color:#e64980;font-weight:600">5</code> bij de start | <code style="color:#845ef7">Opbrengst adopties: 0 euro</code> |
| adoptie van Max en van Rex, dan <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Opbrengst adopties: 225 euro</code> |

**Aanpak.** `kleinste` en `grootste` geven de *index* terug: daarmee vind je in `main` de naam en de leeftijd. `opbrengst` heeft drie arrays nodig, want de prijs hangt af van de soort en de leeftijd, en enkel geadopteerde dieren tellen mee. Ze gebruikt `adoptieprijs` uit punt 2: dezelfde regel, op één plaats.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int kleinste(int[] getallen) {
    int index = 0;
    for (int i = 1; i < getallen.length; i++) {
        if (getallen[i] < getallen[index]) {
            index = i;
        }
    }
    return index;
}

static int opbrengst(int[] soortNummers, int[] leeftijden, int[] statussen) {
    int som = 0;
    for (int i = 0; i < statussen.length; i++) {
        if (statussen[i] == 2) {
            som += adoptieprijs(soortNummers[i], leeftijden[i]);
        }
    }
    return som;
}

// in case 4:
double gemiddelde = gemiddelde(gewichten);
int jongste = kleinste(leeftijden);
int oudste = grootste(leeftijden);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `return i;` binnen de `if` van `kleinste`. De methode stopt dan bij het eerste dier dat jonger is dan Rex: <code style="color:#845ef7">Jongste: Luna (7 maanden)</code> in plaats van Bobby.

---

## 7. Alles samen: de dierfiche als tekst

**Nakijkpunten**

| Invoer na <code style="color:#e64980;font-weight:600">7</code> | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">rex</code> | de fiche uit de opgave, met een kader van 42 `=` |
| <code style="color:#e64980;font-weight:600">lu</code> | <code style="color:#845ef7">Meerdere dieren: Luna, Pluis.</code> |
| <code style="color:#e64980;font-weight:600">x</code> | <code style="color:#845ef7">Meerdere dieren: Rex, Max.</code> |
| <code style="color:#e64980;font-weight:600">Pluis</code> | een fiche met <code style="color:#845ef7">Voer     : 50 gram per dag</code> en <code style="color:#845ef7">Status   : gereserveerd</code> |
| <code style="color:#e64980;font-weight:600">zz</code> | <code style="color:#845ef7">Geen dier met zz in de naam.</code> |
| <code style="color:#e64980;font-weight:600">max</code>, na de adoptie van Max | <code style="color:#845ef7">Leeftijd : 9 jaar en 2 maanden (senior)</code>, <code style="color:#845ef7">Prijs    : 75 euro</code>, <code style="color:#845ef7">Status   : geadopteerd</code>, en een kader van 39 `=` |

**Aanpak.** `fiche` maakt eerst elke regel als aparte `String`. Pas als alle regels er zijn, ken je de langste, en daarmee het kader. Dan plakt een `StringBuilder` alles aan elkaar. In `main` zoek je eerst de exacte naam; is die er niet, dan doorloop je alle namen met `bevat`, en houd je bij hoeveel er passen.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String fiche(String naam, int soortNummer, int leeftijd, double gewicht, int status) {
    String titel = naam.toUpperCase();
    String regelSoort = "Soort    : " + soortNaam(soortNummer);
    String regelLeeftijd = "Leeftijd : " + leeftijdTekst(leeftijd) + " (" + levensfase(leeftijd) + ")";
    String regelGewicht = "Gewicht  : " + kg(gewicht);
    String regelVoer = "Voer     : " + voer(gewicht) + " gram per dag";
    String regelPrijs = "Prijs    : " + adoptieprijs(soortNummer, leeftijd) + " euro";
    String regelStatus = "Status   : " + statusNaam(status);
    int breedte = titel.length();
    breedte = Math.max(breedte, regelSoort.length());
    breedte = Math.max(breedte, regelLeeftijd.length());
    // ... gewicht, voer, prijs en status op dezelfde manier
    String kader = "=".repeat(breedte);
    StringBuilder sb = new StringBuilder();
    sb.append(kader).append("\n");
    sb.append(titel).append("\n");
    sb.append(kader).append("\n");
    sb.append(regelSoort).append("\n");
    // ... de andere regels
    sb.append(kader).append("\n");
    return sb.toString();
}
```

Het zoeken in `main` is hetzelfde als in `Oplossing_03_01.md`, met `Meerdere dieren` en `Geen dier met ... in de naam.` als meldingen.

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De leeftijdsregel vergeten bij de breedte. Het kader van Rex is dan 27 tekens breed, terwijl <code style="color:#845ef7">Leeftijd : 2 jaar en 6 maanden (volwassen)</code> er 42 heeft.

> [!TIP]
> **Zo kan het beter**
>
> - Zeven keer `Math.max` voor zeven regels: met een array van regels doe je het in een lus, en kan je geen regel meer vergeten.
> - De methodes krijgen vijf parameters die samen één dier zijn. In hoofdstuk 04 wordt dat één object `Dier`, met een methode `fiche()` die zelf aan zijn gegevens kan.

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
            toonMenu();
            keuze = sc.nextInt();
            switch (keuze) {
                case 1 -> {
                    toonKop();
                    for (int i = 0; i < namen.length; i++) {
                        toonRij(i + 1, namen[i], soortNummers[i], leeftijden[i], gewichten[i], statussen[i]);
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
                        int leeftijd = leeftijden[i];
                        double gewicht = gewichten[i];
                        int adoptieprijs = adoptieprijs(soortNummers[i], leeftijd);
                        statussen[i] = 2;

                        // de dierfiche
                        sc.nextLine();                         // de Enter na het nummer
                        System.out.print("Adoptant: ");
                        String adoptant = sc.nextLine();
                        System.out.println("==============================");
                        System.out.println("DIERENASIEL - DIERFICHE");
                        System.out.println("==============================");
                        System.out.println("Naam         : " + naam);
                        System.out.println("Soort        : " + soortNaam(soortNummers[i]));
                        System.out.println("Leeftijd     : " + leeftijdTekst(leeftijd) + " (" + levensfase(leeftijd) + ")");
                        System.out.println("Gewicht      : " + kg(gewicht));
                        System.out.println("Voer per dag : " + voer(gewicht) + " gram");
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
                            if (gevonden == 0) {
                                toonKop();
                            }
                            toonRij(i + 1, namen[i], soortNummers[i], leeftijden[i], gewichten[i], statussen[i]);
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
                    int beschikbaar = 0;
                    for (int i = 0; i < namen.length; i++) {
                        if (statussen[i] == 0) {
                            beschikbaar++;
                        }
                    }
                    double gemiddelde = gemiddelde(gewichten);
                    int jongste = kleinste(leeftijden);
                    int oudste = grootste(leeftijden);
                    System.out.println("Gemiddeld gewicht: " + kg(gemiddelde));
                    System.out.println("Jongste: " + namen[jongste] + " (" + leeftijdTekst(leeftijden[jongste]) + ")");
                    System.out.println("Oudste: " + namen[oudste] + " (" + leeftijdTekst(leeftijden[oudste]) + ")");
                    System.out.println("Beschikbaar: " + beschikbaar);
                }
                case 5 -> {
                    System.out.println("--- Asieloverzicht ---");
                    int beschikbaar = 0;
                    int gereserveerd = 0;
                    int geadopteerd = 0;
                    for (int i = 0; i < namen.length; i++) {
                        if (statussen[i] == 0) {
                            beschikbaar++;
                        } else if (statussen[i] == 1) {
                            gereserveerd++;
                        } else {
                            geadopteerd++;
                        }
                        System.out.println((i + 1) + ". " + namen[i] + ": " + statusNaam(statussen[i]));
                    }
                    System.out.println("Beschikbaar: " + beschikbaar + ", gereserveerd: " + gereserveerd + ", geadopteerd: " + geadopteerd);
                    System.out.println("Opbrengst adopties: " + opbrengst(soortNummers, leeftijden, statussen) + " euro");
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
                case 6 -> {
                    sc.nextLine();                         // de Enter na de keuze
                    System.out.print("Zoek: ");
                    String zoek = sc.nextLine().trim();
                    if (zoek.isEmpty()) {
                        System.out.println("Geef minstens een letter in.");
                    } else {
                        int gevonden = 0;
                        for (int i = 0; i < namen.length; i++) {
                            if (bevat(namen[i], zoek)) {
                                if (gevonden == 0) {
                                    toonKop();
                                }
                                toonRij(i + 1, namen[i], soortNummers[i], leeftijden[i], gewichten[i], statussen[i]);
                                gevonden++;
                            }
                        }
                        if (gevonden == 0) {
                            System.out.println("Geen dier met " + zoek + " in de naam.");
                        } else {
                            System.out.println(gevonden + " dier(en) gevonden.");
                        }
                    }
                }
                case 7 -> {
                    sc.nextLine();                         // de Enter na de keuze
                    System.out.print("Dier (naam of een stuk ervan): ");
                    String zoek = sc.nextLine().trim();
                    int i = zoekIndex(namen, zoek);
                    if (i == -1) {                         // geen exacte naam: zoek een stuk
                        int gevonden = 0;
                        StringBuilder lijst = new StringBuilder();
                        for (int k = 0; k < namen.length; k++) {
                            if (bevat(namen[k], zoek)) {
                                if (gevonden > 0) {
                                    lijst.append(", ");
                                }
                                lijst.append(namen[k]);
                                gevonden++;
                                i = k;
                            }
                        }
                        if (gevonden == 0) {
                            System.out.println("Geen dier met " + zoek + " in de naam.");
                        } else if (gevonden > 1) {
                            System.out.println("Meerdere dieren: " + lijst + ".");
                            i = -1;
                        }
                    }
                    if (i != -1) {
                        System.out.print(fiche(namen[i], soortNummers[i], leeftijden[i], gewichten[i], statussen[i]));
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
        System.out.println("1. Overzicht");
        System.out.println("2. Adopteren");
        System.out.println("3. Zoeken tot een maximumleeftijd");
        System.out.println("4. Statistiek");
        System.out.println("5. Asieloverzicht");
        System.out.println("6. Zoeken op naam");
        System.out.println("7. Dierfiche");
        System.out.println("0. Stoppen");
        System.out.print("Keuze: ");
    }

    // van nummer naar woord
    static String soortNaam(int soortNummer) {
        return switch (soortNummer) {
            case 1 -> "hond";
            case 2 -> "kat";
            default -> "konijn";
        };
    }

    static String statusNaam(int status) {
        return switch (status) {
            case 0 -> "beschikbaar";
            case 1 -> "gereserveerd";
            default -> "geadopteerd";
        };
    }

    // de vaste regels van het asiel
    static int adoptieprijs(int soortNummer, int leeftijd) {
        int basisprijs = soortNummer == 1 ? 150 : soortNummer == 2 ? 90 : 40;
        return leeftijd >= 96 ? basisprijs / 2 : basisprijs;
    }

    static String levensfase(int leeftijd) {
        if (leeftijd < 12) {
            return "jong";
        } else if (leeftijd >= 96) {
            return "senior";
        }
        return "volwassen";
    }

    static int voer(double gewicht) {
        return (int) Math.ceil(gewicht * 20 / 10) * 10;
    }

    // leeftijd en gewicht als tekst
    static String leeftijdTekst(int maanden) {
        int jaren = maanden / 12;
        int rest = maanden % 12;
        String tekstMaanden = rest + (rest == 1 ? " maand" : " maanden");
        if (jaren == 0) {
            return tekstMaanden;
        } else if (rest == 0) {
            return jaren + " jaar";
        }
        return jaren + " jaar en " + tekstMaanden;
    }

    static String kg(double gewicht) {
        return String.format("%.1f kg", gewicht);
    }

    // het overzicht als tabel
    static void toonKop() {
        System.out.printf("%2s  %-6s %-7s %-20s %8s  %s%n", "Nr", "Naam", "Soort", "Leeftijd", "Gewicht", "Status");
    }

    static void toonRij(int nr, String naam, int soortNummer, int leeftijd, double gewicht, int status) {
        System.out.printf("%2d  %-6s %-7s %-20s %8s  %s%n", nr, naam, soortNaam(soortNummer), leeftijdTekst(leeftijd), kg(gewicht), statusNaam(status));
    }

    // zoeken zonder hoofdletters
    static boolean bevat(String tekst, String stuk) {
        return tekst.toLowerCase().contains(stuk.toLowerCase());
    }

    // rekenen over het asiel
    static double gemiddelde(double[] getallen) {
        double som = 0;
        for (double g : getallen) {
            som += g;
        }
        return Math.round(som / getallen.length * 100) / 100.0;
    }

    static int kleinste(int[] getallen) {
        int index = 0;
        for (int i = 1; i < getallen.length; i++) {
            if (getallen[i] < getallen[index]) {
                index = i;
            }
        }
        return index;
    }

    static int grootste(int[] getallen) {
        int index = 0;
        for (int i = 1; i < getallen.length; i++) {
            if (getallen[i] > getallen[index]) {
                index = i;
            }
        }
        return index;
    }

    static int opbrengst(int[] soortNummers, int[] leeftijden, int[] statussen) {
        int som = 0;
        for (int i = 0; i < statussen.length; i++) {
            if (statussen[i] == 2) {
                som += adoptieprijs(soortNummers[i], leeftijden[i]);
            }
        }
        return som;
    }

    // een dier op zijn volledige naam, of -1
    static int zoekIndex(String[] namen, String naam) {
        for (int i = 0; i < namen.length; i++) {
            if (namen[i].equalsIgnoreCase(naam)) {
                return i;
            }
        }
        return -1;
    }

    // de dierfiche als tekst: het kader is zo breed als de langste regel
    static String fiche(String naam, int soortNummer, int leeftijd, double gewicht, int status) {
        String titel = naam.toUpperCase();
        String regelSoort = "Soort    : " + soortNaam(soortNummer);
        String regelLeeftijd = "Leeftijd : " + leeftijdTekst(leeftijd) + " (" + levensfase(leeftijd) + ")";
        String regelGewicht = "Gewicht  : " + kg(gewicht);
        String regelVoer = "Voer     : " + voer(gewicht) + " gram per dag";
        String regelPrijs = "Prijs    : " + adoptieprijs(soortNummer, leeftijd) + " euro";
        String regelStatus = "Status   : " + statusNaam(status);
        int breedte = titel.length();
        breedte = Math.max(breedte, regelSoort.length());
        breedte = Math.max(breedte, regelLeeftijd.length());
        breedte = Math.max(breedte, regelGewicht.length());
        breedte = Math.max(breedte, regelVoer.length());
        breedte = Math.max(breedte, regelPrijs.length());
        breedte = Math.max(breedte, regelStatus.length());
        String kader = "=".repeat(breedte);
        StringBuilder sb = new StringBuilder();
        sb.append(kader).append("\n");
        sb.append(titel).append("\n");
        sb.append(kader).append("\n");
        sb.append(regelSoort).append("\n");
        sb.append(regelLeeftijd).append("\n");
        sb.append(regelGewicht).append("\n");
        sb.append(regelVoer).append("\n");
        sb.append(regelPrijs).append("\n");
        sb.append(regelStatus).append("\n");
        sb.append(kader).append("\n");
        return sb.toString();
    }
}
```

</details>
