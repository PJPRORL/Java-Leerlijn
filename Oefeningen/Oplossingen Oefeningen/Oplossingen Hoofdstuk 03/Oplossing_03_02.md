# Oplossing 03_02

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 03</b> &nbsp;·&nbsp; Tekst ontleden &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_03_02.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Het bestelnummer is willekeurig: jouw nummer is anders. De catalogus aan het begin van elke bestelling staat niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 02_02 staat in de map `Oefening_03_02`. Een bestelling van 2 Ryzen 5 7600X en 6 NH-D15 op dag <code style="color:#e64980;font-weight:600">2</code> toont onder meer <code style="color:#845ef7">NH-D15: 6 x 109.0 euro (korting 5 %) = 621.3 euro</code> en <code style="color:#845ef7">Leverdag     : werkdag 3 (deze week)</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_02_02.md` voor de code.

---

## 2. De vaste regels in methodes

**Nakijkpunten**

- Je programma toont bij dezelfde invoer exact hetzelfde als in punt 1, op het bestelnummer na: <code style="color:#845ef7">Spaarpunten  : 101</code> en <code style="color:#845ef7">Dozen        : 3 (laatste doos: 2 stuks)</code>.
- Een bestelling van 10 B650 Tomahawk: <code style="color:#845ef7">Dozen        : 4 (laatste doos: 1 stuk)</code>.

**Aanpak.** Elke regel met een getal als uitkomst wordt een methode met een returnwaarde. `main` vraagt de waarde op, en gebruikt ze zoals vroeger. `PER_DOOS` verhuist naar boven in de klasse, want twee methodes hebben de constante nodig.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    static final int PER_DOOS = 3;

    // ... main

    static int korting(int aantal) {
        return aantal >= 10 ? 10 : aantal >= 5 ? 5 : 0;
    }

    static double verzendkost(double lijnbedragen) {
        return lijnbedragen < 50 ? 4.99 : 0.0;
    }

    static int aantalDozen(int stuks) {
        return (stuks + PER_DOOS - 1) / PER_DOOS;
    }

    static int inLaatsteDoos(int stuks) {
        int rest = stuks % PER_DOOS;
        return rest == 0 ? PER_DOOS : rest;
    }

    static double rond(double bedrag) {
        return Math.round(bedrag * 100) / 100.0;
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `final int PER_DOOS = 3;` in `main` laten staan. De methodes zien het niet: `cannot find symbol`, met `symbol: variable PER_DOOS`.

---

## 3. De pakbon in kolommen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| bestelling van punt 1 | <code style="color:#845ef7">Te betalen: 1019,30 euro</code>, en de pakbon uit de opgave met <code style="color:#845ef7">NH-D15                  6    109,00      5 %    621,30</code> |
| daarna <code style="color:#e64980;font-weight:600">1</code> x <code style="color:#e64980;font-weight:600">10</code> op dag <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">B650 Tomahawk          10    219,00     10 %   1971,00</code> en <code style="color:#845ef7">Leverdag     : werkdag 1 (volgende week)</code> |

- De catalogus: <code style="color:#845ef7">2. Ryzen 5 7600X       199,00 euro -  3 op voorraad</code>.

**Aanpak.** Schrijf de kop en de rij met dezelfde breedtes; enkel de soort waarde verschilt: `%s` in de kop, `%d` en `%.2f` in de rij. Het procentteken na de korting schrijf je als `%%`. Een streep van 54 tekens maak je met `"=".repeat(54)`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String streep = "=".repeat(54);
System.out.println(streep);
System.out.println("BIT & BYTE - PAKBON");
System.out.println(streep);
System.out.println("Bestelnummer : " + bestelnummer);
System.out.printf("%-18s %6s %9s %8s %9s%n", "Artikel", "Aantal", "Prijs", "Korting", "Bedrag");
for (int k = 0; k < aantalLijnen; k++) {
    int a = lijnArtikel[k];
    System.out.printf("%-18s %6d %9.2f %6d %% %9.2f%n", namen[a], lijnAantal[k], prijzen[a], lijnKorting[k], lijnBedrag[k]);
}
System.out.println("-".repeat(54));
System.out.printf("Lijnbedragen : %.2f euro%n", lijnbedragen);
System.out.printf("Verzendkost  : %.2f euro%n", verzendkost);
System.out.printf("Totaal       : %.2f euro%n", totaal);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `%6d % %9.2f` met één procentteken. Java leest `% ` als het begin van een nieuwe opmaak, en stopt bij de eerste lijn met `IllegalFormatFlagsException: Flags = ' '`.

---

## 4. Een bestellijn als tekst

**Nakijkpunten**

| Lijn | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2x2</code>, <code style="color:#e64980;font-weight:600">6 x 6</code> | niets: de lijnen zijn aanvaard |
| <code style="color:#e64980;font-weight:600">2X1</code> | <code style="color:#845ef7">Bijgeteld bij lijn 1: nu 3 stuks.</code> |
| <code style="color:#e64980;font-weight:600">9x1</code> | <code style="color:#845ef7">Artikel 9 bestaat niet.</code> |
| <code style="color:#e64980;font-weight:600">2x</code> | <code style="color:#845ef7">Gebruik de vorm artikel x aantal, zoals 2x3.</code> |
| <code style="color:#e64980;font-weight:600">twee x 3</code> | <code style="color:#845ef7">Geen geldig getal: twee.</code> |
| <code style="color:#e64980;font-weight:600">3x25</code> | <code style="color:#845ef7">Het aantal moet tussen 1 en 20 liggen.</code> |
| een lege lijn, dag <code style="color:#e64980;font-weight:600">2</code>, <code style="color:#e64980;font-weight:600">j</code> | een pakbon met <code style="color:#845ef7">Totaal       : 1218,30 euro</code> |
| <code style="color:#e64980;font-weight:600">j</code> voor nog een bestelling, dan <code style="color:#e64980;font-weight:600">2x1</code> | je programma wacht op <code style="color:#e64980;font-weight:600">2x1</code>, en vraagt daarna de volgende lijn |

**Aanpak.** `controleerLijn` doet alle controles en geeft de eerste melding terug, of een lege tekst. Zo blijft de lus in `main` kort: is de melding niet leeg, dan toon je ze en ga je met `continue` naar de volgende lijn. `artikelVan` en `aantalVan` mogen ervan uitgaan dat de lijn klopt, want ze worden pas na de controle opgeroepen.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
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

static String controleerLijn(String lijn, int aantalArtikelen, int maxPerArtikel) {
    String[] delen = lijn.toLowerCase().split("x");
    if (delen.length != 2) {
        return "Gebruik de vorm artikel x aantal, zoals 2x3.";
    }
    String artikel = delen[0].trim();
    String aantal = delen[1].trim();
    if (artikel.isEmpty() || aantal.isEmpty()) {
        return "Gebruik de vorm artikel x aantal, zoals 2x3.";
    }
    if (!isGetal(artikel)) {
        return "Geen geldig getal: " + artikel + ".";
    }
    if (!isGetal(aantal)) {
        return "Geen geldig getal: " + aantal + ".";
    }
    int id = Integer.parseInt(artikel);
    int stuks = Integer.parseInt(aantal);
    if (id < 1 || id > aantalArtikelen) {
        return "Artikel " + id + " bestaat niet.";
    }
    if (stuks < 1 || stuks > maxPerArtikel) {
        return "Het aantal moet tussen 1 en " + maxPerArtikel + " liggen.";
    }
    return "";
}

static int artikelVan(String lijn) {
    return Integer.parseInt(lijn.toLowerCase().split("x")[0].trim()) - 1;
}

// in main:
System.out.print("Lijn (artikel x aantal, leeg = klaar): ");
String lijn = sc.nextLine().trim();
if (lijn.isEmpty()) {
    break;
}
String fout = controleerLijn(lijn, namen.length, MAX_PER_ARTIKEL);
if (!fout.isEmpty()) {
    System.out.println(fout);
    continue;
}
int artikel = artikelVan(lijn);
int aantal = aantalVan(lijn);
// ... bijtellen of een nieuwe lijn, zoals in 02_02

// en helemaal onderaan de lus:
nogEen = sc.next().charAt(0);
sc.nextLine();                         // de Enter na j of n
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De extra `sc.nextLine()` na *Nog een bestelling* vergeten. De tweede bestelling leest meteen de Enter: `Lijn (artikel x aantal, leeg = klaar): Lege bestelling: er is niets besteld.`, zonder dat de magazijnier iets typte.
> - `split("x")` zonder `toLowerCase()`. Een hoofdletter `X` is een ander teken: <code style="color:#e64980;font-weight:600">2X1</code> geeft dan <code style="color:#845ef7">Gebruik de vorm artikel x aantal, zoals 2x3.</code>

---

## 5. Weekdagen als tekst

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">za</code> | <code style="color:#845ef7">Geen werkdag: za.</code>, en de vraag opnieuw |
| <code style="color:#e64980;font-weight:600">Dinsdag</code> | de pakbon met <code style="color:#845ef7">Leverdag     : woensdag (deze week)</code> |
| 10 B650 Tomahawk op <code style="color:#e64980;font-weight:600">vr</code> | <code style="color:#845ef7">Leverdag     : maandag (volgende week)</code> |

**Aanpak.** `dagNummer` zet de tekst in kleine letters en kiest met een `switch` op tekst. Een onbekende dag geeft -1; daarop herhaalt de `do-while` in `main` de vraag. `dagNaam` doet het omgekeerde, voor de pakbon. De rekening met `dagenVerder` blijft dezelfde als in 02_02.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int dagNummer(String dag) {
    return switch (dag.toLowerCase()) {
        case "ma", "maandag" -> 1;
        case "di", "dinsdag" -> 2;
        case "wo", "woensdag" -> 3;
        case "do", "donderdag" -> 4;
        case "vr", "vrijdag" -> 5;
        default -> -1;
    };
}

// in main:
int besteldag;
do {
    System.out.print("Besteldag (ma-vr): ");
    String dag = sc.nextLine().trim();
    besteldag = dagNummer(dag);
    if (besteldag == -1) {
        System.out.println("Geen werkdag: " + dag + ".");
    }
} while (besteldag == -1);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `switch (dag)` zonder `toLowerCase()`. <code style="color:#e64980;font-weight:600">di</code> werkt, maar <code style="color:#e64980;font-weight:600">Dinsdag</code> geeft <code style="color:#845ef7">Geen werkdag: Dinsdag.</code>

---

## 6. De bestelling op één regel

**Nakijkpunten**

- De bestelling van punt 5, op <code style="color:#e64980;font-weight:600">di</code>: na de pakbon het bestelnummer met <code style="color:#845ef7">: 2 x Ryzen 5 7600X, 6 x NH-D15 = 1019,30 euro</code>.
- Een bestelling van één lijn, 2 Radeon RX 7800 XT: <code style="color:#845ef7">: 2 x Radeon RX 7800 XT = 998,00 euro</code>, zonder komma.

**Aanpak.** Begin de `StringBuilder` met het bestelnummer. Zet de komma *voor* elke lijn behalve de eerste: dan staat er nooit een komma te veel. Het totaal voeg je toe met `String.format`, zodat het dezelfde opmaak heeft als op de pakbon.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String samenvatting(String bestelnummer, String[] namen, int[] lijnArtikel, int[] lijnAantal, int aantalLijnen, double totaal) {
    StringBuilder sb = new StringBuilder(bestelnummer);
    sb.append(": ");
    for (int k = 0; k < aantalLijnen; k++) {
        if (k > 0) {
            sb.append(", ");
        }
        sb.append(lijnAantal[k]).append(" x ").append(namen[lijnArtikel[k]]);
    }
    sb.append(String.format(" = %.2f euro", totaal));
    return sb.toString();
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Na elke lijn `", "` toevoegen. De regel eindigt dan op `6 x NH-D15,  = 1019,30 euro`.

---

## 7. Alles samen: de bestelbon uit één regel invoer

**Nakijkpunten**

| Bestelling | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2x2, 6x6, 2x1</code>, <code style="color:#e64980;font-weight:600">di</code>, <code style="color:#e64980;font-weight:600">j</code> | de pakbon met <code style="color:#845ef7">Totaal       : 1218,30 euro</code> en <code style="color:#845ef7">: 3 x Ryzen 5 7600X, 6 x NH-D15 = 1218,30 euro</code> |
| <code style="color:#e64980;font-weight:600">2x2, 9x1, 1 x twee</code> | <code style="color:#845ef7">9x1: Artikel 9 bestaat niet.</code><br><code style="color:#845ef7">1 x twee: Geen geldig getal: twee.</code><br><code style="color:#845ef7">Er is niets besteld: verbeter de regel.</code> |
| <code style="color:#e64980;font-weight:600">1x1, 2x1, 3x1, 5x1, 6x1, 4x1</code> | <code style="color:#845ef7">4x1: maximaal 5 lijnen per bestelling.</code> |
| <code style="color:#e64980;font-weight:600">1x20, 1x1</code> | <code style="color:#845ef7">1x1: te veel, maximaal 20 stuks per artikel.</code> |
| daarna <code style="color:#e64980;font-weight:600">5x2</code>, <code style="color:#e64980;font-weight:600">vr</code>, <code style="color:#e64980;font-weight:600">j</code> | een pakbon met één lijn: <code style="color:#845ef7">Radeon RX 7800 XT       2    499,00      0 %    998,00</code> |
| een lege regel | <code style="color:#845ef7">Lege bestelling: er is niets besteld.</code> |
| <code style="color:#e64980;font-weight:600">3x1</code>, <code style="color:#e64980;font-weight:600">ma</code>, <code style="color:#e64980;font-weight:600">n</code> | <code style="color:#845ef7">Bestelling geannuleerd: er is niets veranderd.</code> |

**Aanpak.** Een `do-while` vraagt de regel tot er geen fout meer in staat. Bij elke poging begin je opnieuw met nul lijnen en `fout` op `false`. De regel splits je op de komma's; elk deel gaat door `controleerLijn` uit punt 4, en daarna door het bijtellen van 02_02. Een fout in een deel zet `fout` op `true`, maar de lus gaat verder: zo ziet de magazijnier alle fouten in één keer.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
boolean fout;
do {
    System.out.print("Bestelling (bv. 2x3, 6x2): ");
    String regel = sc.nextLine().trim();
    fout = false;
    aantalLijnen = 0;
    if (regel.isEmpty()) {
        break;
    }
    String[] delen = regel.split(",");
    for (String deel : delen) {
        String lijn = deel.trim();
        String melding = controleerLijn(lijn, namen.length, MAX_PER_ARTIKEL);
        if (!melding.isEmpty()) {
            System.out.println(lijn + ": " + melding);
            fout = true;
            continue;
        }
        int artikel = artikelVan(lijn);
        int aantal = aantalVan(lijn);
        int bestaand = -1;
        for (int k = 0; k < aantalLijnen; k++) {
            if (lijnArtikel[k] == artikel) {
                bestaand = k;
            }
        }
        if (bestaand >= 0) {
            if (lijnAantal[bestaand] + aantal > MAX_PER_ARTIKEL) {
                System.out.println(lijn + ": te veel, maximaal " + MAX_PER_ARTIKEL + " stuks per artikel.");
                fout = true;
            } else {
                lijnAantal[bestaand] += aantal;
            }
        } else if (aantalLijnen == MAX_LIJNEN) {
            System.out.println(lijn + ": maximaal " + MAX_LIJNEN + " lijnen per bestelling.");
            fout = true;
        } else {
            lijnArtikel[aantalLijnen] = artikel;
            lijnAantal[aantalLijnen] = aantal;
            aantalLijnen++;
        }
    }
    if (fout) {
        System.out.println("Er is niets besteld: verbeter de regel.");
    }
} while (fout);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `aantalLijnen = 0` niet herhalen bij elke poging. De goede delen van een foute regel blijven dan staan: na <code style="color:#e64980;font-weight:600">2x2, 9x1</code> en <code style="color:#e64980;font-weight:600">5x2</code> staan de Ryzen en de Radeon samen op de pakbon, voor `Te betalen: 1396,00 euro`.
> - `break` bij de eerste fout. De magazijnier ziet dan enkel de eerste fout, verbetert die, en krijgt pas daarna de volgende te zien.

> [!TIP]
> **Zo kan het beter**
>
> - Een lijn wordt drie keer ontleed: in `controleerLijn`, `artikelVan` en `aantalVan`. Met een object `Bestellijn` in hoofdstuk 04 ontleed je één keer, en houdt het object artikel en aantal samen bij.
> - `Integer.parseInt` op een getal van elf cijfers stopt je programma nog altijd: `isGetal` kijkt niet naar de lengte. In hoofdstuk 08 vang je zo'n fout op met `try`/`catch`.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Random;
import java.util.Scanner;

public class Main {
    static final int PER_DOOS = 3;

    public static void main(String[] args) {
        System.out.println("Bit & Byte - magazijn");
        final int JAAR = 2026;
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
                System.out.printf("%d. %-18s %7.2f euro - %2d op voorraad%n", i + 1, namen[i], prijzen[i], voorraden[i]);
            }

            // het bestelnummer
            int volgnummer = random.nextInt(99999 - 10000 + 1) + 10000;
            String bestelnummer = "BB-" + JAAR + "-" + volgnummer;

            // de bestellijnen
            int[] lijnArtikel = new int[MAX_LIJNEN];
            int[] lijnAantal = new int[MAX_LIJNEN];
            int aantalLijnen = 0;
            boolean fout;
            do {
                System.out.print("Bestelling (bv. 2x3, 6x2): ");
                String regel = sc.nextLine().trim();
                fout = false;
                aantalLijnen = 0;
                if (regel.isEmpty()) {
                    break;
                }
                String[] delen = regel.split(",");
                for (String deel : delen) {
                    String lijn = deel.trim();
                    String melding = controleerLijn(lijn, namen.length, MAX_PER_ARTIKEL);
                    if (!melding.isEmpty()) {
                        System.out.println(lijn + ": " + melding);
                        fout = true;
                        continue;
                    }
                    int artikel = artikelVan(lijn);
                    int aantal = aantalVan(lijn);
                    int bestaand = -1;
                    for (int k = 0; k < aantalLijnen; k++) {
                        if (lijnArtikel[k] == artikel) {
                            bestaand = k;
                        }
                    }
                    if (bestaand >= 0) {
                        if (lijnAantal[bestaand] + aantal > MAX_PER_ARTIKEL) {
                            System.out.println(lijn + ": te veel, maximaal " + MAX_PER_ARTIKEL + " stuks per artikel.");
                            fout = true;
                        } else {
                            lijnAantal[bestaand] += aantal;
                        }
                    } else if (aantalLijnen == MAX_LIJNEN) {
                        System.out.println(lijn + ": maximaal " + MAX_LIJNEN + " lijnen per bestelling.");
                        fout = true;
                    } else {
                        lijnArtikel[aantalLijnen] = artikel;
                        lijnAantal[aantalLijnen] = aantal;
                        aantalLijnen++;
                    }
                }
                if (fout) {
                    System.out.println("Er is niets besteld: verbeter de regel.");
                }
            } while (fout);
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
                    lijnKorting[k] = korting(aantal);
                    lijnBedrag[k] = Math.round(aantal * prijzen[lijnArtikel[k]] * (100 - lijnKorting[k])) / 100.0;
                    lijnbedragen += lijnBedrag[k];
                    stuksTotaal += aantal;
                }
                lijnbedragen = rond(lijnbedragen);

                // de verzenddozen
                int dozen = aantalDozen(stuksTotaal);
                int inLaatsteDoos = inLaatsteDoos(stuksTotaal);
                String stuks = inLaatsteDoos == 1 ? "stuk" : "stuks";

                // verzendkost, totaal en spaarpunten
                double verzendkost = verzendkost(lijnbedragen);
                double totaal = rond(lijnbedragen + verzendkost);
                int spaarpunten = (int) (lijnbedragen / 10);

                // levertermijn en leverdag
                int besteldag;
                do {
                    System.out.print("Besteldag (ma-vr): ");
                    String dag = sc.nextLine().trim();
                    besteldag = dagNummer(dag);
                    if (besteldag == -1) {
                        System.out.println("Geen werkdag: " + dag + ".");
                    }
                } while (besteldag == -1);
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
                System.out.printf("Te betalen: %.2f euro%n", totaal);
                System.out.print("Bevestigen (j/n): ");
                char bevestig = sc.next().charAt(0);
                if (bevestig == 'j') {
                    for (int k = 0; k < aantalLijnen; k++) {
                        int a = lijnArtikel[k];
                        voorraden[a] = Math.max(0, voorraden[a] - lijnAantal[k]);
                    }
                    String streep = "=".repeat(54);
                    System.out.println(streep);
                    System.out.println("BIT & BYTE - PAKBON");
                    System.out.println(streep);
                    System.out.println("Bestelnummer : " + bestelnummer);
                    System.out.printf("%-18s %6s %9s %8s %9s%n", "Artikel", "Aantal", "Prijs", "Korting", "Bedrag");
                    for (int k = 0; k < aantalLijnen; k++) {
                        int a = lijnArtikel[k];
                        System.out.printf("%-18s %6d %9.2f %6d %% %9.2f%n", namen[a], lijnAantal[k], prijzen[a], lijnKorting[k], lijnBedrag[k]);
                    }
                    System.out.println("-".repeat(54));
                    System.out.printf("Lijnbedragen : %.2f euro%n", lijnbedragen);
                    System.out.printf("Verzendkost  : %.2f euro%n", verzendkost);
                    System.out.printf("Totaal       : %.2f euro%n", totaal);
                    System.out.println("Spaarpunten  : " + spaarpunten);
                    System.out.println("Dozen        : " + dozen + " (laatste doos: " + inLaatsteDoos + " " + stuks + ")");
                    System.out.println("Levertermijn : " + levertermijn + " " + werkdagen);
                    System.out.println("Leverdag     : " + dagNaam(leverdag) + " (" + week + ")");
                    System.out.println(streep);
                    System.out.println(samenvatting(bestelnummer, namen, lijnArtikel, lijnAantal, aantalLijnen, totaal));
                } else {
                    System.out.println("Bestelling geannuleerd: er is niets veranderd.");
                }
            }
            System.out.print("Nog een bestelling (j/n): ");
            nogEen = sc.next().charAt(0);
            sc.nextLine();                         // de Enter na j of n
        } while (nogEen == 'j');
    }

    // de vaste regels van het magazijn
    static int korting(int aantal) {
        return aantal >= 10 ? 10 : aantal >= 5 ? 5 : 0;
    }

    static double verzendkost(double lijnbedragen) {
        return lijnbedragen < 50 ? 4.99 : 0.0;
    }

    static int aantalDozen(int stuks) {
        return (stuks + PER_DOOS - 1) / PER_DOOS;
    }

    static int inLaatsteDoos(int stuks) {
        int rest = stuks % PER_DOOS;
        return rest == 0 ? PER_DOOS : rest;
    }

    static double rond(double bedrag) {
        return Math.round(bedrag * 100) / 100.0;
    }

    // een bestellijn als tekst, zoals 2x3
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

    static String controleerLijn(String lijn, int aantalArtikelen, int maxPerArtikel) {
        String[] delen = lijn.toLowerCase().split("x");
        if (delen.length != 2) {
            return "Gebruik de vorm artikel x aantal, zoals 2x3.";
        }
        String artikel = delen[0].trim();
        String aantal = delen[1].trim();
        if (artikel.isEmpty() || aantal.isEmpty()) {
            return "Gebruik de vorm artikel x aantal, zoals 2x3.";
        }
        if (!isGetal(artikel)) {
            return "Geen geldig getal: " + artikel + ".";
        }
        if (!isGetal(aantal)) {
            return "Geen geldig getal: " + aantal + ".";
        }
        int id = Integer.parseInt(artikel);
        int stuks = Integer.parseInt(aantal);
        if (id < 1 || id > aantalArtikelen) {
            return "Artikel " + id + " bestaat niet.";
        }
        if (stuks < 1 || stuks > maxPerArtikel) {
            return "Het aantal moet tussen 1 en " + maxPerArtikel + " liggen.";
        }
        return "";
    }

    static int artikelVan(String lijn) {
        return Integer.parseInt(lijn.toLowerCase().split("x")[0].trim()) - 1;
    }

    static int aantalVan(String lijn) {
        return Integer.parseInt(lijn.toLowerCase().split("x")[1].trim());
    }

    // weekdagen als tekst
    static int dagNummer(String dag) {
        return switch (dag.toLowerCase()) {
            case "ma", "maandag" -> 1;
            case "di", "dinsdag" -> 2;
            case "wo", "woensdag" -> 3;
            case "do", "donderdag" -> 4;
            case "vr", "vrijdag" -> 5;
            default -> -1;
        };
    }

    static String dagNaam(int dag) {
        return switch (dag) {
            case 1 -> "maandag";
            case 2 -> "dinsdag";
            case 3 -> "woensdag";
            case 4 -> "donderdag";
            default -> "vrijdag";
        };
    }

    // de bestelling op een regel
    static String samenvatting(String bestelnummer, String[] namen, int[] lijnArtikel, int[] lijnAantal, int aantalLijnen, double totaal) {
        StringBuilder sb = new StringBuilder(bestelnummer);
        sb.append(": ");
        for (int k = 0; k < aantalLijnen; k++) {
            if (k > 0) {
                sb.append(", ");
            }
            sb.append(lijnAantal[k]).append(" x ").append(namen[lijnArtikel[k]]);
        }
        sb.append(String.format(" = %.2f euro", totaal));
        return sb.toString();
    }
}
```

</details>
