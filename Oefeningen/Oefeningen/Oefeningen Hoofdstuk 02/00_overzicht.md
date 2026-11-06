# Oefeningen — hoofdstuk 02

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; zes oefeningen &nbsp;·&nbsp; opgesteld op basis van leerboek 1.5 tot en met 1.7

Zes programma's voor de console, in twee reeksen die elkaar spiegelen: dezelfde techniek, ander domein. Doe je ze allebei, dan oefen je elk concept twee keer zonder twee keer hetzelfde te lezen.

Elk bestand sluit af met een <span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span> : één overzicht of één afhandeling waarin alle voorgaande punten van dat bestand samenkomen. Dat is telkens het laatste genummerde punt.

De kleuren en de kaders staan uitgelegd in het algemene overzicht, `Oefeningen/00_overzicht.md`.

---

## Hoofdstuk 02 — beslissingen, lussen en arrays

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 02_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | De voorraad in arrays, een menu met `switch` in een lus, zoeken, tellen en een gemiddelde | Het voorraadoverzicht |
| 02_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Bestellen met controles vóór je iets aanpast, `do-while`, `break` en `continue` | Een bestelling afronden |
| 02_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een compatibiliteitstabel als tweedimensionale array, geneste lussen, alle combinaties doorlopen | Alle geldige builds onder een budget |
| 02_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 02_01 | Het asieloverzicht |
| 02_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 02_02, met verhuren en terugbrengen | Een fiets terugbrengen |
| 02_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 02_03, met het zaalplan als tweedimensionale array | Vier plaatsen naast elkaar |

### Hoe hoofdstuk 02 op hoofdstuk 01 bouwt

Elke oefening vertrekt van jouw oplossing van de oefening uit hoofdstuk 01 met hetzelfde nummer: 02_01 van 01_01, 02_02 van 01_02, enzovoort. Kopieer de map, en geef de kopie de naam van de nieuwe oefening.

Wat je in hoofdstuk 01 uitrekende, blijft: de kassabon, de pakbon, het buildrapport, de dierfiche, het huurcontract en het ticket. Elke oefening begint met een tabel van wat na je wijzigingen nog precies hetzelfde moet werken. Heb je de oefening uit hoofdstuk 01 niet meer, maak die dan eerst opnieuw vanuit haar eigen opgave.

Wat verandert: de gegevens staan niet langer één voor één in je code of in de invoer, maar in arrays. Je programma kent nu een catalogus, een asiel, een vloot of een zaalplan, en weet na een verkoop wat er veranderd is.

---

## Over de samengestelde oefeningen

In hoofdstuk 02 doet de samengestelde oefening telkens één van drie dingen:

- **Ze vat de toestand van alle gegevens samen.** In 02_01 en 02_04 toont een overzicht wat er na een reeks verkopen of adopties overblijft. Klopt één verkoop niet, dan zie je het hier.
- **Ze verandert pas iets als alles gecontroleerd en bevestigd is.** In 02_02 en 02_05 mag een geannuleerde bestelling of een onbetaalde terugbrenging geen enkele array veranderen. Dat test je door daarna hetzelfde opnieuw te doen.
- **Ze zoekt door een tabel naar wat de klant nodig heeft.** In 02_03 en 02_06 doorlopen geneste lussen alle mogelijkheden, en houdt je programma enkel bij wat telt: de builds binnen het budget, of de eerste reeks vrije plaatsen.

---

## Volgorde

Werk van laag naar hoog: eerst 02_01, dan 02_02, dan 02_03. Doe de samengestelde oefening pas als de punten ervoor werken: ze bouwt er letterlijk op verder.

De spiegelreeks, 02_04 tot en met 02_06, kan je nadien gebruiken als herhaling, of meteen als je merkt dat een concept nog niet zit.

> [!IMPORTANT]
> Begin niet aan hoofdstuk 03 voor 02_03 volledig lukt zonder terug te bladeren.

---

## Buiten bereik

De oefeningen van hoofdstuk 02 blijven binnen wat in leerboek 1.1 tot en met 1.7 staat. Je hebt geen eigen methodes, geen `printf`, geen methodes van `String` zoals `equals` of `length()`, en geen `ArrayList` nodig.

> [!WARNING]
> Heb je het gevoel dat je die wel nodig hebt, dan los je de oefening waarschijnlijk te ingewikkeld op. Je `main` wordt lang: dat is de bedoeling. In hoofdstuk 03 knip je hem op.

Vier elementen komen voor die het leerboek niet zelf voordoet, of slechts in één zin noemt:

- **Parallelle arrays**: meerdere arrays waarin dezelfde index over hetzelfde artikel, dier of onderdeel gaat. Het leerboek toont telkens één array; de opgaven leggen uit hoe ze samen horen.
- **Een status als getal**: 0, 1 of 2 in plaats van een woord, omdat je teksten pas in hoofdstuk 03 leert vergelijken. In hoofdstuk 06 wordt het een enum.
- **`sc.next().charAt(0)`** om een `j` of een `n` te lezen. Leerboek 1.5 noemt het bij `switch`; de methode `charAt` zelf komt in 1.8.
- **`Math.max` en `Integer.MAX_VALUE`**, uit naslag 10.2.
