# Oefeningen — hoofdstuk 03

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; zes oefeningen &nbsp;·&nbsp; opgesteld op basis van leerboek 1.8 en 1.9

Zes programma's voor de console, in twee reeksen die elkaar spiegelen: dezelfde techniek, ander domein. Doe je ze allebei, dan oefen je elk concept twee keer zonder twee keer hetzelfde te lezen.

Elk bestand sluit af met een <span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span> : één overzicht of één afhandeling waarin alle voorgaande punten van dat bestand samenkomen. Dat is telkens het laatste genummerde punt.

De kleuren en de kaders staan uitgelegd in het algemene overzicht, `Oefeningen/00_overzicht.md`.

---

## Hoofdstuk 03 — strings en methodes

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 03_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | 02_01 opsplitsen in methodes, zoeken in tekst zonder hoofdletters, een nette tabel met `printf` | De artikelfiche als tekst |
| 03_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Een bestelregel ontleden met `split` en `parseInt`, een bon opbouwen met `StringBuilder`, methodes met een returnwaarde | De bestelbon uit één regel invoer |
| 03_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Onderdeelcodes ontleden en controleren, overloading, builds samenstellen met recursie | Het buildrapport |
| 03_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 03_01, met zoeken op een stukje van de naam | De dierfiche als tekst |
| 03_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 03_02, met tijden als tekst, zoals `09:45` | De huurbon uit één regel invoer |
| 03_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 03_03, met plaatscodes zoals `C12` | Het zaalrapport |

### Hoe hoofdstuk 03 op hoofdstuk 02 bouwt

Elke oefening vertrekt van jouw oplossing van de oefening uit hoofdstuk 02 met hetzelfde nummer: 03_01 van 02_01, 03_02 van 02_02, enzovoort. Kopieer de map, en geef de kopie de naam van de nieuwe oefening.

Wat je in hoofdstuk 02 bouwde, blijft: het menu, de controles, de arrays en de samengestelde oefening van toen. Punt 2 van elke opgave verandert niets aan wat je programma toont. Je haalt herhaalde stukken uit `main` en zet ze in methodes. Pas daarna verandert er iets aan de uitvoer.

Wat verandert: je programma leest en schrijft tekst. Het zoekt zonder op hoofdletters te letten, ontleedt een regel invoer zoals `2x3`, `09:45` of `C12`, en zet getallen netjes in kolommen met `printf`.

> [!IMPORTANT]
> Vanaf dit hoofdstuk toont je programma bedragen met `printf` of `String.format`. Op een Belgische of Nederlandse Windows krijg je dan een komma: `398,00 euro`. De voorbeelden in de opgaven tonen het zo.

---

## Over de samengestelde oefeningen

In hoofdstuk 03 doet de samengestelde oefening telkens één van drie dingen:

- **Ze bouwt tekst op in een methode, en geeft die terug.** In 03_01 en 03_04 maakt een methode de volledige fiche. Het kader rond de fiche is zo breed als de langste regel. Dat kan alleen als je alle regels kent voor je iets toont.
- **Ze leest alles uit één regel invoer, en aanvaardt die pas als elk deel klopt.** In 03_02 en 03_05 typt de gebruiker een volledige bestelling of verhuring op één regel. Is één deel fout, dan verandert er niets.
- **Ze laat een methode zichzelf oproepen.** In 03_03 en 03_06 zoekt een recursieve methode door alle mogelijkheden, of door een rij plaatsen. Het rapport op het einde gebruikt die methodes om een voorstel te doen, of om de grootste vrije reeks te vinden.

---

## Volgorde

Werk van laag naar hoog: eerst 03_01, dan 03_02, dan 03_03. Doe de samengestelde oefening pas als de punten ervoor werken: ze bouwt er letterlijk op verder.

De spiegelreeks, 03_04 tot en met 03_06, kan je nadien gebruiken als herhaling, of meteen als je merkt dat een concept nog niet zit.

> [!IMPORTANT]
> Begin niet aan hoofdstuk 04 voor 03_03 volledig lukt zonder terug te bladeren.

---

## Buiten bereik

De oefeningen van hoofdstuk 03 blijven binnen wat in leerboek 1.1 tot en met 1.9 staat. Je hebt geen eigen klassen of objecten, geen `ArrayList` en geen `try`/`catch` nodig.

> [!WARNING]
> Een fout getal in de invoer controleer je vóór `Integer.parseInt`, teken per teken. Een `try`/`catch` rond `parseInt` komt pas in hoofdstuk 08.

Vier elementen komen voor die het leerboek niet zelf voordoet, of slechts in één zin noemt:

- **`Integer.parseInt`, `Character.isDigit` en `Character.toUpperCase`**, uit naslag 10.1 en 10.2. De opgaven tonen telkens hoe je ze gebruikt.
- **`"=".repeat(n)`**, uit naslag 10.1: een tekst `n` keer na elkaar. Met een lus en een `StringBuilder` lukt het ook.
- **Klassevariabelen met `static`** bovenaan de klasse, in 03_02, 03_03 en 03_06. Leerboek 1.2 toont ze in de tabel *Scope*. Een methode kan ze gebruiken zonder dat je ze als parameter doorgeeft.
- **`null` als returnwaarde**, in 03_03: een methode die een array teruggeeft, geeft `null` als de invoer niet klopt. Leerboek 1.2 noemt `null` als standaardwaarde.
