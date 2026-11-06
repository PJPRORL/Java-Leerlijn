# Oefeningen — hoofdstuk 01

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; zes oefeningen &nbsp;·&nbsp; opgesteld op basis van leerboek 1.1 tot en met 1.4

Zes programma's voor de console, in twee reeksen die elkaar spiegelen: dezelfde techniek, ander domein. Doe je ze allebei, dan oefen je elk concept twee keer zonder twee keer hetzelfde te lezen.

Elk bestand sluit af met een <span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span> : één overzicht, een bon of een fiche, waarin alle voorgaande punten van dat bestand samenkomen. Dat is telkens het laatste genummerde punt.

De kleuren en de kaders staan uitgelegd in het algemene overzicht, `Oefeningen/00_overzicht.md`.

---

## Hoofdstuk 01 — variabelen, operatoren en invoer

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 01_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Variabelen en datatypes, rekenen met btw, invoer met `Scanner`, de ternary operator | De kassabon voor één artikel |
| 01_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Gehele deling en rest, typecasting, een bestelling verdelen over dozen, een willekeurig bestelnummer met `Random` | De pakbon |
| 01_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Rekenen in eurocent met gehele getallen, naar boven afronden zonder `if`, voorrang van operatoren | Het buildrapport met voedingsadvies |
| 01_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 01_01, met een leeftijd in jaren en maanden | De dierfiche |
| 01_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 01_02, met minuten die je omzet naar begonnen uren | Het huurcontract |
| 01_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 01_03, met een plaatsnummer dat je omzet naar rij en stoel | Het ticket |

### Hoe hoofdstuk 01 begint

Elke oefening begint met een nieuw project: een map met de naam van de oefening, daarin `src/Main.java`. Je start het programma in de terminal van VS Code met `java src/Main.java`, vanuit de map van de oefening.

In de volgende hoofdstukken bouwt elke reeks verder op dit begin: 02_01 vertrekt van jouw 01_01, 02_02 van jouw 01_02, enzovoort. Bewaar je oplossingen dus goed.

In een voorbeeldrun staat wat je intypt op dezelfde regel als de vraag, zoals in je terminal:

```text
Aantal: 2
```

Hier toont het programma `Aantal: `, en typt de gebruiker `2` en Enter.

---

## Over de samengestelde oefeningen

In hoofdstuk 01 heeft een programma nog geen `if`, geen lussen en geen methodes. Het loopt van boven naar onder, één keer. De samengestelde oefening doet daarom telkens drie dingen:

- **De vaste gegevens maken plaats voor invoer.** In de gewone punten staat een artikel, een dier of een fiets vast in je code. In het laatste punt typt de gebruiker alles zelf in, en moet je programma werken voor elke invoer, niet enkel voor het voorbeeld.
- **De losse regels worden één overzicht.** De regels van de vorige punten verdwijnen; wat ze uitrekenden, komt samen op een bon, een fiche of een ticket. Alle regels moeten tegelijk kloppen.
- **Je loopt tegen de grens van hoofdstuk 01 aan.** Een verkoop met te weinig voorraad gaat gewoon door, een soortnummer 7 wordt stilletjes een konijn, en dezelfde omzetting van cent naar euro staat vijf keer in je code. In hoofdstuk 02 en 03 los je dat op.

---

## Volgorde

Werk van laag naar hoog: eerst 01_01, dan 01_02, dan 01_03. Doe de samengestelde oefening pas als de punten ervoor werken: ze bouwt er letterlijk op verder.

De spiegelreeks, 01_04 tot en met 01_06, kan je nadien gebruiken als herhaling, of meteen als je merkt dat een concept nog niet zit.

> [!IMPORTANT]
> Begin niet aan hoofdstuk 02 voor 01_03 volledig lukt zonder terug te bladeren.

---

## Buiten bereik

De oefeningen van hoofdstuk 01 blijven binnen wat in leerboek 1.1 tot en met 1.4 staat. Je hebt geen `if`, geen `switch`, geen lussen, geen arrays, geen eigen methodes en geen `printf` nodig. Ook de methodes van `String`, zoals `equals` of `length`, gebruik je nog niet.

> [!WARNING]
> Heb je het gevoel dat je die wel nodig hebt, dan los je de oefening waarschijnlijk te ingewikkeld op. De ternary operator, de gehele deling en de rest kunnen meer dan je denkt.

Vier elementen komen voor die het leerboek niet zelf voordoet:

- **Een ternary operator in een andere ternary operator**, om uit drie mogelijkheden te kiezen. Leerboek 1.3 toont er één; twee na elkaar werken op dezelfde manier.
- **`Math.round`, `Math.ceil` en `Math.min`** voor afronden en het kleinste van twee getallen. Je vindt ze in naslag 10.2, die je altijd mag gebruiken.
- **Afronden met gehele getallen**, in 01_03 en 01_06: eerst de helft van de deler optellen, dan delen. De opgave legt het uit.
- **Een kommagetal intypen met een komma.** `Scanner` leest getallen in de taal van je computer. Op een Belgische of Nederlandse Windows typ je `199,00`, en de voorbeeldruns tonen het zo. Op een Engelstalige computer typ je `199.00`.
