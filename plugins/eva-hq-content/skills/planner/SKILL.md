---
name: planner
description: De maandplanner voor social content. Maakt aan het begin van de maand het skelet als lege items in de Content-database in Notion, vult per week de slots met een voorselectie, bouwt een lanceerbriefing als een lancering in beeld komt, en doet aan het eind van de maand de review. Plant alleen, schrijft geen posts; dat doen @content en @repurpose via hun route "uit het maandplan". TRIGGERS op @planner, "@planner maand", "@planner week", "@planner review", "maak mijn maandplan", "plan deze maand", "wat post ik deze week", "vul de slots van deze week", "welke posts staan er deze week", "maandreview", "hoe ging de contentmaand", "plan mijn lancering in", "lanceerplanning", of elke vraag om content voor een maand of week in te plannen, een lancering in de contentplanning te zetten, of terug te kijken op de contentmaand. Vereist planner.md uit de planner-intake (les 2) en de save-skill (voor de link van de systeempagina waar de Content-database op staat).
---

# planner

De planner plant, de makers maken. Deze skill onderhoudt het maandplan in de Content-database in Notion: één regel per slot. Hij bepaalt wanneer er iets komt, op welk platform, met welke pillar en uit welke bron, en legt per week de opties voor. Het schrijven van de post gebeurt daarna in `@content` (hergebruik, inspiratie, eigen) of `@repurpose` (oogst), die allebei een route "uit het maandplan" hebben en aan het eind status en bestandsnaam terugschrijven naar het slot.

Waarom zo gescheiden: het maandplan is het enige deel van het systeem waar de klant en een VA zelf naar kijken, op de telefoon of in de kalender. Dat moet één plek zijn (Notion) en één geheel blijven (het skelet). De uitwerking per post leeft lokaal in `Output/Content/` als before/after-bestand, precies zoals de makers dat nu al doen.

## Drie aanroepen

| Aanroep | Wanneer | Wat |
|---|---|---|
| `@planner maand` | Begin van de maand | Lanceerbriefing als er een lancering in beeld komt, dan het skelet voor de hele maand naar Notion |
| `@planner week` | Elke week | De Inbox-slots van deze week vullen met een voorselectie; de klant kiest, het slot gaat op Scheduled |
| `@planner review` | Laatste week van de maand | Gepland versus geplaatst, overgeslagen slots, voorstel voor bronverdeling en etalage van de volgende maand |

Zegt de klant alleen `@planner`, kijk dan zelf wat aan de beurt is en zeg dat in één zin: geen skelet voor deze maand, dan maand. Wel een skelet en de huidige week heeft nog Inbox-slots, dan week. Laatste zeven dagen van de maand en de review is nog niet gedaan (geen regel met deze maand in planner.md onder `laatst_herzien`), stel dan de review voor.

## Wat je leest, en waar

Lees alleen wat de aanroep nodig heeft. Wijzig deze bronnen niet, behalve planner.md sectie 2, 6 en 7 waar deze skill dat expliciet zegt.

1. `Resources/Fundament/planner.md`: ritme en platforms (sectie 1), bronverdeling (2), hergebruik (3), Bronarchief-reeksen (4), werkwijze met opties per slot en plaatsen per platform (5), etalage (6), koppelingen en paden (7). Ontbreekt het bestand, stop en verwijs naar de planner-intake van les 2; zonder profiel valt er niets te plannen.
2. `Resources/Fundament/strategie.md`: basismix over de vijf intent-pillars (sectie 3) en de topic-pillars (sectie 1).
3. `Resources/Input/09 - Bestaande voorraad.md`: de indextabel bovenin, voor hergebruik-kandidaten en voor de Launch-posts van eerdere lanceringen.
4. De Content-database in Notion. De `save`-skill is de ene persoonlijke bron voor de Notion-databases: laad die skill (zoals `@ideate` doet), lees daar de regel `Systeempagina:` en fetch die pagina. Dat geeft alle databases op die pagina met data source ID; pak de database die volgens de matchregel van de save-skill "Content" heet (naam uit de uitlegregel onder het blok, anders uit de titel van de data source). Zo hoeft een klant maar op één plek iets aan te passen. Na de eerste run staat de URL ook in planner.md sectie 7; lees hem daarna van daar. De Inspiratiedatabase (voor inspiratie-slots) haal je van dezelfde pagina, op de naam "Inspiratie" of "Inspiration".
5. Bij een lancering: de Offer Snapshot van het aanbod in About Me, als die er is, en `Resources/Fundament/Lanceringen/` voor eerdere briefings.
6. Referentiebestanden van deze skill, per aanroep: `references/skelet.md` (maand), `references/lancering.md` (maand, alleen bij een lancering), `references/review.md` (review). Lees ze op het moment dat je die stap doet, niet vooraf.

## Eerste run

Doe dit één keer, en alleen wat nog ontbreekt. Controleer eerst, want de klant kan een stap al hebben gedaan.

1. Bestaat `planner.md`? Nee: stop, verwijs naar les 2.
2. Staat het Content-database-ID al in planner.md sectie 7 (regel "Maandplan:")? Nee: bepaal het via de systeempagina uit de save-skill (zie "Wat je leest") en schrijf in sectie 7 de regel `Maandplan: [URL van de Content-database]` in plaats van `Maandplannen: Output/Planning/`. Zeg dat je dat gedaan hebt.
3. Heeft de Content-database de eigenschappen Pillar, Bron, Etalage en Bestand? Voeg toe wat ontbreekt met één schema-update op de data source (ADD COLUMN-statements):
   - `Pillar` select: Authority, Vision, Connection, Embodiment, Launch
   - `Bron` select: Oogst, Hergebruik, Inspiratie, Eigen
   - `Etalage` text (de naam van wat er die maand in de etalage staat, of leeg)
   - `Bestand` text (de bestandsnaam in `Output/Content/`, ingevuld door de maker)
   Bestaande eigenschappen en items blijven onaangeroerd: deze database is ook de plek waar `@save` concepten neerzet.
4. Is er een kalenderweergave op de datum Publicatie? Nee: maak de weergave "Planning kalender" aan (calendar by Publicatie, toon Naam, Platform, Status). Zeg daarna één keer in de chat dat de klant de kalender in Notion zelf op "week" kan zetten voor de weekweergave; dat kan de koppeling niet instellen.

## Statusmapping

De Content-database heeft al een status-veld. Het maandplan gebruikt die statussen zo, en niet anders, zodat `@save`, `@content` en `@repurpose` er zonder vertaling mee kunnen werken:

| Status | Betekent in het maandplan |
|---|---|
| Inbox | Leeg slot uit het skelet, nog geen keuze |
| Scheduled | Optie gekozen, nog niet geschreven |
| Drafting | Concept staat in `Output/Content/` (door de maker gezet) |
| Review | Definitieve tekst staat er, Caption gevuld (door de maker gezet) |
| Published | Geplaatst |
| Discarded | Overgeslagen; blijft staan en telt mee in de review |

De optienamen in de database dragen meestal een emoji (`📥 Inbox`, `📅 Scheduled`, `📸 Instagram`). Lees de exacte namen uit het schema (fetch op de data source) en gebruik ze letterlijk bij aanmaken, bijwerken en filteren; een naam zonder emoji matcht niet. Slots van een week haal je op met een SQL-query op de data source: `Status = '📥 Inbox'`, `Pillar IS NOT NULL` (zo blijven concepten van `@save` buiten beeld) en `date("date:Publicatie:start") BETWEEN date(maandag) AND date(zondag)`.

Een slot dat overgeslagen wordt schuift niet op naar de week erna. Het skelet blijft staan zoals het is; wat niet gehaald is, is informatie voor de review, geen achterstand die meegesleept wordt.

## @planner maand

Lees `references/skelet.md` voor de afleiding. In het kort:

1. **Maand bepalen.** Standaard de komende maand als het de laatste week is, anders de huidige. Bestaat er al een skelet voor die maand (Inbox- of latere items met een Publicatie-datum in die maand en een gevulde Pillar), meld dat en vraag of je aanvult of stopt. Nooit stilzwijgend een tweede skelet naast het eerste zetten.
2. **Etalage lezen.** Sectie 6 van planner.md voor deze én de volgende maand. Staat er een lancering waarvan het opwarmen in deze maand begint (dat kan tot twee weken vóór de opening zijn, dus ook een lancering van volgende maand), doe dan eerst de lanceerbriefing uit `references/lancering.md`. Zonder lancering: sla dit over.
3. **Skelet afleiden.** Per week de slots uit het ritme (sectie 1), per slot platform, pillar volgens de basismix, bron volgens de bronverdeling, etalage-label als er iets in de etalage staat. In een lanceerperiode volgens `lancering.md`.
4. **Voorleggen.** Toon het skelet als overzicht per week (datum, platform, pillar, bron, etalage of lanceerfase) en wacht op akkoord. Dit is het moment om te schuiven; daarna staat het.
5. **Naar Notion.** Per slot één item: Naam `JJJJ-MM-DD Platform Pillar` (bijvoorbeeld `2026-11-04 Instagram Vision`), Status Inbox, Platform, Type Marketing, Publicatie de datum, Pillar, Bron, Etalage, en bij een Launch-slot de fase in Notities. Maak de items in één batch aan.
6. **Afsluiten.** Zeg hoeveel slots er staan, hoeveel daarvan Launch, en dat `@planner week` de eerste week vult.

## @planner week

1. **Slots ophalen.** Query de Content-database op Status Inbox en Publicatie binnen deze week (maandag tot en met zondag; vraag welke week als de klant een andere bedoelt). Geen Inbox-slots: zeg dat de week al gevuld is of dat er geen skelet is, en stel de passende aanroep voor.
2. **Per slot een voorselectie.** Werk slot voor slot, één slot per bericht, tenzij de klant zegt dat ze alles in één keer wil. Het aantal opties staat in planner.md sectie 5 (opties per slot), altijd met één aanrader en één regel waarom. De bron bepaalt waar de opties vandaan komen:
   - **Oogst**: fragmenten uit de Bronarchief-reeksen in sectie 4. Kijk in de REEKS.md of de indextabel van die reeks naar onderwerpen die bij de pillar passen; noem reeks en aflevering. Alleen geanonimiseerd materiaal uit gesprekken met klanten of coachees.
   - **Hergebruik**: posts uit de indextabel van het voorraadbestand, ouder dan de hergebruiktermijn uit sectie 3, niet op de nooit-lijst, passend bij de pillar. Noem datum en eerste regel.
   - **Inspiratie**: Inbox-items uit de Inspiratiedatabase die bij de pillar en het platform passen. Noem titel en de kern uit de Notities.
   - **Eigen**: geen opties uit een bron; vraag waar de klant deze post over wil hebben en stel op basis van de topic-pillars twee invalshoeken voor.
   - **Launch**: de opties volgen uit de fase in Notities en de briefing; zie `references/lancering.md` onder "Opties per fase".
   Staat "Claude kiest bij twijfel: ja" in sectie 5, kies dan de aanrader zodra de klant "kies jij maar" zegt.
3. **Vastleggen.** Na de keuze: Naam wordt de gekozen optie in een paar woorden (de datum blijft in Publicatie), Status Scheduled, de keuze en de herkomst in Notities (reeks en aflevering, voorraaddatum, of de kern van het idee), bij een inspiratie-item de relatie Inspired by. Pillar, Bron en Etalage blijven zoals het skelet ze zette; een bewuste afwijking noteer je in Notities.
4. **Doorgeven.** Zeg per gevuld slot welke skill het maakt: oogst naar `@repurpose`, de rest naar `@content`, allebei met "uit het maandplan" en de naam van het slot. De planner schrijft zelf geen post, ook niet als de klant erom vraagt; verwijs dan naar de maker, want daar zit de before/after-stap die de eigen stem bewaakt.

## @planner review

Lees `references/review.md`. In het kort: haal alle slots van de maand op, zet gepland tegenover geplaatst per pillar en per bron, tel de Discarded-slots en noem ze, en bekijk de lanceerperiode apart als die er was. Doe daaruit één voorstel voor de bronverdeling (sectie 2) en de etalage (sectie 6) van de volgende maand. Pas na akkoord schrijf je die twee secties bij en zet je `laatst_herzien` op vandaag. Ga daarna door met `@planner maand` voor de volgende maand, tenzij de klant dat later wil doen.

## Regels

- Alleen feedposts. Stories, mails en ads vallen buiten deze skill; het aftellen in stories tijdens een lancering loopt naast de feedslots, dat zeg je één keer bij de lanceerbriefing.
- Het maandplan leeft alleen in Notion. Geen lokale spiegel, geen maandplan-bestand in de map; wat in Notion staat is de waarheid.
- Getallen en voorkeuren komen uit planner.md en strategie.md. Verzin geen ritme, geen verdeling en geen termijn; ontbreekt iets, zeg dat en vraag het, of stuur terug naar les 2.
- Vraag nooit iets dat je zelf kunt nakijken: of het skelet er al staat, of een eigenschap bestaat, of de vorige lancering een briefing heeft.
- Wat je automatisch doet of juist niet kunt (de kalender wel, de weekweergave niet; de bestandsnaam komt van de maker, niet van jou), zeg je één keer in de chat op het moment dat het speelt, in plaats van het stil te noteren.
- Voorstel eerst, dan pas schrijven. Het skelet, de lanceerbriefing en de review-aanpassingen gaan pas naar Notion of planner.md na een akkoord in de chat.

## Foutafhandeling

- `planner.md` ontbreekt: stop, verwijs naar de planner-intake (les 2). Zonder profiel kun je geen ritme of verdeling afleiden en ga je die niet gokken.
- Save-skill niet geïnstalleerd, geen link op de regel `Systeempagina:`, of geen database "Content" op die pagina: waarschijnlijk is Module 1 nog niet ingericht. Zeg wat er ontbreekt, verwijs naar les 5 en 7 van Module 1 (Business HQ-pagina en save-skill) en stop; zonder database is er geen plek voor het maandplan.
- Schema-update of aanmaken van de weergave faalt: meld het en geef de vier eigenschappen en de weergave-instelling zodat de klant ze zelf in Notion kan aanmaken; ga daarna gewoon door met het skelet.
- Voorraadbestand ontbreekt of is ouder dan twee maanden: plan de hergebruik-slots wel, maar zeg bij de weekvulling dat de opties uit een verouderde of ontbrekende voorraad komen en verwijs naar les 1.
- Geen Bronarchief (sectie 4 op nee) terwijl de bronverdeling oogst bevat: meld de tegenstrijdigheid en stel voor de oogst-slots deze maand als hergebruik of eigen te plannen, of het profiel bij te werken.
- Inspiratiedatabase leeg: zeg dat de inspiratie-slots deze week open staan en stel voor eerst `@ideate`, `@briefing` of `@inspire` te draaien, of het slot als eigen te vullen.
- Een lancering in de etalage zonder datum of aanbod: doe de briefing niet op aannames, vraag de vier feiten en plan pas daarna.
