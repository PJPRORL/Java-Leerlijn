# Oplossingen — hoofdstuk 02

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; zes oplossingen &nbsp;·&nbsp; bij de map *Oefeningen Hoofdstuk 02*

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
> Vanaf dit hoofdstuk toont je programma vaak een menu dat terugkomt na elke keuze. De nakijkpunten tonen enkel de regels die ertoe doen; het menu zelf staat er niet telkens bij.

---

## De oplossingen

### Hoofdstuk 02 — beslissingen, lussen en arrays

| Oplossing | Domein | Waar het om draait |
|---|---|---|
| [02_01](Oplossing_02_01.md) | PC-onderdelen | parallelle arrays, een `switch` in een `do-while`, en zoeken met een vlag |
| [02_02](Oplossing_02_02.md) | PC-onderdelen | eerst alles controleren, dan pas iets aanpassen, met `break` en `continue` in een lus |
| [02_03](Oplossing_02_03.md) | PC-onderdelen | een tabel als tweedimensionale array, en alle combinaties doorlopen met geneste lussen |
| [02_04](Oplossing_02_04.md) | Dierenasiel | zoals 02_01, met een status als getal en een gemiddelde dat je afrondt |
| [02_05](Oplossing_02_05.md) | Fietsverhuur | zoals 02_02, met wat tussen twee menukeuzes bewaard moet blijven |
| [02_06](Oplossing_02_06.md) | Concertzaal | zoals 02_03, met een zaalplan en een gelabelde `break` |

---

## Wat in elke oplossing terugkomt

- **Eén index voor alles wat bij elkaar hoort.** Naam, prijs en voorraad staan in aparte arrays, op dezelfde plaats. Wie een element verwijdert of verplaatst, doet dat in elke array.
- **Eerst controleren, dan veranderen.** Een fout in de invoer stopt een actie voordat er iets aangepast is. Dan hoef je nooit iets terug te draaien.
- **Een teller of een vlag op de juiste plaats.** Wat per rij of per klant opnieuw moet beginnen, staat binnen de buitenste lus; wat voor alles samen geldt, ervoor.
- **Enkel leerstof van leerboek 1.1 tot en met 1.7**, plus naslag 10.2. Waar een later hoofdstuk iets beters biedt, zoals een methode of een enum, staat dat er in één zin bij.

> [!NOTE]
> Elk programma is gecompileerd en uitgevoerd met Java 21, met Belgisch-Nederlandse taalinstellingen: elk punt apart, met de invoer van de opgave en de randgevallen, samen 74 runs. Alle uitvoer in de opgaven en de nakijkpunten komt uit die runs. Ook de veelgemaakte fouten met een concrete uitvoer zijn zo nagegaan.
