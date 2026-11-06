# Formaat van een oefening en een oplossing (versie 1)

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; de vaste opbouw van elke opgave en elke oplossing

Dit bestand legt vast hoe een oefening en een oplossing eruitzien. Elke nieuwe oefening volgt het, zodat je overal dezelfde dingen op dezelfde plaats terugvindt. Het formaat is dat van de oefeningen van Programming Advanced (C#), aangepast aan Java.

---

## 1. Mappen en bestandsnamen

```text
Oefeningen/
├─ Oefeningen/
│  ├─ 00_overzicht.md                          alle hoofdstukken en reeksen
│  └─ Oefeningen Hoofdstuk 01/
│     ├─ 00_overzicht.md                       dit hoofdstuk: tabel, opbouw, volgorde, buiten bereik
│     └─ Oefening_01_01.md … Oefening_01_06.md
├─ Oplossingen Oefeningen/
│  └─ Oplossingen Hoofdstuk 01/
│     ├─ 00_oplossingen_overzicht.md           zo kijk je na, en de oplossingen van dit hoofdstuk
│     └─ Oplossing_01_01.md … Oplossing_01_06.md
├─ Verbeteringen Oefeningen/                   het nakijkscript (later)
└─ Ontwerp/
   ├─ formaat-oefening.md                      dit bestand
   └─ verhaallijn.md                           startgegevens en vaste regels per reeks
```

- Nummer: `HH_RR`, met `HH` het hoofdstuk (01 tot en met 24) en `RR` de reeks (01 tot en met 06).
- Je eigen werk: één map per oefening, met dezelfde naam als de opgave zonder `.md`, bv. `Oefening_05_02`. Je begint met een kopie van de map van de vorige oefening uit dezelfde reeks.

---

## 2. Kop van een opgave

```markdown
# 05_02

<b>Hoofdstuk 05</b> &nbsp;·&nbsp; Overriding en encapsulatie &nbsp;·&nbsp; PC-onderdelen &nbsp;·&nbsp; niveau <span style="color:#f08c00">●●○</span> &nbsp;·&nbsp; ±90 min
```

De metaregel bevat altijd vijf delen: hoofdstuk, ondertitel, domein, niveau en duur. De ondertitel is gelijk voor de twee oefeningen van hetzelfde niveau: 05_02 en 05_05 hebben dezelfde ondertitel.

---

## 3. Opbouw van een opgave

| Onderdeel | Inhoud |
|---|---|
| `## Leerdoel` | twee of drie korte alinea's: *Na deze oefening kan je …*, *Je leert …*, *Daarnaast oefen je …* |
| `## Opdracht` | het scenario in een paar zinnen; een genummerde lijst van wat het programma of de API moet kunnen; de zin *Implementeer onderstaande functionaliteiten.* |
| `### 1. Je vertrekpunt` | vanaf hoofdstuk 02: van welke oefening je vertrekt, en een tabel van wat na deze oefening nog precies hetzelfde moet werken. In hoofdstuk 01: het nieuwe project opzetten |
| `### 2.` tot `### N-1.` | één techniek per punt, met de exacte invoer, uitvoer of verzoeken |
| `### N. Alles samen: …` | de samengestelde oefening, met de paarse badge onder de titel |

- Tussen twee punten staat een lijn `---`.
- Een punt eindigt met *Test je … en controleer:* en een korte lijst, als er iets te testen valt.
- Zeven punten is de richtlijn; nooit meer dan negen.

### Per soort oefening

| Hoofdstuk | Wat je test | Hoe de opgave het toont |
|---|---|---|
| 01 – 09 | een programma in de console | een **voorbeeldrun** in een `text`-blok, en een tabel *Invoer* → *Wat je programma toont* |
| 10 – 18 | een REST-API | een tabel *Verzoek* → *Antwoord* met verb, route en statuscode; bodies in een `json`-blok |
| 14 – 23 | Docker, Kafka, Kubernetes | een tabel *Commando* → *Wat je ziet*; commando's in een `powershell`-blok, met de Linux-variant erbij als die verschilt |
| 24 | werken met een coding agent | de bestanden die je oplevert (`PRD.md`, `AGENTS.md`, een skill) en een checklist |

### Afspraken voor de console (hoofdstuk 01 tot en met 09)

- In een voorbeeldrun staat wat je intypt op dezelfde regel als de vraag, zoals in je terminal. De vraag eindigt op `: `.
- Bedragen: in hoofdstuk 01 en 02 druk je ze af met `+`, dus met een punt (`219.0`). Vanaf hoofdstuk 03 met `printf("%.2f")`. Dat toont een komma of een punt, afhankelijk van de taalinstelling van je computer. De nakijkpunten aanvaarden beide.
- Je programma schrijft `euro`, nooit `€`. In de Windows-console wordt `€` vaak een `?`. Om dezelfde reden staan er in letterlijke uitvoer geen letters met accenten.

### Afspraken voor de API (vanaf hoofdstuk 10)

- Routes beginnen met `/api/` en gebruiken het meervoud: `/api/artikelen`, `/api/artikelen/{id}`.
- Lagen zoals in leerboek 4.5: `ArtikelController`, `ArtikelService`, `ArtikelRepository`. Een service met een interface en een `…Impl` mag; de opgave zegt het als het moet.
- Een foutboodschap is gewone tekst in de body, tot hoofdstuk 12 het foutformaat met `@RestControllerAdvice` invoert.
- JSON-namen zijn de namen van je velden: `aantalOpVoorraad`.

### Namen

- Het domein in het Nederlands: `Artikel`, `aantalOpVoorraad`, `Bestellijn`.
- Technische rollen in het Engels, zoals in het leerboek: `Repository`, `Service`, `Controller`, `Dto`, `Mapper`.
- Een verwijzing naar het leerboek schrijf je als *(leerboek 2.7)*, naar de naslag als *(naslag 10.5)*.

---

## 4. De kaders

| Kader | Gebruik | Hoe vaak |
|---|---|---|
| `> [!NOTE]` | iets om te onthouden, of een vraag die je zelf moet beantwoorden | waar het past |
| `> [!TIP]` | een aanwijzing die het werk korter maakt, met een verwijzing naar het leerboek of de naslag | hoogstens één per punt |
| `> [!IMPORTANT]` | een eis die je oplossing moet halen | enkel voor wat echt telt |
| `> [!WARNING]` | een valkuil waar de meeste oplossingen stuklopen | hoogstens één per punt |

---

## 5. Kleuren en badges

Alle kleuren staan in HTML, zodat ze werken in de preview van VS Code. Het nakijkscript en de Word-versie herkennen dezelfde fragmenten.

| Element | Fragment |
|---|---|
| Verb | `<span style="background:#1c7ed6;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>GET</b></span>`; POST `#2f9e44`, PUT `#d9822b`, DELETE `#e03131` |
| Route | `<code style="color:#0c8599;background:rgba(34,184,207,.18);white-space:nowrap">/api/artikelen/<span style="color:#e64980"><b>{id}</b></span></code>` |
| Statuscode | `<code style="color:#37b24d;font-weight:600">200 OK</code>`; 4xx `#f08c00`, 5xx `#f03e3e` |
| Invoer of parameter | `<code style="color:#e64980;font-weight:600">3</code>` |
| Letterlijke tekst | `<code style="color:#845ef7">We vonden geen artikel met Id 7.</code>` |
| Niveau | `<span style="color:#37b24d">●○○</span>`, `<span style="color:#f08c00">●●○</span>`, `<span style="color:#f03e3e">●●●</span>` |
| Samengestelde oefening | `<span style="background:#7048e8;…"><b>SAMENGESTELDE OEFENING</b></span>` |
| Oplossing | `<span style="background:#c2255c;…"><b>OPLOSSING</b></span>` |

---

## 6. Opbouw van een oplossing

```markdown
# Oplossing 05_02

<badge OPLOSSING> · <b>Hoofdstuk 05</b> · Overriding en encapsulatie · PC-onderdelen · bij `Oefening_05_02.md`
```

Daaronder altijd een kader *Eerst zelf* (`[!IMPORTANT]`), en waar nodig een `[!NOTE]` over welke gegevens de nakijkpunten gebruiken.

Daarna per punt van de opgave een stuk `## N. titel`, in deze volgorde:

| Onderdeel | Inhoud | Zichtbaar |
|---|---|---|
| **Nakijkpunten** | wat je programma of API moet doen: invoer en uitvoer, of verzoek, status en antwoord | meteen |
| **Aanpak.** | welke bouwstenen, in welke volgorde en waarom, met verwijzingen naar het leerboek | meteen |
| **Toon de code** | een uitgewerkte oplossing van dat ene punt, in `<details>` | dichtgeklapt |
| **Veelgemaakte fouten** | in een `[!WARNING]`: waar de meeste oplossingen stuklopen | meteen |
| **Zo kan het beter** | in een `[!TIP]`: wat juist is, maar netter of veiliger kan | meteen |

Onderaan staat `## Alles samen: de volledige bestanden`, met elk bestand in een eigen `<details>`.

---

## 7. Kwaliteitsregels

1. **Enkel leerstof tot dan.** Een oefening gebruikt niets wat het leerboek pas later behandelt. De naslag (deel 10) mag altijd. Gebruikt een oefening toch iets wat het leerboek niet voordoet, dan staat dat in het overzicht van het hoofdstuk, onder *Buiten bereik*.
2. **Het vertrekpunt klopt.** Wat *Je vertrekpunt* opsomt, is exact wat de oefening ervoor opleverde.
3. **Dezelfde gegevens.** Startgegevens, regels en letterlijke teksten komen uit `verhaallijn.md`. Een bericht dat in 02_01 bestaat, is in 10_01 nog letter voor letter hetzelfde, tenzij de opgave zegt dat het verandert.
4. **Echte uitvoer.** De voorbeeldruns van hoofdstuk 01 tot en met 09 komen uit een echte run van de oplossing met Java 21.
5. **Eerlijk over wat niet gedraaid heeft.** Code die niet uitgevoerd kon worden (Spring, Docker, Kafka en Kubernetes in de werkomgeving van de auteur), staat zo vermeld in het overzicht van de oplossingen.
6. **Nederlands, vaktermen in het Engels**, korte zinnen, aanspreken met *je*.
