# Java-Leerlijn

Leerboek ter voorbereiding op een job als Java-developer: van de JVM tot microservices met Spring Boot, Docker, Apache Kafka en Kubernetes. Het bundelt acht online cursussen tot één doorlopende cursus in het Nederlands, met de vaktermen in het Engels, oefeningen met uitgewerkte antwoorden en een naslag van de belangrijkste methodes per type.

## Bestanden

| Bestand | Inhoud |
|---|---|
| `Java - leerboek V3.docx` | de huidige versie: 10 delen, 112 hoofdstukken, 921 pagina's, 630 oefeningen met antwoord |
| `Java - leerboek V2.docx` | deel 1 tot 9, 102 hoofdstukken (vervangen door V3) |
| `Java - leerboek V1.docx` | deel 1 tot 4, 49 hoofdstukken (vervangen door V2) |
| `Java Leerboek - Exercises - Deel 1.docx` | de oefeningen bij deel 1 (Java-basis): hoofdstuk 01 tot en met 03, met uitgewerkte oplossingen |
| `Oefeningen/` | dezelfde oefeningen en oplossingen als Markdown, met onderaan elke oplossing het volledige programma |

## Inhoud van het leerboek

| Deel | Onderwerp |
|---|---|
| 1 | Java-basis: JVM, datatypes, operatoren, lussen, arrays, strings, methodes |
| 2 | Objectgeoriënteerd programmeren: klassen, overerving, interfaces, SOLID |
| 3 | Gevorderd Java: collections, exceptions, I/O, regex, threads, modules, streams |
| 4 | Spring Boot en REST-API's: controllers, JPA, Flyway, validatie, testen |
| 5 | Docker: images, containers, Dockerfile, netwerken, Compose |
| 6 | Microservices: Eureka, OpenFeign, Gateway, Resilience4J, RabbitMQ |
| 7 | Apache Kafka: topics, partities, producers, consumers, foutafhandeling, SSL |
| 8 | Kubernetes: pods, Deployments, Services, ConfigMaps, probes, StatefulSets |
| 9 | Ontwikkelen met AI: agentic engineering, PRD, `AGENTS.md`, skills |
| 10 | Naslag: methodes van `String`, getallen, datums, arrays, `List`, `Map`, `Set`, wachtrijen, streams en `Optional` |

Elk hoofdstuk heeft een Bronnen-regel die verwijst naar de video's van de cursussen, uitleg met codevoorbeelden, valkuilen en oefeningen. De pure Java-code (deel 1 tot 3 en deel 10) is uitgevoerd met Java 21.

## Oefeningen

De oefeningen staan los van het leerboek, in **Java Leerboek – Exercises**. Ze volgen de opbouw van de oefeningen van Programming Advanced (C#): een leerdoel, een opdracht in genummerde punten met één techniek per punt, en als laatste punt een samengestelde oefening. De oplossingen staan apart, met nakijkpunten, een aanpak, de code, veelgemaakte fouten en wat beter kan.

| | |
|---|---|
| Hoofdstukken | 24, elk bij een of meer hoofdstukken van het leerboek |
| Oefeningen | 6 per hoofdstuk, in zes reeksen die meegroeien van een console-app tot Kubernetes |
| Reeksen | _01 Bit & Byte, artikelen ●○○ · _02 Bit & Byte, bestellingen ●●○ · _03 Bit & Byte, PC-configurator ●●● · _04 Dierenasiel ●○○ · _05 Fietsverhuur ●●○ · _06 Concertzaal ●●● |
| Word | één bestand per deel van het leerboek: `Java Leerboek - Exercises - Deel <n>.docx` |
| Markdown | `Oefeningen/Oefeningen` (opgaven), `Oefeningen/Oplossingen Oefeningen` (oplossingen), `Oefeningen/Ontwerp` (verhaallijn en vaste regels van de reeksen) |

Klaar: hoofdstuk 01 tot en met 03 van 24; deel 1 is volledig. Elk programma in de pure Java-hoofdstukken is per punt gecompileerd en uitgevoerd met Java 21; de voorbeelduitvoer en de nakijkpunten komen uit die runs.

## Wat niet in deze repository staat

De cursussen zelf (video's, ondertitels, zips en democode) blijven lokaal, in de map `Cursussen`. Ze zijn gekocht materiaal en mogen niet openbaar gedeeld worden, en ze zijn samen ongeveer 36 GB groot. Het bestand `.gitignore` zorgt ervoor dat git ze nooit meeneemt.
