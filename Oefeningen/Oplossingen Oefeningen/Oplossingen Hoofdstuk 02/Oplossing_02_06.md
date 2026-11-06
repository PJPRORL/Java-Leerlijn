# Oplossing 02_06

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 02</b> &nbsp;·&nbsp; Tabellen en geneste lussen &nbsp;·&nbsp; Concertzaal &nbsp;·&nbsp; bij `Oefening_02_06.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21. Vanaf punt 2 toont je programma bij de start het zaalplan, en vanaf punt 3 op het einde nog eens; dat staat niet telkens in de nakijkpunten.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Je oplossing van 01_06 staat in de map `Oefening_02_06`. Met concert <code style="color:#e64980;font-weight:600">1</code>, plaats <code style="color:#e64980;font-weight:600">52</code> en leeftijd <code style="color:#e64980;font-weight:600">19</code> toont het ticket onder meer <code style="color:#845ef7">Plaats       : C12 (rij C, stoel 12)</code>, <code style="color:#845ef7">Te betalen   : 54,50 euro</code> en <code style="color:#845ef7">Waarvan btw  : 2,94 euro (6 % op het ticket)</code>.

**Aanpak.** Je schrijft hier nog niets. Zie `Oplossing_01_06.md` voor de code.

---

## 2. Het zaalplan

**Nakijkpunten**

- Bij de start toont je programma het zaalplan precies zoals in de opgave, met rij F volledig bezet: <code style="color:#845ef7">F  XXXXXXXXXXXXXXXXXXXX</code>.
- Daaronder staat <code style="color:#845ef7">Vrij: 150 van 200</code>.
- Daarna werkt het ticket nog zoals in punt 1.

**Aanpak.** De array maak je met `new boolean[RIJEN][STOELEN_PER_RIJ]`: alle plaatsen zijn dan `false`, dus vrij. De verkochte plaatsen zet je met een lus over de plaatsnummers op `true`, met dezelfde formule als voor het ticket. Om het plan te tonen, overloop je de rijen, en per rij de stoelen. De teller voor de vrije plaatsen loopt mee in de binnenste lus.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final int STOELEN_PER_RIJ = 20;
final int RIJEN = 10;
// ...
boolean[][] bezet = new boolean[RIJEN][STOELEN_PER_RIJ];
int[] bezetteNummers = {1, 2, 3, 4, 5, 6, 7, 8, 13, 14, 15, 16, 17, 18, 19, 20, 25, 26, 27, 28, 29, 30, 52, 61, 62, 63, 64,
        101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 150, 199, 200};
for (int nummer : bezetteNummers) {
    bezet[(nummer - 1) / STOELEN_PER_RIJ][(nummer - 1) % STOELEN_PER_RIJ] = true;
}
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
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `'A' + r + "  "` zonder `(char)`. Een `char` plus een `int` is een `int`: je zaalplan begint dan met `65  XXXXXXXX....XXXXXXXX` in plaats van `A`.
> - De min 1 vergeten bij het plaatsnummer: `bezet[nummer / 20][nummer % 20]`. Plaats 200 wordt dan rij 10, die niet bestaat, en je programma stopt al bij de start met `ArrayIndexOutOfBoundsException: Index 10 out of bounds for length 10`.

---

## 3. Een plaats kiezen

**Nakijkpunten**

| Invoer na het concert | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Plaats 0 bestaat niet.</code>, en de vraag opnieuw |
| <code style="color:#e64980;font-weight:600">201</code> | <code style="color:#845ef7">Plaats 201 bestaat niet.</code>, en de vraag opnieuw |
| <code style="color:#e64980;font-weight:600">52</code> | <code style="color:#845ef7">Plaats C12 is al bezet.</code>, en de vraag opnieuw |
| <code style="color:#e64980;font-weight:600">33</code> <code style="color:#e64980;font-weight:600">19</code> | het ticket voor <code style="color:#845ef7">Plaats       : B13 (rij B, stoel 13)</code> |

- Het zaalplan op het einde toont <code style="color:#845ef7">B  ....XXXXXX..X.......</code> en <code style="color:#845ef7">Vrij: 149 van 200</code>.

**Aanpak.** Een `do-while`, want je vraagt het nummer altijd minstens één keer. Een `boolean` onthoudt of het antwoord goed is; de lus herhaalt zolang dat niet zo is. Pas na de lus reken je de rij en de stoel uit, zoals in 01_06. Na de berekening van het ticket zet je de plaats op bezet, en op het einde toon je het zaalplan van punt 2 nog eens, met een nieuwe teller.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int plaatsnummer;
boolean geldig;
do {
    System.out.print("Plaatsnummer (1 - 200): ");
    plaatsnummer = sc.nextInt();
    geldig = false;
    if (plaatsnummer < 1 || plaatsnummer > RIJEN * STOELEN_PER_RIJ) {
        System.out.println("Plaats " + plaatsnummer + " bestaat niet.");
    } else if (bezet[(plaatsnummer - 1) / STOELEN_PER_RIJ][(plaatsnummer - 1) % STOELEN_PER_RIJ]) {
        System.out.println("Plaats " + (char) ('A' + (plaatsnummer - 1) / STOELEN_PER_RIJ) + ((plaatsnummer - 1) % STOELEN_PER_RIJ + 1) + " is al bezet.");
    } else {
        geldig = true;
    }
} while (!geldig);
// ... rij, stoel, categorie, korting en btw zoals in 01_06
bezet[rijIndex][stoel - 1] = true;
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `bezet[rijIndex][stoel] = true`, zonder min 1. De stoel op het ticket telt vanaf 1, de array vanaf 0: na een ticket voor B13 krijgt B14 de `X`, en toont het zaalplan `B  ....XXXXXX...X......`.

---

## 4. Vrije plaatsen per categorie

**Nakijkpunten**

| Invoer | Wat je programma toont na het ticket |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">33</code> <code style="color:#e64980;font-weight:600">40</code> | <code style="color:#845ef7">Categorie 1: 36 vrij</code><br><code style="color:#845ef7">Categorie 2: 56 vrij</code><br><code style="color:#845ef7">Categorie 3: 57 vrij</code><br><code style="color:#845ef7">Nog te verkopen: 7079,00 euro</code> |

**Aanpak.** Opnieuw twee lussen over het zaalplan. Per rij bepaal je eerst de categorie; voor elke vrije stoel in die rij verhoog je de teller van die categorie, en tel je de basisprijs bij het totaal. Omdat de verkochte plaats al op bezet staat, telt ze niet meer mee.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int[] vrijPerCategorie = new int[3];
int nogTeVerkopen = 0;
for (int r = 0; r < RIJEN; r++) {
    int cat = r <= 2 ? 1 : r <= 6 ? 2 : 3;
    for (int s = 0; s < STOELEN_PER_RIJ; s++) {
        if (!bezet[r][s]) {
            vrijPerCategorie[cat - 1]++;
            nogTeVerkopen += cat == 1 ? 6500 : cat == 2 ? 4900 : 3500;
        }
    }
}
for (int c = 0; c < 3; c++) {
    System.out.println("Categorie " + (c + 1) + ": " + vrijPerCategorie[c] + " vrij");
}
System.out.println("Nog te verkopen: " + nogTeVerkopen / 100 + "," + (nogTeVerkopen % 100 < 10 ? "0" : "") + nogTeVerkopen % 100 + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `vrijPerCategorie[cat]++`, zonder min 1. Zodra de lus rij H bereikt, is de index 3, en stopt je programma met `ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3`.

---

## 5. De beste vrije plaats

**Nakijkpunten**

| Ticket op plaats | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">33</code> | <code style="color:#845ef7">Beste vrije plaats: A9 (categorie 1)</code> |
| <code style="color:#e64980;font-weight:600">9</code> | <code style="color:#845ef7">Beste vrije plaats: A10 (categorie 1)</code> |

**Aanpak.** Je begint met -1 als rij: dat betekent *nog niets gevonden*. De eerste vrije stoel die je tegenkomt, onthoud je, en met `break zoek` verlaat je meteen allebei de lussen. Staat de rij na de lussen nog op -1, dan was er geen enkele vrije plaats.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
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
    System.out.println("Beste vrije plaats: " + (char) ('A' + besteRij) + (besteStoel + 1) + " (categorie " + (besteRij <= 2 ? 1 : besteRij <= 6 ? 2 : 3) + ")");
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Een gewone `break`. Die stopt enkel de lus over de stoelen: de buitenste lus gaat verder, en elke rij overschrijft de vorige vondst. Je programma toont dan `Beste vrije plaats: J1 (categorie 3)`, de eerste vrije plaats van de laatste rij.

---

## 6. Plaatsen naast elkaar

**Nakijkpunten**

| Ticket op plaats 33, dan groep | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">4 plaatsen naast elkaar: A9 tot A12</code> |
| <code style="color:#e64980;font-weight:600">9</code>, dan <code style="color:#e64980;font-weight:600">5</code> | de vraag <code style="color:#845ef7">Groepsgrootte (1-8): </code> twee keer, dan <code style="color:#845ef7">5 plaatsen naast elkaar: B14 tot B18</code> |
| <code style="color:#e64980;font-weight:600">8</code> | <code style="color:#845ef7">8 plaatsen naast elkaar: C1 tot C8</code> |

**Aanpak.** De teller `naElkaar` staat binnen de lus over de rijen: elke rij begint opnieuw bij 0. Een bezette stoel zet hem ook op 0. Haalt hij de grootte van de groep, dan is stoel `s` de laatste van de reeks, en begint de reeks `groep - 1` stoelen eerder. Ook hier verlaat een gelabelde `break` allebei de lussen.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int groep;
do {
    System.out.print("Groepsgrootte (1-" + MAX_GROEP + "): ");
    groep = sc.nextInt();
} while (groep < 1 || groep > MAX_GROEP);
int groepRij = -1;
int groepStart = -1;
zoekGroep:
for (int r = 0; r < RIJEN; r++) {
    int naElkaar = 0;
    for (int s = 0; s < STOELEN_PER_RIJ; s++) {
        if (bezet[r][s]) {
            naElkaar = 0;
        } else {
            naElkaar++;
            if (naElkaar == groep) {
                groepRij = r;
                groepStart = s - groep + 1;
                break zoekGroep;
            }
        }
    }
}
if (groepRij == -1) {
    System.out.println("Geen " + groep + " plaatsen naast elkaar.");
} else {
    char gr = (char) ('A' + groepRij);
    System.out.println(groep + " plaatsen naast elkaar: " + gr + (groepStart + 1) + " tot " + gr + (groepStart + groep));
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `naElkaar` één keer voor de lussen declareren. Een reeks loopt dan door over het einde van een rij: voor een groep van 8 tellen de 7 vrije stoelen op het einde van rij B mee, en toont je programma `8 plaatsen naast elkaar: C-6 tot C1`.
> - `groepStart = s - groep`, zonder plus 1. Voor een groep van 4 toont je programma dan `A8 tot A11`, met A8 die al verkocht is.

---

## 7. Alles samen: vier plaatsen naast elkaar

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| concert <code style="color:#e64980;font-weight:600">1</code>, plaats <code style="color:#e64980;font-weight:600">33</code>, <code style="color:#e64980;font-weight:600">40</code>, groep <code style="color:#e64980;font-weight:600">4</code>, leeftijden <code style="color:#e64980;font-weight:600">19</code> <code style="color:#e64980;font-weight:600">30</code> <code style="color:#e64980;font-weight:600">22</code> <code style="color:#e64980;font-weight:600">45</code> | het groepsticket uit de opgave, met <code style="color:#845ef7">Korting -26  : 26,00 euro</code>, <code style="color:#845ef7">Te betalen   : 244,00 euro</code> en <code style="color:#845ef7">Waarvan btw  : 13,25 euro (6 % op de tickets)</code> |
| daarna, op het einde | <code style="color:#845ef7">A  XXXXXXXXXXXXXXXXXXXX</code> en <code style="color:#845ef7">Vrij: 145 van 200</code> |
| concert <code style="color:#e64980;font-weight:600">2</code>, plaats <code style="color:#e64980;font-weight:600">33</code>, <code style="color:#e64980;font-weight:600">40</code>, groep <code style="color:#e64980;font-weight:600">5</code>, vijf keer <code style="color:#e64980;font-weight:600">30</code> | <code style="color:#845ef7">Plaatsen     : B14 tot B18 (categorie 1)</code>, <code style="color:#845ef7">Korting -26  : 0,00 euro</code>, <code style="color:#845ef7">Te betalen   : 337,50 euro</code> en <code style="color:#845ef7">B  ....XXXXXX..XXXXXX..</code> |

**Aanpak.** Alles komt in de `else` van punt 6: enkel als er een reeks gevonden is, vraag je leeftijden. De korting tel je per persoon op, al afgerond, zoals bij één ticket. Pas na alle leeftijden zet je de stoelen van de reeks op bezet. De btw reken je één keer uit, op het totaal van de tickets na korting.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
// in de else van punt 6:
char gr = (char) ('A' + groepRij);
int groepCategorie = groepRij <= 2 ? 1 : groepRij <= 6 ? 2 : 3;
int groepBasis = groepCategorie == 1 ? 6500 : groepCategorie == 2 ? 4900 : 3500;
int groepKorting = 0;
for (int p = 1; p <= groep; p++) {
    System.out.print("Leeftijd persoon " + p + ": ");
    int l = sc.nextInt();
    if (l < 26) {
        groepKorting += (groepBasis * 20 + 50) / 100;
    }
}
for (int s = groepStart; s < groepStart + groep; s++) {
    bezet[groepRij][s] = true;
}
int tickets = groep * groepBasis - groepKorting;
int groepTotaal = tickets + groep * SERVICEKOST;
int groepBtw = tickets - (tickets * 100 + 53) / 106;
int service = groep * SERVICEKOST;
// ... het groepsticket, met de labels uit de opgave
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De btw per persoon afronden en dan optellen. Dat geeft 2 × 3,68 + 2 × 2,94 = 13,24 euro, één cent minder dan de 13,25 die de opgave vraagt.
> - De stoelen van de groep niet op bezet zetten. Het groepsticket klopt, maar rij A blijft `A  XXXXXXXX....XXXXXXXX`, en het zaalplan toont `Vrij: 149 van 200` in plaats van 145.

> [!TIP]
> **Zo kan het beter**
>
> - De beste vrije plaats van punt 5 is een groep van 1. Met `groep = 1` vindt de zoektocht van punt 6 dezelfde plaats; met een methode in hoofdstuk 03 heb je de eerste zoektocht dus niet meer nodig.
> - Een bedrag in cent tonen als euro staat elf keer in je code, telkens met dezelfde drie stukken. In hoofdstuk 03 wordt dat één methode `euro(int cent)`.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Concertzaal - ticketbalie");
        final int STOELEN_PER_RIJ = 20;
        final int RIJEN = 10;
        final int SERVICEKOST = 250;
        final int MAX_GROEP = 8;
        Scanner sc = new Scanner(System.in);

        // het zaalplan
        boolean[][] bezet = new boolean[RIJEN][STOELEN_PER_RIJ];
        int[] bezetteNummers = {1, 2, 3, 4, 5, 6, 7, 8, 13, 14, 15, 16, 17, 18, 19, 20, 25, 26, 27, 28, 29, 30, 52, 61, 62, 63, 64,
                101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 150, 199, 200};
        for (int nummer : bezetteNummers) {
            bezet[(nummer - 1) / STOELEN_PER_RIJ][(nummer - 1) % STOELEN_PER_RIJ] = true;
        }
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

        // het concert
        System.out.print("Concert (1, 2 of 3): ");
        int concert = sc.nextInt();
        String artiest = concert == 1 ? "The Lunar Echoes" : concert == 2 ? "Brasserie Blues Band" : "Noor & de Strijkers";
        String datum = concert == 1 ? "2026-12-04" : concert == 2 ? "2026-12-12" : "2027-01-09";

        // van plaatsnummer naar rij en stoel
        int plaatsnummer;
        boolean geldig;
        do {
            System.out.print("Plaatsnummer (1 - 200): ");
            plaatsnummer = sc.nextInt();
            geldig = false;
            if (plaatsnummer < 1 || plaatsnummer > RIJEN * STOELEN_PER_RIJ) {
                System.out.println("Plaats " + plaatsnummer + " bestaat niet.");
            } else if (bezet[(plaatsnummer - 1) / STOELEN_PER_RIJ][(plaatsnummer - 1) % STOELEN_PER_RIJ]) {
                System.out.println("Plaats " + (char) ('A' + (plaatsnummer - 1) / STOELEN_PER_RIJ) + ((plaatsnummer - 1) % STOELEN_PER_RIJ + 1) + " is al bezet.");
            } else {
                geldig = true;
            }
        } while (!geldig);
        int rijIndex = (plaatsnummer - 1) / STOELEN_PER_RIJ;
        char rij = (char) ('A' + rijIndex);
        int stoel = (plaatsnummer - 1) % STOELEN_PER_RIJ + 1;
        String plaats = rij + "" + stoel;

        // categorie en basisprijs, in cent
        int categorie = rijIndex <= 2 ? 1 : rijIndex <= 6 ? 2 : 3;
        int basisprijs = categorie == 1 ? 6500 : categorie == 2 ? 4900 : 3500;

        // jongerenkorting en servicekost
        System.out.print("Leeftijd: ");
        int leeftijd = sc.nextInt();
        int korting = leeftijd < 26 ? (basisprijs * 20 + 50) / 100 : 0;
        int teBetalen = basisprijs - korting + SERVICEKOST;

        // btw met gehele getallen
        int ticketdeel = basisprijs - korting;
        int zonderBtw = (ticketdeel * 100 + 53) / 106;
        int btw = ticketdeel - zonderBtw;
        bezet[rijIndex][stoel - 1] = true;

        // het ticket
        System.out.println("==============================");
        System.out.println("CONCERTZAAL - TICKET");
        System.out.println("==============================");
        System.out.println("Concert      : " + artiest + " (" + datum + ")");
        System.out.println("Plaats       : " + plaats + " (rij " + rij + ", stoel " + stoel + ")");
        System.out.println("Categorie    : " + categorie);
        System.out.println("Basisprijs   : " + basisprijs / 100 + "," + (basisprijs % 100 < 10 ? "0" : "") + basisprijs % 100 + " euro");
        System.out.println("Korting -26  : " + korting / 100 + "," + (korting % 100 < 10 ? "0" : "") + korting % 100 + " euro");
        System.out.println("Servicekost  : " + SERVICEKOST / 100 + "," + (SERVICEKOST % 100 < 10 ? "0" : "") + SERVICEKOST % 100 + " euro");
        System.out.println("Te betalen   : " + teBetalen / 100 + "," + (teBetalen % 100 < 10 ? "0" : "") + teBetalen % 100 + " euro");
        System.out.println("Waarvan btw  : " + btw / 100 + "," + (btw % 100 < 10 ? "0" : "") + btw % 100 + " euro (6 % op het ticket)");
        System.out.println("==============================");

        // vrije plaatsen per categorie
        int[] vrijPerCategorie = new int[3];
        int nogTeVerkopen = 0;
        for (int r = 0; r < RIJEN; r++) {
            int cat = r <= 2 ? 1 : r <= 6 ? 2 : 3;
            for (int s = 0; s < STOELEN_PER_RIJ; s++) {
                if (!bezet[r][s]) {
                    vrijPerCategorie[cat - 1]++;
                    nogTeVerkopen += cat == 1 ? 6500 : cat == 2 ? 4900 : 3500;
                }
            }
        }
        for (int c = 0; c < 3; c++) {
            System.out.println("Categorie " + (c + 1) + ": " + vrijPerCategorie[c] + " vrij");
        }
        System.out.println("Nog te verkopen: " + nogTeVerkopen / 100 + "," + (nogTeVerkopen % 100 < 10 ? "0" : "") + nogTeVerkopen % 100 + " euro");

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
            System.out.println("Beste vrije plaats: " + (char) ('A' + besteRij) + (besteStoel + 1) + " (categorie " + (besteRij <= 2 ? 1 : besteRij <= 6 ? 2 : 3) + ")");
        }

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
            int naElkaar = 0;
            for (int s = 0; s < STOELEN_PER_RIJ; s++) {
                if (bezet[r][s]) {
                    naElkaar = 0;
                } else {
                    naElkaar++;
                    if (naElkaar == groep) {
                        groepRij = r;
                        groepStart = s - groep + 1;
                        break zoekGroep;
                    }
                }
            }
        }
        if (groepRij == -1) {
            System.out.println("Geen " + groep + " plaatsen naast elkaar.");
        } else {
            char gr = (char) ('A' + groepRij);
                int groepCategorie = groepRij <= 2 ? 1 : groepRij <= 6 ? 2 : 3;
                int groepBasis = groepCategorie == 1 ? 6500 : groepCategorie == 2 ? 4900 : 3500;
                int groepKorting = 0;
                for (int p = 1; p <= groep; p++) {
                    System.out.print("Leeftijd persoon " + p + ": ");
                    int l = sc.nextInt();
                    if (l < 26) {
                        groepKorting += (groepBasis * 20 + 50) / 100;
                    }
                }
                for (int s = groepStart; s < groepStart + groep; s++) {
                    bezet[groepRij][s] = true;
                }
                int tickets = groep * groepBasis - groepKorting;
                int groepTotaal = tickets + groep * SERVICEKOST;
                int groepBtw = tickets - (tickets * 100 + 53) / 106;
                int service = groep * SERVICEKOST;
                System.out.println("==============================");
                System.out.println("CONCERTZAAL - GROEPSTICKET");
                System.out.println("==============================");
                System.out.println("Concert      : " + artiest + " (" + datum + ")");
                System.out.println("Plaatsen     : " + gr + (groepStart + 1) + " tot " + gr + (groepStart + groep) + " (categorie " + groepCategorie + ")");
                System.out.println("Tickets      : " + groep + " x " + groepBasis / 100 + "," + (groepBasis % 100 < 10 ? "0" : "") + groepBasis % 100 + " euro");
                System.out.println("Korting -26  : " + groepKorting / 100 + "," + (groepKorting % 100 < 10 ? "0" : "") + groepKorting % 100 + " euro");
                System.out.println("Servicekost  : " + service / 100 + "," + (service % 100 < 10 ? "0" : "") + service % 100 + " euro");
                System.out.println("Te betalen   : " + groepTotaal / 100 + "," + (groepTotaal % 100 < 10 ? "0" : "") + groepTotaal % 100 + " euro");
                System.out.println("Waarvan btw  : " + groepBtw / 100 + "," + (groepBtw % 100 < 10 ? "0" : "") + groepBtw % 100 + " euro (6 % op de tickets)");
                System.out.println("==============================");
        }

        // het zaalplan na de verkoop
        System.out.println("   12345678901234567890");
        int nogVrij = 0;
        for (int r = 0; r < RIJEN; r++) {
            String lijn = (char) ('A' + r) + "  ";
            for (int s = 0; s < STOELEN_PER_RIJ; s++) {
                if (bezet[r][s]) {
                    lijn += "X";
                } else {
                    lijn += ".";
                    nogVrij++;
                }
            }
            System.out.println(lijn);
        }
        System.out.println("Vrij: " + nogVrij + " van " + RIJEN * STOELEN_PER_RIJ);
    }
}
```

</details>
