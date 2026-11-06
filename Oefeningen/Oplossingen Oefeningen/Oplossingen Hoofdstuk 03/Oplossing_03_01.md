# Oplossing 03_01

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 03</b> &nbsp;·&nbsp; Opsplitsen in methodes &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_03_01.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Na elke keuze toont je programma opnieuw het menu; dat staat niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 02_01 staat in de map `Oefening_03_01`. Een verkoop van 2 stuks Ryzen 5 7600X aan Lotte Peeters toont onder meer <code style="color:#845ef7">Subtotaal   : 398.0 euro</code> en <code style="color:#845ef7">Voorraad na verkoop: 1 (bijna uitverkocht)</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_02_01.md` voor de code.

---

## 2. Wat dubbel staat, in een methode

**Nakijkpunten**

- Je programma toont bij dezelfde invoer exact hetzelfde als in punt 1, ook bij <code style="color:#e64980;font-weight:600">9</code>: <code style="color:#845ef7">Ongeldige keuze.</code>
- `main` bevat geen ternary operator meer voor de status, en geen `Math.round(... * 100) / 100.0`.

**Aanpak.** Een methode staat in de klasse, naast `main`, nooit erin. Wat ze nodig heeft, krijgt ze als parameter; wat ze uitrekent, geeft ze terug met `return`. In de ladder van `status` stopt elke `return` de methode: na de eerste die uitgevoerd wordt, gebeurt er niets meer, dus heb je geen `else` meer nodig voor de laatste regel.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static void toonMenu() {
    System.out.println("--- Menu ---");
    System.out.println("1. Catalogus");
    // ... de andere keuzes
    System.out.print("Keuze: ");
}

static String status(int voorraad) {
    if (voorraad == 0) {
        return "uitverkocht";
    } else if (voorraad <= 2) {
        return "bijna uitverkocht";
    }
    return "op voorraad";
}

static double rond(double bedrag) {
    return Math.round(bedrag * 100) / 100.0;
}

// in main, op de kassabon:
double subtotaal = rond(prijs * aantal);
System.out.println("Voorraad na verkoop: " + nieuweVoorraad + " (" + status(nieuweVoorraad) + ")");
// en in het voorraadoverzicht:
System.out.println((i + 1) + ". " + namen[i] + ": " + voorraden[i] + " (" + status(voorraden[i]) + ")");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `status(voorraad)` op de kassabon in plaats van `status(nieuweVoorraad)`. De bon toont dan de status van voor de verkoop: <code style="color:#845ef7">Voorraad na verkoop: 1 (op voorraad)</code>.
> - `static` vergeten. De compiler meldt `non-static method status(int) cannot be referenced from a static context`.

---

## 3. Bedragen met twee cijfers na de komma

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| verkoop <code style="color:#e64980;font-weight:600">2</code>, <code style="color:#e64980;font-weight:600">2</code>, <code style="color:#e64980;font-weight:600">Lotte Peeters</code> | <code style="color:#845ef7">Aantal      : 2 x 199,00 euro</code><br><code style="color:#845ef7">Te betalen  : 398,00 euro</code><br><code style="color:#845ef7">Waarvan btw : 69,07 euro</code> |
| <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">2. Ryzen 5 7600X (AMD, processor) - 199,00 euro - 1 stuks</code> |
| <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">250</code> | <code style="color:#845ef7">1. B650 Tomahawk - 219,00 euro</code> en <code style="color:#845ef7">3 artikel(en) tot 250,00 euro.</code> |
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Voorraadwaarde: 6533,00 euro</code> |

**Aanpak.** `euro` rekent niets uit: ze maakt van een getal een tekst. Daarom geeft ze een `String` terug, en blijft het rekenen bij `rond` en de `double`s. Waar vroeger `+ prijs + " euro"` stond, staat nu `+ euro(prijs)`: het woord `euro` zit in de tekst die de methode teruggeeft.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static String euro(double bedrag) {
    return String.format("%.2f euro", bedrag);
}

// op de kassabon:
System.out.println("Aantal      : " + aantal + " x " + euro(prijs));
System.out.println("Subtotaal   : " + euro(subtotaal));
// bij het zoeken:
System.out.println("Geen artikelen tot " + euro(maximum) + ".");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `%2f` in plaats van `%.2f`. Zonder punt is de 2 de breedte, niet het aantal cijfers na de komma: je krijgt `334,000000 euro`.
> - `euro(prijs) + " euro"`. Het woord staat dan twee keer op de bon.

---

## 4. De catalogus als tabel

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> | de tabel uit de opgave, met <code style="color:#845ef7"> 5  Radeon RX 7800 XT  Sapphire  videokaart    499,00      2</code> |
| <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">300</code> | de kop, de artikelen 1, 2 en 6 in de tabel, en <code style="color:#845ef7">3 artikel(en) tot 300,00 euro.</code> |
| <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">100</code> | geen kop, enkel <code style="color:#845ef7">Geen artikelen tot 100,00 euro.</code> |

**Aanpak.** `toonRij` krijgt de waarden van één artikel, niet de arrays: zo kan ze elk artikel tonen, uit de catalogus of uit een zoekresultaat. Bij het zoeken toon je de kop pas bij het eerste gevonden artikel. Vindt de lus niets, dan staat er geen lege tabel.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static void toonKop() {
    System.out.printf("%2s  %-18s %-9s %-11s %8s %6s%n", "Nr", "Artikel", "Merk", "Soort", "Prijs", "Stuks");
}

static void toonRij(int nr, String naam, String merk, String soort, double prijs, int voorraad) {
    System.out.printf("%2d  %-18s %-9s %-11s %8.2f %6d%n", nr, naam, merk, soort, prijs, voorraad);
}

// in case 3:
if (prijzen[i] <= maximum) {
    if (gevonden == 0) {
        toonKop();
    }
    toonRij(i + 1, namen[i], merken[i], soorten[i], prijzen[i], voorraden[i]);
    gevonden++;
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `%8d` voor de prijs. Een `double` past niet in `%d`: je programma stopt na de eerste woorden van de rij met `IllegalFormatConversionException: d != java.lang.Double`.
> - De kop met andere breedtes dan de rij. Dan staan de titels niet boven hun kolom. Kopieer de opmaak van de rij, en vervang enkel `d` en `.2f` door `s`.

---

## 5. Zoeken op naam of merk

**Nakijkpunten**

| Zoek | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">ryzen</code> | de kop, <code style="color:#845ef7"> 2  Ryzen 5 7600X      AMD       processor     199,00      3</code>, de rij van artikel 3, en <code style="color:#845ef7">2 artikel(en) gevonden.</code> |
| <code style="color:#e64980;font-weight:600">AMD</code> | dezelfde twee rijen |
| <code style="color:#e64980;font-weight:600">  noctua </code> | de rij van artikel 6, en <code style="color:#845ef7">1 artikel(en) gevonden.</code> |
| <code style="color:#e64980;font-weight:600">xyz</code> | <code style="color:#845ef7">Niets gevonden voor xyz.</code> |
| enkel Enter | <code style="color:#845ef7">Geef minstens een teken in.</code> |

**Aanpak.** Maak beide kanten klein: de naam en de zoektekst. `trim()` haalt de spaties voor en na de zoektekst weg, zodat ook een lege invoer met spaties leeg is. De extra `sc.nextLine()` haalt de Enter weg die na de keuze bleef staan.

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
        System.out.println("Geef minstens een teken in.");
    } else {
        int gevonden = 0;
        for (int i = 0; i < namen.length; i++) {
            if (bevat(namen[i], zoek) || bevat(merken[i], zoek)) {
                if (gevonden == 0) {
                    toonKop();
                }
                toonRij(i + 1, namen[i], merken[i], soorten[i], prijzen[i], voorraden[i]);
                gevonden++;
            }
        }
        if (gevonden == 0) {
            System.out.println("Niets gevonden voor " + zoek + ".");
        } else {
            System.out.println(gevonden + " artikel(en) gevonden.");
        }
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Enkel de zoektekst klein maken: `tekst.contains(stuk.toLowerCase())`. `Ryzen` bevat geen `ryzen`, en je programma toont <code style="color:#845ef7">Niets gevonden voor ryzen.</code>
> - De extra `sc.nextLine()` vergeten. Je programma toont `Zoek: Geef minstens een teken in.` zonder te wachten, en leest daarna `ryzen` als keuze: `InputMismatchException`.

---

## 6. Rekenen in methodes

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> bij de start | <code style="color:#845ef7">Gemiddelde prijs: 334,00 euro</code><br><code style="color:#845ef7">Goedkoopste: NH-D15 (109,00 euro)</code><br><code style="color:#845ef7">Duurste: GeForce RTX 4070 (589,00 euro)</code> |
| <code style="color:#e64980;font-weight:600">5</code> bij de start | <code style="color:#845ef7">Voorraadwaarde: 6931,00 euro</code> |
| <code style="color:#e64980;font-weight:600">5</code> na een verkoop van 2 Radeon RX 7800 XT | <code style="color:#845ef7">Voorraadwaarde: 5933,00 euro</code> |

**Aanpak.** Elke methode krijgt de array die ze nodig heeft, en geeft één getal terug. `kleinste` en `grootste` geven een index terug, geen prijs: met de index vind je in `main` zowel de naam als de prijs. De lus van de statistiek telt nog enkel de stuks; de rest staat in de methodes.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static double gemiddelde(double[] getallen) {
    double som = 0;
    for (double g : getallen) {
        som += g;
    }
    return rond(som / getallen.length);
}

static int kleinste(double[] getallen) {
    int index = 0;
    for (int i = 1; i < getallen.length; i++) {
        if (getallen[i] < getallen[index]) {
            index = i;
        }
    }
    return index;
}

static double voorraadwaarde(double[] prijzen, int[] voorraden) {
    double waarde = 0;
    for (int i = 0; i < prijzen.length; i++) {
        waarde += prijzen[i] * voorraden[i];
    }
    return rond(waarde);
}

// in case 4:
double gemiddelde = gemiddelde(prijzen);
int goedkoopste = kleinste(prijzen);
int duurste = grootste(prijzen);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `return i;` binnen de `if` van `kleinste`. De methode stopt dan bij het eerste artikel dat goedkoper is dan het eerste, niet bij het goedkoopste: <code style="color:#845ef7">Goedkoopste: Ryzen 5 7600X (199,00 euro)</code> in plaats van de NH-D15.

---

## 7. Alles samen: de artikelfiche als tekst

**Nakijkpunten**

| Invoer na <code style="color:#e64980;font-weight:600">7</code> | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">ryzen 5 7600x</code> | een fiche met <code style="color:#845ef7">RYZEN 5 7600X</code>, <code style="color:#845ef7">Voorraad : 3 (op voorraad)</code>, <code style="color:#845ef7">Waarde   : 597,00 euro</code>, en een kader van 26 `=` |
| <code style="color:#e64980;font-weight:600">rx</code> | de fiche uit de opgave, met een kader van 32 `=` |
| <code style="color:#e64980;font-weight:600">ryzen</code> | <code style="color:#845ef7">Meerdere artikelen: Ryzen 5 7600X, Ryzen 7 7800X3D.</code> |
| <code style="color:#e64980;font-weight:600">hdd</code> | <code style="color:#845ef7">Niets gevonden voor hdd.</code> |
| <code style="color:#e64980;font-weight:600">NH-D15</code> | een fiche met <code style="color:#845ef7">Waarde   : 763,00 euro</code> |
| <code style="color:#e64980;font-weight:600">Radeon RX 7800 XT</code>, na twee verkopen ervan | <code style="color:#845ef7">Voorraad : 0 (uitverkocht)</code> en <code style="color:#845ef7">Waarde   : 0,00 euro</code> |

**Aanpak.** `fiche` maakt eerst elke regel als aparte `String`. Pas als alle regels er zijn, ken je de langste, en daarmee het kader. Dan plakt een `StringBuilder` alles aan elkaar. In `main` zoek je eerst de exacte naam; is die er niet, dan doorloop je alle namen met `bevat`, en houd je bij hoeveel er passen. Bij precies één treffer heb je in `i` al de index van dat artikel.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
static int zoekIndex(String[] namen, String naam) {
    for (int i = 0; i < namen.length; i++) {
        if (namen[i].equalsIgnoreCase(naam)) {
            return i;
        }
    }
    return -1;
}

static String fiche(String naam, String merk, String soort, double prijs, int voorraad) {
    String titel = naam.toUpperCase();
    String regelMerk = "Merk     : " + merk;
    String regelSoort = "Soort    : " + soort;
    String regelPrijs = "Prijs    : " + euro(prijs);
    String regelVoorraad = "Voorraad : " + voorraad + " (" + status(voorraad) + ")";
    String regelWaarde = "Waarde   : " + euro(rond(prijs * voorraad));
    int breedte = titel.length();
    breedte = Math.max(breedte, regelMerk.length());
    breedte = Math.max(breedte, regelSoort.length());
    breedte = Math.max(breedte, regelPrijs.length());
    breedte = Math.max(breedte, regelVoorraad.length());
    breedte = Math.max(breedte, regelWaarde.length());
    String kader = "=".repeat(breedte);
    StringBuilder sb = new StringBuilder();
    sb.append(kader).append("\n");
    sb.append(titel).append("\n");
    sb.append(kader).append("\n");
    sb.append(regelMerk).append("\n");
    // ... soort, prijs, voorraad en waarde op dezelfde manier
    sb.append(kader).append("\n");
    return sb.toString();
}

// in main:
case 7 -> {
    sc.nextLine();                         // de Enter na de keuze
    System.out.print("Artikel (naam of een stuk ervan): ");
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
            System.out.println("Niets gevonden voor " + zoek + ".");
        } else if (gevonden > 1) {
            System.out.println("Meerdere artikelen: " + lijst + ".");
            i = -1;
        }
    }
    if (i != -1) {
        System.out.print(fiche(namen[i], merken[i], soorten[i], prijzen[i], voorraden[i]));
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Eén regel vergeten bij de breedte. Zonder de voorraadregel is het kader van de Radeon 22 tekens breed, terwijl <code style="color:#845ef7">Voorraad : 2 (bijna uitverkocht)</code> er 32 heeft.
> - Na elke naam een komma toevoegen: `lijst.append(namen[k]).append(", ")`. Dan krijg je `Meerdere artikelen: Ryzen 5 7600X, Ryzen 7 7800X3D, .`
> - `System.out.println(fiche(...))` in plaats van `print`. De fiche eindigt al op `\n`: je krijgt een lege regel onder het kader.

> [!TIP]
> **Zo kan het beter**
>
> - Zes keer `Math.max` voor zes regels: met een array van regels doe je het in een lus, en voeg je een regel toe zonder de breedte te vergeten. `fiche` wordt dan korter, en de fout van hierboven kan niet meer.
> - De methodes krijgen vijf parameters die samen één artikel zijn. In hoofdstuk 04 wordt dat één object `Artikel`, met een methode `fiche()` die zelf aan zijn gegevens kan.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - kassa");
        final double BTW = 0.21;
        Scanner sc = new Scanner(System.in);

        // de catalogus
        String[] namen = {"B650 Tomahawk", "Ryzen 5 7600X", "Ryzen 7 7800X3D", "GeForce RTX 4070", "Radeon RX 7800 XT", "NH-D15"};
        String[] merken = {"MSI", "AMD", "AMD", "ASUS", "Sapphire", "Noctua"};
        String[] soorten = {"moederbord", "processor", "processor", "videokaart", "videokaart", "koeler"};
        double[] prijzen = {219.00, 199.00, 389.00, 589.00, 499.00, 109.00};
        int[] voorraden = {12, 3, 5, 0, 2, 7};

        int keuze;
        do {
            toonMenu();
            keuze = sc.nextInt();
            switch (keuze) {
                case 1 -> {
                    toonKop();
                    for (int i = 0; i < namen.length; i++) {
                        toonRij(i + 1, namen[i], merken[i], soorten[i], prijzen[i], voorraden[i]);
                    }
                }
                case 2 -> {
                    // de verkoop
                    System.out.print("Artikel (1-6): ");
                    int id = sc.nextInt();
                    if (id < 1 || id > namen.length) {
                        System.out.println("Artikel " + id + " bestaat niet.");
                    } else {
                        int i = id - 1;
                        String naam = namen[i];
                        String merk = merken[i];
                        double prijs = prijzen[i];
                        int voorraad = voorraden[i];
                        System.out.print("Aantal: ");
                        int aantal = sc.nextInt();
                        if (aantal < 1) {
                            System.out.println("Het aantal moet minstens 1 zijn.");
                        } else if (aantal > voorraad) {
                            System.out.println("Niet genoeg op voorraad: nog " + voorraad + " stuks.");
                        } else {
                            double subtotaal = rond(prijs * aantal);
                            double verzendkost = subtotaal < 50 ? 4.99 : 0.0;
                            double teBetalen = rond(subtotaal + verzendkost);
                            int nieuweVoorraad = voorraad - aantal;
                            voorraden[i] = nieuweVoorraad;
                            sc.nextLine();                         // de Enter na het aantal
                            System.out.print("Klant: ");
                            String klant = sc.nextLine();
                            double waarvanBtw = rond(teBetalen - teBetalen / (1 + BTW));
                            System.out.println("==============================");
                            System.out.println("BIT & BYTE - KASSABON");
                            System.out.println("==============================");
                            System.out.println("Klant       : " + klant);
                            System.out.println("Artikel     : " + naam + " (" + merk + ")");
                            System.out.println("Aantal      : " + aantal + " x " + euro(prijs));
                            System.out.println("Subtotaal   : " + euro(subtotaal));
                            System.out.println("Verzendkost : " + euro(verzendkost));
                            System.out.println("Te betalen  : " + euro(teBetalen));
                            System.out.println("Waarvan btw : " + euro(waarvanBtw));
                            System.out.println("------------------------------");
                            System.out.println("Voorraad na verkoop: " + nieuweVoorraad + " (" + status(nieuweVoorraad) + ")");
                            System.out.println("==============================");
                        }
                    }
                }
                case 3 -> {
                    System.out.print("Maximumprijs: ");
                    double maximum = sc.nextDouble();
                    int gevonden = 0;
                    for (int i = 0; i < namen.length; i++) {
                        if (prijzen[i] <= maximum) {
                            if (gevonden == 0) {
                                toonKop();
                            }
                            toonRij(i + 1, namen[i], merken[i], soorten[i], prijzen[i], voorraden[i]);
                            gevonden++;
                        }
                    }
                    if (gevonden == 0) {
                        System.out.println("Geen artikelen tot " + euro(maximum) + ".");
                    } else {
                        System.out.println(gevonden + " artikel(en) tot " + euro(maximum) + ".");
                    }
                }
                case 4 -> {
                    int stuks = 0;
                    for (int i = 0; i < namen.length; i++) {
                        stuks += voorraden[i];
                    }
                    double gemiddelde = gemiddelde(prijzen);
                    int goedkoopste = kleinste(prijzen);
                    int duurste = grootste(prijzen);
                    System.out.println("Gemiddelde prijs: " + euro(gemiddelde));
                    System.out.println("Goedkoopste: " + namen[goedkoopste] + " (" + euro(prijzen[goedkoopste]) + ")");
                    System.out.println("Duurste: " + namen[duurste] + " (" + euro(prijzen[duurste]) + ")");
                    System.out.println("Stuks op voorraad: " + stuks);
                }
                case 5 -> {
                    System.out.println("--- Voorraadoverzicht ---");
                    int uitverkocht = 0;
                    for (int i = 0; i < namen.length; i++) {
                        if (voorraden[i] == 0) {
                            uitverkocht++;
                        }
                        System.out.println((i + 1) + ". " + namen[i] + ": " + voorraden[i] + " (" + status(voorraden[i]) + ")");
                    }
                    System.out.println("Voorraadwaarde: " + euro(voorraadwaarde(prijzen, voorraden)));
                    System.out.println("Uitverkocht: " + uitverkocht);
                    System.out.println("Bijbestellen:");
                    int bij = 0;
                    for (int i = 0; i < namen.length; i++) {
                        if (voorraden[i] <= 2) {
                            System.out.println("- " + namen[i]);
                            bij++;
                        }
                    }
                    if (bij == 0) {
                        System.out.println("- niets");
                    }
                }
                case 6 -> {
                    sc.nextLine();                         // de Enter na de keuze
                    System.out.print("Zoek: ");
                    String zoek = sc.nextLine().trim();
                    if (zoek.isEmpty()) {
                        System.out.println("Geef minstens een teken in.");
                    } else {
                        int gevonden = 0;
                        for (int i = 0; i < namen.length; i++) {
                            if (bevat(namen[i], zoek) || bevat(merken[i], zoek)) {
                                if (gevonden == 0) {
                                    toonKop();
                                }
                                toonRij(i + 1, namen[i], merken[i], soorten[i], prijzen[i], voorraden[i]);
                                gevonden++;
                            }
                        }
                        if (gevonden == 0) {
                            System.out.println("Niets gevonden voor " + zoek + ".");
                        } else {
                            System.out.println(gevonden + " artikel(en) gevonden.");
                        }
                    }
                }
                case 7 -> {
                    sc.nextLine();                         // de Enter na de keuze
                    System.out.print("Artikel (naam of een stuk ervan): ");
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
                            System.out.println("Niets gevonden voor " + zoek + ".");
                        } else if (gevonden > 1) {
                            System.out.println("Meerdere artikelen: " + lijst + ".");
                            i = -1;
                        }
                    }
                    if (i != -1) {
                        System.out.print(fiche(namen[i], merken[i], soorten[i], prijzen[i], voorraden[i]));
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
        System.out.println("1. Catalogus");
        System.out.println("2. Verkopen");
        System.out.println("3. Zoeken tot een maximumprijs");
        System.out.println("4. Statistiek");
        System.out.println("5. Voorraadoverzicht");
        System.out.println("6. Zoeken op naam of merk");
        System.out.println("7. Artikelfiche");
        System.out.println("0. Stoppen");
        System.out.print("Keuze: ");
    }

    // de voorraadstatus
    static String status(int voorraad) {
        if (voorraad == 0) {
            return "uitverkocht";
        } else if (voorraad <= 2) {
            return "bijna uitverkocht";
        }
        return "op voorraad";
    }

    // afronden op de cent
    static double rond(double bedrag) {
        return Math.round(bedrag * 100) / 100.0;
    }

    // een bedrag als tekst, met twee cijfers na de komma
    static String euro(double bedrag) {
        return String.format("%.2f euro", bedrag);
    }

    // de catalogus als tabel
    static void toonKop() {
        System.out.printf("%2s  %-18s %-9s %-11s %8s %6s%n", "Nr", "Artikel", "Merk", "Soort", "Prijs", "Stuks");
    }

    static void toonRij(int nr, String naam, String merk, String soort, double prijs, int voorraad) {
        System.out.printf("%2d  %-18s %-9s %-11s %8.2f %6d%n", nr, naam, merk, soort, prijs, voorraad);
    }

    // zoeken zonder hoofdletters
    static boolean bevat(String tekst, String stuk) {
        return tekst.toLowerCase().contains(stuk.toLowerCase());
    }

    // rekenen over de catalogus
    static double gemiddelde(double[] getallen) {
        double som = 0;
        for (double g : getallen) {
            som += g;
        }
        return rond(som / getallen.length);
    }

    static int kleinste(double[] getallen) {
        int index = 0;
        for (int i = 1; i < getallen.length; i++) {
            if (getallen[i] < getallen[index]) {
                index = i;
            }
        }
        return index;
    }

    static int grootste(double[] getallen) {
        int index = 0;
        for (int i = 1; i < getallen.length; i++) {
            if (getallen[i] > getallen[index]) {
                index = i;
            }
        }
        return index;
    }

    static double voorraadwaarde(double[] prijzen, int[] voorraden) {
        double waarde = 0;
        for (int i = 0; i < prijzen.length; i++) {
            waarde += prijzen[i] * voorraden[i];
        }
        return rond(waarde);
    }

    // een artikel op zijn volledige naam, of -1
    static int zoekIndex(String[] namen, String naam) {
        for (int i = 0; i < namen.length; i++) {
            if (namen[i].equalsIgnoreCase(naam)) {
                return i;
            }
        }
        return -1;
    }

    // de artikelfiche als tekst: het kader is zo breed als de langste regel
    static String fiche(String naam, String merk, String soort, double prijs, int voorraad) {
        String titel = naam.toUpperCase();
        String regelMerk = "Merk     : " + merk;
        String regelSoort = "Soort    : " + soort;
        String regelPrijs = "Prijs    : " + euro(prijs);
        String regelVoorraad = "Voorraad : " + voorraad + " (" + status(voorraad) + ")";
        String regelWaarde = "Waarde   : " + euro(rond(prijs * voorraad));
        int breedte = titel.length();
        breedte = Math.max(breedte, regelMerk.length());
        breedte = Math.max(breedte, regelSoort.length());
        breedte = Math.max(breedte, regelPrijs.length());
        breedte = Math.max(breedte, regelVoorraad.length());
        breedte = Math.max(breedte, regelWaarde.length());
        String kader = "=".repeat(breedte);
        StringBuilder sb = new StringBuilder();
        sb.append(kader).append("\n");
        sb.append(titel).append("\n");
        sb.append(kader).append("\n");
        sb.append(regelMerk).append("\n");
        sb.append(regelSoort).append("\n");
        sb.append(regelPrijs).append("\n");
        sb.append(regelVoorraad).append("\n");
        sb.append(regelWaarde).append("\n");
        sb.append(kader).append("\n");
        return sb.toString();
    }
}
```

</details>
