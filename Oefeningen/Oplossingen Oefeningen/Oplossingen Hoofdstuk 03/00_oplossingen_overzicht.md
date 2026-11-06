# Oplossingen — hoofdstuk 03

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; zes oplossingen &nbsp;·&nbsp; bij de map *Oefeningen Hoofdstuk 03*

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
> Vanaf dit hoofdstuk toont je programma bedragen met `printf` en `String.format`. Op een Belgische of Nederlandse Windows staat er een komma: `398,00 euro`. Zie je een punt, dan staan je taalinstellingen anders; je oplossing is daarom niet fout.

---

## De oplossingen

### Hoofdstuk 03 — strings en methodes

| Oplossing | Domein | Waar het om draait |
|---|---|---|
| [03_01](Oplossing_03_01.md) | PC-onderdelen | methodes met en zonder returnwaarde, zoeken zonder hoofdletters, een tabel met `printf`, en een fiche als tekst |
| [03_02](Oplossing_03_02.md) | PC-onderdelen | een lijn als `2x3` ontleden en controleren voor `parseInt`, `%%` in een opmaak, en een regel die pas aanvaard wordt als alles klopt |
| [03_03](Oplossing_03_03.md) | PC-onderdelen | codes ontleden, overloading met `code`, en een recursieve zoektocht die ook een voorstel kan doen |
| [03_04](Oplossing_03_04.md) | Dierenasiel | zoals 03_01, met een leeftijd als tekst en een `switch` die een waarde teruggeeft |
| [03_05](Oplossing_03_05.md) | Fietsverhuur | zoals 03_02, met tijden als `09:45` en een klant op nummer of naam |
| [03_06](Oplossing_03_06.md) | Concertzaal | zoals 03_03, met plaatscodes, `isVrij` voor een plaats of een reeks, en een zaalrapport |

---

## Wat in elke oplossing terugkomt

- **Eerst opsplitsen, dan veranderen.** Punt 2 verandert niets aan de uitvoer. Pas als de methodes werken, verander je wat het programma toont. Zo weet je bij een fout altijd of ze uit het opsplitsen komt, of uit het nieuwe werk.
- **Een methode die rekent, toont niets.** Ze geeft een waarde of een tekst terug, en `main` beslist wat ermee gebeurt. Methodes die iets tonen, beginnen met `toon`.
- **Controleren voor je omzet.** `Integer.parseInt` krijgt enkel tekst die je eerst teken per teken nagekeken hebt. Een fout in de invoer geeft een melding, nooit een crash.
- **Enkel leerstof van leerboek 1.1 tot en met 1.9**, plus naslag 10.1 en 10.2. Waar een later hoofdstuk iets beters biedt, zoals een klasse of `try`/`catch`, staat dat er in één zin bij.

> [!NOTE]
> Elk programma is gecompileerd en uitgevoerd met Java 21, met Belgisch-Nederlandse taalinstellingen: elk punt apart, met de invoer van de opgave en de randgevallen, samen 52 runs. Alle uitvoer in de opgaven en de nakijkpunten komt uit die runs. Ook de veelgemaakte fouten met een concrete uitvoer of foutmelding zijn zo nagegaan, met 48 aangepaste versies van de programma's.
