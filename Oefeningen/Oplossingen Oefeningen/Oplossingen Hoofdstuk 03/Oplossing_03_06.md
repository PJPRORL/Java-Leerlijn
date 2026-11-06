# Oplossing 03_06

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 03</b> &nbsp;·&nbsp; Codes, overloading en recursie &nbsp;·&nbsp; Concertzaal &nbsp;·&nbsp; bij `Oefening_03_06.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Het zaalplan bij de start staat niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 02_06 staat in de map `Oefening_03_06`. Met concert <code style="color:#e64980;font-weight:600">1</code>, plaats <code style="color:#e64980;font-weight:600">33</code>, leeftijd <code style="color:#e64980;font-weight:600">40</code> en een groep van <code style="color:#e64980;font-weight:600">4</code> toont het groepsticket onder meer <code style="color:#845ef7">Plaatsen     : A9 tot A12 (categorie 1)</code> en <code style="color:#845ef7">Te betalen   : 244,00 euro</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_02_06.md` voor de code.

---

## 2. Wat dubbel staat, in een methode

**Nakijkpunten**

- Je programma toont bij dezelfde invoer exact hetzelfde als in punt 1, ook het zaalplan op het einde met <code style="color:#845ef7">Vrij: 145 van 200</code>.
- In je code staat `% 100 < 10` nergens meer, en de categorie van een rij één keer.

**Aanpak.** Wat een methode nodig heeft, krijgt ze als parameter: `toonZaalplan` krijgt het zaalplan, `korting` de basisprijs en de leeftijd. Wat in alle methodes hetzelfde is, de constanten van de zaal, zet je bovenaan de klasse. In de lus voor de groep wordt de korting één regel: `groepKorting += korting(groepBasis, sc.nextInt());`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    static final int STOELEN_PER_RIJ = 20;
    static final int RIJEN = 10;
    static final int SERVICEKOST = 250;

    // ... main

    static String euro(int cent) {
        return String.format("%d,%02d euro", cent / 100, cent % 100);
    }

    static int korting(int basisprijs, int leeftijd) {
        return leeftijd < 26 ? (basisprijs * 20 + 50) / 100 : 0;
    }

    static int btw(int ticket) {
        return ticket - (ticket * 100 + 53) / 106;
    }

    static int categorie(int rijIndex) {
        return rijIndex <= 2 ? 1 : rijIndex <= 6 ? 2 : 3;
    }

    static int basisprijs(int categorie) {
        return categorie == 1 ? 6500 : categorie == 2 ? 4900 : 3500;
    }

    static String plaatsCode(int rijIndex, int stoel) {
        return (char) ('A' + rijIndex) + "" + stoel;
    }

    static void toonZaalplan(boolean[][] bezet) {
        System.out.println("   12345678901234567890");
        int vrij = 0;
        for (int r = 0; r < RIJEN; r++) {
            String lijn = (char) ('A' + r) + "  ";
            for (int s = 0; s < STOELEN_PER_RIJ; s++) {
                if (bezet[r][s]) {
                    lijn += "X";
                } else {
                    lijn += ".";
                    vrij++;
                }
            }
            System.out.println(lijn);
        }
        System.out.println("Vrij: " + vrij + " van " + RIJEN * STOELEN_PER_RIJ);
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De constanten in `main` laten staan. De methodes zien ze niet: `cannot find symbol`.
> - `plaatsCode` zonder `+ ""`. Een `char` plus een `int` is een `int`, en geen tekst: `incompatible types: int cannot be converted to String`.

---

## 3. Een plaats als code

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">K3</code> | <code style="color:#845ef7">Geen geldige plaats: K3.</code>, en de vraag opnieuw |
| <code style="color:#e64980;font-weight:600">c0</code> | <code style="color:#845ef7">Geen geldige plaats: c0.</code> |
| <code style="color:#e64980;font-weight:600">C12</code> | <code style="color:#845ef7">Plaats C12 is al bezet.</code> |
| <code style="color:#e64980;font-weight:600">b13</code>, <code style="color:#e64980;font-weight:600">40</code> | het ticket voor <code style="color:#845ef7">Plaats       : B13 (rij B, stoel 13)</code> |

**Aanpak.** `rijVan` kijkt enkel naar de eerste letter, `stoelVan` enkel naar de rest. Allebei geven ze -1 bij een fout, zodat de `do-while` in `main` één test heeft: is een van beide -1, of is de plaats bezet, dan vraag je opnieuw. Lees de code met `next`: een plaatscode bevat geen spatie.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int rijVan(String code) {
    if (code.length() < 2) {
        return -1;
    }
    char letter = Character.toUpperCase(code.charAt(0));
    if (letter < 'A' || letter >= 'A' + RIJEN) {
        return -1;
    }
    return letter - 'A';
}

static int stoelVan(String code) {
    String nummer = code.substring(1);
    for (int i = 0; i < nummer.length(); i++) {
        if (!Character.isDigit(nummer.charAt(i))) {
            return -1;
        }
    }
    if (nummer.isEmpty() || nummer.length() > 2) {
        return -1;
    }
    int stoel = Integer.parseInt(nummer);
    return stoel >= 1 && stoel <= STOELEN_PER_RIJ ? stoel : -1;
}

// in main:
int rijIndex;
int stoel;
do {
    System.out.print("Plaats (bv. C12): ");
    String code = sc.next();
    rijIndex = rijVan(code);
    stoel = stoelVan(code);
    if (rijIndex == -1 || stoel == -1) {
        System.out.println("Geen geldige plaats: " + code + ".");
    } else if (bezet[rijIndex][stoel - 1]) {
        System.out.println("Plaats " + plaatsCode(rijIndex, stoel) + " is al bezet.");
    }
} while (rijIndex == -1 || stoel == -1 || bezet[rijIndex][stoel - 1]);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Geen `toUpperCase`. <code style="color:#e64980;font-weight:600">b13</code> geeft dan <code style="color:#845ef7">Geen geldige plaats: b13.</code>, want `'b'` ligt na `'J'`.
> - In de voorwaarde van de `while` eerst `bezet[...]` testen. Bij <code style="color:#e64980;font-weight:600">K3</code> is de rij -1, en stopt je programma met een `ArrayIndexOutOfBoundsException`. Met `||` wordt de rest niet meer bekeken zodra een test `true` is.

---

## 4. Vrije plaatsen per categorie als tabel

**Nakijkpunten**

- Na een ticket op <code style="color:#e64980;font-weight:600">B13</code>: de tabel uit de opgave, met <code style="color:#845ef7">Totaal      149              7079,00</code>.

**Aanpak.** De tellers blijven zoals in 02_06. Enkel het tonen verandert: één `printf` voor de kop, één in een lus voor de categorieën, en één voor het totaal. Bedragen in cent deel je door `100.0`, zodat ze een `double` worden voor `%.2f`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.printf("%-9s %5s %9s %10s%n", "Categorie", "Vrij", "Prijs", "Samen");
for (int c = 1; c <= 3; c++) {
    int prijs = basisprijs(c);
    System.out.printf("%-9d %5d %9.2f %10.2f%n", c, vrijPerCategorie[c - 1], prijs / 100.0, vrijPerCategorie[c - 1] * prijs / 100.0);
}
System.out.printf("%-9s %5d %9s %10.2f%n", "Totaal", vrijPerCategorie[0] + vrijPerCategorie[1] + vrijPerCategorie[2], "", nogTeVerkopen / 100.0);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `%9d` voor de prijs, met `prijs / 100.0` als waarde. Je programma stopt na het aantal vrije plaatsen met `IllegalFormatConversionException: d != java.lang.Double`.

---

## 5. Eén plaats of een reeks: dezelfde naam

**Nakijkpunten**

| Groep | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Plaatsen     : A9 tot A12 (categorie 1)</code> |
| <code style="color:#e64980;font-weight:600">5</code>, voor concert 2 | <code style="color:#845ef7">Plaatsen     : B14 tot B18 (categorie 1)</code> |
| <code style="color:#e64980;font-weight:600">9</code>, dan <code style="color:#e64980;font-weight:600">8</code> | de vraag twee keer, en <code style="color:#845ef7">Plaatsen     : C1 tot C8 (categorie 1)</code> |

**Aanpak.** De tweede `isVrij` roept de eerste op voor elke plaats van de reeks, en stopt bij de eerste bezette. In `main` vervangt een lus over de startplaatsen de teller `naElkaar` uit 02_06. De startplaats loopt maar tot `20 - groep`: een reeks die verder loopt, past niet in de rij.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static boolean isVrij(boolean[][] bezet, int rij, int stoel) {
    return !bezet[rij][stoel];
}

static boolean isVrij(boolean[][] bezet, int rij, int start, int aantal) {
    for (int s = start; s < start + aantal; s++) {
        if (!isVrij(bezet, rij, s)) {
            return false;
        }
    }
    return true;
}

// in main:
zoekGroep:
for (int r = 0; r < RIJEN; r++) {
    for (int s = 0; s + groep <= STOELEN_PER_RIJ; s++) {
        if (isVrij(bezet, r, s, groep)) {
            groepRij = r;
            groepStart = s;
            break zoekGroep;
        }
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `s < STOELEN_PER_RIJ` als voorwaarde. Een groep van 4 vindt A9 nog, maar een groep van 8 test in rij B de reeks vanaf B14, en stopt met `ArrayIndexOutOfBoundsException: Index 20 out of bounds for length 20`.

---

## 6. Vrije plaatsen na elkaar, met recursie

**Nakijkpunten**

- Na een ticket op B13: <code style="color:#845ef7">Grootste groep die nog samen kan zitten: 20 (rij E)</code>.
- Na een ticket op E5, voor concert 3: <code style="color:#845ef7">Grootste groep die nog samen kan zitten: 20 (rij G)</code>.
- De groepen van punt 5 krijgen dezelfde plaatsen.

**Aanpak.** Het basisgeval vangt twee situaties op: het einde van de rij, en een bezette plaats. In beide gevallen is er vanaf hier geen vrije plaats meer. Anders is de plaats vrij: 1, plus wat er vanaf de volgende stoel vrij is. `langsteReeks` probeert elke stoel als begin, en houdt het grootste resultaat bij.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static boolean isVrij(boolean[][] bezet, int rij, int start, int aantal) {
    return vrijVanaf(bezet, rij, start) >= aantal;
}

static int vrijVanaf(boolean[][] bezet, int rij, int stoel) {
    if (stoel == STOELEN_PER_RIJ || !isVrij(bezet, rij, stoel)) {
        return 0;                              // basisgeval: einde van de rij of bezet
    }
    return 1 + vrijVanaf(bezet, rij, stoel + 1);
}

static int langsteReeks(boolean[][] bezet, int rij) {
    int langste = 0;
    for (int s = 0; s < STOELEN_PER_RIJ; s++) {
        langste = Math.max(langste, vrijVanaf(bezet, rij, s));
    }
    return langste;
}

// in main, na de beste vrije plaats:
int grootsteRij = 0;
for (int r = 1; r < RIJEN; r++) {
    if (langsteReeks(bezet, r) > langsteReeks(bezet, grootsteRij)) {
        grootsteRij = r;
    }
}
System.out.println("Grootste groep die nog samen kan zitten: " + langsteReeks(bezet, grootsteRij) + " (rij " + (char) ('A' + grootsteRij) + ")");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Enkel `!isVrij(...)` als basisgeval. In een rij die vrij is tot het einde, zoals rij A na B13, kijkt de methode naar stoel 21: `ArrayIndexOutOfBoundsException: Index 20 out of bounds for length 20`.
> - `>=` in plaats van `>` bij het zoeken van de grootste rij. Bij gelijke lengte kies je dan de laatste rij, I in plaats van E, terwijl rij E dichter bij het podium ligt.

---

## 7. Alles samen: het zaalrapport

**Nakijkpunten**

| Invoer | Wat het rapport toont |
|---|---|
| concert <code style="color:#e64980;font-weight:600">1</code>, <code style="color:#e64980;font-weight:600">B13</code>, <code style="color:#e64980;font-weight:600">40</code>, groep van <code style="color:#e64980;font-weight:600">4</code> | het rapport uit de opgave, met <code style="color:#845ef7">B  ....XXXXXX..X.......    13  B14-B20 (7)</code> en <code style="color:#845ef7">Bezetting: 55 van 200 (27,5 %)</code> |
| concert <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">E5</code>, <code style="color:#e64980;font-weight:600">19</code>, groep van <code style="color:#e64980;font-weight:600">8</code> | <code style="color:#845ef7">B  ....XXXXXXXXXXXXXX..     6  B1-B4 (4)</code>, <code style="color:#845ef7">Categorie 1: 29 vrij van 60</code> en <code style="color:#845ef7">Bezetting: 59 van 200 (29,5 %)</code> |

**Aanpak.** Per rij bouw je eerst de regel van het zaalplan op, en tel je de vrije plaatsen. De langste reeks ken je via `langsteReeks`; het begin ervan zoek je met `vrijVanaf`, van links naar rechts. Elke rij komt met één `String.format` in de `StringBuilder`. De categorieën en de bezetting tel je in dezelfde lus mee.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String zaalrapport(boolean[][] bezet, String concert) {
    String streep = "=".repeat(44);
    StringBuilder sb = new StringBuilder();
    sb.append(streep).append("\n");
    sb.append("CONCERTZAAL - ZAALRAPPORT\n");
    sb.append(concert).append("\n");
    sb.append(streep).append("\n");
    sb.append("   12345678901234567890  vrij  langste reeks\n");
    int[] vrijPerCategorie = new int[3];
    int vrij = 0;
    for (int r = 0; r < RIJEN; r++) {
        StringBuilder lijn = new StringBuilder();
        int vrijInRij = 0;
        for (int s = 0; s < STOELEN_PER_RIJ; s++) {
            if (isVrij(bezet, r, s)) {
                lijn.append('.');
                vrijInRij++;
            } else {
                lijn.append('X');
            }
        }
        vrij += vrijInRij;
        vrijPerCategorie[categorie(r) - 1] += vrijInRij;
        String reeks = "-";
        int langste = langsteReeks(bezet, r);
        if (langste > 0) {
            int start = 0;
            while (vrijVanaf(bezet, r, start) < langste) {
                start++;
            }
            reeks = plaatsCode(r, start + 1) + "-" + plaatsCode(r, start + langste) + " (" + langste + ")";
        }
        sb.append(String.format("%c  %s  %4d  %s\n", (char) ('A' + r), lijn, vrijInRij, reeks));
    }
    sb.append("-".repeat(44)).append("\n");
    for (int c = 0; c < 3; c++) {
        int plaatsen = (c == 1 ? 4 : 3) * STOELEN_PER_RIJ;
        sb.append("Categorie " + (c + 1) + ": " + vrijPerCategorie[c] + " vrij van " + plaatsen + "\n");
    }
    int totaal = RIJEN * STOELEN_PER_RIJ;
    sb.append(String.format("Bezetting: %d van %d (%.1f %%)\n", totaal - vrij, totaal, (totaal - vrij) * 100.0 / totaal));
    sb.append(streep).append("\n");
    return sb.toString();
}

// in main, op het einde:
System.out.print(zaalrapport(bezet, artiest + " (" + datum + ")"));
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `(%.1f %)` met één procentteken. Java leest `%)` als een opmaak die niet bestaat: `UnknownFormatConversionException: Conversion = ')'`.
> - `(totaal - vrij) * 100 / totaal` met gehele getallen. Dat is een `int`, en `%.1f` weigert die: `IllegalFormatConversionException: f != java.lang.Integer`. Met `100.0` krijg je een `double`, en `27,5 %`.

> [!TIP]
> **Zo kan het beter**
>
> - Het aantal plaatsen per categorie, `(c == 1 ? 4 : 3) * STOELEN_PER_RIJ`, herhaalt de regel uit `categorie` in een andere vorm. Tel ze liever in dezelfde lus over de rijen: dan blijft het juist als de zaal een rij bijkrijgt.
> - `vrijVanaf` rekent dezelfde reeks vaak opnieuw uit: voor elke startplaats opnieuw tot het einde. Voor 200 plaatsen maakt dat niets uit; voor een zaal van 2000 zou je de lengtes één keer per rij uitrekenen, van rechts naar links.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Scanner;

public class Main {
    static final int STOELEN_PER_RIJ = 20;
    static final int RIJEN = 10;
    static final int SERVICEKOST = 250;

    public static void main(String[] args) {
        System.out.println("Concertzaal - ticketbalie");
        final int MAX_GROEP = 8;
        Scanner sc = new Scanner(System.in);

        // het zaalplan
        boolean[][] bezet = new boolean[RIJEN][STOELEN_PER_RIJ];
        int[] bezetteNummers = {1, 2, 3, 4, 5, 6, 7, 8, 13, 14, 15, 16, 17, 18, 19, 20, 25, 26, 27, 28, 29, 30, 52, 61, 62, 63, 64,
                101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 150, 199, 200};
        for (int nummer : bezetteNummers) {
            bezet[(nummer - 1) / STOELEN_PER_RIJ][(nummer - 1) % STOELEN_PER_RIJ] = true;
        }
        toonZaalplan(bezet);

        // het concert
        System.out.print("Concert (1, 2 of 3): ");
        int concert = sc.nextInt();
        String artiest = concert == 1 ? "The Lunar Echoes" : concert == 2 ? "Brasserie Blues Band" : "Noor & de Strijkers";
        String datum = concert == 1 ? "2026-12-04" : concert == 2 ? "2026-12-12" : "2027-01-09";

        // de plaats
        int rijIndex;
        int stoel;
        do {
            System.out.print("Plaats (bv. C12): ");
            String code = sc.next();
            rijIndex = rijVan(code);
            stoel = stoelVan(code);
            if (rijIndex == -1 || stoel == -1) {
                System.out.println("Geen geldige plaats: " + code + ".");
            } else if (bezet[rijIndex][stoel - 1]) {
                System.out.println("Plaats " + plaatsCode(rijIndex, stoel) + " is al bezet.");
            }
        } while (rijIndex == -1 || stoel == -1 || bezet[rijIndex][stoel - 1]);
        char rij = (char) ('A' + rijIndex);
        String plaats = plaatsCode(rijIndex, stoel);

        // categorie, korting en btw, in cent
        int categorie = categorie(rijIndex);
        int basisprijs = basisprijs(categorie);
        System.out.print("Leeftijd: ");
        int leeftijd = sc.nextInt();
        int korting = korting(basisprijs, leeftijd);
        int teBetalen = basisprijs - korting + SERVICEKOST;
        int btw = btw(basisprijs - korting);
        bezet[rijIndex][stoel - 1] = true;

        // het ticket
        System.out.println("==============================");
        System.out.println("CONCERTZAAL - TICKET");
        System.out.println("==============================");
        System.out.println("Concert      : " + artiest + " (" + datum + ")");
        System.out.println("Plaats       : " + plaats + " (rij " + rij + ", stoel " + stoel + ")");
        System.out.println("Categorie    : " + categorie);
        System.out.println("Basisprijs   : " + euro(basisprijs));
        System.out.println("Korting -26  : " + euro(korting));
        System.out.println("Servicekost  : " + euro(SERVICEKOST));
        System.out.println("Te betalen   : " + euro(teBetalen));
        System.out.println("Waarvan btw  : " + euro(btw) + " (6 % op het ticket)");
        System.out.println("==============================");

        // vrije plaatsen per categorie
        int[] vrijPerCategorie = new int[3];
        int nogTeVerkopen = 0;
        for (int r = 0; r < RIJEN; r++) {
            for (int s = 0; s < STOELEN_PER_RIJ; s++) {
                if (!bezet[r][s]) {
                    vrijPerCategorie[categorie(r) - 1]++;
                    nogTeVerkopen += basisprijs(categorie(r));
                }
            }
        }
        System.out.printf("%-9s %5s %9s %10s%n", "Categorie", "Vrij", "Prijs", "Samen");
        for (int c = 1; c <= 3; c++) {
            int prijs = basisprijs(c);
            System.out.printf("%-9d %5d %9.2f %10.2f%n", c, vrijPerCategorie[c - 1], prijs / 100.0, vrijPerCategorie[c - 1] * prijs / 100.0);
        }
        System.out.printf("%-9s %5d %9s %10.2f%n", "Totaal", vrijPerCategorie[0] + vrijPerCategorie[1] + vrijPerCategorie[2], "", nogTeVerkopen / 100.0);

        // de beste vrije plaats
        int besteRij = -1;
        int besteStoel = -1;
        zoek:
        for (int r = 0; r < RIJEN; r++) {
            for (int s = 0; s < STOELEN_PER_RIJ; s++) {
                if (!bezet[r][s]) {
                    besteRij = r;
                    besteStoel = s;
                    break zoek;
                }
            }
        }
        if (besteRij == -1) {
            System.out.println("De zaal is uitverkocht.");
        } else {
            System.out.println("Beste vrije plaats: " + plaatsCode(besteRij, besteStoel + 1) + " (categorie " + categorie(besteRij) + ")");
        }
        int grootsteRij = 0;
        for (int r = 1; r < RIJEN; r++) {
            if (langsteReeks(bezet, r) > langsteReeks(bezet, grootsteRij)) {
                grootsteRij = r;
            }
        }
        System.out.println("Grootste groep die nog samen kan zitten: " + langsteReeks(bezet, grootsteRij) + " (rij " + (char) ('A' + grootsteRij) + ")");

        // plaatsen naast elkaar
        int groep;
        do {
            System.out.print("Groepsgrootte (1-" + MAX_GROEP + "): ");
            groep = sc.nextInt();
        } while (groep < 1 || groep > MAX_GROEP);
        int groepRij = -1;
        int groepStart = -1;
        zoekGroep:
        for (int r = 0; r < RIJEN; r++) {
            for (int s = 0; s + groep <= STOELEN_PER_RIJ; s++) {
                if (isVrij(bezet, r, s, groep)) {
                    groepRij = r;
                    groepStart = s;
                    break zoekGroep;
                }
            }
        }
        if (groepRij == -1) {
            System.out.println("Geen " + groep + " plaatsen naast elkaar.");
        } else {
            int groepCategorie = categorie(groepRij);
            int groepBasis = basisprijs(groepCategorie);
            int groepKorting = 0;
            for (int p = 1; p <= groep; p++) {
                System.out.print("Leeftijd persoon " + p + ": ");
                groepKorting += korting(groepBasis, sc.nextInt());
            }
            for (int s = groepStart; s < groepStart + groep; s++) {
                bezet[groepRij][s] = true;
            }
            int tickets = groep * groepBasis - groepKorting;
            int service = groep * SERVICEKOST;
            String van = plaatsCode(groepRij, groepStart + 1);
            String tot = plaatsCode(groepRij, groepStart + groep);
            System.out.println("==============================");
            System.out.println("CONCERTZAAL - GROEPSTICKET");
            System.out.println("==============================");
            System.out.println("Concert      : " + artiest + " (" + datum + ")");
            System.out.println("Plaatsen     : " + van + " tot " + tot + " (categorie " + groepCategorie + ")");
            System.out.println("Tickets      : " + groep + " x " + euro(groepBasis));
            System.out.println("Korting -26  : " + euro(groepKorting));
            System.out.println("Servicekost  : " + euro(service));
            System.out.println("Te betalen   : " + euro(tickets + service));
            System.out.println("Waarvan btw  : " + euro(btw(tickets)) + " (6 % op de tickets)");
            System.out.println("==============================");
        }

        // het zaalplan na de verkoop
        System.out.print(zaalrapport(bezet, artiest + " (" + datum + ")"));
    }

    // bedragen in cent
    static String euro(int cent) {
        return String.format("%d,%02d euro", cent / 100, cent % 100);
    }

    static int korting(int basisprijs, int leeftijd) {
        return leeftijd < 26 ? (basisprijs * 20 + 50) / 100 : 0;
    }

    static int btw(int ticket) {
        return ticket - (ticket * 100 + 53) / 106;
    }

    // categorie en basisprijs van een rij
    static int categorie(int rijIndex) {
        return rijIndex <= 2 ? 1 : rijIndex <= 6 ? 2 : 3;
    }

    static int basisprijs(int categorie) {
        return categorie == 1 ? 6500 : categorie == 2 ? 4900 : 3500;
    }

    static String plaatsCode(int rijIndex, int stoel) {
        return (char) ('A' + rijIndex) + "" + stoel;
    }

    // het zaalplan, met daaronder het aantal vrije plaatsen
    static void toonZaalplan(boolean[][] bezet) {
        System.out.println("   12345678901234567890");
        int vrij = 0;
        for (int r = 0; r < RIJEN; r++) {
            String lijn = (char) ('A' + r) + "  ";
            for (int s = 0; s < STOELEN_PER_RIJ; s++) {
                if (bezet[r][s]) {
                    lijn += "X";
                } else {
                    lijn += ".";
                    vrij++;
                }
            }
            System.out.println(lijn);
        }
        System.out.println("Vrij: " + vrij + " van " + RIJEN * STOELEN_PER_RIJ);
    }

    // een plaatscode zoals C12 ontleden, of -1
    static int rijVan(String code) {
        if (code.length() < 2) {
            return -1;
        }
        char letter = Character.toUpperCase(code.charAt(0));
        if (letter < 'A' || letter >= 'A' + RIJEN) {
            return -1;
        }
        return letter - 'A';
    }

    static int stoelVan(String code) {
        String nummer = code.substring(1);
        for (int i = 0; i < nummer.length(); i++) {
            if (!Character.isDigit(nummer.charAt(i))) {
                return -1;
            }
        }
        if (nummer.isEmpty() || nummer.length() > 2) {
            return -1;
        }
        int stoel = Integer.parseInt(nummer);
        return stoel >= 1 && stoel <= STOELEN_PER_RIJ ? stoel : -1;
    }

    // een plaats, of een reeks plaatsen: dezelfde naam, andere parameters
    static boolean isVrij(boolean[][] bezet, int rij, int stoel) {
        return !bezet[rij][stoel];
    }

    static boolean isVrij(boolean[][] bezet, int rij, int start, int aantal) {
        return vrijVanaf(bezet, rij, start) >= aantal;
    }

    // hoeveel vrije plaatsen na elkaar, vanaf een stoel: recursief
    static int vrijVanaf(boolean[][] bezet, int rij, int stoel) {
        if (stoel == STOELEN_PER_RIJ || !isVrij(bezet, rij, stoel)) {
            return 0;                              // basisgeval: einde van de rij of bezet
        }
        return 1 + vrijVanaf(bezet, rij, stoel + 1);
    }

    static int langsteReeks(boolean[][] bezet, int rij) {
        int langste = 0;
        for (int s = 0; s < STOELEN_PER_RIJ; s++) {
            langste = Math.max(langste, vrijVanaf(bezet, rij, s));
        }
        return langste;
    }

    // het zaalrapport als tekst
    static String zaalrapport(boolean[][] bezet, String concert) {
        String streep = "=".repeat(44);
        StringBuilder sb = new StringBuilder();
        sb.append(streep).append("\n");
        sb.append("CONCERTZAAL - ZAALRAPPORT\n");
        sb.append(concert).append("\n");
        sb.append(streep).append("\n");
        sb.append("   12345678901234567890  vrij  langste reeks\n");
        int[] vrijPerCategorie = new int[3];
        int vrij = 0;
        for (int r = 0; r < RIJEN; r++) {
            StringBuilder lijn = new StringBuilder();
            int vrijInRij = 0;
            for (int s = 0; s < STOELEN_PER_RIJ; s++) {
                if (isVrij(bezet, r, s)) {
                    lijn.append('.');
                    vrijInRij++;
                } else {
                    lijn.append('X');
                }
            }
            vrij += vrijInRij;
            vrijPerCategorie[categorie(r) - 1] += vrijInRij;
            String reeks = "-";
            int langste = langsteReeks(bezet, r);
            if (langste > 0) {
                int start = 0;
                while (vrijVanaf(bezet, r, start) < langste) {
                    start++;
                }
                reeks = plaatsCode(r, start + 1) + "-" + plaatsCode(r, start + langste) + " (" + langste + ")";
            }
            sb.append(String.format("%c  %s  %4d  %s\n", (char) ('A' + r), lijn, vrijInRij, reeks));
        }
        sb.append("-".repeat(44)).append("\n");
        for (int c = 0; c < 3; c++) {
            int plaatsen = (c == 1 ? 4 : 3) * STOELEN_PER_RIJ;
            sb.append("Categorie " + (c + 1) + ": " + vrijPerCategorie[c] + " vrij van " + plaatsen + "\n");
        }
        int totaal = RIJEN * STOELEN_PER_RIJ;
        sb.append(String.format("Bezetting: %d van %d (%.1f %%)\n", totaal - vrij, totaal, (totaal - vrij) * 100.0 / totaal));
        sb.append(streep).append("\n");
        return sb.toString();
    }
}
```

</details>
