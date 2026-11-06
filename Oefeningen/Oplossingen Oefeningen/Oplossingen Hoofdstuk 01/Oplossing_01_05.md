# Oplossing 01_05

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 01</b> &nbsp;·&nbsp; Gehele deling en rest &nbsp;·&nbsp; Fietsverhuur &nbsp;·&nbsp; bij `Oefening_01_05.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21. Het contractnummer is willekeurig: jouw nummer is anders.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- `java src/Main.java` in de map `Oefening_01_05` toont <code style="color:#845ef7">Fietsverhuur - balie</code>.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Fietsverhuur - balie");
    }
}
```

</details>

---

## 2. Het contractnummer

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">Contract: FV-2026-</code> gevolgd door vier cijfers, bij elke start andere |

- Het jaar staat in een constante; het getal ligt altijd tussen 1000 en 9999.

**Aanpak.** `random.nextInt(max - min + 1) + min` geeft een getal van `min` tot en met `max` (leerboek 1.4).

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
import java.util.Random;
// ...
final int JAAR = 2026;
Random random = new Random();
int volgnummer = random.nextInt(9999 - 1000 + 1) + 1000;
String contract = "FV-" + JAAR + "-" + volgnummer;
System.out.println("Contract: " + contract);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `random.nextInt(9999)`: ook getallen onder 1000, zoals `37`, en nooit 9999 zelf.

---

## 3. De huurduur

**Nakijkpunten**

| Vertrek en terug | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code>:<code style="color:#e64980;font-weight:600">45</code> tot <code style="color:#e64980;font-weight:600">13</code>:<code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Fiets: e-bike</code><br><code style="color:#845ef7">Duur: 3 u 25 min</code> |
| <code style="color:#e64980;font-weight:600">10</code>:<code style="color:#e64980;font-weight:600">0</code> tot <code style="color:#e64980;font-weight:600">11</code>:<code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Fiets: e-bike</code><br><code style="color:#845ef7">Duur: 1 u 0 min</code> |

- De vragen zijn <code style="color:#845ef7">Vertrek uur: </code>, <code style="color:#845ef7">Vertrek minuut: </code>, <code style="color:#845ef7">Terug uur: </code> en <code style="color:#845ef7">Terug minuut: </code>.

**Aanpak.** Een tijdstip omzetten naar minuten na middernacht: uur × 60 + minuut. Het verschil tussen twee tijdstippen is dan een gewone aftrekking. Terug naar uren en minuten gaat met de gehele deling en de rest door 60 (leerboek 1.3).

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Vertrek uur: ");
int vertrekUur = sc.nextInt();
System.out.print("Vertrek minuut: ");
int vertrekMinuut = sc.nextInt();
System.out.print("Terug uur: ");
int terugUur = sc.nextInt();
System.out.print("Terug minuut: ");
int terugMinuut = sc.nextInt();
int vertrek = vertrekUur * 60 + vertrekMinuut;
int terug = terugUur * 60 + terugMinuut;
int duur = terug - vertrek;
System.out.println("Fiets: " + type);
System.out.println("Duur: " + duur / 60 + " u " + duur % 60 + " min");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De uren en de minuten apart aftrekken: 13 − 9 = 4 uur en 10 − 45 = −35 minuten. Reken eerst alles om naar minuten.

---

## 4. Begonnen uren

**Nakijkpunten**

| Vertrek en terug | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code>:<code style="color:#e64980;font-weight:600">45</code> tot <code style="color:#e64980;font-weight:600">13</code>:<code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Begonnen uren: 4</code> |
| <code style="color:#e64980;font-weight:600">10</code>:<code style="color:#e64980;font-weight:600">0</code> tot <code style="color:#e64980;font-weight:600">11</code>:<code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Begonnen uren: 1</code> |
| <code style="color:#e64980;font-weight:600">10</code>:<code style="color:#e64980;font-weight:600">0</code> tot <code style="color:#e64980;font-weight:600">11</code>:<code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Begonnen uren: 2</code> |

**Aanpak.** Naar boven afronden met een gehele deling: tel er de deler min 1 bij op. `(duur + 59) / 60` duwt elke minuut boven een vol uur naar het volgende uur, maar laat een vol uur ongemoeid.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int begonnenUren = (duur + 59) / 60;
System.out.println("Begonnen uren: " + begonnenUren);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `duur / 60 + 1`: precies 60 minuten worden 2 begonnen uren.
> - `(duur + 60) / 60`: hetzelfde probleem, in een andere vorm.

---

## 5. De huurprijs

**Nakijkpunten**

| Vertrek en terug | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code>:<code style="color:#e64980;font-weight:600">45</code> tot <code style="color:#e64980;font-weight:600">13</code>:<code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Huurprijs: 24 euro</code><br><code style="color:#845ef7">Per minuut: 0.117 euro</code> |
| <code style="color:#e64980;font-weight:600">8</code>:<code style="color:#e64980;font-weight:600">30</code> tot <code style="color:#e64980;font-weight:600">17</code>:<code style="color:#e64980;font-weight:600">15</code> | <code style="color:#845ef7">Huurprijs: 30 euro</code><br><code style="color:#845ef7">Per minuut: 0.057 euro</code> |
| <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">0</code> tot <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">20</code> | <code style="color:#845ef7">Huurprijs: 6 euro</code><br><code style="color:#845ef7">Per minuut: 0.3 euro</code> |

**Aanpak.** `Math.min` geeft het kleinste van twee getallen (naslag 10.2); een ternary operator kan het ook. Voor de prijs per minuut moet één van de twee gehele getallen eerst een `double` worden. De cast `(double)` hoort bij wat er direct achter staat: `(double) huurprijs / duur` zet eerst `huurprijs` om, en deelt dan. Afronden op drie cijfers: × 1000, `Math.round`, / 1000.0.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int huurprijs = Math.min(begonnenUren * uurtarief, dagmaximum);
double perMinuut = Math.round((double) huurprijs / duur * 1000) / 1000.0;
System.out.println("Huurprijs: " + huurprijs + " euro");
System.out.println("Per minuut: " + perMinuut + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `(double) (huurprijs / duur)`. De haakjes laten Java eerst de gehele deling doen: 24 / 205 is 0, en dat wordt 0.0.
> - `huurprijs / duur * 1000` zonder cast: ook dat is 0, want `24 / 205` wordt eerst uitgerekend.

---

## 6. Te laat terug

**Nakijkpunten**

| Vertrek en terug | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">9</code>:<code style="color:#e64980;font-weight:600">45</code> tot <code style="color:#e64980;font-weight:600">13</code>:<code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Te laat: false</code><br><code style="color:#845ef7">Boete: 0 euro</code><br><code style="color:#845ef7">Te betalen bij terugkomst: 24 euro</code> |
| <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">5</code> tot <code style="color:#e64980;font-weight:600">20</code>:<code style="color:#e64980;font-weight:600">30</code> | <code style="color:#845ef7">Te laat: true</code><br><code style="color:#845ef7">Boete: 10 euro</code><br><code style="color:#845ef7">Te betalen bij terugkomst: 40 euro</code> |
| <code style="color:#e64980;font-weight:600">14</code>:<code style="color:#e64980;font-weight:600">5</code> tot <code style="color:#e64980;font-weight:600">20</code>:<code style="color:#e64980;font-weight:600">0</code> | <code style="color:#845ef7">Te laat: false</code><br><code style="color:#845ef7">Boete: 0 euro</code><br><code style="color:#845ef7">Te betalen bij terugkomst: 30 euro</code> |

**Aanpak.** Je vergelijkt het tijdstip van terugkomst in minuten met het sluitingsuur in minuten: `terug > SLUITINGSUUR * 60`. Om precies 20:00 is dat 1200 > 1200, dus `false`. De boete kies je met de `boolean` als voorwaarde.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final int SLUITINGSUUR = 20;
final int BOETE = 10;
// ...
boolean teLaat = terug > SLUITINGSUUR * 60;
int boete = teLaat ? BOETE : 0;
int bijTerugkomst = huurprijs + boete;
System.out.println("Te laat: " + teLaat);
System.out.println("Boete: " + boete + " euro");
System.out.println("Te betalen bij terugkomst: " + bijTerugkomst + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `terugUur > SLUITINGSUUR`. Dan is 20:30 niet te laat, want 20 is niet groter dan 20.
> - `terugUur >= SLUITINGSUUR`. Dan is 20:00 wel te laat.

---

## 7. Alles samen: het huurcontract

**Nakijkpunten**

| Invoer | Wat het contract toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">9</code> <code style="color:#e64980;font-weight:600">45</code> <code style="color:#e64980;font-weight:600">13</code> <code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Fiets         : e-bike (6 euro per uur, max 30 euro)</code><br><code style="color:#845ef7">Vertrek       : 9:45</code><br><code style="color:#845ef7">Duur          : 3 u 25 min (4 begonnen uren)</code><br><code style="color:#845ef7">Huurprijs     : 24 euro</code><br><code style="color:#845ef7">Bij vertrek   : 150 euro (waarborg)</code><br><code style="color:#845ef7">Bij terugkomst: 24 euro (waarborg terug)</code> |
| <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">8</code> <code style="color:#e64980;font-weight:600">5</code> <code style="color:#e64980;font-weight:600">19</code> <code style="color:#e64980;font-weight:600">40</code> | <code style="color:#845ef7">Fiets         : stadsfiets (3 euro per uur, max 15 euro)</code><br><code style="color:#845ef7">Vertrek       : 8:05</code><br><code style="color:#845ef7">Duur          : 11 u 35 min (12 begonnen uren)</code><br><code style="color:#845ef7">Huurprijs     : 15 euro</code><br><code style="color:#845ef7">Bij vertrek   : 50 euro (waarborg)</code> |
| <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">14</code> <code style="color:#e64980;font-weight:600">5</code> <code style="color:#e64980;font-weight:600">20</code> <code style="color:#e64980;font-weight:600">30</code> | <code style="color:#845ef7">Vertrek       : 14:05</code><br><code style="color:#845ef7">Huurprijs     : 40 euro</code><br><code style="color:#845ef7">Boete         : 10 euro</code><br><code style="color:#845ef7">Bij vertrek   : 100 euro (waarborg)</code><br><code style="color:#845ef7">Bij terugkomst: 50 euro (waarborg terug)</code> |

**Aanpak.** Het typenummer bepaalt vier waarden: het type, het uurtarief, het dagmaximum en de waarborg. Elk krijgt zijn eigen ternary operator met dezelfde voorwaarden. De tijden toon je met een extra nul als de minuut kleiner is dan 10, zoals de cent in 01_03.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Type (1 = stadsfiets, 2 = e-bike, 3 = bakfiets): ");
int typeNummer = sc.nextInt();
String type = typeNummer == 1 ? "stadsfiets" : typeNummer == 2 ? "e-bike" : "bakfiets";
int uurtarief = typeNummer == 1 ? 3 : typeNummer == 2 ? 6 : 8;
int dagmaximum = typeNummer == 1 ? 15 : typeNummer == 2 ? 30 : 40;
int waarborg = typeNummer == 1 ? 50 : typeNummer == 2 ? 150 : 100;
// ... de huurduur, de prijs en de boete zoals in punt 3 tot en met 6
String vertrekTijd = vertrekUur + ":" + (vertrekMinuut < 10 ? "0" : "") + vertrekMinuut;
String terugTijd = terugUur + ":" + (terugMinuut < 10 ? "0" : "") + terugMinuut;
System.out.println("==============================");
System.out.println("FIETSVERHUUR - HUURCONTRACT");
System.out.println("==============================");
System.out.println("Contract      : " + contract);
System.out.println("Fiets         : " + type + " (" + uurtarief + " euro per uur, max " + dagmaximum + " euro)");
System.out.println("Vertrek       : " + vertrekTijd);
System.out.println("Terug         : " + terugTijd);
System.out.println("Duur          : " + duur / 60 + " u " + duur % 60 + " min (" + begonnenUren + " begonnen uren)");
System.out.println("Huurprijs     : " + huurprijs + " euro");
System.out.println("Boete         : " + boete + " euro");
System.out.println("------------------------------");
System.out.println("Bij vertrek   : " + waarborg + " euro (waarborg)");
System.out.println("Bij terugkomst: " + bijTerugkomst + " euro (waarborg terug)");
System.out.println("==============================");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Het typenummer pas vragen na de tijden. Dan rekent je programma de prijs uit met de vaste e-bike van punt 3, of compileert het niet omdat `uurtarief` nog niet bestaat.
> - De extra nul vergeten: <code style="color:#845ef7">Vertrek       : 8:5</code>.

> [!TIP]
> **Zo kan het beter**
>
> - Vier ternary operators met dezelfde drie voorwaarden: dat is een teken dat er een betere vorm bestaat. In hoofdstuk 02 maak je er één `switch` van, en in hoofdstuk 06 een enum met het tarief erin.
> - De prijs per minuut uit punt 5 staat niet meer op het contract. Ze hielp om typecasting te oefenen; laat ze weg uit je code als je ze niet meer toont.

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
        Random random = new Random();
        Scanner sc = new Scanner(System.in);

        // het contractnummer
        int volgnummer = random.nextInt(9999 - 1000 + 1) + 1000;
        String contract = "FV-" + JAAR + "-" + volgnummer;

        // de fiets
        System.out.print("Type (1 = stadsfiets, 2 = e-bike, 3 = bakfiets): ");
        int typeNummer = sc.nextInt();
        String type = typeNummer == 1 ? "stadsfiets" : typeNummer == 2 ? "e-bike" : "bakfiets";
        int uurtarief = typeNummer == 1 ? 3 : typeNummer == 2 ? 6 : 8;
        int dagmaximum = typeNummer == 1 ? 15 : typeNummer == 2 ? 30 : 40;
        int waarborg = typeNummer == 1 ? 50 : typeNummer == 2 ? 150 : 100;

        // de huurduur
        System.out.print("Vertrek uur: ");
        int vertrekUur = sc.nextInt();
        System.out.print("Vertrek minuut: ");
        int vertrekMinuut = sc.nextInt();
        System.out.print("Terug uur: ");
        int terugUur = sc.nextInt();
        System.out.print("Terug minuut: ");
        int terugMinuut = sc.nextInt();
        int vertrek = vertrekUur * 60 + vertrekMinuut;
        int terug = terugUur * 60 + terugMinuut;
        int duur = terug - vertrek;

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
        System.out.println("Fiets         : " + type + " (" + uurtarief + " euro per uur, max " + dagmaximum + " euro)");
        System.out.println("Vertrek       : " + vertrekTijd);
        System.out.println("Terug         : " + terugTijd);
        System.out.println("Duur          : " + duur / 60 + " u " + duur % 60 + " min (" + begonnenUren + " begonnen uren)");
        System.out.println("Huurprijs     : " + huurprijs + " euro");
        System.out.println("Boete         : " + boete + " euro");
        System.out.println("------------------------------");
        System.out.println("Bij vertrek   : " + waarborg + " euro (waarborg)");
        System.out.println("Bij terugkomst: " + bijTerugkomst + " euro (waarborg terug)");
        System.out.println("==============================");
    }
}
```

</details>
