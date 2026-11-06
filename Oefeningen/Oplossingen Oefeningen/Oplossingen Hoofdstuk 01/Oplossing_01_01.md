# Oplossing 01_01

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 01</b> &nbsp;·&nbsp; Variabelen en rekenen &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_01_01.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Daarom staan de ingetypte kommagetallen met een komma.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- Er is een map `Oefening_01_01/src` met een bestand `Main.java`.
- `java src/Main.java` toont <code style="color:#845ef7">Bit & Byte - kassa</code>, en niets anders.

**Aanpak.** Een `public` klasse moet in een bestand met dezelfde naam staan: `Main` in `Main.java` (leerboek 1.1). `main` is het startpunt van elk programma.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - kassa");
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `java Main.java` starten vanuit de map `Oefening_01_01`. Het bestand staat in `src`: de juiste opdracht is `java src/Main.java`.
> - `System.out.println` met een kleine `s`. Java maakt verschil tussen hoofdletters en kleine letters: `system` bestaat niet.

---

## 2. Het artikel in variabelen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">B650 Tomahawk van MSI</code><br><code style="color:#845ef7">Prijs met btw: 219.0 euro</code><br><code style="color:#845ef7">Op voorraad: 12 stuks</code> |

- De naam en het merk zijn een `String`, de prijs een `double`, de voorraad een `int`.

**Aanpak.** Een prijs heeft cijfers na de komma, dus een `double`. Een voorraad telt stuks: een `int`. Tekst is een `String`, een referentietype met een hoofdletter (leerboek 1.2). Met `+` plak je tekst en getallen aan elkaar; Java zet het getal daarbij om naar tekst.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String naam = "B650 Tomahawk";
String merk = "MSI";
double prijs = 219.00;
int voorraad = 12;
System.out.println(naam + " van " + merk);
System.out.println("Prijs met btw: " + prijs + " euro");
System.out.println("Op voorraad: " + voorraad + " stuks");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `int prijs = 219.00;` compileert niet: *possible lossy conversion from double to int*. Ook `float prijs = 219.00;` niet: een kommagetal zonder `f` is een `double` (leerboek 1.2).
> - De spatie vergeten in `" van "`. Dan toont je programma `B650 Tomahawkvan MSI`.

> [!TIP]
> **Zo kan het beter**
>
> - Een `double` is voor bedragen niet ideaal: hij kan de meeste kommagetallen niet exact bewaren. Je ziet het effect in punt 3; in 01_03 rekent Bit & Byte daarom in cent, en in het werkveld gebruik je `BigDecimal` (naslag 10.2).

---

## 3. De prijs zonder btw

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">Prijs zonder btw: 180.99 euro</code><br><code style="color:#845ef7">Btw: 38.01 euro</code> |

- Het btw-tarief staat in een constante met `final`.
- 180.99 + 38.01 = 219.00.

**Aanpak.** Een prijs inclusief 21 % btw is 121 % van de prijs zonder btw; de prijs zonder btw is dus de prijs gedeeld door 1,21. `final` maakt van een variabele een constante, met een naam in hoofdletters (leerboek 1.2). Afronden op twee cijfers: vermenigvuldig met 100, rond af met `Math.round`, deel door `100.0` (naslag 10.2). De btw reken je uit met de niet-afgeronde prijs zonder btw, en pas het resultaat rond je af. Zo maak je maar één afrondingsfout, en niet twee na elkaar.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final double BTW = 0.21;
// ...
double prijsZonderBtw = Math.round(prijs / (1 + BTW) * 100) / 100.0;
double btw = Math.round((prijs - prijs / (1 + BTW)) * 100) / 100.0;
System.out.println("Prijs zonder btw: " + prijsZonderBtw + " euro");
System.out.println("Btw: " + btw + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Niet afronden. Dan toont je programma <code style="color:#845ef7">Prijs zonder btw: 180.9917355371901 euro</code> en <code style="color:#845ef7">Btw: 38.008264462809905 euro</code>.
> - Delen door `100` in plaats van `100.0`. `Math.round` geeft een `long`, en `long / int` is een gehele deling: je krijgt <code style="color:#845ef7">Prijs zonder btw: 180 euro</code>.
> - `prijs / 1 + BTW` zonder haakjes. De deling gaat voor de optelling (leerboek 1.3, *Voorrang*): dat geeft 219.21.
> - `prijs - prijsZonderBtw` zonder opnieuw af te ronden: <code style="color:#845ef7">Btw: 38.00999999999999 euro</code>.

> [!TIP]
> **Zo kan het beter**
>
> - `BTW` als `0.21` en de deling door `1 + BTW` leest beter dan een losse `1.21` in je code: wie het programma leest, ziet meteen waar het getal vandaan komt.

---

## 4. Het artikel inlezen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">Ryzen 5 7600X</code> <code style="color:#e64980;font-weight:600">AMD</code> <code style="color:#e64980;font-weight:600">199,00</code> <code style="color:#e64980;font-weight:600">3</code> | <code style="color:#845ef7">Ryzen 5 7600X van AMD</code><br><code style="color:#845ef7">Prijs met btw: 199.0 euro</code><br><code style="color:#845ef7">Op voorraad: 3 stuks</code><br><code style="color:#845ef7">Prijs zonder btw: 164.46 euro</code><br><code style="color:#845ef7">Btw: 34.54 euro</code> |
| <code style="color:#e64980;font-weight:600">Ryzen 5 7600X</code> <code style="color:#e64980;font-weight:600">AMD</code> <code style="color:#e64980;font-weight:600">199.00</code> | het programma stopt met een `InputMismatchException` |

- De vragen zijn <code style="color:#845ef7">Artikel: </code>, <code style="color:#845ef7">Merk: </code>, <code style="color:#845ef7">Prijs: </code> en <code style="color:#845ef7">Voorraad: </code>, met `print`: wat de gebruiker typt, komt op dezelfde regel.

**Aanpak.** `import java.util.Scanner;` bovenaan, één `Scanner` op `System.in` (leerboek 1.4). Tekst met spaties lees je met `nextLine()`, een kommagetal met `nextDouble()`, een geheel getal met `nextInt()`. De variabelen blijven dezelfde als in punt 2: enkel de waarde na `=` verandert.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - kassa");
        final double BTW = 0.21;
        Scanner sc = new Scanner(System.in);

        System.out.print("Artikel: ");
        String naam = sc.nextLine();
        System.out.print("Merk: ");
        String merk = sc.nextLine();
        System.out.print("Prijs: ");
        double prijs = sc.nextDouble();
        System.out.print("Voorraad: ");
        int voorraad = sc.nextInt();
        // ... de regels van punt 2 en 3 blijven ongewijzigd
    }
}
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `sc.next()` voor de naam. `next()` leest één woord: van `Ryzen 5 7600X` blijft `Ryzen` over, en de rest van de regel komt in het merk terecht. Daarna probeert `nextDouble()` het merk als prijs te lezen, en stopt je programma met een `InputMismatchException`.
> - `println` voor de vraag. Dan typt de gebruiker op de regel eronder; het werkt, maar de nakijkpunten verwachten de invoer achter de vraag.
> - De `Scanner` binnen elke vraag opnieuw maken. Eén `Scanner` voor het hele programma volstaat.

---

## 5. Een verkoop

**Nakijkpunten**

| Invoer: prijs en aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">199,00</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Subtotaal: 398.0 euro</code><br><code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Te betalen: 398.0 euro</code> |
| <code style="color:#e64980;font-weight:600">1,99</code> en <code style="color:#e64980;font-weight:600">3</code> | <code style="color:#845ef7">Subtotaal: 5.97 euro</code><br><code style="color:#845ef7">Verzendkost: 4.99 euro</code><br><code style="color:#845ef7">Te betalen: 10.96 euro</code> |
| <code style="color:#e64980;font-weight:600">25,00</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Subtotaal: 50.0 euro</code><br><code style="color:#845ef7">Verzendkost: 0.0 euro</code><br><code style="color:#845ef7">Te betalen: 50.0 euro</code> |

**Aanpak.** De ternary operator `voorwaarde ? waarde1 : waarde2` kiest één van twee waarden (leerboek 1.3). "Onder 50 euro" is `subtotaal < 50`: bij precies 50 is de voorwaarde onwaar, en de verzending dus gratis. Zowel het subtotaal als het bedrag *Te betalen* rond je af, want allebei kunnen ze een rekenfout van een `double` bevatten.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Aantal: ");
int aantal = sc.nextInt();
double subtotaal = Math.round(prijs * aantal * 100) / 100.0;
double verzendkost = subtotaal < 50 ? 4.99 : 0.0;
double teBetalen = Math.round((subtotaal + verzendkost) * 100) / 100.0;
System.out.println("Subtotaal: " + subtotaal + " euro");
System.out.println("Verzendkost: " + verzendkost + " euro");
System.out.println("Te betalen: " + teBetalen + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `subtotaal <= 50`. Dan betaalt een klant van precies 50 euro toch 4.99 verzendkost.
> - Het subtotaal niet afronden. Bij een prijs van <code style="color:#e64980;font-weight:600">0,10</code> en aantal <code style="color:#e64980;font-weight:600">3</code> toont je programma dan <code style="color:#845ef7">Subtotaal: 0.30000000000000004 euro</code>.
> - `int verzendkost = ... ? 4.99 : 0;` compileert niet: 4.99 past niet in een `int`.

> [!TIP]
> **Zo kan het beter**
>
> - Leg ook `50` en `4.99` vast in constanten, zoals het btw-tarief: `GRATIS_VANAF` en `VERZENDKOST`. Verandert Bit & Byte zijn verzendvoorwaarden, dan pas je één regel aan.

---

## 6. De voorraad na de verkoop

**Nakijkpunten**

| Invoer: voorraad en aantal | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">3</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Genoeg op voorraad: true</code><br><code style="color:#845ef7">Voorraad na verkoop: 1 (bijna uitverkocht)</code> |
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">2</code> | <code style="color:#845ef7">Genoeg op voorraad: true</code><br><code style="color:#845ef7">Voorraad na verkoop: 0 (uitverkocht)</code> |
| <code style="color:#e64980;font-weight:600">7</code> en <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Genoeg op voorraad: true</code><br><code style="color:#845ef7">Voorraad na verkoop: 6 (op voorraad)</code> |
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">3</code> | <code style="color:#845ef7">Genoeg op voorraad: false</code><br><code style="color:#845ef7">Voorraad na verkoop: -1 (bijna uitverkocht)</code> |

De laatste rij is geen fout in je programma: zonder `if` kan het een verkoop nog niet weigeren.

**Aanpak.** Een vergelijking zoals `aantal <= voorraad` geeft een `boolean` (leerboek 1.3, *Relationele operatoren*): die kan je bewaren en tonen. Voor drie statussen zet je twee ternary operators na elkaar. Begin met het meest specifieke geval: eerst 0, dan 1 of 2, dan de rest.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
boolean genoegOpVoorraad = aantal <= voorraad;
int nieuweVoorraad = voorraad - aantal;
String status = nieuweVoorraad == 0 ? "uitverkocht"
        : nieuweVoorraad <= 2 ? "bijna uitverkocht" : "op voorraad";
System.out.println("Genoeg op voorraad: " + genoegOpVoorraad);
System.out.println("Voorraad na verkoop: " + nieuweVoorraad + " (" + status + ")");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De volgorde omdraaien: `nieuweVoorraad <= 2 ? "bijna uitverkocht" : nieuweVoorraad == 0 ? ...`. Dan is 0 ook kleiner dan of gelijk aan 2, en wordt een lege voorraad <code style="color:#845ef7">bijna uitverkocht</code>.
> - `nieuweVoorraad = 0` met één `=`. Dat is een toewijzing, geen vergelijking, en het compileert niet in een voorwaarde.

---

## 7. Alles samen: de kassabon

**Nakijkpunten**

| Invoer | Wat de bon toont |
|---|---|
| <code style="color:#e64980;font-weight:600">Ryzen 5 7600X</code> <code style="color:#e64980;font-weight:600">AMD</code> <code style="color:#e64980;font-weight:600">199,00</code> <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">Lotte Peeters</code> | <code style="color:#845ef7">Klant       : Lotte Peeters</code><br><code style="color:#845ef7">Artikel     : Ryzen 5 7600X (AMD)</code><br><code style="color:#845ef7">Aantal      : 2 x 199.0 euro</code><br><code style="color:#845ef7">Te betalen  : 398.0 euro</code><br><code style="color:#845ef7">Waarvan btw : 69.07 euro</code><br><code style="color:#845ef7">Voorraad na verkoop: 1 (bijna uitverkocht)</code> |
| <code style="color:#e64980;font-weight:600">Kabelbinders</code> <code style="color:#e64980;font-weight:600">Velcro</code> <code style="color:#e64980;font-weight:600">1,99</code> <code style="color:#e64980;font-weight:600">40</code> <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">Yusuf Demir</code> | <code style="color:#845ef7">Subtotaal   : 5.97 euro</code><br><code style="color:#845ef7">Verzendkost : 4.99 euro</code><br><code style="color:#845ef7">Te betalen  : 10.96 euro</code><br><code style="color:#845ef7">Waarvan btw : 1.9 euro</code><br><code style="color:#845ef7">Voorraad na verkoop: 37 (op voorraad)</code> |
| <code style="color:#e64980;font-weight:600">Radeon RX 7800 XT</code> <code style="color:#e64980;font-weight:600">Sapphire</code> <code style="color:#e64980;font-weight:600">499,00</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">Bureau Claes bv</code> | <code style="color:#845ef7">Klant       : Bureau Claes bv</code><br><code style="color:#845ef7">Te betalen  : 998.0 euro</code><br><code style="color:#845ef7">Waarvan btw : 173.21 euro</code><br><code style="color:#845ef7">Voorraad na verkoop: 0 (uitverkocht)</code> |

- Na de vraag <code style="color:#845ef7">Klant: </code> wacht het programma tot de gebruiker een naam typt.
- De regels van punt 2 tot en met 6 staan niet meer in de uitvoer.

**Aanpak.** De berekeningen blijven; enkel de `println`-regels van punt 2 tot en met 6 maken plaats voor de bon. `nextInt()` leest het aantal, maar laat de Enter staan. Een `nextLine()` direct daarna leest die lege rest van de regel. Daarom eerst één `sc.nextLine();` om de Enter op te ruimen, en dan pas de naam lezen (leerboek 1.4, *Het Enter-probleem*). De btw op de bon gaat over het bedrag *Te betalen*: de verzendkost bevat ook btw.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
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
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Het Enter-probleem: zonder de extra `sc.nextLine();` vraagt je programma <code style="color:#845ef7">Klant: </code> en toont het meteen de bon, met een lege klant.
> - De btw uitrekenen over het subtotaal in plaats van over *Te betalen*. Bij de kabelbinders geeft dat <code style="color:#845ef7">Waarvan btw : 1.04 euro</code> in plaats van <code style="color:#845ef7">1.9 euro</code>: de btw op de verzendkost ontbreekt.
> - De variabele `prijsZonderBtw` van punt 3 laten staan zonder ze nog te gebruiken. Dat is geen fout, maar VS Code toont een waarschuwing: wat je niet meer nodig hebt, mag weg.

> [!TIP]
> **Zo kan het beter**
>
> - `Waarvan btw : 1.9 euro` met één cijfer na de komma ziet er vreemd uit op een kassabon. Met `printf("%.2f")` toon je altijd twee cijfers: dat leer je in hoofdstuk 03.
> - Het verschil tussen `print` en `println` gebruik je hier bewust: de vragen met `print`, zodat de invoer erachter komt, de bon met `println`.

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

        // het artikel
        System.out.print("Artikel: ");
        String naam = sc.nextLine();
        System.out.print("Merk: ");
        String merk = sc.nextLine();
        System.out.print("Prijs: ");
        double prijs = sc.nextDouble();
        System.out.print("Voorraad: ");
        int voorraad = sc.nextInt();

        // de verkoop
        System.out.print("Aantal: ");
        int aantal = sc.nextInt();
        double subtotaal = Math.round(prijs * aantal * 100) / 100.0;
        double verzendkost = subtotaal < 50 ? 4.99 : 0.0;
        double teBetalen = Math.round((subtotaal + verzendkost) * 100) / 100.0;

        // de voorraad na de verkoop
        int nieuweVoorraad = voorraad - aantal;
        String status = nieuweVoorraad == 0 ? "uitverkocht"
                : nieuweVoorraad <= 2 ? "bijna uitverkocht" : "op voorraad";

        // de kassabon
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
```

</details>
