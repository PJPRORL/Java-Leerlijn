# Oplossing 01_02

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 01</b> &nbsp;·&nbsp; Gehele deling en rest &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_01_02.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Het bestelnummer is willekeurig: jouw nummer is anders.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- `java src/Main.java` in de map `Oefening_01_02` toont <code style="color:#845ef7">Bit & Byte - magazijn</code>.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - magazijn");
    }
}
```

</details>

---

## 2. Het bestelnummer

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">Bestelnummer: BB-2026-</code> gevolgd door vijf cijfers, bij elke start andere |

- Het jaar staat in een constante.
- Het getal ligt altijd tussen 10000 en 99999: nooit vier en nooit zes cijfers.

**Aanpak.** `random.nextInt(n)` geeft 0 tot en met `n - 1`. Voor een bereik van `min` tot en met `max` is dat `random.nextInt(max - min + 1) + min` (leerboek 1.4). Met `+` plak je tekst en getallen aan elkaar tot één `String`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
import java.util.Random;
// ...
final int JAAR = 2026;
Random random = new Random();
int volgnummer = random.nextInt(99999 - 10000 + 1) + 10000;
String bestelnummer = "BB-" + JAAR + "-" + volgnummer;
System.out.println("Bestelnummer: " + bestelnummer);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `random.nextInt(99999)`. Dat geeft ook getallen onder 10000, zoals `482`, en nooit 99999 zelf.
> - `random.nextInt(10000, 99999)`. Die variant bestaat sinds Java 17, maar de bovengrens hoort er niet bij: 99999 komt nooit voor. Wil je ze gebruiken, schrijf dan `nextInt(10000, 100000)`.

---

## 3. De bestellijn

**Nakijkpunten**

| Aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Korting: 0 %</code><br><code style="color:#845ef7">Lijnbedrag: 796.0 euro</code> |
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Korting: 5 %</code><br><code style="color:#845ef7">Lijnbedrag: 945.25 euro</code> |
| <code style="color:#e64980;font-weight:600">10</code> | <code style="color:#845ef7">Korting: 10 %</code><br><code style="color:#845ef7">Lijnbedrag: 1791.0 euro</code> |

- Telkens voorafgegaan door <code style="color:#845ef7">Artikel: Ryzen 5 7600X (199.0 euro)</code>.

**Aanpak.** De korting kies je met twee ternary operators, van de hoogste grens naar de laagste. Daarna het lijnbedrag: aantal × prijs × (100 − korting) / 100. Wil je het meteen afronden op de cent, dan vermenigvuldig je weer met 100. ×100 en /100 heffen elkaar dan op: `Math.round(aantal * prijs * (100 - korting)) / 100.0` doet beide in één keer. Omdat `prijs` een `double` is, rekent Java die hele vermenigvuldiging met `double`: geen gehele deling.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Aantal: ");
int aantal = sc.nextInt();
int korting = aantal >= 10 ? 10 : aantal >= 5 ? 5 : 0;
double lijnbedrag = Math.round(aantal * prijs * (100 - korting)) / 100.0;
System.out.println("Artikel: " + naam + " (" + prijs + " euro)");
System.out.println("Korting: " + korting + " %");
System.out.println("Lijnbedrag: " + lijnbedrag + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `prijs * aantal * ((100 - korting) / 100)`. De haakjes laten Java eerst `95 / 100` uitrekenen: een gehele deling, dus 0. Je programma toont <code style="color:#845ef7">Lijnbedrag: 0.0 euro</code>.
> - De grenzen van laag naar hoog controleren: `aantal >= 5 ? 5 : aantal >= 10 ? 10 : 0`. Dan krijgt een bestelling van 10 stuks maar 5 % korting, want 10 is ook groter dan of gelijk aan 5.

> [!TIP]
> **Zo kan het beter**
>
> - Een korting als geheel percentage (`5`) is leesbaarder dan als kommagetal (`0.05`), en je vermijdt er een `double` mee.

---

## 4. De verzenddozen

**Nakijkpunten**

| Aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Dozen: 2</code><br><code style="color:#845ef7">In de laatste doos: 2 stuks</code> |
| <code style="color:#e64980;font-weight:600">6</code> | <code style="color:#845ef7">Dozen: 2</code><br><code style="color:#845ef7">In de laatste doos: 3 stuks</code> |
| <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Dozen: 1</code><br><code style="color:#845ef7">In de laatste doos: 1 stuk</code> |

**Aanpak.** `aantal / 3` geeft de volle dozen. Tel je er eerst `3 - 1` bij op, dan duwt elke onvolle doos het resultaat één hoger: `(aantal + 2) / 3` is naar boven afronden met een gehele deling. De rest `aantal % 3` zegt hoeveel stuks er in de onvolle doos zitten, maar is 0 als alle dozen vol zijn: dan bevat de laatste doos er 3. Dat kies je met een ternary operator, net als `stuk` of `stuks`.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final int PER_DOOS = 3;
// ...
int dozen = (aantal + PER_DOOS - 1) / PER_DOOS;
int rest = aantal % PER_DOOS;
int inLaatsteDoos = rest == 0 ? PER_DOOS : rest;
String stuks = inLaatsteDoos == 1 ? "stuk" : "stuks";
System.out.println("Dozen: " + dozen);
System.out.println("In de laatste doos: " + inLaatsteDoos + " " + stuks);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `aantal / PER_DOOS` als aantal dozen: bij 5 stuks geeft dat 1 doos, en blijven er 2 artikelen achter in het magazijn.
> - `aantal / PER_DOOS + 1`: werkt bij 5, maar geeft 3 dozen bij 6 stuks.
> - De rest rechtstreeks tonen: bij 6 stuks toont je programma dan <code style="color:#845ef7">In de laatste doos: 0 stuks</code>.

---

## 5. Verzendkost, totaal en spaarpunten

**Nakijkpunten**

| Aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Totaal: 945.25 euro</code><br><code style="color:#845ef7">Spaarpunten: 94</code> |
| <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Totaal: 199.0 euro</code><br><code style="color:#845ef7">Spaarpunten: 19</code> |

**Aanpak.** Een `double` past niet zomaar in een `int`: Java eist een expliciete cast, `(int)`, omdat je er informatie mee verliest (leerboek 1.3, *Typecasting*). Een cast kapt af: precies wat *volle* 10 euro betekent. Zet de deling tussen haakjes, zodat de cast het resultaat van de deling afkapt.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
double verzendkost = lijnbedrag < 50 ? 4.99 : 0.0;
double totaal = Math.round((lijnbedrag + verzendkost) * 100) / 100.0;
int spaarpunten = (int) (lijnbedrag / 10);
System.out.println("Verzendkost: " + verzendkost + " euro");
System.out.println("Totaal: " + totaal + " euro");
System.out.println("Spaarpunten: " + spaarpunten);
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `int spaarpunten = lijnbedrag / 10;` compileert niet: *possible lossy conversion from double to int*.
> - `(int) Math.round(lijnbedrag / 10)`. Dat rondt af in plaats van af te kappen: 945.25 euro geeft dan 95 punten.
> - De verzendkost meetellen voor de spaarpunten. De opgave zegt: per volle 10 euro van het *lijnbedrag*.

---

## 6. Levertermijn en leverdag

**Nakijkpunten**

| Aantal en besteldag | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">5</code> en <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Levertermijn: 5 werkdagen</code><br><code style="color:#845ef7">Leverdag: werkdag 4 (volgende week)</code> |
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Levertermijn: 1 werkdag</code><br><code style="color:#845ef7">Leverdag: werkdag 1 (volgende week)</code> |
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Levertermijn: 1 werkdag</code><br><code style="color:#845ef7">Leverdag: werkdag 3 (deze week)</code> |

**Aanpak.** Rekenen met een rest werkt het eenvoudigst als je vanaf 0 telt. Van een besteldag 1 tot en met 5 maak je 0 tot en met 4, je telt er de levertermijn bij, en neemt de rest na deling door 5. Dat geeft opnieuw 0 tot en met 4; tel er 1 bij om terug een werkdag van 1 tot en met 5 te krijgen. De gehele deling door 5 zegt hoeveel volle weken je verder bent: 0 is deze week.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Besteldag (1 = maandag, 5 = vrijdag): ");
int besteldag = sc.nextInt();
int levertermijn = aantal <= voorraad ? 1 : 5;
String werkdagen = levertermijn == 1 ? "werkdag" : "werkdagen";
int dagenVerder = besteldag - 1 + levertermijn;
int leverdag = dagenVerder % 5 + 1;
String week = dagenVerder / 5 == 0 ? "deze week" : "volgende week";
System.out.println("Levertermijn: " + levertermijn + " " + werkdagen);
System.out.println("Leverdag: werkdag " + leverdag + " (" + week + ")");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `(besteldag + levertermijn) % 5` zonder eerst 1 af te trekken. Op donderdag met 1 werkdag geeft dat werkdag 0.
> - De levertermijn vergelijken met de voorraad in plaats van het aantal: `voorraad > 0`. Bij 5 stuks en een voorraad van 3 kan Bit & Byte niet alles morgen leveren.

> [!TIP]
> **Zo kan het beter**
>
> - Een levertermijn van 5 werkdagen valt altijd op dezelfde weekdag, een week later. Test daarom ook met een levertermijn van 1 op een vrijdag: daar zie je of je rest klopt.

---

## 7. Alles samen: de pakbon

**Nakijkpunten**

| Invoer | Wat de pakbon toont |
|---|---|
| <code style="color:#e64980;font-weight:600">Ryzen 5 7600X</code> <code style="color:#e64980;font-weight:600">199,00</code> <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">5</code> <code style="color:#e64980;font-weight:600">4</code> | <code style="color:#845ef7">Aantal       : 5 x 199.0 euro (korting 5 %)</code><br><code style="color:#845ef7">Lijnbedrag   : 945.25 euro</code><br><code style="color:#845ef7">Spaarpunten  : 94</code><br><code style="color:#845ef7">Dozen        : 2 (laatste doos: 2 stuks)</code><br><code style="color:#845ef7">Leverdag     : werkdag 4 (volgende week)</code> |
| <code style="color:#e64980;font-weight:600">Kabelbinders</code> <code style="color:#e64980;font-weight:600">1,99</code> <code style="color:#e64980;font-weight:600">40</code> <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Lijnbedrag   : 5.97 euro</code><br><code style="color:#845ef7">Verzendkost  : 4.99 euro</code><br><code style="color:#845ef7">Totaal       : 10.96 euro</code><br><code style="color:#845ef7">Spaarpunten  : 0</code><br><code style="color:#845ef7">Dozen        : 1 (laatste doos: 3 stuks)</code><br><code style="color:#845ef7">Levertermijn : 1 werkdag</code><br><code style="color:#845ef7">Leverdag     : werkdag 2 (deze week)</code> |
| <code style="color:#e64980;font-weight:600">Kabelbinders</code> <code style="color:#e64980;font-weight:600">1,99</code> <code style="color:#e64980;font-weight:600">40</code> <code style="color:#e64980;font-weight:600">10</code> <code style="color:#e64980;font-weight:600">5</code> | <code style="color:#845ef7">Aantal       : 10 x 1.99 euro (korting 10 %)</code><br><code style="color:#845ef7">Lijnbedrag   : 17.91 euro</code><br><code style="color:#845ef7">Totaal       : 22.9 euro</code><br><code style="color:#845ef7">Spaarpunten  : 1</code><br><code style="color:#845ef7">Dozen        : 4 (laatste doos: 1 stuk)</code><br><code style="color:#845ef7">Leverdag     : werkdag 1 (volgende week)</code> |
| <code style="color:#e64980;font-weight:600">GeForce RTX 4070</code> <code style="color:#e64980;font-weight:600">589,00</code> <code style="color:#e64980;font-weight:600">0</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">3</code> | <code style="color:#845ef7">Levertermijn : 5 werkdagen</code><br><code style="color:#845ef7">Leverdag     : werkdag 3 (volgende week)</code> |

**Aanpak.** Het artikel komt nu van de magazijnier, zoals in 01_01: `nextLine()` voor de naam, `nextDouble()` voor de prijs, `nextInt()` voor de voorraad. Omdat de naam als eerste komt, is er geen Enter-probleem. Alle berekeningen van punt 2 tot en met 6 blijven staan; enkel hun `println`-regels maken plaats voor de pakbon.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.println("==============================");
System.out.println("BIT & BYTE - PAKBON");
System.out.println("==============================");
System.out.println("Bestelnummer : " + bestelnummer);
System.out.println("Artikel      : " + naam);
System.out.println("Aantal       : " + aantal + " x " + prijs + " euro (korting " + korting + " %)");
System.out.println("Lijnbedrag   : " + lijnbedrag + " euro");
System.out.println("Verzendkost  : " + verzendkost + " euro");
System.out.println("Totaal       : " + totaal + " euro");
System.out.println("Spaarpunten  : " + spaarpunten);
System.out.println("Dozen        : " + dozen + " (laatste doos: " + inLaatsteDoos + " " + stuks + ")");
System.out.println("Levertermijn : " + levertermijn + " " + werkdagen);
System.out.println("Leverdag     : werkdag " + leverdag + " (" + week + ")");
System.out.println("==============================");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De vaste waarden van punt 3 laten staan naast de nieuwe invoer. Twee variabelen met dezelfde naam in dezelfde methode compileren niet: *variable naam is already defined*.
> - Het lijnbedrag niet afronden. Bij 10 kabelbinders toont je pakbon dan <code style="color:#845ef7">Lijnbedrag   : 17.909999999999997 euro</code>.

> [!TIP]
> **Zo kan het beter**
>
> - Bij een bestelling van 10 kabelbinders zie je alle regels tegelijk: korting, verzendkost onder 50 euro, 1 spaarpunt, 4 dozen met 1 stuk in de laatste, en een levering over het weekend. Eén goed gekozen testgeval vindt meer fouten dan tien gewone.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Random;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - magazijn");
        final int JAAR = 2026;
        final int PER_DOOS = 3;
        Random random = new Random();
        Scanner sc = new Scanner(System.in);

        // het bestelnummer
        int volgnummer = random.nextInt(99999 - 10000 + 1) + 10000;
        String bestelnummer = "BB-" + JAAR + "-" + volgnummer;

        // de bestellijn
        System.out.print("Artikel: ");
        String naam = sc.nextLine();
        System.out.print("Prijs: ");
        double prijs = sc.nextDouble();
        System.out.print("Voorraad: ");
        int voorraad = sc.nextInt();
        System.out.print("Aantal: ");
        int aantal = sc.nextInt();
        int korting = aantal >= 10 ? 10 : aantal >= 5 ? 5 : 0;
        double lijnbedrag = Math.round(aantal * prijs * (100 - korting)) / 100.0;

        // de verzenddozen
        int dozen = (aantal + PER_DOOS - 1) / PER_DOOS;
        int rest = aantal % PER_DOOS;
        int inLaatsteDoos = rest == 0 ? PER_DOOS : rest;
        String stuks = inLaatsteDoos == 1 ? "stuk" : "stuks";

        // verzendkost, totaal en spaarpunten
        double verzendkost = lijnbedrag < 50 ? 4.99 : 0.0;
        double totaal = Math.round((lijnbedrag + verzendkost) * 100) / 100.0;
        int spaarpunten = (int) (lijnbedrag / 10);

        // levertermijn en leverdag
        System.out.print("Besteldag (1 = maandag, 5 = vrijdag): ");
        int besteldag = sc.nextInt();
        int levertermijn = aantal <= voorraad ? 1 : 5;
        String werkdagen = levertermijn == 1 ? "werkdag" : "werkdagen";
        int dagenVerder = besteldag - 1 + levertermijn;
        int leverdag = dagenVerder % 5 + 1;
        String week = dagenVerder / 5 == 0 ? "deze week" : "volgende week";

        // de pakbon
        System.out.println("==============================");
        System.out.println("BIT & BYTE - PAKBON");
        System.out.println("==============================");
        System.out.println("Bestelnummer : " + bestelnummer);
        System.out.println("Artikel      : " + naam);
        System.out.println("Aantal       : " + aantal + " x " + prijs + " euro (korting " + korting + " %)");
        System.out.println("Lijnbedrag   : " + lijnbedrag + " euro");
        System.out.println("Verzendkost  : " + verzendkost + " euro");
        System.out.println("Totaal       : " + totaal + " euro");
        System.out.println("Spaarpunten  : " + spaarpunten);
        System.out.println("Dozen        : " + dozen + " (laatste doos: " + inLaatsteDoos + " " + stuks + ")");
        System.out.println("Levertermijn : " + levertermijn + " " + werkdagen);
        System.out.println("Leverdag     : werkdag " + leverdag + " (" + week + ")");
        System.out.println("==============================");
    }
}
```

</details>
