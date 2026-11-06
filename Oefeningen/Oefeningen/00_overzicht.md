# Oefeningen — overzicht van hoofdstuk 01 tot en met 24

<b>Java Leerboek – Exercises</b> &nbsp;·&nbsp; zes oefeningen per hoofdstuk &nbsp;·&nbsp; opgesteld op basis van het Java-leerboek, deel 1 tot en met 9

Dezelfde opmaak en dezelfde opbouw als de oefeningen van Programming Advanced. Twee reeksen die elkaar spiegelen: dezelfde techniek, ander domein. Doe je ze allebei, dan oefen je elk concept twee keer zonder twee keer hetzelfde te lezen.

Elk bestand sluit af met een <span style="background:#7048e8;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>SAMENGESTELDE OEFENING</b></span> : één opdracht waarin alle voorgaande punten van dat bestand samenkomen rond één onderwerp. Dat is telkens het laatste genummerde punt.

De oefeningen staan los van de korte oefeningen in het leerboek. Die controleren of je een hoofdstuk begrepen hebt; deze oefeningen laten je met die leerstof iets bouwen dat groeit.

---

## Zes reeksen die meegroeien

Elke reeks is één project dat van hoofdstuk 01 tot en met 24 meegroeit: van een programma in de console, via klassen en collecties, naar een REST-API, een container, microservices, Kafka en Kubernetes. Elke oefening vertrekt van jouw oplossing van dezelfde reeks in het vorige hoofdstuk: 05_02 van 04_02, 05_03 van 04_03, enzovoort.

| Reeks | Niveau | Domein | Waar het om draait |
|---|---|---|---|
| _01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | De catalogus en de voorraad van Bit & Byte: één centraal ding, het artikel |
| _02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Klanten die bestellen bij Bit & Byte: dingen die bij elkaar horen, regels die op elkaar inwerken |
| _03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | De PC-configurator: past alles samen? Je ontwerpt meer zelf, en elke regel heeft randgevallen |
| _04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau en technieken als _01 |
| _05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau en technieken als _02 |
| _06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau en technieken als _03 |

De startgegevens en de vaste regels van elke reeks, zoals prijzen, tarieven en de volgorde van controles, staan in `Ontwerp/verhaallijn.md`. Elke opgave herhaalt wat ze ervan nodig heeft.

---

## Wat je nodig hebt

| Vanaf hoofdstuk | Wat | Leerboek |
|---|---|---|
| 01 | JDK 21 en VS Code met het *Extension Pack for Java* | 1.1 |
| 10 | Maven via de wrapper (`mvnw`), een project van start.spring.io, Postman of een `.http`-bestand | 4.3 |
| 11 | Docker Desktop, voor PostgreSQL | 4.7, 5.1 |
| 22 | Minikube en kubectl | 8.3 |
| 24 | GitHub Copilot in VS Code | 9.2 |

---

## De kleuren in deze bestanden

De HTTP-verbs dragen dezelfde kleuren als in Swagger UI (leerboek 4.11), zodat je ze herkent uit je eigen testomgeving. De kleuren zijn gekozen om zowel op een licht als op een donker thema leesbaar te blijven.

| Kleur | Waarvoor |
|---|---|
| <span style="background:#1c7ed6;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>GET</b></span> | gegevens ophalen |
| <span style="background:#2f9e44;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>POST</b></span> | gegevens toevoegen |
| <span style="background:#d9822b;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>PUT</b></span> | gegevens bijwerken |
| <span style="background:#e03131;color:#fff;padding:1px 7px;border-radius:4px;font-size:90%"><b>DELETE</b></span> | gegevens verwijderen |
| <code style="color:#37b24d;font-weight:600">200</code> <code style="color:#37b24d;font-weight:600">201</code> <code style="color:#37b24d;font-weight:600">204</code> | gelukt |
| <code style="color:#f08c00;font-weight:600">400</code> <code style="color:#f08c00;font-weight:600">404</code> <code style="color:#f08c00;font-weight:600">409</code> | de client maakte een fout |
| <code style="color:#f03e3e;font-weight:600">500</code> <code style="color:#f03e3e;font-weight:600">503</code> | jouw code of een andere service maakte een fout |
| <code style="color:#0c8599;background:rgba(34,184,207,.18);white-space:nowrap">/api/artikelen/soort/<span style="color:#e64980"><b>{soort}</b></span></code> | een route; het vaste deel blauwgroen |
| <code style="color:#e64980;font-weight:600">{soort}</code> <code style="color:#e64980;font-weight:600">3</code> | wat de gebruiker zelf invult: een routeparameter, of wat je intypt in de console |
| <code style="color:#845ef7">We vonden geen artikel met Id 7.</code> | tekst die je programma of je API letterlijk moet tonen of teruggeven |
| `Artikel` `aantalOpVoorraad` `artikelen.csv` `artikel-events` | klassenamen, eigenschappen, bestanden, topics en invoerwaarden blijven neutraal |

De kaders in de oefeningen betekenen het volgende:

> [!NOTE]
> Iets om te onthouden, meestal een keuze die verderop terugkomt.

> [!TIP]
> Een aanwijzing die je werk korter maakt, vaak met een verwijzing naar het leerboek.

> [!IMPORTANT]
> Een eis waaraan je oplossing moet voldoen.

> [!WARNING]
> Een valkuil. Hier lopen de meeste oplossingen stuk.

De kleuren en de kaders komen volledig tot hun recht in de preview van VS Code. Open een bestand en druk op `Ctrl+Shift+V`.

---

## Deel 1 — Java-basis

### Hoofdstuk 01 — variabelen, operatoren en invoer

Leerboek 1.1 – 1.4

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 01_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Variabelen en datatypes, rekenen met btw, invoer met `Scanner`, de ternary operator | De kassabon voor één artikel |
| 01_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Gehele deling en rest, typecasting, een bestelling verdelen over dozen, een willekeurig bestelnummer met `Random` | De pakbon |
| 01_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Rekenen in eurocent met gehele getallen, naar boven afronden zonder `if`, voorrang van operatoren | Het buildrapport met voedingsadvies |
| 01_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 01_01, met een leeftijd in jaren en maanden | De dierfiche |
| 01_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 01_02, met minuten die je omzet naar begonnen uren | Het huurcontract |
| 01_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 01_03, met een plaatsnummer dat je omzet naar rij en stoel | Het ticket |

### Hoofdstuk 02 — beslissingen, lussen en arrays

Leerboek 1.5 – 1.7

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 02_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | De voorraad in arrays, een menu met `switch` in een lus, zoeken, tellen en een gemiddelde | Het voorraadoverzicht |
| 02_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Bestellen met controles vóór je iets aanpast, `do-while`, `break` en `continue` | Een bestelling afronden |
| 02_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een compatibiliteitstabel als tweedimensionale array, geneste lussen, alle combinaties doorlopen | Alle geldige builds onder een budget |
| 02_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 02_01 | Het asieloverzicht |
| 02_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 02_02, met verhuren en terugbrengen | Een fiets terugbrengen |
| 02_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 02_03, met het zaalplan als tweedimensionale array | Vier plaatsen naast elkaar |

### Hoofdstuk 03 — strings en methodes

Leerboek 1.8 – 1.9

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 03_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | 02_01 opsplitsen in methodes, zoeken in tekst zonder hoofdletters, een nette tabel met `printf` | De artikelfiche als tekst |
| 03_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Een bestelregel ontleden met `split` en `parseInt`, een bon opbouwen met `StringBuilder`, methodes met een returnwaarde | De bestelbon uit één regel invoer |
| 03_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Onderdeelcodes ontleden en controleren, overloading, builds samenstellen met recursie | Het buildrapport |
| 03_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 03_01, met zoeken op een stukje van de naam | De dierfiche als tekst |
| 03_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 03_02, met tijden als tekst, zoals `09:45` | De huurbon uit één regel invoer |
| 03_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 03_03, met plaatscodes zoals `C12` | Het zaalrapport |

## Deel 2 — Objectgeoriënteerd programmeren

### Hoofdstuk 04 — klassen en objecten

Leerboek 2.1 – 2.4

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 04_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | 03_01 omzetten naar een klasse `Artikel`: private velden, constructors, getters en setters die controleren | De voorraad als objecten |
| 04_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | `Klant`, `Bestelling` en `Bestellijn` die naar elkaar verwijzen, constructor chaining met `this(...)`, access modifiers | Een bestelling met lijnen |
| 04_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Zelf de klassen voor een build ontwerpen: elk onderdeel een eigen klasse, de controles in methodes | Een build als object |
| 04_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 04_01 | Het asiel als objecten |
| 04_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 04_02, met klant, fiets en verhuring | Een verhuring met klant en fiets |
| 04_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 04_03, met zaal, concert en ticket | Een concert als object |

### Hoofdstuk 05 — static, final, overerving en polymorfisme

Leerboek 2.5 – 2.8

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 05_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Een automatische Id met een `static` teller, een `final` btw-tarief, subklassen van `Artikel` met `super` | Eén lijst, verschillende artikelen |
| 05_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Particuliere en zakelijke klanten, de prijsberekening overriden, de bestellijnen afschermen | Twee klanten, één bestelling |
| 05_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een klassenhiërarchie van onderdelen, vermogen en prijs polymorf berekenen, `equals` en `toString` | Het vermogen polymorf berekend |
| 05_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 05_01, met honden, katten en konijnen | Eén asiel, verschillende dieren |
| 05_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 05_02, met een tarief per type fiets | Drie fietsen, één verhuurregel |
| 05_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 05_03, met staanplaatsen, zitplaatsen en VIP | De opbrengst van een avond |

### Hoofdstuk 06 — abstractie, interfaces, enums, packages en SOLID

Leerboek 2.9 – 2.14

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 06_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Een abstracte `Artikel`, een enum `Soort` met eigen velden, alles in packages | Een catalogus in packages |
| 06_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Kortingsregels achter een interface, een enum `BestelStatus` met toegelaten overgangen, een nested class | De bestelstatus volgen |
| 06_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Elke compatibiliteitsregel een eigen klasse achter één interface, open/closed en dependency inversion | Een nieuwe regel zonder bestaande code te wijzigen |
| 06_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 06_01 | Een asiel in packages |
| 06_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 06_02, met een enum `FietsStatus` | De status van een fiets volgen |
| 06_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 06_03, met tariefregels | Een nieuwe tariefregel zonder bestaande code te wijzigen |

## Deel 3 — Gevorderd Java

### Hoofdstuk 07 — collections, generics en sorteren

Leerboek 3.1 – 3.5

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 07_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Arrays vervangen door `ArrayList`, een `Map` per soort, een `Set` van merken, sorteren met `Comparator` | Het voorraadrapport per soort |
| 07_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Een generieke `Repository<T>`, bestellingen in een `Map` op nummer, `Comparable` op datum | Openstaande bestellingen per klant |
| 07_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Onderdelen per socket in een `TreeMap`, dubbels weren met `equals` en `hashCode`, builds rangschikken | De top drie van de builds |
| 07_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 07_01 | Het asielrapport per soort |
| 07_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 07_02 | Openstaande verhuringen per klant |
| 07_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 07_03, met de vrije plaatsen per rij in een `TreeMap` | De beste vrije plaatsen |

### Hoofdstuk 08 — exceptions, bestanden en reguliere expressies

Leerboek 3.6 – 3.9

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 08_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Een eigen exception, de voorraad bewaren in en lezen uit een CSV-bestand, try-with-resources | Een herstart overleven |
| 08_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Checked en unchecked exceptions, een bestelling die bij een fout niets achterlaat, invoer controleren met regex | Een mislukte bestelling laat geen spoor |
| 08_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een buildbestand regel per regel inlezen met regex, een foutrapport per regel, serialisatie | Een buildbestand inlezen en controleren |
| 08_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 08_01 | Een herstart overleven |
| 08_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 08_02 | Een mislukte verhuring laat geen spoor |
| 08_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 08_03, met een bestand vol reservaties | Een reservatiebestand inlezen en controleren |

### Hoofdstuk 09 — threads, lambdas en streams

Leerboek 3.10 – 3.13

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 09_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Lussen vervangen door streams: `filter`, `map`, `sorted`, `groupingBy`; `Optional` bij zoeken | Het dashboard in één pijplijn |
| 09_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Meerdere kassa's tegelijk met een `ExecutorService`, een race condition zien en oplossen, concurrent collections | Tien kassa's, één voorraad |
| 09_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | De configurator opgesplitst in modules, de beste build zoeken met streams, parallel rekenen en meten | De snelste zoektocht naar de beste build |
| 09_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 09_01 | Het asieldashboard in één pijplijn |
| 09_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 09_02, met tien balies tegelijk | Tien balies, één vloot |
| 09_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 09_03, met de beste plaatsen voor een groep | De beste plaatsen voor een groep |

## Deel 4 — Spring Boot en REST-API's

### Hoofdstuk 10 — je eerste REST-API

Leerboek 4.1 – 4.5

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 10_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | De catalogus wordt een Spring Boot-API: GET-endpoints, `ResponseEntity`, 200 tegenover 404, controller, service en repository | De artikelfiche |
| 10_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Volledige CRUD op klanten en bestellingen, 201 en 204, invoer weigeren met 400, de regels in de service | Een bestelling plaatsen |
| 10_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | De compatibiliteitsregels uit 06_03 achter een POST-endpoint, een antwoordmodel dat niet uit de echte wereld komt | Het buildrapport als endpoint |
| 10_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 10_01 | De dierfiche |
| 10_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 10_02, met verhuren en terugbrengen als endpoints | Een fiets verhuren |
| 10_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 10_03, met de plaatsregels achter een POST-endpoint | Tickets voor een groep |

### Hoofdstuk 11 — JPA, PostgreSQL en Flyway

Leerboek 4.6 – 4.7

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 11_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | De repository op JPA, afgeleide querymethodes, PostgreSQL in Docker, startgegevens met Flyway | Een kolom erbij |
| 11_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Relaties tussen klant, bestelling en bestellijn, `@Transactional`, twee migraties na elkaar | Een bestelling met lijnen in de database |
| 11_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Onderdelen met overerving in de database, een eigen `@Query`, een migratie die bestaande gegevens omzet | Builds bewaren en opnieuw controleren |
| 11_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 11_01 | Een kolom erbij |
| 11_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 11_02 | Een verhuring met klant en fiets in de database |
| 11_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 11_03 | Concerten en tickets bewaren |

### Hoofdstuk 12 — validatie, profiles, Actuator en OpenAPI

Leerboek 4.8 – 4.11

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 12_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Bean Validation, één foutformaat met `@RestControllerAdvice`, profiles voor H2 en PostgreSQL | Een fout die overal hetzelfde antwoordt |
| 12_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Een eigen validatieregel, bedrijfsregels met 409, instellingen per profiel, Actuator health | Regels per omgeving |
| 12_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | OpenAPI-documentatie met voorbeelden, eigen Actuator-info, één fout per regel in het rapport | Een API die zichzelf uitlegt |
| 12_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 12_01 | Een fout die overal hetzelfde antwoordt |
| 12_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 12_02 | Tarieven per omgeving |
| 12_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 12_03 | Een ticket-API die zichzelf uitlegt |

### Hoofdstuk 13 — testen en packagen

Leerboek 4.12 – 4.13

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 13_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Unittests van de service met Mockito, controllertests met MockMvc, een uitvoerbare JAR | De artikel-API onder test |
| 13_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Een integratietest met Testcontainers over een hele bestelling, de JAR starten met een profiel | Bestellen, van test tot JAR |
| 13_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Geparametriseerde tests voor elke compatibiliteitsregel, randgevallen zoeken en testen | Elke regel bewezen |
| 13_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 13_01 | De dier-API onder test |
| 13_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 13_02 | Verhuren, van test tot JAR |
| 13_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 13_03 | Elke plaatsregel bewezen |

## Deel 5 — Docker

### Hoofdstuk 14 — een image bouwen en draaien

Leerboek 5.1 – 5.5

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 14_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Een Dockerfile voor de API, poorten koppelen, logs en `exec`, een profiel via een omgevingsvariabele | De API in een container |
| 14_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Tags en versies, pushen naar Docker Hub, twee versies naast elkaar draaien | Versie 2 naast versie 1 |
| 14_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een multi-stage build, een kleiner image, het geheugen van de JVM begrenzen | Een image klaar voor productie |
| 14_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 14_01 | De dier-API in een container |
| 14_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 14_02 | Versie 2 naast versie 1 |
| 14_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 14_03 | Een image klaar voor productie |

### Hoofdstuk 15 — netwerken en Docker Compose

Leerboek 5.6 – 5.8

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 15_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Een eigen netwerk voor API en PostgreSQL, daarna alles in één `compose.yaml` | Alles met één commando |
| 15_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Volumes, pgAdmin, wachten tot de database klaar is, een profiel `docker` | Een herstart van de hele stack overleven |
| 15_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Configuratie buiten het image: een `.env`-bestand, een profiel per omgeving, hetzelfde image twee keer gestart | Eén image, twee omgevingen |
| 15_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 15_01 | Alles met één commando |
| 15_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 15_02 | Een herstart van de hele stack overleven |
| 15_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 15_03 | Eén image, twee omgevingen |

## Deel 6 — Microservices

### Hoofdstuk 16 — een applicatie opsplitsen

Leerboek 6.1 – 6.3

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 16_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | De artikel-API opsplitsen in een catalogus- en een voorraadservice, `RestTemplate`, een DTO | De artikelfiche over twee services |
| 16_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Een bestelservice die klant en artikel bij andere services opvraagt, fouten van een andere service doorgeven | Een bestelling over drie services |
| 16_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | De configurator haalt zijn onderdelen bij een andere service, en weet wat te doen als die niet antwoordt | Het buildrapport uit twee services |
| 16_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 16_01, met een dier- en een adoptieservice | De dierfiche over twee services |
| 16_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 16_02, met een fiets-, klant- en verhuurservice | Een verhuring over drie services |
| 16_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 16_03, met een concert- en een ticketservice | Het zaalplan uit twee services |

### Hoofdstuk 17 — Eureka, OpenFeign, Zipkin en Config Server

Leerboek 6.4 – 6.7

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 17_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Een service registry met Eureka, `RestTemplate` vervangen door OpenFeign | Services die elkaar vinden |
| 17_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Tracing met Zipkin en Micrometer over een hele bestelling, time-outs voor Feign | Eén verzoek gevolgd door alle services |
| 17_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | De compatibiliteitsregels in een Config Server, per omgeving andere grenzen | De regels centraal beheerd |
| 17_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 17_01 | Services die elkaar vinden |
| 17_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 17_02 | Eén verhuring gevolgd door alle services |
| 17_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 17_03, met de tarieven in de Config Server | De tarieven centraal beheerd |

### Hoofdstuk 18 — API Gateway, Resilience4J en RabbitMQ

Leerboek 6.8 – 6.10

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 18_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Eén API Gateway voor alle services, routes en paden herschrijven | Eén ingang |
| 18_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Circuit breaker, retry en fallback op de oproep naar de artikelservice | Bestellen terwijl een service plat ligt |
| 18_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een buildcontrole als bericht over RabbitMQ, het resultaat later ophalen | Asynchroon controleren |
| 18_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 18_01 | Eén ingang |
| 18_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 18_02 | Verhuren terwijl een service plat ligt |
| 18_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 18_03, met de toewijzing van plaatsen als bericht | Asynchroon plaatsen toewijzen |

## Deel 7 — Apache Kafka

### Hoofdstuk 19 — Kafka op de commandoregel

Leerboek 7.1 – 7.7

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 19_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Kafka lokaal starten, een topic met drie partities, berichten met een key versturen en lezen | Elk artikel in zijn eigen volgorde |
| 19_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Consumer groups, offsets en lag, een groep opnieuw laten lezen, retentie instellen | Een nieuwe afnemer die alles opnieuw leest |
| 19_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een cluster van drie brokers, replicatie en ISR, een broker laten uitvallen | Een broker valt uit |
| 19_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 19_01 | Elk dier in zijn eigen volgorde |
| 19_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 19_02 | Een nieuwe afnemer die alles opnieuw leest |
| 19_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 19_03 | Een broker valt uit |

### Hoofdstuk 20 — Spring Kafka

Leerboek 7.8 – 7.12

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 20_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Een producer met `KafkaTemplate` in de catalogus, een consumer met `@KafkaListener` | Elke prijswijziging als event |
| 20_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | De bestelservice publiceert, de artikelservice verwerkt; `acks`, idempotentie, offsets zelf vastleggen | De voorraad bijgewerkt via events |
| 20_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Concurrent consumers, de juiste key kiezen voor de volgorde, wat er bij een rebalance gebeurt | Drie consumers, één volgorde per build |
| 20_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 20_01 | Elke statuswijziging als event |
| 20_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 20_02 | De vloot bijgewerkt via events |
| 20_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 20_03 | Drie consumers, één volgorde per concert |

### Hoofdstuk 21 — betrouwbare events

Leerboek 7.13 – 7.20

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 21_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Events opslaan in de database, testen met Embedded Kafka, Actuator health voor Kafka | Een eventlog met tests |
| 21_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Retry en een dead letter topic in de consumer, fouten in de producer opvangen | Een vergiftigd bericht |
| 21_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Transacties en exactly-once, de timing van de consumer, verbinding over SSL | Alles of niets, ook over Kafka |
| 21_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 21_01 | Een eventlog met tests |
| 21_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 21_02 | Een vergiftigd bericht |
| 21_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 21_03 | Alles of niets, ook over Kafka |

## Deel 8 — Kubernetes

### Hoofdstuk 22 — Pods, Deployments en Services

Leerboek 8.1 – 8.6

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 22_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Minikube en kubectl, de API als Deployment met een Service, port-forward | De API in de cluster |
| 22_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Meerdere services in de cluster, een Ingress, schalen naar drie replicas | Drie replicas achter één adres |
| 22_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een rolling update, de uitrol volgen en terugdraaien, wat er gebeurt met een kapot image | Een nieuwe versie zonder onderbreking |
| 22_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 22_01 | De dier-API in de cluster |
| 22_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 22_02 | Drie replicas achter één adres |
| 22_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 22_03 | Een nieuwe versie zonder onderbreking |

### Hoofdstuk 23 — configuratie, health checks en infrastructuur

Leerboek 8.7 – 8.10

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 23_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Configuratie in een ConfigMap, het wachtwoord in een Secret, liveness- en readiness-checks met Actuator | Configuratie buiten het image |
| 23_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | PostgreSQL en RabbitMQ in de cluster met blijvende opslag, alle services deployen | De volledige bestelstraat in Kubernetes |
| 23_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Zipkin in de cluster, een readiness-check die afhangt van een andere service, fouten opsporen met logs en events | Een pod die niet klaar is, krijgt geen verkeer |
| 23_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 23_01 | Configuratie buiten het image |
| 23_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 23_02 | De volledige verhuurstraat in Kubernetes |
| 23_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 23_03 | Een pod die niet klaar is, krijgt geen verkeer |

## Deel 9 — Ontwikkelen met AI

### Hoofdstuk 24 — ontwikkelen met een coding agent

Leerboek 9.1 – 9.5

| Oefening | Niveau | Domein | Wat je oefent | Samengestelde oefening |
|----------|--------|--------|----------------|------------------------|
| 24_01 | <span style="color:#37b24d">●○○</span> | PC-onderdelen | Copilot instellen, een `AGENTS.md` voor je project, een kleine feature laten bouwen en elke wijziging nakijken | Een verlanglijst voor klanten |
| 24_02 | <span style="color:#f08c00">●●○</span> | PC-onderdelen | Een PRD en een implementatieplan schrijven, de agent stap voor stap laten werken, Git als vangnet | Retourzendingen, van PRD tot werkende code |
| 24_03 | <span style="color:#f03e3e">●●●</span> | PC-onderdelen | Een skill die je conventies vastlegt, een nieuwe compatibiliteitsregel laten bouwen en testen | Een skill voor een nieuwe regel |
| 24_04 | <span style="color:#37b24d">●○○</span> | Dierenasiel | Zelfde niveau als 24_01 | Een wachtlijst voor adopties |
| 24_05 | <span style="color:#f08c00">●●○</span> | Fietsverhuur | Zelfde niveau als 24_02 | Schademeldingen, van PRD tot werkende code |
| 24_06 | <span style="color:#f03e3e">●●●</span> | Concertzaal | Zelfde niveau als 24_03, met een nieuwe tariefregel | Een skill voor een nieuwe tariefregel |

---

## Over de samengestelde oefeningen

In de gewone punten staat elke opdracht op zichzelf: één berekening, één endpoint, één commando. Handig om één techniek te oefenen, maar het lijkt niet op een echt project.

De samengestelde oefening zet daar één onderwerp tegenover. Dat doet drie dingen met je code:

- **Je logica moet herbruikbaar worden.** Merk je dat je dezelfde berekening of hetzelfde bericht een tweede keer uitschrijft, dan is je code nog niet opgesplitst zoals het hoort.
- **Fouten die je apart niet ziet, komen naar boven.** Een bestelling die halverwege mislukt, mag niets veranderd hebben. Een plaats die vrijkomt, moet echt vrij zijn. Zulke fouten blijven onzichtbaar zolang je elk punt apart test.
- **Je ziet waarom de volgende stap bestaat.** De samengestelde oefening van een hoofdstuk loopt vaak net tegen de grens van wat je al kent. In het volgende hoofdstuk leer je wat die grens wegneemt.

---

## Volgorde

Werk per hoofdstuk van laag naar hoog: eerst NN_01, dan NN_02, dan NN_03. Doe de samengestelde oefening pas als de punten ervoor werken: ze bouwt er letterlijk op verder.

De spiegelreeks, NN_04 tot en met NN_06, kan je nadien gebruiken als herhaling, of meteen als je merkt dat een concept nog niet zit.

Wil je sneller door de leerstof, doe dan enkel NN_01 tot en met NN_03: samen raken ze alle leerstof van het hoofdstuk aan. Heb je nog minder tijd, volg dan de reeks _01. Die geeft je de basis van elk hoofdstuk, maar onderwerpen als modules, een Kafka-cluster of transacties komen pas in _02 en _03 aan bod.

> [!IMPORTANT]
> Begin niet aan het volgende hoofdstuk voor NN_03 volledig lukt zonder terug te bladeren.

> [!NOTE]
> Elke reeks bouwt op zichzelf verder. Sla je 07_02 over, dan heb je geen vertrekpunt voor 08_02. Elke opgave begint daarom met een samenvatting van wat je vertrekpunt moet kunnen; heb je de vorige oefening niet meer, maak die dan eerst opnieuw vanuit haar eigen opgave.

---

## Buiten bereik

Elke oefening blijft binnen wat het leerboek tot en met dat hoofdstuk behandelt. Een oefening van hoofdstuk 02 heeft geen klassen nodig, een oefening van hoofdstuk 10 geen database. De naslag in deel 10 mag je altijd gebruiken.

> [!WARNING]
> Heb je het gevoel dat je iets van later nodig hebt, dan los je de oefening waarschijnlijk te ingewikkeld op.

Het overzicht in de map van elk hoofdstuk zegt precies wat buiten bereik valt, en welke elementen de oefeningen gebruiken die het leerboek niet voordoet.
