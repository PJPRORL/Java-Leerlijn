# Verhaallijn van de zes reeksen

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; vaste gegevens, vaste regels en hoe elk project groeit van hoofdstuk 01 tot en met 24

Elke reeks is één project dat zes delen lang meegroeit: van een programma in de console tot services in een Kubernetes-cluster. Dit bestand legt vast wat in alle oefeningen van een reeks hetzelfde blijft: de startgegevens, de bedrijfsregels met hun getallen, en de namen van klassen, services en topics.

> [!IMPORTANT]
> Gebruik exact deze startgegevens en deze regels. De voorbeelden in de opgaven, de nakijkpunten in de oplossingen en het nakijkscript rekenen ermee. Een andere prijs of een ander tarief geeft een ander resultaat, ook als je code juist is.

> [!NOTE]
> Je hoeft dit bestand niet vooraf te lezen. Elke opgave herhaalt de gegevens en regels die ze nodig heeft. Kom hier kijken als je twijfelt, of als je een reeks halverwege oppikt.

---

## 1. Hoe je project groeit

Elke oefening krijgt een eigen map, bijvoorbeeld `Oefening_05_02`. Je begint telkens met een kopie van de map van de vorige oefening uit dezelfde reeks: 05_02 vertrekt van 04_02. Zo blijft elke oefening apart nakijkbaar.

| Hoofdstuk | Vorm van je project | Starten |
|---|---|---|
| 01 – 03 | één bestand `src/Main.java` | `java src/Main.java` |
| 04 – 05 | meerdere klassen in `src`, zonder package | `javac -d out src/*.java` en `java -cp out Main` |
| 06 – 09 | packages onder `src`, bv. `src/be/bitenbyte/model/Artikel.java` | de knop **Run** boven `main` in VS Code, of `javac -d out` met alle bestanden en `java -cp out be.bitenbyte.app.Main` |
| 09 (●●●) | Java-modules met `module-info.java` | zoals in leerboek 3.12 |
| 10 – 13 | Spring Boot met Maven, gemaakt op start.spring.io | `.\mvnw.cmd spring-boot:run` (Windows) of `./mvnw spring-boot:run` |
| 11 – 13 | + PostgreSQL in Docker via `compose.yaml`, zoals in leerboek 4.7 | `docker compose up -d`, daarna je API |
| 14 – 15 | + `Dockerfile`, daarna alles in `compose.yaml` | `docker compose up -d --build` |
| 16 – 18 | één map per service naast elkaar, bv. `Oefening_16_01/catalogus-service` | elke service apart, of alles via Compose |
| 19 | Kafka in Docker (KRaft) | de Kafka-CLI via `docker exec` |
| 20 – 21 | je services + Kafka in `compose.yaml` | `docker compose up -d` |
| 22 – 23 | + map `k8s/` met je manifests | `minikube start`, `kubectl apply -f k8s/` |
| 24 | + `AGENTS.md` en een map met skills, zoals in leerboek 9.4 en 9.5 | Copilot in VS Code |

**Packages en groepsnamen.** De drie Bit & Byte-reeksen gebruiken `be.bitenbyte`, de spiegelreeksen `be.dierenasiel`, `be.fietsverhuur` en `be.concertzaal`.

**Poorten vanaf hoofdstuk 16.** Gateway 8080 · eerste service 8081 · tweede service 8082 · derde service 8083 · Eureka 8761 · Config Server 8888 · Zipkin 9411 · PostgreSQL 5432 · RabbitMQ 5672 en 15672 · Kafka 9092.

---

## 2. Reeks _01 — Bit & Byte, artikelen <span style="color:#37b24d">●○○</span>

De catalogus en de voorraad van de webwinkel Bit & Byte. Eén centraal ding: het artikel.

### Startgegevens

| Id | Naam | Merk | Soort | Prijs | Voorraad |
|---|---|---|---|---|---|
| 1 | B650 Tomahawk | MSI | moederbord | 219.00 | 12 |
| 2 | Ryzen 5 7600X | AMD | processor | 199.00 | 3 |
| 3 | Ryzen 7 7800X3D | AMD | processor | 389.00 | 5 |
| 4 | GeForce RTX 4070 | ASUS | videokaart | 589.00 | 0 |
| 5 | Radeon RX 7800 XT | Sapphire | videokaart | 499.00 | 2 |
| 6 | NH-D15 | Noctua | koeler | 109.00 | 7 |

### Vaste regels

| Regel | Waarde |
|---|---|
| Prijzen | inclusief 21 % btw. Prijs zonder btw = prijs / 1,21; btw = prijs − prijs zonder btw |
| Voorraadstatus | 0: `uitverkocht` · 1 of 2: `bijna uitverkocht` · 3 of meer: `op voorraad` |
| Verzendkost | 4,99 euro onder 50 euro; vanaf 50 euro gratis |
| Merk en soort zoeken | hoofdletters spelen geen rol |

### Wat erbij komt

| Hoofdstuk | Wat het project dan is |
|---|---|
| 01 – 03 | één artikel, daarna zes in arrays, daarna opgesplitst in methodes |
| 04 – 06 | klasse `Artikel`, subklassen per soort, enum `Soort`, packages `model`, `repository`, `app` |
| 07 – 09 | `List`, `Map` en `Set`, opslaan in `artikelen.csv`, rapporten met streams |
| 10 – 13 | REST-API op `/api/artikelen`, JPA en PostgreSQL, validatie, tests |
| 14 – 15 | image `bitenbyte/artikel-api`, Compose met PostgreSQL |
| 16 – 18 | `catalogus-service` (8081) en `voorraad-service` (8082), achter een gateway |
| 19 – 21 | topic `artikel-events`, key = Id van het artikel |
| 22 – 23 | namespace `bitenbyte` |
| 24 | feature: een verlanglijst per klant |

---

## 3. Reeks _02 — Bit & Byte, bestellingen <span style="color:#f08c00">●●○</span>

Klanten die bestellen bij Bit & Byte. Meerdere dingen die bij elkaar horen: klant, bestelling en bestellijn. De regels werken op elkaar in.

### Startgegevens

| Nummer | Naam | Soort klant | Btw-nummer |
|---|---|---|---|
| K001 | Lotte Peeters | particulier | |
| K002 | Yusuf Demir | particulier | |
| K003 | Bureau Claes bv | zakelijk | BE0456789123 |

De artikelen zijn dezelfde zes als in reeks _01, met dezelfde Ids, prijzen en voorraad.

### Vaste regels

| Regel | Waarde |
|---|---|
| Bestellijnen | hoogstens 5 per bestelling; hetzelfde artikel opnieuw bestellen telt bij de bestaande lijn bij |
| Aantal per artikel | 1 tot en met 20, ook samen over meerdere keren bestellen |
| Staffelkorting per lijn | vanaf 5 stuks 5 %, vanaf 10 stuks 10 % |
| Verzendkost | 4,99 euro als het totaal na korting onder 50 euro ligt; vanaf 50 euro gratis |
| Zakelijke klant | ziet bedragen zonder btw, het btw-nummer staat op de bon |
| Btw-nummer | `BE0` of `BE1` gevolgd door 9 cijfers |
| Verzenddoos | maximaal 3 artikelen per doos |
| Spaarpunten | 1 punt per volle 10 euro van het lijnbedrag, zonder verzendkost |
| Levertermijn | alles op voorraad: 1 werkdag; anders 5 werkdagen |
| Bestelnummer | `BB-` + jaar + `-` + 5 cijfers, bv. `BB-2026-00042` |
| Status | `NIEUW` → `BETAALD` → `VERZONDEN` → `GELEVERD`; `GEANNULEERD` enkel vanuit `NIEUW` of `BETAALD` |
| Voorraad | een bestelling verlaagt de voorraad pas als alle lijnen geldig zijn |

### Wat erbij komt

| Hoofdstuk | Wat het project dan is |
|---|---|
| 01 – 03 | één bestellijn, daarna een bestelling in arrays, daarna een bestelbon uit één regel invoer |
| 04 – 06 | klassen `Klant`, `Bestelling`, `Bestellijn`; particulier en zakelijk; enum `BestelStatus`; kortingsregels achter een interface |
| 07 – 09 | generieke repository, bestellingen in een `Map`, exceptions die niets half achterlaten, meerdere kassa's tegelijk |
| 10 – 13 | REST-API op `/api/klanten` en `/api/bestellingen`, relaties in PostgreSQL, Testcontainers |
| 14 – 15 | image `bitenbyte/bestel-api`, Compose met PostgreSQL en pgAdmin |
| 16 – 18 | `klant-service` (8081), `artikel-service` (8082), `bestelling-service` (8083); resilience op de oproep naar de artikelen |
| 19 – 21 | topic `bestelling-events`, key = bestelnummer; dead letter topic `bestelling-events.DLT` |
| 22 – 23 | namespace `bitenbyte-bestellingen` |
| 24 | feature: retourzendingen |

---

## 4. Reeks _03 — Bit & Byte, PC-configurator <span style="color:#f03e3e">●●●</span>

Klanten stellen zelf een pc samen; de configurator controleert of de onderdelen samen passen. Je ontwerpt meer zelf, en elke regel heeft randgevallen.

### Startgegevens

| Code | Soort | Naam | Prijs | Technische gegevens |
|---|---|---|---|---|
| CPU-01 | processor | Ryzen 5 7600X | 199.00 | socket AM5 · 105 W |
| CPU-02 | processor | Ryzen 7 7800X3D | 389.00 | socket AM5 · 120 W |
| CPU-03 | processor | Core i5-14600K | 299.00 | socket LGA1700 · 125 W |
| MB-01 | moederbord | B650 Tomahawk | 219.00 | socket AM5 · DDR5 · ATX |
| MB-02 | moederbord | B760M DS3H DDR4 | 119.00 | socket LGA1700 · DDR4 · mATX |
| RAM-01 | geheugen | 32 GB DDR5-6000 | 109.00 | DDR5 |
| RAM-02 | geheugen | 16 GB DDR4-3200 | 45.00 | DDR4 |
| GPU-01 | videokaart | GeForce RTX 4070 | 589.00 | 200 W · 267 mm |
| GPU-02 | videokaart | Radeon RX 7800 XT | 499.00 | 263 W · 320 mm |
| PSU-01 | voeding | Voeding 550 W | 69.00 | 550 W |
| PSU-02 | voeding | Voeding 750 W | 109.00 | 750 W |
| PSU-03 | voeding | Voeding 850 W | 139.00 | 850 W |
| KAST-01 | behuizing | Compact M | 79.00 | mATX · videokaart tot 300 mm |
| KAST-02 | behuizing | Midi Air | 99.00 | ATX · videokaart tot 360 mm |

### Vaste regels, in deze volgorde gecontroleerd

| Nr | Regel | Fout als |
|---|---|---|
| 1 | Socket | de socket van de processor verschilt van die van het moederbord |
| 2 | Geheugen | het geheugentype verschilt van dat van het moederbord |
| 3 | Formfactor | een ATX-moederbord in een mATX-kast (mATX past in beide kasten) |
| 4 | Lengte | de videokaart is langer dan de kast toelaat |
| 5 | Vermogen | de voeding levert minder dan het advies |

**Voedingsadvies** = (watt processor + watt videokaart + 75 W) × 1,3, naar boven afgerond op een veelvoud van 50 W.

| Voorbeeld | Berekening | Advies |
|---|---|---|
| CPU-01 + GPU-01 | (105 + 200 + 75) × 1,3 = 494 | 500 W |
| CPU-02 + GPU-02 | (120 + 263 + 75) × 1,3 = 595,4 | 600 W |
| CPU-03 + GPU-02 | (125 + 263 + 75) × 1,3 = 601,9 | 650 W |

### Wat erbij komt

| Hoofdstuk | Wat het project dan is |
|---|---|
| 01 – 03 | het voedingsadvies, daarna een compatibiliteitstabel in een tweedimensionale array, daarna codes ontleden en builds zoeken met recursie |
| 04 – 06 | een klasse per soort onderdeel, een klassenhiërarchie, elke regel een eigen klasse achter de interface `CompatibiliteitsRegel` |
| 07 – 09 | onderdelen per socket in een `TreeMap`, een buildbestand inlezen, de beste build zoeken met streams en modules |
| 10 – 13 | `POST /api/builds/controle` met een buildrapport, onderdelen in PostgreSQL, een geparametriseerde test per regel |
| 14 – 15 | een image klaar voor productie, één image in twee omgevingen |
| 16 – 18 | `onderdelen-service` (8081) en `configurator-service` (8082); de regels in een Config Server; controle via RabbitMQ |
| 19 – 21 | topics `build-controles` en `build-resultaten`, key = Id van de build; transacties |
| 22 – 23 | namespace `bitenbyte-configurator` |
| 24 | een skill voor een nieuwe compatibiliteitsregel |

---

## 5. Reeks _04 — Dierenasiel <span style="color:#37b24d">●○○</span>

Dierenasiel Pootjes vangt honden, katten en konijnen op tot ze een nieuwe thuis vinden. Zelfde niveau en zelfde technieken als reeks _01, in een ander domein.

### Startgegevens

| Id | Naam | Soort | Leeftijd (maanden) | Gewicht (kg) | Status |
|---|---|---|---|---|---|
| 1 | Rex | hond | 30 | 28.5 | beschikbaar |
| 2 | Luna | kat | 7 | 3.1 | beschikbaar |
| 3 | Max | hond | 110 | 31.0 | beschikbaar |
| 4 | Pluis | konijn | 14 | 2.2 | gereserveerd |
| 5 | Mimi | kat | 50 | 4.4 | beschikbaar |
| 6 | Bobby | hond | 4 | 6.0 | beschikbaar |

### Vaste regels

| Regel | Waarde |
|---|---|
| Jong | jonger dan 12 maanden |
| Senior | 96 maanden (8 jaar) of ouder |
| Adoptieprijs | hond 150 euro · kat 90 euro · konijn 40 euro; een senior betaalt de helft |
| Voer per dag | 20 gram per kg lichaamsgewicht, naar boven afgerond op 10 gram |
| Leeftijd tonen | in jaren en maanden, bv. `2 jaar en 6 maanden` |
| Status | `beschikbaar` → `gereserveerd` → `geadopteerd`; een reservatie kan terug naar `beschikbaar`. Als getal, tot hoofdstuk 06: 0, 1 en 2 |

### Wat erbij komt

Zelfde opbouw als reeks _01, met `Dier` in plaats van `Artikel`: REST-API op `/api/dieren`, image `dierenasiel/dier-api`, services `dier-service` (8081) en `adoptie-service` (8082), topic `adoptie-events` met key = Id van het dier, namespace `dierenasiel`. Feature in hoofdstuk 24: een wachtlijst voor adopties.

---

## 6. Reeks _05 — Fietsverhuur <span style="color:#f08c00">●●○</span>

Fietsverhuur Trapper verhuurt stadsfietsen, e-bikes en bakfietsen per uur. Zelfde niveau en zelfde technieken als reeks _02, in een ander domein: dingen die bij elkaar horen (fiets, klant, verhuring) en regels die op elkaar inwerken.

### Startgegevens

| Id | Type | Merk | Status |
|---|---|---|---|
| 1 | stadsfiets | Gazelle | beschikbaar |
| 2 | stadsfiets | Oxford | beschikbaar |
| 3 | e-bike | Stromer | beschikbaar |
| 4 | e-bike | Cube | verhuurd |
| 5 | bakfiets | Babboe | beschikbaar |
| 6 | bakfiets | Urban Arrow | in herstelling |

| Klantnummer | Naam |
|---|---|
| F001 | Sara Janssens |
| F002 | Tom Wouters |

Fiets 4 is bij de start verhuurd aan Tom Wouters, sinds 8:30 uur, met contract `FV-2026-4821`.

### Vaste regels

| Type | Per begonnen uur | Maximum per dag | Waarborg |
|---|---|---|---|
| stadsfiets | 3 euro | 15 euro | 50 euro |
| e-bike | 6 euro | 30 euro | 150 euro |
| bakfiets | 8 euro | 40 euro | 100 euro |

| Regel | Waarde |
|---|---|
| Begonnen uren | (minuten + 59) / 60 met gehele getallen: 61 minuten zijn 2 begonnen uren |
| Prijs per dag | het kleinste van begonnen uren × uurtarief en het dagmaximum |
| Langer dan 24 uur | elke begonnen 24 uur wordt apart berekend |
| Te laat | terug na 20:00 uur: 10 euro boete; om precies 20:00 uur is op tijd |
| Contractnummer | `FV-` + jaar + `-` + 4 cijfers, bv. `FV-2026-4821` |
| Per klant | maximaal 2 fietsen tegelijk |
| Status fiets | `beschikbaar` → `verhuurd` → `beschikbaar`; `in herstelling` kan je niet verhuren. Als getal, tot hoofdstuk 06: 0, 1 en 2 |
| Terugbrengen | de fiets wordt pas weer beschikbaar als de klant betaald heeft |

### Wat erbij komt

Zelfde opbouw als reeks _02: REST-API op `/api/fietsen`, `/api/klanten` en `/api/verhuringen`, image `fietsverhuur/verhuur-api`, services `fiets-service` (8081), `klant-service` (8082) en `verhuur-service` (8083), topic `verhuur-events` met key = Id van de fiets, namespace `fietsverhuur`. Feature in hoofdstuk 24: schademeldingen.

---

## 7. Reeks _06 — Concertzaal <span style="color:#f03e3e">●●●</span>

Concertzaal De Klankkast verkoopt tickets voor genummerde plaatsen. Zelfde niveau en zelfde technieken als reeks _03, in een ander domein: een zaalplan met plaatsen die nooit twee keer verkocht mogen worden.

### Startgegevens

De zaal heeft 10 rijen, `A` tot en met `J`, en 20 stoelen per rij: 200 plaatsen.

| Id | Artiest | Datum |
|---|---|---|
| 1 | The Lunar Echoes | 2026-12-04 |
| 2 | Brasserie Blues Band | 2026-12-12 |
| 3 | Noor & de Strijkers | 2027-01-09 |

Bij de start zijn 50 plaatsen al verkocht, voor elk concert dezelfde:

| Rij | Bezet |
|---|---|
| A | stoel 1 tot en met 8, en 13 tot en met 20 |
| B | stoel 5 tot en met 10 |
| C | stoel 12 |
| D | stoel 1 tot en met 4 |
| F | de hele rij |
| H | stoel 10 |
| J | stoel 19 en 20 |

### Vaste regels

| Regel | Waarde |
|---|---|
| Plaatsnummer | 1 tot en met 200; rij = (nummer − 1) / 20, met 0 = `A`; stoel = (nummer − 1) % 20 + 1 |
| Plaatscode | rij en stoel, bv. plaats 52 is `C12` |
| Categorie en basisprijs | rij A–C: categorie 1, 65 euro · rij D–G: categorie 2, 49 euro · rij H–J: categorie 3, 35 euro (inclusief 6 % btw) |
| Servicekost | 2,50 euro per ticket |
| Jongerenkorting | jonger dan 26: 20 % op de basisprijs, niet op de servicekost |
| Per bestelling | maximaal 8 tickets |
| Beste vrije plaats | de eerste vrije plaats, gelezen zoals een boek: rij A van links naar rechts, dan rij B |
| Groep | 1 tot en met 8 personen, op stoelen naast elkaar in dezelfde rij |
| Annuleren | tot 48 uur voor het concert |
| Een plaats | wordt per concert nooit twee keer verkocht |

### Wat erbij komt

Zelfde opbouw als reeks _03: REST-API op `/api/concerten` en `/api/tickets`, image `concertzaal/ticket-api`, services `concert-service` (8081) en `ticket-service` (8082), topic `ticket-events` met key = Id van het concert, namespace `concertzaal`. In hoofdstuk 24: een skill voor een nieuwe tariefregel.
