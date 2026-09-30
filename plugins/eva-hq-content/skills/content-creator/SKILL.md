---
name: content-creator
description: De dagelijkse content-skill. Werkt langs drie routes: route A vanuit een opgeslagen idee uit de Inspiratiedatabase, route B vanuit een directe opdracht van de gebruiker ("schrijf nu een post over X") met eventueel eigen instructies, route C vanuit een slot in het maandplan van @planner (de Content-database in Notion). Schrijft een concept in de Brand Voice, laat de gebruiker het herschrijven in de eigen stem, en slaat het before/after-paar lokaal op. TRIGGERS op @content, "schrijf een post", "schrijf nu een post over", "maak content", "werk een idee uit", "ik wil iets uitwerken uit mijn inspiratie", "content maken", "uit het maandplan", "schrijf het slot van [datum]", "maak de post voor [dag]", of elke vraag om een post te schrijven, met of zonder opgeslagen idee of gepland slot. Werkt samen met de utm-skill: checkt de UTM Links-database in Notion voordat er een link naar een aanbod in een post of mail komt.
---

# content-creator

Creatie-skill. Werkt vanuit een opgeslagen idee (route A), een directe opdracht van de gebruiker (route B) of een gepland slot uit het maandplan (route C), stelt samen met de gebruiker een mini-brief op, schrijft een concept in de Brand Voice, laat de gebruiker dat herschrijven in de eigen stem, en bewaart concept en definitieve versie als één before/after-bestand in de projectmap. De ideeën leven in Notion, de uitwerkingen lokaal.

## Inputs

1. Idee, opdracht of slot. Route A: een idee uit de Notion Inspiratiedatabase, alleen items met `Status = 📥 Inbox`, onder andere geschreven door `ideate`. De `save`-skill is de ene persoonlijke bron voor de Notion-databases: laad die skill (zoals `@ideate` doet), lees daar de regel `Systeempagina:` en fetch die pagina. Dat geeft alle databases op die pagina met data source ID; pak de database die volgens de matchregel van de save-skill "Inspiratie" of "Inspiration" heet (naam uit de uitlegregel onder het blok, anders uit de titel van de data source). De statuswaarden staan in het schema van de gevonden database. Zo staat er niets workspace-specifieks in deze skill en past een klant alleen de save-skill aan. Route B: een directe opdracht van de gebruiker ("schrijf een post over X"), met eventueel eigen instructies (invalshoek, punten, format, platform). Geen database nodig. Route C: een slot uit het maandplan dat `@planner` heeft gevuld: een item in de Content-database in Notion (van dezelfde systeempagina, op de naam "Content") met `Status = 📅 Scheduled`, een Publicatie-datum, Platform, Pillar, Bron en in Notities de gekozen optie en de herkomst.
2. Context (lezen, niet wijzigen), om in de Brand Voice te schrijven:
   - de Brand Voice Guide uit About Me (tone, woordkeuze, ritme, do's en don'ts)
   - het fundament in de projectmap: `identiteit.md` (stem, villain, anti-visie, grondtoon) en `strategie.md` (pillars, mix, en de regel `Content-bestemmingen:` die bepaalt welke Bestemming-waarden route A toont)
   - de engine in de skillmap: `hooks.md` (5 mechanismen), `frameworks.md` (frameworks plus video-skeletten), `carousel.md` (rendering), `villain.md` (overlay), `platforms.md` (platform-laag) en `micro-specificity.md` (diepte-lat plus AI-vormen)
   - de eigen contentvoorbeelden per platform in `Resources/Input/06 - Contentvoorbeelden/[platform]/`
   - het topic-pillars document in `Resources/Input/`
3. Output-map: `Output/Content/` in de projectmap. Maak de map aan als die nog niet bestaat.

## Output

Per uitgewerkt idee één bestand: `Output/Content/JJJJ-MM-DD-[korte-slug].md`, met een kop-blok en twee tekstblokken (concept en definitief). Zie stap 6 voor het format.

## Procedure

1. **Route bepalen en input ophalen.** Drie ingangen:
   - **Route A, uit de Inspiratiedatabase.** De gebruiker wil een opgeslagen idee uitwerken. Lees eerst in `strategie.md` de regel `Content-bestemmingen:` (de waarden van het veld Bestemming die bij deze gebruiker content zijn, bijvoorbeeld alleen `📣 Marketing`; les- en webinar-ideeën staan in dezelfde database en horen hier niet). Query de Inspiratiedatabase op `Status = 📥 Inbox` en `Bestemming` in die waarden, gesorteerd op `createdTime` aflopend. Ontbreekt de regel in strategie.md, query dan alleen op Status, presenteer de items gegroepeerd per Bestemming en zeg één keer dat de gebruiker de regel `Content-bestemmingen:` aan strategie.md kan toevoegen om dit voortaan te filteren. Geen Inbox-items: meld het en stel voor eerst `ideate`, `briefing` of `inspire` te draaien, of over te stappen op route B. Anders: presenteer de items genummerd (titel, de kern in één zin uit de eerste regel van Notities, en Bestemming) en wacht op de keuze. Token-fallback: noemt de gebruiker zelf een item, sla de lijst over en haal alleen dat op.
   - **Route B, directe opdracht.** De gebruiker zegt "schrijf nu een post over X" en levert eventueel zelf instructies aan (invalshoek, punten, format, platform). Neem het onderwerp en die instructies als input; aangeleverde instructies zijn leidend, net als een zelf-aangeleverd haakje. Is het onderwerp te dun, vraag kort door. Er is geen Notion-item.
   - **Route C, uit het maandplan.** De gebruiker noemt een slot ("het slot van dinsdag", "de post voor 14 oktober", de naam van het item) of zegt "uit het maandplan". Zoek in de Content-database het item met `Status = 📅 Scheduled` op die datum of naam; zijn er meerdere Scheduled-slots zonder datum, toon ze genummerd en laat kiezen. Staat het slot nog op Inbox, dan is de weekvulling nog niet gedaan: verwijs naar `@planner week`. Slots met Bron Oogst horen bij `@repurpose`; zeg dat en stop. Werk één slot per keer af.
2. **Idee of slot lezen (route A en C).** Route A: fetch de gekozen pagina volledig. Lees Naam, Notities, Bestemming en Bron URL. Route C: lees uit het slot Naam, Publicatie, Platform, Pillar, Bron, Etalage, Notities en Inspired by. De Notities bevatten de gekozen optie en de herkomst (een voorraaddatum bij hergebruik, een idee-titel bij inspiratie, een onderwerp bij eigen, en bij een Launch-slot de lanceerfase en het aanbod). Is het een hergebruik-slot, haal dan de oorspronkelijke post op uit `Resources/Input/09 - Bestaande voorraad.md` (datum en eerste regel staan in Notities). Is Inspired by gevuld, fetch dan ook dat Inspiratie-item. De Notities bevatten minstens een kern en een INTENTIE; bij een door `ideate` geschreven item staan daar ook pillar, hook/angle, format en eventueel de opgehaalde bron-copy in. Route B slaat deze stap over.
3. **Mini-brief opstellen.** Werk het idee (route A) of de opdracht (route B) uit tot een concrete brief en bevestig die voordat je schrijft. Gaf de gebruiker expliciete instructies, dan structureer je die in plaats van zelf af te leiden.
   - **Pillar.** Route C: de pillar staat vast in het slot; leg hem niet opnieuw voor, want de planner heeft hem uit de basismix afgeleid. Bij een Launch-slot bepaalt de lanceerfase in Notities de invalshoek (tease deelt zonder link, opening kondigt aan met deadline, inhoud en bezwaren licht één ding uit of weerlegt een misconceptie, aftellen noemt de dagen, na-launch verwelkomt). Route A rijk item: neem de pillar over uit de Notities. Route A dun item of route B: leid de pillar af uit het onderwerp plus het topic-pillars document en leg kort voor. Noemde de gebruiker zelf de pillar of invalshoek, neem die over.
   - **Format en framework.** Stel het format voor met onderbouwing en ruimte om te wisselen (carrousel, reel of tekstpost). Het framework kies je stil op basis van pillar en post-doel (`frameworks.md`; bij carrousel de slide-mapping uit `carousel.md`, bij video een van de twee skeletten); leg het alleen voor bij echte twijfel tussen twee goede opties. De framework-namen blijven intern. Gebruik de bron-copy (indien aanwezig) als referentie voor de structuur die werkte, niet om te kopiëren.
   - **Hook kiezen (de kern van deze stap).** Lees `hooks.md`. Filter intern welke van de vijf mechanismen (Reframe, Cost Reveal, Contrarian, Warning, Curiosity Gap) passen bij dit idee en deze pillar: een mechanisme valt af als het ruwe materiaal ontbreekt (geen onverwachte ratio betekent geen Curiosity Gap, geen concreet gedrag of getal betekent geen Cost Reveal). De mechanisme-namen zijn intern: ze horen in dit document en in de metadata, niet in het gesprek. Noem ze nooit tegen de gebruiker. Presenteer in plaats daarvan de 2 à 3 sterkste als concrete haakjes in gewone taal, per optie de hook-zin plus één regel die laat zien waar de post heen gaat, met een aanbeveling. Respecteer de grondtoon uit `identiteit.md`: staat "hoe confronterend" op zacht, leid dan niet met de Contrarian-optie. Vraag welk haakje de gebruiker aanspreekt. Optioneel en kort, in gewone taal zonder labels: waarom een richting niet past.
   - **Overslaan of sneller.** Twee gevallen waarin je de hook-keuze overslaat: (a) de gebruiker levert haar eigen haakje of openingszin aan, gebruik die zoals ze 'm geeft en stel niets voor; (b) de gebruiker zegt "kies jij maar" of wil vaart maken, pak dan je eigen aanbeveling. In beide gevallen bepaal je intern welk mechanisme het is, voor de metadata.
   - **Platform.** Route C: het platform staat vast in het slot; bevestigen hoeft niet. Anders: bevestig het platform. Lees de eigen voorbeelden in `Resources/Input/06 - Contentvoorbeelden/[platform]/` en de deltas in `platforms.md` voor de toon en de vorm van dat kanaal. Ontbreken de platform-voorbeelden, meld dan dat je voor de toon terugvalt op de brand voice.
   - **Bevestigen.** Wacht op bevestiging van het format, het haakje en het platform voordat je gaat schrijven. Het framework is stil gekozen, tenzij je het hebt voorgelegd.
4. **Concept schrijven (de "before").** Schrijf één sterke versie in de Brand Voice. Volg de beats van het gekozen framework uit `frameworks.md` (bij carrousel de slide-mapping uit `carousel.md`), open met het gekozen hook-mechanisme uit `hooks.md`, en leg waar passend de villain-overlay uit `villain.md` eroverheen (sla 'm over als deze pillar geen villain heeft). Pas de platform-toon en de technische deltas uit `platforms.md` toe. Houd hook en body op de kwaliteitslat van `micro-specificity.md` (laag 3-4, concreet anker, vermijd de AI-vormen). Dit is het AI-concept, het vertrekpunt, niet het eindproduct.

   **Links in het concept.** Komt er een link naar een aanbod, checkout, opt-in of landingspagina in de tekst, gebruik dan nooit de kale URL. Zoek in Notion de database "UTM Links" en kijk of er al een regel is voor deze plek: deze mail, deze post, dit kanaal. Bestaat die regel, gebruik dan de Volledige URL daaruit. Bestaat hij niet, roep dan de utm-skill aan om de link te bouwen en als regel op te slaan, en gebruik daarna die link.
5. **Gebruiker herschrijft (de "after").** Nodig de gebruiker uit het concept te herschrijven in de eigen stem. Dit is essentieel: de eigen stem is de kern van het systeem. Ontvang de definitieve versie in de chat of geplakt. Herschrijft de gebruiker (nog) niet, ga door maar markeer in het bestand dat de definitieve versie nog ontbreekt.
6. **Opslaan.** Schrijf `Output/Content/JJJJ-MM-DD-[slug].md` met dit format:

   ```
   ---
   idee: [Naam van het Notion-item, of het onderwerp bij route B]
   notion: [Bron URL van het Notion-item, of leeg bij route B]
   slot: [URL van het slot in de Content-database bij route C, anders leeg]
   pillar: [pillar]
   hook: [gekozen mechanisme, bijv. Reframe]
   format: [gekozen format]
   framework: [gekozen framework]
   platform: [platform]
   datum: JJJJ-MM-DD
   status: concept | definitief
   ---

   ## Concept (AI)

   [het AI-concept uit stap 4]

   ## Definitief (eigen voice)

   [de herschreven versie uit stap 5, of: "nog te doen"]
   ```

7. **Status bijwerken (route A en C).** Route A: zet het Notion-item op `Status = 📝 → Content`, zodat het van de openstaande lijst verdwijnt en niet dubbel wordt uitgewerkt. Doe dit zodra het concept is opgeslagen. Route C: schrijf terug naar het slot zodra het bestand staat: `Bestand` = de bestandsnaam uit stap 6 en `Status = ✍️ Drafting`. Komt de definitieve versie binnen (stap 5, nu of later), zet dan `Status = 👀 Review` en de definitieve tekst in `Caption`, zodat het maandplan in Notion de tekst bevat waarmee geplaatst wordt. Heeft het slot een Inspired by, zet dat Inspiratie-item ook op `📝 → Content`. Plaatsen en de status Published blijven aan de gebruiker (of aan de plaatsroute uit planner.md). Route B heeft geen Notion-item; sla deze stap over (optioneel: bied aan het onderwerp via `@save` in de Inspiratiedatabase te bewaren).
8. **Afsluiten.** Meld kort welk idee of onderwerp is uitgewerkt en naar welk bestand het is opgeslagen. Bij route A: ook dat het Notion-item op `📝 → Content` staat. Bij route C: dat het slot op Drafting of Review staat, en dat de status Published pas na plaatsen gezet wordt.

## Foutafhandeling

- Geen Inbox-items (route A): meld het, stel voor eerst `ideate`, `briefing` of `inspire` te draaien, of stap over op route B.
- Save-skill niet geïnstalleerd, geen link op de regel `Systeempagina:`, of geen database "Inspiratie" op die pagina: dan kan route A niet zoeken. Zeg wat er ontbreekt, wijs op les 5 en 7 van Module 1 (Business HQ-pagina en save-skill) en bied aan ondertussen via route B verder te werken.
- Slot niet gevonden of niet op Scheduled (route C): geen skelet of geen weekvulling voor die datum. Verwijs naar `@planner maand` of `@planner week`, of bied route B aan voor een losse post.
- Terugschrijven naar het slot faalt: meld dat het bestand wel lokaal staat maar het slot niet is bijgewerkt, en geef de slot-URL zodat de gebruiker status en bestandsnaam zelf kan zetten.
- Dun item (alleen kern en INTENTIE, geen pillar of mechanisme): leid de pillar af uit de kern plus het topic-pillars document en draai de hook-voorstel-stap (stap 3) zoals normaal. Ontbreekt `hooks.md` of het topic-pillars document, ga door maar meld dat de invalshoek algemener is.
- Gebruiker herschrijft niet: sla het bestand op met alleen het concept, zet `status: concept` en noteer "nog te doen" bij de definitieve versie. Zet het Notion-item alsnog op `📝 → Content` (het idee is in behandeling).
- Notion-update faalt: meld dat het bestand wel lokaal staat maar de status in Notion niet is bijgewerkt, en geef het item-URL zodat de gebruiker het handmatig kan verzetten.
- UTM Links-database niet gevonden: gebruik de kale URL in het concept en meld dat deze link nog niet getagd is. De database wordt opgezet in de data-module.
- Brand Voice Guide ontbreekt: ga door met algemene best practices, maar meld dat het concept minder afgestemd is op de eigen stem.
- Fundament ontbreekt (`identiteit.md` of `strategie.md`): ga door met de Brand Voice en wat er wel is, maar meld dat de villain, anti-visie, grondtoon of mix niet meegenomen kon worden.

---

*Versie 30-09-2026: route C "uit het maandplan" (slot uit de Content-database van @planner, met terugschrijven van Bestand, Drafting en Review). De Inspiratie- en Content-database komen van de systeempagina uit de save-skill (regel `Systeempagina:`), op naam; geen ID's in deze skill. De content-bestemmingen komen uit strategie.md; de inrichtingsprompt is voor deze skill niet meer nodig.*
