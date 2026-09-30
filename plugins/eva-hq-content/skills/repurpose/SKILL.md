---
name: repurpose
description: De repurpose-skill voor longform content. Neemt één ankerstuk (webinar, academy-les, nieuwsbrief, blogpost, podcast- of videotranscript, Q&A-call) en haalt daar in twee fasen meerdere shortform-posts uit. Fase 1 extraheert de fragmenten die op zichzelf kunnen staan en legt ze voor; fase 2 werkt de gekozen fragmenten uit tot concepten via de engine van de Content Creator, elk met een eigen invalshoek. TRIGGERS op @repurpose, "repurpose", "hergebruik deze content", "maak posts uit deze longform", "haal posts uit dit transcript", "knip deze les op", "wat kan ik uit deze webinar halen", "longform naar shortform", "zet deze nieuwsbrief om naar posts", "het oogst-slot van [datum]", "uit het maandplan" bij een oogst-slot van @planner, of elke vraag om uit bestaand langer materiaal meerdere social posts te maken. Vereist de content-creator skill (gedeelde engine).
---

# repurpose

Repurpose-skill. Neemt één stuk longform (het anker) en rolt dat uit naar meerdere shortform-posts. Niet "maak dit korter", maar: welke losse inzichten, verhalen, quotes en standpunten zitten hierin die op zichzelf kunnen staan als post? Werkt in twee fasen: eerst extraheren en selecteren, daarna per gekozen fragment een concept schrijven zoals de Content Creator dat doet, met de gedeelde engine en de before/after-kern. De gebruiker herschrijft elk concept in de eigen stem; deze skill levert vertrekpunten, geen eindproducten.

## Verhouding tot de andere skills

- `ideate` begint bij een topic en doet research. `repurpose` begint bij bestaand eigen materiaal en haalt er de post-waardige stukken uit. Andere denkstap, andere input.
- `content-creator` werkt één idee uit. `repurpose` werkt meerdere fragmenten uit één anker uit, met dezelfde engine en hetzelfde opslagformat.
- Deze skill schrijft en bewaart concepten (zelfstandig uit te voeren). Plaatsen of publiceren doet ze nooit; dat blijft altijd aan de gebruiker.

## Vereisten

Deze skill heeft geen eigen engine. Ze leest de engine-bestanden uit de map `content-creator` die naast de map van deze skill staat, in dezelfde skills-map (vanaf de basismap van deze skill: `../content-creator/`; in de plugin eva-hq-content en bij losse installatie is dat dezelfde plek): `hooks.md`, `frameworks.md`, `carousel.md`, `villain.md`, `platforms.md` en `micro-specificity.md`. Is de content-creator skill niet geïnstalleerd, zie Foutafhandeling.

## Inputs

1. **Het anker.** Eén stuk longform, aangeleverd als:
   - geplakte tekst in de chat (transcript, artikel, nieuwsbrief, aantekeningen),
   - een bestand in de projectmap (bijvoorbeeld een transcript uit `@transcribe`),
   - of een URL (ophalen en de kerntekst extraheren).
   - of een **oogst-slot uit het maandplan** (route C): een item in de Content-database in Notion (laad de `save`-skill, de ene persoonlijke bron voor de databases, lees daar de regel `Systeempagina:`, fetch die pagina en pak de database die volgens de matchregel van de save-skill "Content" heet) met `Status = 📅 Scheduled` en `Bron = Oogst`, waarvan de Notities de reeks en de aflevering uit het Bronarchief noemen; het anker is dan dat bestand.
   Geen anker aangeleverd: vraag ernaar ("Wat wil je hergebruiken? Plak de tekst, wijs een bestand aan of geef een URL.").
2. **Context** (lezen, niet wijzigen), dezelfde bronnen als de Content Creator:
   - de Brand Voice Guide uit About Me
   - het fundament in de projectmap: `identiteit.md` (stem, villain, anti-visie, grondtoon) en `strategie.md` (pillars, mix)
   - de engine uit de content-creator skillmap (zie Vereisten)
   - de eigen contentvoorbeelden per platform in `Resources/Input/06 - Contentvoorbeelden/[platform]/`
   - het topic-pillars document in `Resources/Input/`
3. **Output-map:** `Output/Content/` in de projectmap. Maak de map aan als die nog niet bestaat.

## Output

- Per uitgewerkt fragment één before/after-bestand: `Output/Content/JJJJ-MM-DD-[korte-slug].md`, in hetzelfde format als de Content Creator, aangevuld met anker-metadata (zie stap 8).
- Eén anker-overzicht: `Output/Content/JJJJ-MM-DD-anker-[slug].md`, met de bron, alle gevonden fragmenten en welke daarvan zijn uitgewerkt. Zo blijft traceerbaar wat er nog in het anker zit voor later.

## Procedure

### Fase 1: extraheren en selecteren

1. **Anker innemen en herkennen.** Komt de vraag uit het maandplan ("het oogst-slot van dinsdag", "uit het maandplan"), zoek dan het slot in de Content-database (Scheduled, Bron Oogst, op die datum of naam), lees Notities, Platform, Pillar en Publicatie, en open het genoemde bestand in het Bronarchief als anker. Slot op Inbox: verwijs naar `@planner week`. Slot met een andere bron dan Oogst: dat is werk voor `@content`, zeg dat en stop. Onthoud bij een slot dat er precies één post voor dat slot nodig is; extra fragmenten mogen, maar als losse concepten of via `@save`. Neem verder de bron in (tekst, bestand of URL) en herken het brontype: webinar- of lestranscript (spreektaal, uitweidingen, filler), blogpost of artikel (gestructureerd), nieuwsbrief (één kernidee, spreektaal op papier), podcast- of videotranscript (meerdere sprekers mogelijk), Q&A-call (vragen als goudmijn), salespagina (overtuigend, met aanbod). Het brontype bepaalt hoe je leest: bij transcripten negeer je filler en zoek je de momenten, bij geschreven stukken volg je de structuur. Is het aangeleverde stuk te dun om meerdere posts uit te halen (richtlijn: minder dan een pagina zonder duidelijke deelinzichten), meld dat eerlijk en stel voor het als één post uit te werken via de Content Creator.
2. **Context laden.** Lees de Brand Voice Guide, het fundament (`identiteit.md`, `strategie.md`), het topic-pillars document en de engine-bestanden uit de content-creator skillmap. Ontbreekt iets, zie Foutafhandeling.
3. **Fragmenten extraheren.** Doorzoek het anker op vijf soorten materiaal:
   - **Standalone inzichten**: feiten, frameworks of modellen die op zichzelf waarde hebben
   - **Quotable momenten**: zinnen die als hook of quote-post kunnen werken
   - **Verhalen en voorbeelden**: klantverhalen of eigen ervaringen die als story-post werken
   - **Standpunten**: uitspraken die tegen de gangbare aanname ingaan en gesprek uitlokken
   - **How-to fragmenten**: stappen of tips die als educatieve post kunnen staan

   Harde regels bij het extraheren: gebruik alleen wat er echt in het anker staat, verzin geen data, voorbeelden of quotes erbij. Elk fragment moet zonder het anker te begrijpen zijn. Vat samen, kopieer niet.

   **Eén-idee-ankers.** Sommige bronnen (een nieuwsbrief, een korte mail, een enkele blogpost) dragen niet meerdere losse fragmenten maar één kernidee. Forceer dan geen fragmenten die er niet zijn. Schakel over op invalshoek-varianten: hetzelfde kernidee, uitgewerkt langs verschillende pillars, hook-mechanismen en formats (bijvoorbeeld een keer educatief uitgelegd, een keer als persoonlijk verhaal, een keer als standpunt tegen de gangbare aanname). Label en presenteer die varianten in stap 4 en 5 precies zoals fragmenten; de rest van de procedure blijft gelijk. Een mail levert zo doorgaans 2 tot 3 stukken op waar een webinar er 6 tot 8 geeft, en dat is goed: het aantal volgt uit de bron, niet uit een streefgetal.
4. **Fragmenten labelen.** Geef elk fragment:
   - de **kern in één zin**
   - de **pillar** (uit `strategie.md` en het topic-pillars document)
   - een **format-suggestie** (carrousel, reel, tekstpost of quote-post, passend bij pillar en materiaal; gebruik `frameworks.md` en `carousel.md` om te toetsen of het materiaal het format draagt)
   - een **platform-suggestie** (Instagram, LinkedIn of beide, via `platforms.md`)
   - een **hook-richting** in gewone taal: één mogelijke openingszin plus waar de post heen gaat. Bepaal intern welk mechanisme uit `hooks.md` dit is; de mechanisme-namen blijven intern en komen alleen in de metadata.
5. **Voorleggen en selecteren.** Presenteer eerst de hoofdboodschap van het anker in één zin met de vraag of die focus klopt. Presenteer daarna de fragmenten genummerd (kern, pillar, format, platform, hook-richting). Bij een oogst-slot: adviseer eerst het ene fragment dat het beste past bij de pillar en het platform van het slot, en laat de gebruiker daarna kiezen of ze uit de rest nog extra stukken wil. Zonder slot: adviseer een selectie van 3 tot 5 fragmenten die samen variëren op drie assen: verschillende pillars, verschillende hook-mechanismen en verschillende formats. Geen twee aanbevolen fragmenten met dezelfde combinatie. Wijs de sterkste aan met één regel waarom. De gebruiker kiest; bijsturen van kern, pillar of format mag gewoon. Voor de niet-gekozen fragmenten: bied aan ze via `@save` in de Inspiratiedatabase te bewaren, zodat ze later alsnog via de Content Creator (route A) uitgewerkt kunnen worden.

### Fase 2: uitwerken per gekozen fragment

6. **Mini-briefs in één overzicht.** Werk elk gekozen fragment uit tot een korte brief: pillar, format met framework (het framework kies je stil, uit `frameworks.md`; bij carrousel de slide-mapping uit `carousel.md`), platform, en het definitieve haakje. Toets de set als geheel op de variatie-assen: geen twee stukken met hetzelfde hook-mechanisme of dezelfde opening, elk stuk leunt op andere delen van het anker. Respecteer de grondtoon uit `identiteit.md` (staat "hoe confronterend" op zacht, leid dan niet met een confronterend haakje). Leg alle briefs in één overzicht voor en wacht op één akkoord; per stuk wisselen van haakje of format kan in die ronde. Levert de gebruiker eigen haakjes of instructies aan, dan zijn die leidend. Zegt de gebruiker "kies jij maar", pak je eigen aanbevelingen en ga door.
7. **Concepten schrijven (de "befores").** Schrijf per stuk één sterke versie in de Brand Voice, zoals de Content Creator dat doet: de beats van het gekozen framework, het gekozen hook-mechanisme, waar passend de villain-overlay uit `villain.md` (sla over als de pillar geen villain heeft), de platform-toon en technische deltas uit `platforms.md`, en de kwaliteitslat van `micro-specificity.md` (laag 3-4, concreet anker, vermijd de AI-vormen). Extra regels die alleen hier gelden:
   - **Niets letterlijk uit het anker.** Elke post krijgt een eigen formulering en een eigen invalshoek; het anker is grondstof, geen tekstbron. Uitzondering: een bewust gekozen quote in een quote-post.
   - **Elk stuk staat los.** Een lezer die het anker nooit zag moet de post volledig begrijpen.
   - **Geen herhaling over de set.** Verschillende openingen, verschillende kernpunten, verschillende voorbeelden.
8. **Presenteren en opslaan.** Presenteer de concepten per stuk, met per stuk in één regel uit welk fragment het komt. Sluit af met een kort overzicht: anker, aantal stukken, gebruikte invalshoeken. Sla elk stuk op als `Output/Content/JJJJ-MM-DD-[slug].md` in dit format:

   ```
   ---
   idee: [kern van het fragment in één zin]
   anker: [titel of bestandsnaam van het anker]
   anker-overzicht: [bestandsnaam van het anker-overzicht]
   notion: [leeg, of de @save-URL als het fragment ook bewaard is]
   slot: [URL van het oogst-slot in de Content-database, alleen bij het stuk dat voor dat slot is geschreven; anders leeg]
   pillar: [pillar]
   hook: [gekozen mechanisme, bijv. Reframe]
   format: [gekozen format]
   framework: [gekozen framework]
   platform: [platform]
   datum: JJJJ-MM-DD
   status: concept | definitief
   ---

   ## Concept (AI)

   [het AI-concept uit stap 7]

   ## Definitief (eigen voice)

   [de herschreven versie, of: "nog te doen"]
   ```

   Schrijf daarnaast het anker-overzicht `Output/Content/JJJJ-MM-DD-anker-[slug].md`: de bron (titel, herkomst, datum), de hoofdboodschap, alle gevonden fragmenten met hun labels, en per fragment de status (uitgewerkt naar [bestand], bewaard via @save, of onbenut).

   **Slot bijwerken (alleen bij een oogst-slot).** Schrijf voor het stuk dat bij het slot hoort terug naar het Notion-item: `Bestand` = de bestandsnaam, `Status = ✍️ Drafting`. Komt de definitieve versie binnen (stap 9), zet dan `Status = 👀 Review` en de definitieve tekst in `Caption`. Extra stukken uit hetzelfde anker krijgen geen slot; die staan in `Output/Content/` en in het anker-overzicht, en kunnen bij een volgende weekvulling als optie terugkomen.

   **Oogst-log bijwerken (stille stap).** Komt het anker uit een bestand in een map met een README die een indextabel met een oogst-log bevat (de Bronarchief-conventie), voeg dan bij het betreffende item één regel toe aan dat oogst-log: datum, aantal uitgewerkte stukken, de invalshoeken in een paar woorden, en de bestandsnaam van het anker-overzicht. Het oogst-log is een logboek, geen afvinkstatus: laat bestaande regels staan; meerdere oogsten per item is normaal en betekent nooit dat een item "op" is. Is het anker geplakte tekst of een URL, of staat er geen README met indextabel in de map van het anker: sla deze stap zonder melding over. Stel de gebruiker hier nooit een vraag over.
9. **Herschrijven op eigen tempo.** Nodig de gebruiker uit de concepten te herschrijven in de eigen stem; dit is de kern van het systeem. Dat hoeft niet nu en niet alles tegelijk: elk stuk houdt `status: concept` tot de definitieve versie binnen is. Komt een herschreven versie later binnen (in deze of een volgende sessie), zet die dan in het juiste bestand en werk de status bij.
10. **Afsluiten.** Meld welke bestanden er staan, welke stukken nog herschreven moeten worden, en wat er onbenut in het anker-overzicht achterblijft. Is er een oogst-log in een reeks-README bijgewerkt, noem dat in één regel. Het inplannen of plaatsen van de posts valt buiten deze skill.

## Foutafhandeling

- **Content-creator skillmap niet gevonden**: de engine ontbreekt. Meld dat deze skill de Content Creator-installatie nodig heeft en verwijs naar de les. Bied als overbrugging alleen fase 1 in kale vorm aan (fragmenten met kern in één zin, zonder pillar-, format- en hook-labels), zodat het denkwerk niet verloren gaat.
- **Anker te dun**: meld dat er te weinig in zit voor meerdere posts en stel voor het als één post uit te werken via de Content Creator (route B).
- **URL niet op te halen**: vraag de tekst te plakken of het bestand aan te wijzen.
- **Fundament ontbreekt** (`identiteit.md` of `strategie.md`): ga door met de Brand Voice en wat er wel is, maar meld dat pillar-labels, villain en grondtoon niet meegenomen konden worden.
- **Brand Voice Guide ontbreekt**: ga door met algemene best practices, maar meld dat de concepten minder afgestemd zijn op de eigen stem.
- **Platform-voorbeelden ontbreken**: meld dat je voor de toon terugvalt op de brand voice.
- **@save niet beschikbaar of Notion niet ingericht**: sla de niet-gekozen fragmenten niet in Notion op; ze staan hoe dan ook in het anker-overzicht en blijven zo vindbaar.
- **Reeks-README niet gevonden of niet schrijfbaar**: sla de oogst-log-stap stil over; het anker-overzicht bevat dezelfde informatie.
- **Gebruiker herschrijft (nog) niet**: sla op met alleen het concept en `status: concept`, noteer "nog te doen" bij de definitieve versie.
- **Oogst-slot niet gevonden, niet op Scheduled, of het bestand uit de Notities bestaat niet**: meld het en verwijs naar `@planner week` (slot) of naar de reeks-README in het Bronarchief (bestand); bied aan het anker dan handmatig aan te wijzen.
- **Terugschrijven naar het slot faalt**: meld dat het bestand wel lokaal staat maar het slot niet is bijgewerkt, en geef de slot-URL.

---

*Versie 19-07-2026: oogst-log toegevoegd (Bronarchief-koppeling). Werkt onveranderd voor ankers zonder Bronarchief.*
*Versie 30-09-2026: route C "uit het maandplan": een oogst-slot van @planner als anker, met terugschrijven van Bestand, Drafting en Review naar het slot. Geen ID's in deze skill; de Content-database komt van de systeempagina uit de save-skill (regel `Systeempagina:`), op naam.*
