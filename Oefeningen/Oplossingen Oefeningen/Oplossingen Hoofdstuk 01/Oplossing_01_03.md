# Oplossing 01_03

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 01</b> &nbsp;·&nbsp; Rekenen zonder afrondingsfouten &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; bij `Oefening_01_03.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Daarom staan de ingetypte kommagetallen met een komma.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- `java src/Main.java` in de map `Oefening_01_03` toont <code style="color:#845ef7">Bit & Byte - configurator</code>.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - configurator");
    }
}
```

</details>

---

## 2. Bedragen in cent

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">Onderdelen: 1284,00 euro</code> |

- Elke prijs is een `int` in cent; er staat geen `double` in dit punt.
- Verander één prijs tijdelijk naar `19905`: dan moet je programma <code style="color:#845ef7">Onderdelen: 1284,05 euro</code> tonen, niet <code style="color:#845ef7">1284,5</code>.

**Aanpak.** `totaal / 100` geeft de euro, `totaal % 100` de cent (leerboek 1.3). Een rest kleiner dan 10 heeft één cijfer; de ternary operator zet er dan een `0` voor. Omdat de ternary operator een lagere voorrang heeft dan `+`, staat hij tussen haakjes.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int processor = 19900;
int moederbord = 21900;
int geheugen = 10900;
int videokaart = 58900;
int voeding = 6900;
int behuizing = 9900;
int onderdelen = processor + moederbord + geheugen + videokaart + voeding + behuizing;
System.out.println("Onderdelen: " + onderdelen / 100 + "," + (onderdelen % 100 < 10 ? "0" : "") + onderdelen % 100 + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De extra nul vergeten: bij 1284,05 euro toont je programma <code style="color:#845ef7">Onderdelen: 1284,5 euro</code>, wat iedereen leest als 1284,50.
> - De ternary operator zonder haakjes: `"..." + onderdelen % 100 < 10 ? "0" : ""`. Java plakt dan eerst alles links van `<` aan elkaar tot één `String`, en een `String` kan je niet met `<` vergelijken: *bad operand types for binary operator '<'*.
> - `onderdelen / 100.0`. Dan krijg je een `double` (`1284.0`), en ben je terug bij af.

---

## 3. Waarom geen double

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">Accessoires met double: 0.30000000000000004 euro</code><br><code style="color:#845ef7">Accessoires in cent: 0,30 euro</code> |

**Aanpak.** Een `double` bewaart 0,1 en 0,2 als het dichtstbijzijnde getal dat hij in het tweetallig stelsel kan voorstellen. De som is dan net niet 0,3. In cent zijn het gehele getallen, 10 en 20, en die bewaart een `int` exact.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
double kabelbinders = 0.10;
double schroeven = 0.20;
System.out.println("Accessoires met double: " + (kabelbinders + schroeven) + " euro");
int accessoires = 10 + 20;
System.out.println("Accessoires in cent: " + accessoires / 100 + "," + (accessoires % 100 < 10 ? "0" : "") + accessoires % 100 + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `"..." + kabelbinders + schroeven` zonder haakjes. Dan plakt Java beide getallen achter elkaar als tekst: <code style="color:#845ef7">Accessoires met double: 0.10.2 euro</code>.

---

## 4. Een bedrag inlezen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">19,99</code> | <code style="color:#845ef7">Afgekapt: 1998 cent</code><br><code style="color:#845ef7">Afgerond: 1999 cent</code> |
| <code style="color:#e64980;font-weight:600">4,35</code> | <code style="color:#845ef7">Afgekapt: 434 cent</code><br><code style="color:#845ef7">Afgerond: 435 cent</code> |
| <code style="color:#e64980;font-weight:600">0,29</code> | <code style="color:#845ef7">Afgekapt: 28 cent</code><br><code style="color:#845ef7">Afgerond: 29 cent</code> |
| <code style="color:#e64980;font-weight:600">219,00</code> | <code style="color:#845ef7">Afgekapt: 21900 cent</code><br><code style="color:#845ef7">Afgerond: 21900 cent</code> |

**Aanpak.** `19.99 * 100` is in een `double` net iets minder dan 1999. Een cast naar `int` kapt alles na de komma af (leerboek 1.3, *Typecasting*), en dus ook die ene cent. `Math.round` rondt af naar het dichtstbijzijnde gehele getal, en geeft bij een `double` een `long` terug (naslag 10.2). Een `long` past niet zomaar in een `int`: daarom nog een cast. Die cast verliest hier niets meer, want het getal is al geheel.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Prijs in euro: ");
double prijsInEuro = sc.nextDouble();
int afgekapt = (int) (prijsInEuro * 100);
int afgerond = (int) Math.round(prijsInEuro * 100);
System.out.println("Afgekapt: " + afgekapt + " cent");
System.out.println("Afgerond: " + afgerond + " cent");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `int afgerond = Math.round(prijsInEuro * 100);` compileert niet: *possible lossy conversion from long to int*.
> - `(int) prijsInEuro * 100`. De cast hoort bij `prijsInEuro` alleen: van 19,99 blijft 19 over, en je krijgt 1900 cent.

---

## 5. Btw met gehele getallen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">Zonder btw: 1061,16 euro</code><br><code style="color:#845ef7">Btw: 222,84 euro</code> |

**Aanpak.** Delen door 1,21 is vermenigvuldigen met 100 en delen door 121. Een gehele deling kapt af; tel je eerst de helft van de deler op, dan rondt ze af: `(onderdelen * 100 + 60) / 121`. De helft van 121 is 60,5; 60 volstaat, want de teller is altijd een geheel getal. De btw is het verschil met het totaal: zo tellen de twee bedragen altijd op tot het totaal, tot op de cent.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int zonderBtw = (onderdelen * 100 + 60) / 121;
int btw = onderdelen - zonderBtw;
System.out.println("Zonder btw: " + zonderBtw / 100 + "," + (zonderBtw % 100 < 10 ? "0" : "") + zonderBtw % 100 + " euro");
System.out.println("Btw: " + btw / 100 + "," + (btw % 100 < 10 ? "0" : "") + btw % 100 + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De `+ 60` vergeten. Dan kapt Java af: <code style="color:#845ef7">Zonder btw: 1061,15 euro</code> en <code style="color:#845ef7">Btw: 222,85 euro</code>. Een cent verschil, en precies wat de boekhouding weigert.
> - `onderdelen / 121 * 100`. Dan deel je eerst, en verlies je de cent al voor je vermenigvuldigt: <code style="color:#845ef7">Zonder btw: 1061,00 euro</code>.
> - De btw apart uitrekenen als 21 % van de prijs zonder btw. Dat kan door afronding een cent verschillen met *totaal min zonder btw*, en dan klopt de som niet meer.

---

## 6. Het voedingsadvies

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">105</code> <code style="color:#e64980;font-weight:600">200</code> <code style="color:#e64980;font-weight:600">550</code> | <code style="color:#845ef7">Vermogen: 380 W</code><br><code style="color:#845ef7">Advies: 500 W</code><br><code style="color:#845ef7">Voeding: 550 W (volstaat, marge 50 W)</code> |
| <code style="color:#e64980;font-weight:600">120</code> <code style="color:#e64980;font-weight:600">263</code> <code style="color:#e64980;font-weight:600">550</code> | <code style="color:#845ef7">Vermogen: 458 W</code><br><code style="color:#845ef7">Advies: 600 W</code><br><code style="color:#845ef7">Voeding: 550 W (te zwak, 50 W tekort)</code> |
| <code style="color:#e64980;font-weight:600">125</code> <code style="color:#e64980;font-weight:600">263</code> <code style="color:#e64980;font-weight:600">850</code> | <code style="color:#845ef7">Vermogen: 463 W</code><br><code style="color:#845ef7">Advies: 650 W</code><br><code style="color:#845ef7">Voeding: 850 W (volstaat, marge 200 W)</code> |
| <code style="color:#e64980;font-weight:600">100</code> <code style="color:#e64980;font-weight:600">175</code> <code style="color:#e64980;font-weight:600">400</code> | <code style="color:#845ef7">Vermogen: 350 W</code><br><code style="color:#845ef7">Advies: 500 W</code><br><code style="color:#845ef7">Voeding: 400 W (te zwak, 100 W tekort)</code> |
| <code style="color:#e64980;font-weight:600">125</code> <code style="color:#e64980;font-weight:600">300</code> <code style="color:#e64980;font-weight:600">650</code> | <code style="color:#845ef7">Vermogen: 500 W</code><br><code style="color:#845ef7">Advies: 650 W</code><br><code style="color:#845ef7">Voeding: 650 W (volstaat, marge 0 W)</code> |
| <code style="color:#e64980;font-weight:600">110</code> <code style="color:#e64980;font-weight:600">200</code> <code style="color:#e64980;font-weight:600">550</code> | <code style="color:#845ef7">Vermogen: 385 W</code><br><code style="color:#845ef7">Advies: 550 W</code><br><code style="color:#845ef7">Voeding: 550 W (volstaat, marge 0 W)</code> |

**Aanpak.** Maal 1,3 is maal 13 en gedeeld door 10. Naar boven afronden met een gehele deling doe je door eerst de deler min 1 op te tellen: `(vermogen * 13 + 9) / 10`. Daarna hetzelfde op 50 W: `(metMarge + 49) / 50 * 50`. De deling en de vermenigvuldiging staan na elkaar zonder haakjes; ze hebben dezelfde voorrang en Java rekent van links naar rechts. Het oordeel is een tekst met een getal erin: twee mogelijkheden, dus één ternary operator.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final int REST = 75;
// ...
System.out.print("Watt processor: ");
int wattProcessor = sc.nextInt();
System.out.print("Watt videokaart: ");
int wattVideokaart = sc.nextInt();
System.out.print("Watt voeding: ");
int wattVoeding = sc.nextInt();
int vermogen = wattProcessor + wattVideokaart + REST;
int metMarge = (vermogen * 13 + 9) / 10;
int advies = (metMarge + 49) / 50 * 50;
String oordeel = wattVoeding >= advies
        ? "volstaat, marge " + (wattVoeding - advies) + " W"
        : "te zwak, " + (advies - wattVoeding) + " W tekort";
System.out.println("Vermogen: " + vermogen + " W");
System.out.println("Advies: " + advies + " W");
System.out.println("Voeding: " + wattVoeding + " W (" + oordeel + ")");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `vermogen * 13 / 10` zonder naar boven af te ronden. Voor de meeste invoer valt dat niet op, maar bij <code style="color:#e64980;font-weight:600">110</code> en <code style="color:#e64980;font-weight:600">200</code> wordt 500,5 dan 500, en het advies <code style="color:#845ef7">Advies: 500 W</code> in plaats van 550 W.
> - `(metMarge + 50) / 50 * 50`. Dan gaat een advies dat al een veelvoud van 50 is, een stap te hoog: bij <code style="color:#e64980;font-weight:600">125</code> en <code style="color:#e64980;font-weight:600">300</code> krijg je 700 W.
> - `metMarge + 49 / 50 * 50`: zonder haakjes is `49 / 50` gelijk aan 0, en blijft het getal gewoon `metMarge`.
> - `wattVoeding > advies`. Dan is een voeding van precies het advies te zwak.

> [!TIP]
> **Zo kan het beter**
>
> - Beide stappen in één keer: `(vermogen * 13 + 499) / 500 * 50`. Maal 13 en gedeeld door 500 is maal 1,3 en gedeeld door 50. Het is korter, maar moeilijker te lezen; met twee stappen en twee goede variabelenamen zie je meteen wat er gebeurt.

---

## 7. Alles samen: het buildrapport

**Nakijkpunten**

| Invoer | Wat het rapport toont |
|---|---|
| <code style="color:#e64980;font-weight:600">199,00</code> <code style="color:#e64980;font-weight:600">105</code> <code style="color:#e64980;font-weight:600">589,00</code> <code style="color:#e64980;font-weight:600">200</code> <code style="color:#e64980;font-weight:600">69,00</code> <code style="color:#e64980;font-weight:600">550</code> | <code style="color:#845ef7">Onderdelen   : 1284,00 euro</code><br><code style="color:#845ef7">zonder btw : 1061,16 euro</code><br><code style="color:#845ef7">btw 21 %   : 222,84 euro</code><br><code style="color:#845ef7">Advies       : 500 W</code><br><code style="color:#845ef7">Voeding      : 550 W (volstaat, marge 50 W)</code> |
| <code style="color:#e64980;font-weight:600">389,00</code> <code style="color:#e64980;font-weight:600">120</code> <code style="color:#e64980;font-weight:600">499,00</code> <code style="color:#e64980;font-weight:600">263</code> <code style="color:#e64980;font-weight:600">69,00</code> <code style="color:#e64980;font-weight:600">550</code> | <code style="color:#845ef7">Onderdelen   : 1384,00 euro</code><br><code style="color:#845ef7">zonder btw : 1143,80 euro</code><br><code style="color:#845ef7">btw 21 %   : 240,20 euro</code><br><code style="color:#845ef7">Voeding      : 550 W (te zwak, 50 W tekort)</code> |
| <code style="color:#e64980;font-weight:600">299,00</code> <code style="color:#e64980;font-weight:600">125</code> <code style="color:#e64980;font-weight:600">499,00</code> <code style="color:#e64980;font-weight:600">263</code> <code style="color:#e64980;font-weight:600">139,00</code> <code style="color:#e64980;font-weight:600">850</code> | <code style="color:#845ef7">Onderdelen   : 1364,00 euro</code><br><code style="color:#845ef7">zonder btw : 1127,27 euro</code><br><code style="color:#845ef7">btw 21 %   : 236,73 euro</code><br><code style="color:#845ef7">Advies       : 650 W</code> |
| <code style="color:#e64980;font-weight:600">199,05</code> <code style="color:#e64980;font-weight:600">105</code> <code style="color:#e64980;font-weight:600">589,00</code> <code style="color:#e64980;font-weight:600">200</code> <code style="color:#e64980;font-weight:600">69,00</code> <code style="color:#e64980;font-weight:600">550</code> | <code style="color:#845ef7">Onderdelen   : 1284,05 euro</code><br><code style="color:#845ef7">zonder btw : 1061,20 euro</code><br><code style="color:#845ef7">btw 21 %   : 222,85 euro</code> |
| <code style="color:#e64980;font-weight:600">189,95</code> <code style="color:#e64980;font-weight:600">105</code> <code style="color:#e64980;font-weight:600">4,35</code> <code style="color:#e64980;font-weight:600">10</code> <code style="color:#e64980;font-weight:600">39,99</code> <code style="color:#e64980;font-weight:600">300</code> | <code style="color:#845ef7">Onderdelen   : 661,29 euro</code><br><code style="color:#845ef7">zonder btw : 546,52 euro</code><br><code style="color:#845ef7">btw 21 %   : 114,77 euro</code><br><code style="color:#845ef7">Advies       : 250 W</code> |

**Aanpak.** De drie gekozen prijzen lees je als `double` en zet je meteen om naar cent met `Math.round`, zoals in punt 4. Daarna is alles `int`: de som, de btw en het advies rekenen exact zoals in punt 2, 5 en 6. De vaste onderdelen blijven in cent in je code staan. Enkel de `println`-regels van punt 2 tot en met 6 maken plaats voor het rapport.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Prijs processor: ");
int processor = (int) Math.round(sc.nextDouble() * 100);
System.out.print("Watt processor: ");
int wattProcessor = sc.nextInt();
System.out.print("Prijs videokaart: ");
int videokaart = (int) Math.round(sc.nextDouble() * 100);
System.out.print("Watt videokaart: ");
int wattVideokaart = sc.nextInt();
System.out.print("Prijs voeding: ");
int voeding = (int) Math.round(sc.nextDouble() * 100);
System.out.print("Watt voeding: ");
int wattVoeding = sc.nextInt();
int moederbord = 21900;
int geheugen = 10900;
int behuizing = 9900;
// ... de berekeningen van punt 2, 5 en 6 blijven ongewijzigd
System.out.println("==============================");
System.out.println("BIT & BYTE - BUILDRAPPORT");
System.out.println("==============================");
System.out.println("Onderdelen   : " + onderdelen / 100 + "," + (onderdelen % 100 < 10 ? "0" : "") + onderdelen % 100 + " euro");
System.out.println("  zonder btw : " + zonderBtw / 100 + "," + (zonderBtw % 100 < 10 ? "0" : "") + zonderBtw % 100 + " euro");
System.out.println("  btw 21 %   : " + btw / 100 + "," + (btw % 100 < 10 ? "0" : "") + btw % 100 + " euro");
System.out.println("Vermogen     : " + vermogen + " W");
System.out.println("Advies       : " + advies + " W");
System.out.println("Voeding      : " + wattVoeding + " W (" + oordeel + ")");
System.out.println("==============================");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De prijzen omzetten met een cast in plaats van met `Math.round`. Bij de laatste rij van de nakijkpunten wordt 4,35 dan 434 cent, en toont het rapport <code style="color:#845ef7">Onderdelen   : 661,28 euro</code>.
> - De prijzen als `double` optellen en pas het totaal omzetten naar cent. De optelling heeft dan al een rekenfout gemaakt die je achteraf niet meer ziet.

> [!TIP]
> **Zo kan het beter**
>
> - De omzetting van cent naar euro staat drie keer in je code, telkens met een andere variabele. Een tikfout in één ervan valt pas op als net dat bedrag een rest onder 10 heeft. In hoofdstuk 03 maak je er een methode `euro(int cent)` van, en staat de regel nog maar op één plaats.
> - Ook `REST`, `13` en `50` zijn regels van Bit & Byte. Met constanten zoals `MARGE_PROCENT = 130` en `STAP = 50` zie je meteen wat ze betekenen.

---

## Alles samen: het volledige bestand

<details>
<summary><span style="color:#845ef7"><b>src/Main.java</b></span></summary>

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        System.out.println("Bit & Byte - configurator");
        final int REST = 75;
        Scanner sc = new Scanner(System.in);

        // de onderdelen, in cent
        System.out.print("Prijs processor: ");
        int processor = (int) Math.round(sc.nextDouble() * 100);
        System.out.print("Watt processor: ");
        int wattProcessor = sc.nextInt();
        System.out.print("Prijs videokaart: ");
        int videokaart = (int) Math.round(sc.nextDouble() * 100);
        System.out.print("Watt videokaart: ");
        int wattVideokaart = sc.nextInt();
        System.out.print("Prijs voeding: ");
        int voeding = (int) Math.round(sc.nextDouble() * 100);
        System.out.print("Watt voeding: ");
        int wattVoeding = sc.nextInt();
        int moederbord = 21900;
        int geheugen = 10900;
        int behuizing = 9900;
        int onderdelen = processor + moederbord + geheugen + videokaart + voeding + behuizing;

        // btw met gehele getallen
        int zonderBtw = (onderdelen * 100 + 60) / 121;
        int btw = onderdelen - zonderBtw;

        // het voedingsadvies
        int vermogen = wattProcessor + wattVideokaart + REST;
        int metMarge = (vermogen * 13 + 9) / 10;
        int advies = (metMarge + 49) / 50 * 50;
        String oordeel = wattVoeding >= advies
                ? "volstaat, marge " + (wattVoeding - advies) + " W"
                : "te zwak, " + (advies - wattVoeding) + " W tekort";

        // het buildrapport
        System.out.println("==============================");
        System.out.println("BIT & BYTE - BUILDRAPPORT");
        System.out.println("==============================");
        System.out.println("Onderdelen   : " + onderdelen / 100 + "," + (onderdelen % 100 < 10 ? "0" : "") + onderdelen % 100 + " euro");
        System.out.println("  zonder btw : " + zonderBtw / 100 + "," + (zonderBtw % 100 < 10 ? "0" : "") + zonderBtw % 100 + " euro");
        System.out.println("  btw 21 %   : " + btw / 100 + "," + (btw % 100 < 10 ? "0" : "") + btw % 100 + " euro");
        System.out.println("Vermogen     : " + vermogen + " W");
        System.out.println("Advies       : " + advies + " W");
        System.out.println("Voeding      : " + wattVoeding + " W (" + oordeel + ")");
        System.out.println("==============================");
    }
}
```

</details>
