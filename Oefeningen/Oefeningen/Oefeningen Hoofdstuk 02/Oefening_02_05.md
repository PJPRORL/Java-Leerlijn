# 02_05

<b>Hoofdstuk 02</b> &nbsp;·&nbsp; Eerst controleren, dan veranderen &nbsp;·&nbsp; Fietsverhuur &nbsp;·&nbsp; niveau <span style="color:#f08c00">●●○</span> &nbsp;·&nbsp; ±90 min

## Leerdoel

Na deze oefening kan je de toestand van meerdere dingen tegelijk bijhouden in arrays: welke fiets verhuurd is, sinds wanneer en aan wie.

Je leert met `if`, `else if` en `else` een reeks voorwaarden controleren in een vaste volgorde, en met geneste lussen twee arrays met elkaar verbinden.

Daarnaast oefen je op een belangrijke gewoonte: eerst alles controleren, en pas daarna iets veranderen. Een terugbrenging die niet betaald is, laat geen enkel spoor na.

## Opdracht

Fietsverhuur Trapper gebruikt het programma uit 01_05 voor het huurcontract. Maar het programma weet niet welke fietsen er zijn, welke verhuurd zijn, en aan wie. Aan de balie gebeurt het dat een fiets twee keer verhuurd wordt, of dat een klant drie fietsen meeneemt.

Jouw taak is om het programma de vloot en de klanten te laten kennen, en verhuren en terugbrengen te scheiden.

Het programma moet, net als in 01_05:

1. een huurcontract opstellen met begonnen uren, dagmaximum, boete en waarborg;

en daarnaast:

2. de fietsen uit de vloot halen;
3. een fiets verhuren en later terugbrengen, in een menu;
4. een verhuring weigeren als de fiets of de klant dat niet toelaat;
5. een terugbrenging weigeren die niet kan;
6. tonen welke fietsen elke klant heeft;
7. een fiets pas vrijgeven als de huur betaald is.

Implementeer onderstaande functionaliteiten.

### 1. Je vertrekpunt

Kopieer de map van je oplossing van 01_05 naar een nieuwe map `Oefening_02_05`. Deze regels uit 01_05 blijven gelden, en het contract blijft hetzelfde:

| Regel | Waarde |
|---|---|
| Contractnummer | `FV-2026-` en vier cijfers |
| Begonnen uren | `(minuten + 59) / 60` |
| Tarief | stadsfiets 3, e-bike 6, bakfiets 8 euro per begonnen uur; maximum 15, 30, 40 euro per dag |
| Waarborg | 50, 150, 100 euro |
| Te laat | terug na 20:00 uur: 10 euro boete |

---

### 2. De vloot

Bewaar de zes fietsen in drie arrays: typenummers (1 = stadsfiets, 2 = e-bike, 3 = bakfiets), merken en statussen (0 = <code style="color:#845ef7">beschikbaar</code>, 1 = <code style="color:#845ef7">verhuurd</code>, 2 = <code style="color:#845ef7">in herstelling</code>).

| Id | Type | Merk | Status |
|---|---|---|---|
| 1 | 1 | Gazelle | 0 |
| 2 | 1 | Oxford | 0 |
| 3 | 2 | Stromer | 0 |
| 4 | 2 | Cube | 1 |
| 5 | 3 | Babboe | 0 |
| 6 | 3 | Urban Arrow | 2 |

Toon de vloot bij de start. De medewerker typt niet langer het type, maar het nummer van de fiets; het programma haalt het type uit de array. Het contract blijft zoals in 01_05, met het merk erbij:

```text
Fietsverhuur - balie
1. stadsfiets Gazelle - beschikbaar
2. stadsfiets Oxford - beschikbaar
3. e-bike Stromer - beschikbaar
4. e-bike Cube - verhuurd
5. bakfiets Babboe - beschikbaar
6. bakfiets Urban Arrow - in herstelling
Fiets (1-6): 3
Vertrek uur: 9
...
Fiets         : e-bike Stromer (6 euro per uur, max 30 euro)
```

---

### 3. Verhuren en terugbrengen

In werkelijkheid vertrekt een fiets 's morgens, en komt ze 's avonds terug. Splits je programma in een menu:

```text
--- Menu ---
1. Vloot
2. Verhuren
3. Terugbrengen
0. Stoppen
Keuze: 
```

- **Verhuren**: vraag de fiets en het vertrekuur. Bewaar de status, het vertrek in minuten na middernacht en een nieuw contractnummer in arrays, en toon <code style="color:#845ef7">Fiets 3 verhuurd om 9:45, contract FV-2026-3260.</code>
- **Terugbrengen**: vraag de fiets en het uur van terugkomst. Haal het vertrek en het contractnummer uit de arrays, toon het contract van 01_05, en zet de status terug op beschikbaar.

Fiets 4 is bij de start al verhuurd: sinds 8:30, met contract `FV-2026-4821`. Zet dat in de beginwaarden van je arrays.

> [!TIP]
> Een fiets die niet verhuurd is, heeft geen vertrek. Geef die plaatsen in de array een waarde die je nooit gebruikt, zoals 0 of een lege tekst.

---

### 4. Een verhuring weigeren

Controleer voor je iets verandert, in deze volgorde:

| Situatie | Wat je programma toont |
|---|---|
| het nummer ligt niet tussen 1 en 6 | <code style="color:#845ef7">Fiets 9 bestaat niet.</code> |
| de fiets is verhuurd | <code style="color:#845ef7">Fiets 4 is al verhuurd.</code> |
| de fiets is in herstelling | <code style="color:#845ef7">Fiets 6 is in herstelling.</code> |
| de klant bestaat niet | <code style="color:#845ef7">Klant 3 bestaat niet.</code> |
| de klant heeft al 2 fietsen | <code style="color:#845ef7">Tom Wouters heeft al 2 fietsen.</code> |

Daarvoor vraag je na een beschikbare fiets de klant: <code style="color:#845ef7">Klant (1 = Sara Janssens, 2 = Tom Wouters): </code>. Bewaar de twee klanten in een array, en houd per fiets bij aan welke klant ze verhuurd is. Fiets 4 is verhuurd aan Tom Wouters.

Lukt de verhuring, dan toont je programma ook de waarborg die de klant nu betaalt:

<code style="color:#845ef7">Fiets 3 verhuurd aan Sara Janssens om 9:45, contract FV-2026-3644.</code><br>
<code style="color:#845ef7">Nu te betalen: 150 euro waarborg.</code>

> [!TIP]
> Hoeveel fietsen een klant al heeft, staat nergens: je telt het, met een lus over alle fietsen.

> [!WARNING]
> Een geweigerde verhuring mag geen enkele array veranderen. Zet de status pas op *verhuurd* als alle controles geslaagd zijn.

Test je programma en controleer: verhuur fiets 1 aan Tom Wouters, en probeer hem daarna fiets 3 te geven. Dat moet <code style="color:#845ef7">Tom Wouters heeft al 2 fietsen.</code> geven, want fiets 4 had hij al.

---

### 5. Een terugbrenging weigeren

Ook terugbrengen kan mislopen. Controleer, in deze volgorde:

| Situatie | Wat je programma toont |
|---|---|
| het nummer ligt niet tussen 1 en 6 | <code style="color:#845ef7">Fiets 9 bestaat niet.</code> |
| de fiets is niet verhuurd | <code style="color:#845ef7">Fiets 1 is niet verhuurd.</code> |
| de terugkomst ligt niet na het vertrek | <code style="color:#845ef7">De terugkomst ligt niet na het vertrek.</code> |

In die gevallen verschijnt er geen contract, en blijft de fiets zoals ze was. Het contract toont nu ook de klant:

<code style="color:#845ef7">Klant         : Tom Wouters</code>

Test je programma en controleer:

- fiets <code style="color:#e64980;font-weight:600">4</code> terug om <code style="color:#e64980;font-weight:600">8</code>:<code style="color:#e64980;font-weight:600">0</code>: geweigerd, want ze vertrok om 8:30;
- fiets <code style="color:#e64980;font-weight:600">4</code> terug om <code style="color:#e64980;font-weight:600">17</code>:<code style="color:#e64980;font-weight:600">45</code>: <code style="color:#845ef7">Duur          : 9 u 15 min (10 begonnen uren)</code> en <code style="color:#845ef7">Huurprijs     : 30 euro</code>.

---

### 6. Overzicht per klant

Voeg de keuze <code style="color:#845ef7">4. Overzicht per klant</code> toe. Toon voor elke klant welke fietsen die nu heeft, en sinds wanneer:

```text
Sara Janssens:
- geen fietsen
Tom Wouters:
- fiets 4, e-bike Cube, sinds 8:30
```

> [!TIP]
> Twee lussen in elkaar: de buitenste over de klanten, de binnenste over de fietsen. Het aantal fietsen tel je per klant opnieuw vanaf 0.

---

### 7. Alles samen: een fiets terugbrengen

<span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span>

Een fiets die terugkomt, is pas echt terug als de huur betaald is. In dit laatste punt verandert de status van een fiets pas na de betaling.

Na het contract vraagt je programma <code style="color:#845ef7">Betaald (j/n): </code>.

- Bij <code style="color:#e64980;font-weight:600">j</code> wordt de fiets weer beschikbaar, is ze niet langer aan de klant verhuurd, en toont je programma <code style="color:#845ef7">Fiets 4 is terug en weer beschikbaar.</code>
- Bij <code style="color:#e64980;font-weight:600">n</code> toont je programma <code style="color:#845ef7">Niet betaald: fiets 4 blijft verhuurd.</code>, en verandert er niets.

```text
Fiets (1-6): 4
Terug uur: 17
Terug minuut: 45
==============================
FIETSVERHUUR - HUURCONTRACT
==============================
Contract      : FV-2026-4821
Klant         : Tom Wouters
Fiets         : e-bike Cube (6 euro per uur, max 30 euro)
Vertrek       : 8:30
Terug         : 17:45
Duur          : 9 u 15 min (10 begonnen uren)
Huurprijs     : 30 euro
Boete         : 0 euro
------------------------------
Bij vertrek   : 150 euro (waarborg)
Bij terugkomst: 30 euro (waarborg terug)
==============================
Betaald (j/n): j
Fiets 4 is terug en weer beschikbaar.
```

> [!TIP]
> Een `j` of `n` lees je met `sc.next().charAt(0)`: het eerste teken van wat de gebruiker typt (leerboek 1.5).

> [!IMPORTANT]
> Na een *n* moet alles nog staan zoals voor de terugbrenging: de fiets verhuurd, aan dezelfde klant, met hetzelfde vertrek. Controleer het met het overzicht per klant.

Test je programma en controleer:

- breng fiets 4 terug zonder te betalen, en vraag het overzicht per klant: Tom Wouters heeft nog altijd <code style="color:#845ef7">- fiets 4, e-bike Cube, sinds 8:30</code>;
- breng ze daarna terug met betaling: Tom Wouters heeft <code style="color:#845ef7">- geen fietsen</code>;
- verhuur fiets 5 aan Sara Janssens om 14:05, en breng ze terug om 20:30: <code style="color:#845ef7">Huurprijs     : 40 euro</code>, <code style="color:#845ef7">Boete         : 10 euro</code> en <code style="color:#845ef7">Bij terugkomst: 50 euro (waarborg terug)</code>.

> [!NOTE]
> Het woord voor het type van een fiets kies je nu op drie plaatsen met dezelfde ternary operator. Verandert er een type, dan moet je drie plaatsen aanpassen. In hoofdstuk 03 maak je er een methode van, en in hoofdstuk 06 een enum.
