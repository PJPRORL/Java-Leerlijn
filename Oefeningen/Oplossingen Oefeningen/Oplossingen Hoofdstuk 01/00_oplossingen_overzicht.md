# Oplossingen — hoofdstuk 01

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; zes oplossingen &nbsp;·&nbsp; bij de map *Oefeningen Hoofdstuk 01*

Per oefening één bestand, met dezelfde nummering als de opgave. Elk punt van een oefening heeft hier een eigen stuk, in deze volgorde:

| Onderdeel | Wat je eraan hebt | Zichtbaar |
|---|---|---|
| **Nakijkpunten** | wat je programma moet tonen bij welke invoer | meteen |
| **Aanpak** | welke bouwstenen, in welke volgorde, en waarom | meteen |
| **Toon de code** | een uitgewerkte oplossing van dat ene punt | dichtgeklapt |
| <span style="color:#f08c00"><b>Veelgemaakte fouten</b></span> | waar de meeste oplossingen stuklopen | meteen |
| <span style="color:#37b24d"><b>Zo kan het beter</b></span> | wat juist is, maar netter of veiliger kan | meteen |

Onderaan elk bestand staat het volledige programma in één stuk, ook dichtgeklapt.

---

## Zo kijk je na

1. Werk een punt eerst zelf af, tot het werkt of tot je echt vastzit.
2. Start je programma met de invoer uit de nakijkpunten. Klopt elke regel letter voor letter, dan is je punt **juist**, ook als je code er anders uitziet. Er bestaan meerdere juiste oplossingen. Het aantal spaties voor een dubbelpunt telt niet mee.
3. Lees de veelgemaakte fouten. Herken je er een in je eigen code, dan weet je waar je **fout** zit, ook als de nakijkpunten toevallig nog kloppen.
4. Open pas dan de code, en lees *Zo kan het beter*. Daar zie je wat **beter** kan.

> [!IMPORTANT]
> Vergelijk niet regel per regel. Vraag je bij elk verschil af: doet mijn code hetzelfde, en zo ja, welke van de twee lees je over een maand nog het vlotst?

> [!TIP]
> Een kommagetal typ je in zoals je computer het verwacht: met een komma op een Belgische of Nederlandse Windows. De nakijkpunten tonen het zo.

---

## De oplossingen

### Hoofdstuk 01 — variabelen, operatoren en invoer

| Oplossing | Domein | Waar het om draait |
|---|---|---|
| [01_01](Oplossing_01_01.md) | PC-onderdelen | het juiste datatype, afronden na elke berekening, en het Enter-probleem |
| [01_02](Oplossing_01_02.md) | PC-onderdelen | naar boven afronden met `(n + 2) / 3`, en een rest die 0 is |
| [01_03](Oplossing_01_03.md) | PC-onderdelen | alles in cent, afronden met de helft van de deler, en `Math.round` in plaats van een cast |
| [01_04](Oplossing_01_04.md) | Dierenasiel | zoals 01_01, met jaren en maanden en `Math.ceil` voor het voer |
| [01_05](Oplossing_01_05.md) | Fietsverhuur | zoals 01_02, met tijden in minuten en een cast op de juiste plaats |
| [01_06](Oplossing_01_06.md) | Concertzaal | zoals 01_03, met een `char` die rekent en tellen vanaf 0 |

---

## Wat in elke oplossing terugkomt

- **Eerst rekenen, dan tonen.** Elke waarde krijgt een eigen variabele met een duidelijke naam. De samengestelde oefening hergebruikt die variabelen; enkel de `println`-regels veranderen.
- **Een constante voor elke regel van het bedrijf.** Het btw-tarief, het aantal stuks per doos, het sluitingsuur: elk staat één keer in de code, met `final`.
- **Tellen vanaf 0 bij gehele deling en rest.** Werkdagen, plaatsnummers en dozen worden eenvoudiger als je eerst 1 aftrekt en op het einde weer optelt.
- **Enkel leerstof van leerboek 1.1 tot en met 1.4**, plus naslag 10.2. Waar een later hoofdstuk iets beters biedt, zoals `if`, `printf` of een methode, staat dat er in één zin bij.

> [!NOTE]
> Elk programma is gecompileerd en uitgevoerd met Java 21, met Belgisch-Nederlandse taalinstellingen: elk punt apart, met de invoer van de opgave en de randgevallen, samen 106 runs. Alle uitvoer in de opgaven en de nakijkpunten komt uit die runs. Ook de veelgemaakte fouten met een concrete uitvoer zijn zo nagegaan.
