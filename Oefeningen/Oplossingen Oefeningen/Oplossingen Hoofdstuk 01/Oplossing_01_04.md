# Oplossing 01_04

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 01</b> &nbsp;·&nbsp; Variabelen en rekenen &nbsp;·&nbsp; Dierenasiel &nbsp;·&nbsp; bij `Oefening_01_04.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21, met Belgisch-Nederlandse taalinstellingen. Daarom staan de ingetypte kommagetallen met een komma.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- `java src/Main.java` in de map `Oefening_01_04` toont <code style="color:#845ef7">Dierenasiel - onthaal</code>, en niets anders.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Dierenasiel - onthaal");
    }
}
```

</details>

---

## 2. Het dier in variabelen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">Rex (hond)</code><br><code style="color:#845ef7">Leeftijd: 30 maanden</code><br><code style="color:#845ef7">Gewicht: 28.5 kg</code> |

- De naam en de soort zijn een `String`, de leeftijd een `int`, het gewicht een `double`.

**Aanpak.** Een leeftijd in maanden is een geheel getal: `int`. Een gewicht heeft cijfers na de komma: `double` (leerboek 1.2).

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String naam = "Rex";
String soort = "hond";
int leeftijd = 30;
double gewicht = 28.5;
System.out.println(naam + " (" + soort + ")");
System.out.println("Leeftijd: " + leeftijd + " maanden");
System.out.println("Gewicht: " + gewicht + " kg");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `int gewicht = 28.5;` compileert niet: *possible lossy conversion from double to int*.
> - `String leeftijd = "30";` compileert wel, maar met tekst kan je in punt 3 niet rekenen.

---

## 3. Leeftijd in jaren en maanden

**Nakijkpunten**

| Leeftijd in je variabele | Wat je programma toont |
|---|---|
| 30 | <code style="color:#845ef7">Leeftijd: 2 jaar en 6 maanden</code> |
| 7 | <code style="color:#845ef7">Leeftijd: 0 jaar en 7 maanden</code> |
| 12 | <code style="color:#845ef7">Leeftijd: 1 jaar en 0 maanden</code> |
| 110 | <code style="color:#845ef7">Leeftijd: 9 jaar en 2 maanden</code> |

- De regel staat op dezelfde plaats als de leeftijd in punt 2: tussen de naam en het gewicht.

**Aanpak.** `leeftijd / 12` is een gehele deling: het aantal volle jaren. `leeftijd % 12` is de rest: de maanden die overblijven (leerboek 1.3). Beide zijn een `int`; je bewaart ze in een eigen variabele, zodat je ze op de dierfiche opnieuw kan gebruiken.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int jaren = leeftijd / 12;
int maanden = leeftijd % 12;
System.out.println(naam + " (" + soort + ")");
System.out.println("Leeftijd: " + jaren + " jaar en " + maanden + " maanden");
System.out.println("Gewicht: " + gewicht + " kg");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `double jaren = leeftijd / 12.0;`. Dan zijn het geen volle jaren meer, en toont je programma <code style="color:#845ef7">Leeftijd: 2.5 jaar en 6 maanden</code>.

---

## 4. Het dier inlezen

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">Luna</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">7</code> <code style="color:#e64980;font-weight:600">3,1</code> | <code style="color:#845ef7">Luna (kat)</code><br><code style="color:#845ef7">Leeftijd: 0 jaar en 7 maanden</code><br><code style="color:#845ef7">Gewicht: 3.1 kg</code> |

- De vragen zijn <code style="color:#845ef7">Naam: </code>, <code style="color:#845ef7">Soort (1 = hond, 2 = kat, 3 = konijn): </code>, <code style="color:#845ef7">Leeftijd in maanden: </code> en <code style="color:#845ef7">Gewicht in kg: </code>.

**Aanpak.** Eén `Scanner` op `System.in` (leerboek 1.4). Het soortnummer lees je met `nextInt()`, en twee ternary operators maken er een woord van. Begin bij het eerste nummer; wat overblijft, is een konijn.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
Scanner sc = new Scanner(System.in);
System.out.print("Naam: ");
String naam = sc.nextLine();
System.out.print("Soort (1 = hond, 2 = kat, 3 = konijn): ");
int soortNummer = sc.nextInt();
String soort = soortNummer == 1 ? "hond" : soortNummer == 2 ? "kat" : "konijn";
System.out.print("Leeftijd in maanden: ");
int leeftijd = sc.nextInt();
System.out.print("Gewicht in kg: ");
double gewicht = sc.nextDouble();
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `3.1` intypen op een Belgische of Nederlandse Windows: het programma stopt met een `InputMismatchException`. Typ `3,1`.
> - De soort als tekst inlezen en vergelijken met `==`: `soort == "hond"`. Dat vergelijkt geen tekst, maar verwijzingen, en geeft hier `false` (leerboek 1.8). Daarom vraagt de opgave een nummer.

---

## 5. De adoptieprijs

**Nakijkpunten**

| Invoer: soort en leeftijd | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">7</code> | <code style="color:#845ef7">Senior: false</code><br><code style="color:#845ef7">Adoptieprijs: 90 euro</code> |
| <code style="color:#e64980;font-weight:600">1</code> en <code style="color:#e64980;font-weight:600">110</code> | <code style="color:#845ef7">Senior: true</code><br><code style="color:#845ef7">Adoptieprijs: 75 euro</code> |
| <code style="color:#e64980;font-weight:600">3</code> en <code style="color:#e64980;font-weight:600">14</code> | <code style="color:#845ef7">Senior: false</code><br><code style="color:#845ef7">Adoptieprijs: 40 euro</code> |
| <code style="color:#e64980;font-weight:600">2</code> en <code style="color:#e64980;font-weight:600">96</code> | <code style="color:#845ef7">Senior: true</code><br><code style="color:#845ef7">Adoptieprijs: 45 euro</code> |

**Aanpak.** De basisprijs hangt af van het soortnummer, net als het woord in punt 4. Een senior is `leeftijd >= 96`: die vergelijking geeft een `boolean`, die je bewaart. Met die `boolean` als voorwaarde kies je tussen de halve en de volle prijs. Alle prijzen zijn even, dus de gehele deling door 2 verliest niets.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int basisprijs = soortNummer == 1 ? 150 : soortNummer == 2 ? 90 : 40;
boolean senior = leeftijd >= 96;
int adoptieprijs = senior ? basisprijs / 2 : basisprijs;
System.out.println("Senior: " + senior);
System.out.println("Adoptieprijs: " + adoptieprijs + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `leeftijd > 96`. Dan is een kat van precies 8 jaar nog geen senior, en betaalt ze 90 euro in plaats van 45.
> - `senior == true ? ...`. Werkt, maar is dubbel: `senior` is zelf al `true` of `false`.

---

## 6. Levensfase en voer

**Nakijkpunten**

| Invoer: leeftijd en gewicht | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">7</code> en <code style="color:#e64980;font-weight:600">3,1</code> | <code style="color:#845ef7">Levensfase: jong</code><br><code style="color:#845ef7">Voer per dag: 70 gram</code> |
| <code style="color:#e64980;font-weight:600">30</code> en <code style="color:#e64980;font-weight:600">28,5</code> | <code style="color:#845ef7">Levensfase: volwassen</code><br><code style="color:#845ef7">Voer per dag: 570 gram</code> |
| <code style="color:#e64980;font-weight:600">110</code> en <code style="color:#e64980;font-weight:600">31,0</code> | <code style="color:#845ef7">Levensfase: senior</code><br><code style="color:#845ef7">Voer per dag: 620 gram</code> |
| <code style="color:#e64980;font-weight:600">12</code> en <code style="color:#e64980;font-weight:600">6,0</code> | <code style="color:#845ef7">Levensfase: volwassen</code><br><code style="color:#845ef7">Voer per dag: 120 gram</code> |
| <code style="color:#e64980;font-weight:600">11</code> en <code style="color:#e64980;font-weight:600">6,0</code> | <code style="color:#845ef7">Levensfase: jong</code><br><code style="color:#845ef7">Voer per dag: 120 gram</code> |

**Aanpak.** Voor de levensfase hergebruik je de `boolean` van punt 5: eerst *jong*, dan *senior*, de rest is *volwassen*. Voor het voer: gewicht × 20 geeft grammen. Gedeeld door 10 krijg je tientallen grammen; `Math.ceil` rondt die naar boven af (naslag 10.2), × 10 maakt er weer grammen van. `Math.ceil` geeft een `double`; de cast maakt er een `int` van zonder iets te verliezen, want het getal is al geheel.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
String levensfase = leeftijd < 12 ? "jong" : senior ? "senior" : "volwassen";
int voer = (int) Math.ceil(gewicht * 20 / 10) * 10;
System.out.println("Levensfase: " + levensfase);
System.out.println("Voer per dag: " + voer + " gram");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - `(int) (gewicht * 20 / 10) * 10`, zonder `Math.ceil`. De cast kapt af: Luna krijgt dan 60 gram in plaats van 70. `Math.round` geeft hier ook 60.
> - `(int) Math.ceil(gewicht * 20) / 10 * 10`. Nu rondt `Math.ceil` op hele grammen af, en kapt de gehele deling daarna af op tientallen: weer 60 gram.

---

## 7. Alles samen: de dierfiche

**Nakijkpunten**

| Invoer | Wat de fiche toont |
|---|---|
| <code style="color:#e64980;font-weight:600">Luna</code> <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">7</code> <code style="color:#e64980;font-weight:600">3,1</code> <code style="color:#e64980;font-weight:600">Ines Maes</code> | <code style="color:#845ef7">Naam         : Luna</code><br><code style="color:#845ef7">Soort        : kat</code><br><code style="color:#845ef7">Leeftijd     : 0 jaar en 7 maanden (jong)</code><br><code style="color:#845ef7">Gewicht      : 3.1 kg</code><br><code style="color:#845ef7">Voer per dag : 70 gram</code><br><code style="color:#845ef7">Adoptieprijs : 90 euro</code><br><code style="color:#845ef7">Verzorger    : Ines Maes</code> |
| <code style="color:#e64980;font-weight:600">Max</code> <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">110</code> <code style="color:#e64980;font-weight:600">31,0</code> <code style="color:#e64980;font-weight:600">Ines Maes</code> | <code style="color:#845ef7">Leeftijd     : 9 jaar en 2 maanden (senior)</code><br><code style="color:#845ef7">Voer per dag : 620 gram</code><br><code style="color:#845ef7">Adoptieprijs : 75 euro</code> |
| <code style="color:#e64980;font-weight:600">Pluis</code> <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">14</code> <code style="color:#e64980;font-weight:600">2,2</code> <code style="color:#e64980;font-weight:600">Wout Smets</code> | <code style="color:#845ef7">Soort        : konijn</code><br><code style="color:#845ef7">Leeftijd     : 1 jaar en 2 maanden (volwassen)</code><br><code style="color:#845ef7">Voer per dag : 50 gram</code><br><code style="color:#845ef7">Adoptieprijs : 40 euro</code><br><code style="color:#845ef7">Verzorger    : Wout Smets</code> |

- Na de vraag <code style="color:#845ef7">Verzorger: </code> wacht het programma tot de gebruiker een naam typt.

**Aanpak.** `nextDouble()` leest het gewicht, maar laat de Enter staan. Een `nextLine()` direct daarna leest die lege rest van de regel. Daarom eerst één `sc.nextLine();`, en dan pas de naam van de verzorger (leerboek 1.4, *Het Enter-probleem*). De berekeningen blijven; enkel de `println`-regels van punt 2 tot en met 6 maken plaats voor de fiche.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
sc.nextLine();                         // de Enter na het gewicht
System.out.print("Verzorger: ");
String verzorger = sc.nextLine();
System.out.println("==============================");
System.out.println("DIERENASIEL - DIERFICHE");
System.out.println("==============================");
System.out.println("Naam         : " + naam);
System.out.println("Soort        : " + soort);
System.out.println("Leeftijd     : " + jaren + " jaar en " + maanden + " maanden (" + levensfase + ")");
System.out.println("Gewicht      : " + gewicht + " kg");
System.out.println("Voer per dag : " + voer + " gram");
System.out.println("Adoptieprijs : " + adoptieprijs + " euro");
System.out.println("Verzorger    : " + verzorger);
System.out.println("==============================");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Het Enter-probleem: zonder de extra `sc.nextLine();` vraagt je programma <code style="color:#845ef7">Verzorger: </code> en toont het meteen de fiche, met een lege verzorger.

> [!TIP]
> **Zo kan het beter**
>
> - <code style="color:#845ef7">0 jaar en 7 maanden</code> leest stroef. Met een ternary operator toon je enkel de maanden als het dier jonger is dan een jaar: `jaren == 0 ? maanden + " maanden" : jaren + " jaar en " + maanden + " maanden"`. De opgave vraagt het niet, dus laat het gerust zo staan.
> - Leg ook `96`, `12` en `20` vast in constanten, zoals `SENIOR_VANAF` en `GRAM_PER_KG`. Wie het programma leest, ziet dan meteen wat ze betekenen.

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

        // het dier
        System.out.print("Naam: ");
        String naam = sc.nextLine();
        System.out.print("Soort (1 = hond, 2 = kat, 3 = konijn): ");
        int soortNummer = sc.nextInt();
        String soort = soortNummer == 1 ? "hond" : soortNummer == 2 ? "kat" : "konijn";
        System.out.print("Leeftijd in maanden: ");
        int leeftijd = sc.nextInt();
        System.out.print("Gewicht in kg: ");
        double gewicht = sc.nextDouble();

        // leeftijd in jaren en maanden
        int jaren = leeftijd / 12;
        int maanden = leeftijd % 12;

        // de adoptieprijs
        int basisprijs = soortNummer == 1 ? 150 : soortNummer == 2 ? 90 : 40;
        boolean senior = leeftijd >= 96;
        int adoptieprijs = senior ? basisprijs / 2 : basisprijs;

        // levensfase en voer
        String levensfase = leeftijd < 12 ? "jong" : senior ? "senior" : "volwassen";
        int voer = (int) Math.ceil(gewicht * 20 / 10) * 10;

        // de dierfiche
        sc.nextLine();                         // de Enter na het gewicht
        System.out.print("Verzorger: ");
        String verzorger = sc.nextLine();
        System.out.println("==============================");
        System.out.println("DIERENASIEL - DIERFICHE");
        System.out.println("==============================");
        System.out.println("Naam         : " + naam);
        System.out.println("Soort        : " + soort);
        System.out.println("Leeftijd     : " + jaren + " jaar en " + maanden + " maanden (" + levensfase + ")");
        System.out.println("Gewicht      : " + gewicht + " kg");
        System.out.println("Voer per dag : " + voer + " gram");
        System.out.println("Adoptieprijs : " + adoptieprijs + " euro");
        System.out.println("Verzorger    : " + verzorger);
        System.out.println("==============================");
    }
}
```

</details>
