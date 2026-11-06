# Oplossing 01_06

<span style="background:#c2255c;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>OPLOSSING</b></span> &nbsp;·&nbsp; <b>Hoofdstuk 01</b> &nbsp;·&nbsp; Rekenen zonder afrondingsfouten &nbsp;·&nbsp; Concertzaal &nbsp;·&nbsp; bij `Oefening_01_06.md`

> [!IMPORTANT]
> Eerst zelf. Kijk per punt eerst naar de **nakijkpunten**. Haalt jouw programma ze allemaal, dan is je punt juist, ook als je code er anders uitziet. Open de code pas daarna, om te vergelijken.

> [!NOTE]
> Alle uitvoer in dit bestand komt uit een echte run met Java 21.

---

## 1. Je vertrekpunt

**Nakijkpunten**

- `java src/Main.java` in de map `Oefening_01_06` toont <code style="color:#845ef7">Concertzaal - ticketbalie</code>.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Concertzaal - ticketbalie");
    }
}
```

</details>

---

## 2. Van plaatsnummer naar rij en stoel

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">52</code> | <code style="color:#845ef7">Plaats: C12 (rij C, stoel 12)</code> |
| <code style="color:#e64980;font-weight:600">1</code> | <code style="color:#845ef7">Plaats: A1 (rij A, stoel 1)</code> |
| <code style="color:#e64980;font-weight:600">20</code> | <code style="color:#845ef7">Plaats: A20 (rij A, stoel 20)</code> |
| <code style="color:#e64980;font-weight:600">21</code> | <code style="color:#845ef7">Plaats: B1 (rij B, stoel 1)</code> |
| <code style="color:#e64980;font-weight:600">200</code> | <code style="color:#845ef7">Plaats: J20 (rij J, stoel 20)</code> |

- De rij is een `char`; het aantal stoelen per rij staat in een constante.

**Aanpak.** Tel eerst vanaf 0: plaats 1 tot en met 200 wordt 0 tot en met 199. Dan is de rij de gehele deling door 20 (0 tot en met 9) en de stoel de rest plus 1. Van rijnummer naar letter: `'A' + rijIndex` is een `int`, want Java rekent met een `char` als met een getal; de cast `(char)` maakt er weer een letter van (leerboek 1.3, *Typecasting*). Voor de plaatscode zet je een lege `String` tussen de letter en het getal, zodat `+` tekst plakt in plaats van op te tellen.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final int STOELEN_PER_RIJ = 20;
Scanner sc = new Scanner(System.in);
System.out.print("Plaatsnummer (1 - 200): ");
int plaatsnummer = sc.nextInt();
int rijIndex = (plaatsnummer - 1) / STOELEN_PER_RIJ;
char rij = (char) ('A' + rijIndex);
int stoel = (plaatsnummer - 1) % STOELEN_PER_RIJ + 1;
String plaats = rij + "" + stoel;
System.out.println("Plaats: " + plaats + " (rij " + rij + ", stoel " + stoel + ")");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Niet vanaf 0 tellen: `plaatsnummer / 20` en `plaatsnummer % 20`. Plaats 20 wordt dan rij `B`, stoel 0.
> - `char rij = 'A' + rijIndex;` compileert niet: de som is een `int`, en een `int` past niet zomaar in een `char`.
> - `String plaats = rij + stoel;` compileert niet, en `"" + (rij + stoel)` geeft `79` voor plaats 52: eerst de som van `'C'` (67) en 12, dan pas tekst.

---

## 3. Categorie en basisprijs, in cent

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">52</code> | <code style="color:#845ef7">Categorie: 1</code><br><code style="color:#845ef7">Basisprijs: 65,00 euro</code> |
| <code style="color:#e64980;font-weight:600">61</code> | <code style="color:#845ef7">Categorie: 2</code><br><code style="color:#845ef7">Basisprijs: 49,00 euro</code> |
| <code style="color:#e64980;font-weight:600">140</code> | <code style="color:#845ef7">Categorie: 2</code><br><code style="color:#845ef7">Basisprijs: 49,00 euro</code> |
| <code style="color:#e64980;font-weight:600">141</code> | <code style="color:#845ef7">Categorie: 3</code><br><code style="color:#845ef7">Basisprijs: 35,00 euro</code> |

**Aanpak.** De rijindex van punt 2 loopt van 0 tot en met 9: A tot en met C is 0 tot en met 2, D tot en met G is 3 tot en met 6. Twee ternary operators geven de categorie, twee andere de prijs in cent. Van cent naar euro: gehele deling en rest door 100, met een extra nul onder 10.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int categorie = rijIndex <= 2 ? 1 : rijIndex <= 6 ? 2 : 3;
int basisprijs = categorie == 1 ? 6500 : categorie == 2 ? 4900 : 3500;
System.out.println("Categorie: " + categorie);
System.out.println("Basisprijs: " + basisprijs / 100 + "," + (basisprijs % 100 < 10 ? "0" : "") + basisprijs % 100 + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De grenzen met plaatsnummers in plaats van rijen: `plaatsnummer <= 60`. Dat werkt, maar breekt zodra de zaal 22 stoelen per rij krijgt. Met de rij blijft het kloppen.
> - <code style="color:#845ef7">Basisprijs: 65,0 euro</code>: de extra nul vergeten.

---

## 4. Waarom geen double

**Nakijkpunten**

| Invoer | Wat je programma toont |
|---|---|
| geen | <code style="color:#845ef7">Groep met double: 125.10000000000001 euro</code><br><code style="color:#845ef7">Groep in cent: 125,10 euro</code> |

**Aanpak.** 0,8 kan een `double` niet exact bewaren, en de kleine fout wordt groter bij elke bewerking. In cent: 20 % van 4900 is 980, en drie keer 4900 − 980 + 250 is 12510, zonder enige fout.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
double groepMetDouble = 3 * (49.0 * 0.8 + 2.5);
int groepInCent = 3 * (4900 - 980 + 250);
System.out.println("Groep met double: " + groepMetDouble + " euro");
System.out.println("Groep in cent: " + groepInCent / 100 + "," + (groepInCent % 100 < 10 ? "0" : "") + groepInCent % 100 + " euro");
```

</details>

---

## 5. Jongerenkorting en servicekost

**Nakijkpunten**

| Invoer: plaats en leeftijd | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">52</code> en <code style="color:#e64980;font-weight:600">19</code> | <code style="color:#845ef7">Korting: 13,00 euro</code><br><code style="color:#845ef7">Servicekost: 2,50 euro</code><br><code style="color:#845ef7">Te betalen: 54,50 euro</code> |
| <code style="color:#e64980;font-weight:600">52</code> en <code style="color:#e64980;font-weight:600">26</code> | <code style="color:#845ef7">Korting: 0,00 euro</code><br><code style="color:#845ef7">Servicekost: 2,50 euro</code><br><code style="color:#845ef7">Te betalen: 67,50 euro</code> |
| <code style="color:#e64980;font-weight:600">75</code> en <code style="color:#e64980;font-weight:600">22</code> | <code style="color:#845ef7">Korting: 9,80 euro</code><br><code style="color:#845ef7">Servicekost: 2,50 euro</code><br><code style="color:#845ef7">Te betalen: 41,70 euro</code> |

**Aanpak.** 20 % in cent: `basisprijs * 20 / 100`, met `+ 50` voor de deling zodat het afrondt in plaats van afkapt. Bij de prijzen van de zaal maakt dat geen verschil, maar wel zodra een prijs niet deelbaar is door 5 cent. "Jonger dan 26" is `leeftijd < 26`: wie 26 is, betaalt de volle prijs.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
final int SERVICEKOST = 250;
// ...
System.out.print("Leeftijd: ");
int leeftijd = sc.nextInt();
int korting = leeftijd < 26 ? (basisprijs * 20 + 50) / 100 : 0;
int teBetalen = basisprijs - korting + SERVICEKOST;
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De korting ook op de servicekost: `(basisprijs + SERVICEKOST) * 20 / 100`. Bij plaats 52 kost het ticket dan 54,00 euro in plaats van 54,50.
> - `leeftijd <= 26`. Dan krijgt iemand van 26 nog korting.

---

## 6. Btw met gehele getallen

**Nakijkpunten**

| Invoer: plaats en leeftijd | Wat je programma toont |
|---|---|
| <code style="color:#e64980;font-weight:600">52</code> en <code style="color:#e64980;font-weight:600">19</code> | <code style="color:#845ef7">Btw (6 %): 2,94 euro</code> |
| <code style="color:#e64980;font-weight:600">52</code> en <code style="color:#e64980;font-weight:600">40</code> | <code style="color:#845ef7">Btw (6 %): 3,68 euro</code> |
| <code style="color:#e64980;font-weight:600">75</code> en <code style="color:#e64980;font-weight:600">22</code> | <code style="color:#845ef7">Btw (6 %): 2,22 euro</code> |
| <code style="color:#e64980;font-weight:600">181</code> en <code style="color:#e64980;font-weight:600">30</code> | <code style="color:#845ef7">Btw (6 %): 1,98 euro</code> |

**Aanpak.** Het ticket is de basisprijs min de korting, zonder servicekost. Gedeeld door 1,06 is maal 100 en gedeeld door 106; de helft van 106 is 53, en die tel je op voor je deelt, zodat de gehele deling afrondt. De btw is het verschil: zo tellen ticket zonder btw en btw altijd op tot het ticket.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
int ticketdeel = basisprijs - korting;
int zonderBtw = (ticketdeel * 100 + 53) / 106;
int btw = ticketdeel - zonderBtw;
System.out.println("Btw (6 %): " + btw / 100 + "," + (btw % 100 < 10 ? "0" : "") + btw % 100 + " euro");
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - De `+ 53` vergeten. Bij plaats 52 en leeftijd 19 kapt Java dan af, en toont je programma <code style="color:#845ef7">Btw (6 %): 2,95 euro</code>: een cent te veel btw.
> - De btw uitrekenen over *Te betalen*, met de servicekost erbij. De opgave zegt: enkel over het ticket.

---

## 7. Alles samen: het ticket

**Nakijkpunten**

| Invoer | Wat het ticket toont |
|---|---|
| <code style="color:#e64980;font-weight:600">1</code> <code style="color:#e64980;font-weight:600">52</code> <code style="color:#e64980;font-weight:600">19</code> | <code style="color:#845ef7">Concert      : The Lunar Echoes (2026-12-04)</code><br><code style="color:#845ef7">Plaats       : C12 (rij C, stoel 12)</code><br><code style="color:#845ef7">Korting -26  : 13,00 euro</code><br><code style="color:#845ef7">Te betalen   : 54,50 euro</code><br><code style="color:#845ef7">Waarvan btw  : 2,94 euro (6 % op het ticket)</code> |
| <code style="color:#e64980;font-weight:600">2</code> <code style="color:#e64980;font-weight:600">75</code> <code style="color:#e64980;font-weight:600">40</code> | <code style="color:#845ef7">Concert      : Brasserie Blues Band (2026-12-12)</code><br><code style="color:#845ef7">Plaats       : D15 (rij D, stoel 15)</code><br><code style="color:#845ef7">Korting -26  : 0,00 euro</code><br><code style="color:#845ef7">Te betalen   : 51,50 euro</code><br><code style="color:#845ef7">Waarvan btw  : 2,77 euro (6 % op het ticket)</code> |
| <code style="color:#e64980;font-weight:600">3</code> <code style="color:#e64980;font-weight:600">200</code> <code style="color:#e64980;font-weight:600">25</code> | <code style="color:#845ef7">Concert      : Noor & de Strijkers (2027-01-09)</code><br><code style="color:#845ef7">Plaats       : J20 (rij J, stoel 20)</code><br><code style="color:#845ef7">Korting -26  : 7,00 euro</code><br><code style="color:#845ef7">Te betalen   : 30,50 euro</code><br><code style="color:#845ef7">Waarvan btw  : 1,58 euro (6 % op het ticket)</code> |

**Aanpak.** Het concertnummer bepaalt twee teksten: artiest en datum. Elk krijgt een ternary operator. Alle berekeningen van punt 2, 3, 5 en 6 blijven; de `println`-regels maken plaats voor het ticket. Het voorbeeld van punt 4 hoort niet bij een ticket en verdwijnt.

<details>
<summary><span style="color:#845ef7"><b>Toon de code</b></span></summary>

```java
System.out.print("Concert (1, 2 of 3): ");
int concert = sc.nextInt();
String artiest = concert == 1 ? "The Lunar Echoes" : concert == 2 ? "Brasserie Blues Band" : "Noor & de Strijkers";
String datum = concert == 1 ? "2026-12-04" : concert == 2 ? "2026-12-12" : "2027-01-09";
// ... plaats, prijs, korting en btw zoals in punt 2 tot en met 6
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
```

</details>

> [!WARNING]
> **Veelgemaakte fouten**
>
> - Een tikfout in één van de vijf omzettingen naar euro, bijvoorbeeld `korting % 100` waar `btw % 100` hoort. Die valt pas op bij een bedrag waarvan net die cent niet klopt.

> [!TIP]
> **Zo kan het beter**
>
> - Vijf keer dezelfde omzetting van cent naar euro. In hoofdstuk 03 maak je er een methode `euro(int cent)` van; dan staat de regel op één plaats en kan de tikfout hierboven niet meer.
> - Je programma aanvaardt plaats 0 of 250, en een concert 7. Dat controleer je in hoofdstuk 02 met `if`.

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
        final int SERVICEKOST = 250;
        Scanner sc = new Scanner(System.in);

        // het concert
        System.out.print("Concert (1, 2 of 3): ");
        int concert = sc.nextInt();
        String artiest = concert == 1 ? "The Lunar Echoes" : concert == 2 ? "Brasserie Blues Band" : "Noor & de Strijkers";
        String datum = concert == 1 ? "2026-12-04" : concert == 2 ? "2026-12-12" : "2027-01-09";

        // van plaatsnummer naar rij en stoel
        System.out.print("Plaatsnummer (1 - 200): ");
        int plaatsnummer = sc.nextInt();
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
    }
}
```

</details>
