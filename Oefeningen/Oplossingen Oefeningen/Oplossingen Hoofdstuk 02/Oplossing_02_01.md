# Oplossing 02_01

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 02</b> &nbsp;·&nbsp; Arrays en een menu &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_02_01.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Vanaf punt 4 toont je programma na elke keuze opnieuw het menu; dat staat niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 01_01 staat in de map `Oefening_02_01` en toont nog de kassabon van 01_01. Met <code style="color:#e64980;font-weight:600">Ryzen 5 7600X</code>, <code style="color:#e64980;font-weight:600">AMD</code>, <code style="color:#e64980;font-weight:600">199,00</code>, <code style="color:#e64980;font-weight:600">3</code>, <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">Lotte Peeters</code> is dat onder meer <code style="color:#845ef7">Te betalen  : 398.0 euro</code> en <code style="color:#845ef7">Voorraad na verkoop: 1 (bijna uitverkocht)</code>.

**Aanpak.** Je schrijft hier nog niets. Bewaar de uitvoer van deze run: elke kassabon die je programma in deze oefening maakt, moet er net zo uitzien. Zie `Oplossing_01_01.md` voor de code.

---

## 2. De catalogus in arrays

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">1. B650 Tomahawk (MSI, moederbord) - 219.0 euro - 12 stuks</code> tot en met <code style="color:#845ef7">6. NH-D15 (Noctua, koeler) - 109.0 euro - 7 stuks</code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">Lotte Peeters</code> | dezelfde kassabon als in punt 1 |

- De catalogus staat in vijf arrays; de lus loopt tot `namen.length`.

**Aanpak.** Een array met beginwaarden maak je met accolades: `{"B650 Tomahawk", "Ryzen 5 7600X", ...}` (leerboek 1.7). De vijf arrays zijn parallel: index `i` gaat in elke array over hetzelfde artikel. Het nummer dat de verkoper typt, is de index plus 1. Daarna haal je naam, merk, prijs en voorraad uit de arrays, in variabelen met dezelfde namen als in 01_01: zo blijft de rest van de verkoop ongewijzigd.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String[] namen = {"B650 Tomahawk", "Ryzen 5 7600X", "Ryzen 7 7800X3D", "GeForce RTX 4070", "Radeon RX 7800 XT", "NH-D15"};
String[] merken = {"MSI", "AMD", "AMD", "ASUS", "Sapphire", "Noctua"};
String[] soorten = {"moederbord", "processor", "processor", "videokaart", "videokaart", "koeler"};
double[] prijzen = {219.00, 199.00, 389.00, 589.00, 499.00, 109.00};
int[] voorraden = {12, 3, 5, 0, 2, 7};

for (int i = 0; i < namen.length; i++) {
    System.out.println((i + 1) + ". " + namen[i] + " (" + merken[i] + ", " + soorten[i] + ") - " + prijzen[i] + " euro - " + voorraden[i] + " stuks");
}

System.out.print("Artikel (1-6): ");
int id = sc.nextInt();
int i = id - 1;
String naam = namen[i];
String merk = merken[i];
double prijs = prijzen[i];
int voorraad = voorraden[i];
// ... de verkoop van 01_01, ongewijzigd
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `i <= namen.length` in de lus. De laatste index is `namen.length - 1`: je programma stopt met *ArrayIndexOutOfBoundsException: Index 6 out of bounds for length 6*.
> - `System.out.println(namen)`. Dat toont geen namen, maar iets als `[Ljava.lang.String;@1b6d3586` (leerboek 1.7).
> - `namen[id]` in plaats van `namen[id - 1]`. Dan verkoopt je kassa telkens het volgende artikel, en crasht ze bij artikel 6.

---

## 3. Een verkoop weigeren

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code> | <code style="color:#845ef7">Artikel 9 bestaat niet.</code>, zonder naar het aantal te vragen |
| <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Artikel 0 bestaat niet.</code> |
| <code style="color:#e64980;font-weight:600">5</code> <code style="color:#e64980;font-weight:600">3</code> | <code style="color:#845ef7">Niet genoeg op voorraad: nog 2 stuks.</code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Het aantal moet minstens 1 zijn.</code> |
| <code style="color:#e64980;font-weight:600">4</code> <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Niet genoeg op voorraad: nog 0 stuks.</code> |
| <code style="color:#e64980;font-weight:600">5</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">Bureau Claes bv</code> | een kassabon met <code style="color:#845ef7">Voorraad na verkoop: 0 (uitverkocht)</code> |

**Aanpak.** Twee controles op twee momenten: het nummer meteen na het inlezen, het aantal pas als je weet dat het artikel bestaat. Daarom een `if` met in de `else` de rest van de verkoop, en daarin een ladder voor het aantal (leerboek 1.5). Pas helemaal binnenin, als alles klopt, verander je de array.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Artikel (1-6): ");
int id = sc.nextInt();
if (id < 1 || id > namen.length) {
    System.out.println("Artikel " + id + " bestaat niet.");
} else {
    int i = id - 1;
    // ... naam, merk, prijs en voorraad uit de arrays
    System.out.print("Aantal: ");
    int aantal = sc.nextInt();
    if (aantal < 1) {
        System.out.println("Het aantal moet minstens 1 zijn.");
    } else if (aantal > voorraad) {
        System.out.println("Niet genoeg op voorraad: nog " + voorraad + " stuks.");
    } else {
        // ... subtotaal, verzendkost, te betalen
        int nieuweVoorraad = voorraad - aantal;
        voorraden[i] = nieuweVoorraad;
        // ... de klant en de kassabon
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `id < 1 && id > namen.length`. Een getal kan niet tegelijk kleiner dan 1 en groter dan 6 zijn: de voorwaarde is nooit waar, en artikel 9 laat je programma crashen.
> - De voorraad in de array niet aanpassen. De bon toont dan een juiste *Voorraad na verkoop*, maar de volgende verkoop vertrekt weer van de oude voorraad. Dat zie je pas vanaf punt 4, met meerdere verkopen.

---

## 4. Een menu in een lus

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> | de catalogus |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">Lotte Peeters</code>, dan <code style="color:#e64980;font-weight:600">1</code> | de kassabon, daarna <code style="color:#845ef7">2. Ryzen 5 7600X (AMD, processor) - 199.0 euro - 1 stuks</code> |
| <code style="color:#e64980;font-weight:600">7</code> | <code style="color:#845ef7">Ongeldige keuze.</code> en opnieuw het menu |
| <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Tot ziens!</code>, en het programma stopt |

**Aanpak.** Het menu moet minstens één keer verschijnen: een `do-while` (leerboek 1.6). De variabele `keuze` declareer je *voor* de lus, want de voorwaarde onderaan moet ze kunnen zien. In de lus kiest een `switch` met pijlen wat er gebeurt; een `default` vangt elke andere keuze op (leerboek 1.5). De code van punt 2 en 3 schuift ongewijzigd in `case 1` en `case 2`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int keuze;
do {
    System.out.println("--- Menu ---");
    System.out.println("1. Catalogus");
    System.out.println("2. Verkopen");
    System.out.println("0. Stoppen");
    System.out.print("Keuze: ");
    keuze = sc.nextInt();
    switch (keuze) {
        case 1 -> {
            // ... de lus van punt 2
        }
        case 2 -> {
            // ... de verkoop van punt 3
        }
        case 0 -> System.out.println("Tot ziens!");
        default -> System.out.println("Ongeldige keuze.");
    }
} while (keuze != 0);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `int keuze = sc.nextInt();` *in* de lus. De voorwaarde `while (keuze != 0)` staat buiten het blok, en kent `keuze` dan niet: *cannot find symbol*.
> - Een klassieke `switch` zonder `break`. Na de catalogus loopt je programma dan door in `case 2`, en begint het een verkoop die niemand vroeg.

> [!TIP]
> **Zo kan het beter**
>
> - Een `switch` met pijlen heeft geen fall-through. Met een klassieke `switch` is de oplossing even juist, zolang elke `case` eindigt op `break`.

---

## 5. Zoeken tot een maximumprijs

**Nakijkpunten**

| Maximumprijs | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">200</code> | <code style="color:#845ef7">2. Ryzen 5 7600X - 199.0 euro</code><br><code style="color:#845ef7">6. NH-D15 - 109.0 euro</code><br><code style="color:#845ef7">2 artikel(en) tot 200.0 euro.</code> |
| <code style="color:#e64980;font-weight:600">199,00</code> | dezelfde twee artikelen, en <code style="color:#845ef7">2 artikel(en) tot 199.0 euro.</code> |
| <code style="color:#e64980;font-weight:600">100</code> | <code style="color:#845ef7">Geen artikelen tot 100.0 euro.</code> |

**Aanpak.** Eén lus over alle artikelen, met een `if` die beslist of een artikel getoond wordt. Een teller houdt bij hoeveel er getoond zijn. Of er *niets* gevonden is, weet je pas na de lus: daar komt de tweede `if`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
case 3 -> {
    System.out.print("Maximumprijs: ");
    double maximum = sc.nextDouble();
    int gevonden = 0;
    for (int i = 0; i < namen.length; i++) {
        if (prijzen[i] <= maximum) {
            System.out.println((i + 1) + ". " + namen[i] + " - " + prijzen[i] + " euro");
            gevonden++;
        }
    }
    if (gevonden == 0) {
        System.out.println("Geen artikelen tot " + maximum + " euro.");
    } else {
        System.out.println(gevonden + " artikel(en) tot " + maximum + " euro.");
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `prijzen[i] < maximum`. Dan valt de Ryzen 5 7600X weg bij een maximum van 199,00.
> - De melding *Geen artikelen* in de lus zetten, in een `else`. Dan toont je programma ze voor elk artikel dat te duur is: vier keer bij een maximum van 200.

---

## 6. Statistiek

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> bij de start | <code style="color:#845ef7">Gemiddelde prijs: 334.0 euro</code><br><code style="color:#845ef7">Goedkoopste: NH-D15 (109.0 euro)</code><br><code style="color:#845ef7">Duurste: GeForce RTX 4070 (589.0 euro)</code><br><code style="color:#845ef7">Stuks op voorraad: 29</code> |
| na een verkoop van 7 × NH-D15 | <code style="color:#845ef7">Stuks op voorraad: 22</code>, de rest gelijk |

**Aanpak.** Eén lus doet vier dingen tegelijk: de prijzen optellen, de stuks optellen, en twee indexen bijwerken. Je begint met index 0 als goedkoopste en duurste: dat is een echt artikel, en elk ander artikel wordt ermee vergeleken. Het gemiddelde is de som gedeeld door `namen.length`, afgerond zoals in 01_01.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
case 4 -> {
    double som = 0;
    int stuks = 0;
    int goedkoopste = 0;
    int duurste = 0;
    for (int i = 0; i < namen.length; i++) {
        som += prijzen[i];
        stuks += voorraden[i];
        if (prijzen[i] < prijzen[goedkoopste]) {
            goedkoopste = i;
        }
        if (prijzen[i] > prijzen[duurste]) {
            duurste = i;
        }
    }
    double gemiddelde = Math.round(som / namen.length * 100) / 100.0;
    System.out.println("Gemiddelde prijs: " + gemiddelde + " euro");
    System.out.println("Goedkoopste: " + namen[goedkoopste] + " (" + prijzen[goedkoopste] + " euro)");
    System.out.println("Duurste: " + namen[duurste] + " (" + prijzen[duurste] + " euro)");
    System.out.println("Stuks op voorraad: " + stuks);
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De laagste prijs bijhouden met beginwaarde 0. Geen enkel artikel is goedkoper dan 0 euro: je programma noemt dan het eerste artikel, de B650 Tomahawk, het goedkoopste.
> - Delen door `6` in plaats van door `namen.length`. Vandaag geeft het hetzelfde, maar zodra er een artikel bijkomt, klopt je gemiddelde niet meer, zonder dat iemand het merkt.

---

## 7. Alles samen: het voorraadoverzicht

**Nakijkpunten**

| Invoer | Wat het overzicht toont |
|---|---|
| <code style="color:#e64980;font-weight:600">5</code> bij de start | <code style="color:#845ef7">4. GeForce RTX 4070: 0 (uitverkocht)</code><br><code style="color:#845ef7">5. Radeon RX 7800 XT: 2 (bijna uitverkocht)</code><br><code style="color:#845ef7">Voorraadwaarde: 6931.0 euro</code><br><code style="color:#845ef7">Uitverkocht: 1</code><br><code style="color:#845ef7">- GeForce RTX 4070</code><br><code style="color:#845ef7">- Radeon RX 7800 XT</code> |
| na 2 × Ryzen 5 7600X en 5 × NH-D15 | <code style="color:#845ef7">2. Ryzen 5 7600X: 1 (bijna uitverkocht)</code><br><code style="color:#845ef7">6. NH-D15: 2 (bijna uitverkocht)</code><br><code style="color:#845ef7">Voorraadwaarde: 5988.0 euro</code><br>vier regels onder <code style="color:#845ef7">Bijbestellen:</code> |

**Aanpak.** De eerste lus doet het werk per artikel: de status met een `if`-ladder (leerboek 1.5), een teller voor de uitverkochte artikelen, en de som van prijs × voorraad. *Bijbestellen* komt pas na de totalen, dus volgt er een tweede lus. Of er niets bij te bestellen is, weet je pas na die lus, net als bij het zoeken in punt 5.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
case 5 -> {
    System.out.println("--- Voorraadoverzicht ---");
    double waarde = 0;
    int uitverkocht = 0;
    for (int i = 0; i < namen.length; i++) {
        String toestand;
        if (voorraden[i] == 0) {
            toestand = "uitverkocht";
            uitverkocht++;
        } else if (voorraden[i] <= 2) {
            toestand = "bijna uitverkocht";
        } else {
            toestand = "op voorraad";
        }
        waarde += prijzen[i] * voorraden[i];
        System.out.println((i + 1) + ". " + namen[i] + ": " + voorraden[i] + " (" + toestand + ")");
    }
    System.out.println("Voorraadwaarde: " + Math.round(waarde * 100) / 100.0 + " euro");
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
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `voorraden[i] < 2` voor bijbestellen. Dan ontbreekt de Radeon RX 7800 XT, met een voorraad van precies 2.
> - De voorraadwaarde zonder afronden tonen. Bij deze prijzen valt het niet op, maar met een prijs als 1,99 euro krijg je dezelfde rekenfouten als in 01_01.

> [!TIP]
> **Zo kan het beter**
>
> - De status staat nu twee keer in je code: met ternary operators op de kassabon, met een ladder in het overzicht. Verandert de grens van *bijna uitverkocht*, dan moet je twee plaatsen aanpassen. In hoofdstuk 03 maak je er één methode van, die beide plaatsen oproepen.
> - Een `String` declareren zonder waarde, zoals `String toestand;`, mag, zolang elke tak van de ladder er een waarde in zet. Vergeet je de `else`, dan weigert de compiler: *variable toestand might not have been initialized*.

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
            System.out.println("--- Menu ---");
            System.out.println("1. Catalogus");
            System.out.println("2. Verkopen");
            System.out.println("3. Zoeken tot een maximumprijs");
            System.out.println("4. Statistiek");
            System.out.println("5. Voorraadoverzicht");
            System.out.println("0. Stoppen");
            System.out.print("Keuze: ");
            keuze = sc.nextInt();
            switch (keuze) {
                case 1 -> {
                    for (int i = 0; i < namen.length; i++) {
                        System.out.println((i + 1) + ". " + namen[i] + " (" + merken[i] + ", " + soorten[i] + ") - " + prijzen[i] + " euro - " + voorraden[i] + " stuks");
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
                            double subtotaal = Math.round(prijs * aantal * 100) / 100.0;
                            double verzendkost = subtotaal < 50 ? 4.99 : 0.0;
                            double teBetalen = Math.round((subtotaal + verzendkost) * 100) / 100.0;
                            int nieuweVoorraad = voorraad - aantal;
                            voorraden[i] = nieuweVoorraad;
                            String status = nieuweVoorraad == 0 ? "uitverkocht"
                                    : nieuweVoorraad <= 2 ? "bijna uitverkocht" : "op voorraad";
                            sc.nextLine();                         // de Enter na het aantal
                            System.out.print("Klant: ");
                            String klant = sc.nextLine();
                            double waarvanBtw = Math.round((teBetalen - teBetalen / (1 + BTW)) * 100) / 100.0;
                            System.out.println("==============================");
                            System.out.println("BIT & BYTE - KASSABON");
                            System.out.println("==============================");
                            System.out.println("Klant       : " + klant);
                            System.out.println("Artikel     : " + naam + " (" + merk + ")");
                            System.out.println("Aantal      : " + aantal + " x " + prijs + " euro");
                            System.out.println("Subtotaal   : " + subtotaal + " euro");
                            System.out.println("Verzendkost : " + verzendkost + " euro");
                            System.out.println("Te betalen  : " + teBetalen + " euro");
                            System.out.println("Waarvan btw : " + waarvanBtw + " euro");
                            System.out.println("------------------------------");
                            System.out.println("Voorraad na verkoop: " + nieuweVoorraad + " (" + status + ")");
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
                            System.out.println((i + 1) + ". " + namen[i] + " - " + prijzen[i] + " euro");
                            gevonden++;
                        }
                    }
                    if (gevonden == 0) {
                        System.out.println("Geen artikelen tot " + maximum + " euro.");
                    } else {
                        System.out.println(gevonden + " artikel(en) tot " + maximum + " euro.");
                    }
                }
                case 4 -> {
                    double som = 0;
                    int stuks = 0;
                    int goedkoopste = 0;
                    int duurste = 0;
                    for (int i = 0; i < namen.length; i++) {
                        som += prijzen[i];
                        stuks += voorraden[i];
                        if (prijzen[i] < prijzen[goedkoopste]) {
                            goedkoopste = i;
                        }
                        if (prijzen[i] > prijzen[duurste]) {
                            duurste = i;
                        }
                    }
                    double gemiddelde = Math.round(som / namen.length * 100) / 100.0;
                    System.out.println("Gemiddelde prijs: " + gemiddelde + " euro");
                    System.out.println("Goedkoopste: " + namen[goedkoopste] + " (" + prijzen[goedkoopste] + " euro)");
                    System.out.println("Duurste: " + namen[duurste] + " (" + prijzen[duurste] + " euro)");
                    System.out.println("Stuks op voorraad: " + stuks);
                }
                case 5 -> {
                    System.out.println("--- Voorraadoverzicht ---");
                    double waarde = 0;
                    int uitverkocht = 0;
                    for (int i = 0; i < namen.length; i++) {
                        String toestand;
                        if (voorraden[i] == 0) {
                            toestand = "uitverkocht";
                            uitverkocht++;
                        } else if (voorraden[i] <= 2) {
                            toestand = "bijna uitverkocht";
                        } else {
                            toestand = "op voorraad";
                        }
                        waarde += prijzen[i] * voorraden[i];
                        System.out.println((i + 1) + ". " + namen[i] + ": " + voorraden[i] + " (" + toestand + ")");
                    }
                    System.out.println("Voorraadwaarde: " + Math.round(waarde * 100) / 100.0 + " euro");
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
                case 0 -> System.out.println("Tot ziens!");
                default -> System.out.println("Ongeldige keuze.");
            }
        } while (keuze != 0);
    }
}
```

</details>
