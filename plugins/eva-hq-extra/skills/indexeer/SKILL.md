---
name: indexeer
description: De indexeer-skill voor het Bronarchief. Zet in één run een reeks (podcast, programma of training, webinars en Q&A's) helemaal goed: reekstype detecteren, mappen en README's opzetten of aanvullen (inclusief de hoofdindex), transcripten lezen en de indextabel vullen met kern, trefwoorden, metadata en status, en het REEKS.md-distillaat opbouwen of bijwerken. Haalt bij reeksen met klanttaal de letterlijke uitspraken van klanten uit nieuwe calls en zet ze in de Notion-database Klanttaal. Ook een nieuwe reeks toevoegen aan een bestaand Bronarchief loopt via deze skill. Optioneel ververst de skill de Notion-spiegel voor teamgebruik. TRIGGERS op @indexeer, "indexeer deze reeks", "indexeer mijn bronarchief", "werk de index bij", "werk het distillaat bij", "voeg een reeks toe aan mijn bronarchief", "zet deze transcripten in de index", "deze aflevering is er nieuw bij", of elke vraag om een reeks in het Bronarchief te indexeren, bij te werken of toe te voegen.
---

# indexeer

Indexeer-skill voor het Bronarchief. Eén run zet één reeks helemaal goed: de mappen en indexen staan er, elk transcript heeft een regel in de indextabel, het distillaat (REEKS.md) vat samen wat je in de reeks als geheel zegt, en bij een reeks met klanttaal staan de uitspraken van klanten in de database Klanttaal. Draai de skill na het transcriberen van een reeks, en daarna telkens opnieuw wanneer er items bij komen. Herdraaien is altijd veilig: bestaand werk blijft staan en wordt alleen aangevuld of aangescherpt.

## Het Bronarchief in het kort

Het Bronarchief is een zelfstandige hoofdmap in de Cowork-structuur. Drie lagen, elk met één taak:

```
Bronarchief/
├── README.md            (hoofdindex: alle reeksen plus status per reeks)
├── MEMORY.md
└── [Reeks]/
    ├── README.md        (indextabel: navigatie per item)
    ├── REEKS.md         (distillaat: kennis over de reeks als geheel)
    └── transcripten (.md)
```

## Token-discipline (harde regel)

Lees nooit het hele archief. Per run: eerst de hoofdindex, dan de README van de betreffende reeks, dan alleen de transcripten van díé reeks. Werk per reeks, nooit over het hele archief tegelijk. Wil de gebruiker meerdere reeksen indexeren, doe ze dan één voor één, met na elke reeks een korte tussenstand.

## Inputs

1. **De reeks.** De gebruiker noemt welke reeks(map) aan de beurt is, of levert een nieuwe reeks aan (naam, type, waar de transcripten staan of komen). Geen reeks genoemd: lees de hoofdindex en stel voor welke reeks logisch is (bijvoorbeeld een reeks met ongeïndexeerde items), laat de gebruiker kiezen.
2. **Het Bronarchief zelf**, als gekoppelde map. Niet gekoppeld of niet te vinden: zie Foutafhandeling.

## Kolommen per reekstype

Detecteer het reekstype en kies het bijbehorende sjabloon:

- **Podcast**: nummer, titel, bestand, publicatiedatum, gast, kern, trefwoorden, status, oogst-log
- **Programma of training**: module- en lesnummer, titel, bestand, kern, trefwoorden, status, oogst-log
- **Webinars en Q&A's**: datum, titel of aanleiding, bestand, kern, trefwoorden, status, oogst-log

Semantiek van de drie vaste kolommen:

- **Bestand** is de bestandsnaam van het transcript in de reeks-map, en de sleutel van de administratie: de skill herkent nieuw werk door de maplijst naast deze kolom te leggen. Leeg bij status "transcript nodig".

- **Status** is een echte status, met precies drie waarden: transcript aanwezig / transcript nodig / onbruikbaar. Bij "onbruikbaar" hoort altijd een korte reden in dezelfde cel, achter een koppelteken: "onbruikbaar - audio onverstaanbaar", "onbruikbaar - re-run van aflevering 12", "onbruikbaar - oud aanbod". Tref je een "onbruikbaar" zonder reden aan, of markeert de gebruiker iets als onbruikbaar zonder te zeggen waarom, vraag er dan kort naar; zo weet de gebruiker over een half jaar nog waarom het item is afgevallen.
- **Oogst-log** is een logboek, geen afvinkstatus. Per oogst-moment één regel (datum, aantal posts, invalshoeken, verwijzing naar het anker-overzicht), meerdere regels per item is normaal, en "geoogst" betekent nooit "leeg". Deze skill maakt de kolom aan en laat hem verder met rust: vullen doet @repurpose, wegen doet de ankers-prompt.

## Procedure

1. **Reeks bepalen en type detecteren.** Stel vast welke reeks aan de beurt is (zie Inputs). Detecteer het reekstype op basis van mapnaam, bestandsnamen en een blik in het eerste transcript. Twijfel tussen twee types: vraag het kort, gok niet.
2. **De kast controleren en aanvullen.** Ontbreekt de hoofdindex-README, de reeks-README of REEKS.md: maak aan volgens het juiste sjabloon, zonder te vragen. Nieuwe reeks: maak de submap, een lege README-indextabel met de juiste kolommen en een leeg REEKS.md aan, en voeg de reeks toe aan de hoofdindex.
   Nieuwe reeks en de database Klanttaal staat op de systeempagina (zie Klanttaal hieronder): vraag één keer of uit deze reeks klanttaal gehaald moet worden, en zet het antwoord als regel `Klanttaal: ja` of `Klanttaal: nee` bovenaan de reeks-README.
3. **Transcripten lezen en de indextabel vullen.** Bepaal eerst wat er te doen staat: maak een lijst van de bestanden in de reeks-map (alleen namen, geen inhoud) en leg die naast de kolom bestand in de indextabel. Bestanden zonder rij zijn nieuw en worden geïndexeerd; bestanden met een rij worden overgeslagen. Lees dan per nieuw item het transcript en vul de regel: kern in één zin, twee à drie trefwoorden (thema's waarop je later wilt kunnen zoeken), de typespecifieke metadata, en de status. Regels voor bestaand werk: reeds geïndexeerde regels en gevulde oogst-logs blijven staan; alleen aanvullen of corrigeren. Items zonder transcript krijgen status "transcript nodig" met een lege kernregel.
4. **Het distillaat opbouwen of bijwerken.** Werk REEKS.md bij tijdens het lezen (de transcripten zijn toch open). Duimregels:
   - Terugkerende thema's en standpunten: altijd opnemen.
   - Kenmerkende verhalen en uitspraken (origin story, de scherpste uitleg van de methode): ook bij één vindplaats.
   - Plichtmatige herhaling (intro's, abonneer-oproepen, logistiek): nooit.
   - Harde grens: maximaal twee pagina's. Loopt het vol, dan aanscherpen in plaats van uitdijen.
   - Bij herindexeren: aanvullen en aanscherpen, nooit blind overschrijven.
5. **Klanttaal oogsten.** Alleen als de reeks-README de regel `Klanttaal: ja` heeft en de database Klanttaal op de systeempagina staat; anders sla je deze stap zonder melding over. Werk met de transcripten die je in stap 3 nieuw hebt gelezen, zie Klanttaal hieronder.
6. **De hoofdindex bijwerken.** Zet in de hoofdindex-README per reeks de actuele status: aantal items, hoeveel geïndexeerd, hoeveel nog transcript nodig.
7. **Optioneel: de Notion-spiegel verversen.** Alleen als de gebruiker dit heeft ingesteld of er in deze run om vraagt; zie de sectie hieronder. Nooit standaard uitvoeren, en er tijdens een gewone indexeer-run nooit naar vragen.
8. **Afsluiten.** Rapporteer kort: welke reeks, hoeveel items geïndexeerd, hoeveel met status "transcript nodig" (verwijs daarvoor naar @transcribe), of het distillaat nieuw is of bijgewerkt, en hoeveel uitspraken er in Klanttaal zijn gezet. Meer niet: wat de gebruiker met het archief gaat doen (content, kennisvragen, salespages) is aan de gebruiker en aan de andere instrumenten.

## Klanttaal

De database Klanttaal bevat letterlijke uitspraken van klanten (pijn, verlangen, bezwaar, vraag), waar de content-skills in zoeken. Hij wordt aangemaakt door de Voice of the Customer-prompt uit de les Jouw boodschappenlijst; deze skill maakt hem nooit zelf aan.

- **De database vinden.** Laad de `save`-skill, lees daar de regel `Systeempagina:`, fetch die pagina en pak de database die volgens de matchregel van de save-skill "Klanttaal" heet. Staat hij er niet, dan sla je alles over klanttaal zonder melding over. Lees vóór het schrijven het schema van de database voor de velden en opties.
- **Wat je oogst.** Per nieuw gelezen transcript de uitspraken waarin een klant of deelnemer in eigen woorden een pijn, verlangen, bezwaar of vraag verwoordt. Letterlijk, in de originele taal, geen samenvattingen en geen namen. Niet je eigen uitleg of die van de host: alleen wat klanten zeggen, ook in de chat van een call.
- **Velden.** Naam: korte samenvatting van de uitspraak. Soort: Pijn, Verlangen, Bezwaar of Vraag. Trefwoorden: hergebruik eerst de bestaande opties uit het schema, maak alleen een nieuw trefwoord aan als niets past, en voeg het dan eerst als optie toe aan het schema. Aanbod: alleen als het uit de call blijkt. Bron: Call. Vindplaats: reeksnaam en bestandsnaam van het transcript. Datum: de datum van het item.
- **Niet dubbel.** Staat de Vindplaats van een transcript al in Klanttaal, sla dat transcript over.
- **Voorleggen.** Toon de uitspraken per transcript in één overzicht en schrijf ze weg na één akkoord; de gebruiker kan uitspraken schrappen of aanpassen.

## Notion-spiegel (optionele stap)

Voor teamgebruik kan de skill de reeks-indexen spiegelen naar Notion. Spelregels:

- **Eénrichtingsverkeer.** De README's in het Bronarchief zijn en blijven de bron van waarheid; Notion is een leesvenster voor teamleden, geen tweede administratie. Er wordt nooit iets uit Notion terug het archief in geschreven.
- **Eén database voor het hele archief**, op de systeempagina: laad de `save`-skill, lees de regel `Systeempagina:`, fetch die pagina en pak de database die volgens de matchregel van de save-skill "Bronarchief" heet. Kolommen: Reeks, Type, Nummer/Datum, Titel, Kern, Trefwoorden, Status, Laatste oogst, Metadata. Typespecifieke details (gast, module- en lesnummer, aanleiding) gaan samengevat in het veld Metadata; Laatste oogst is de datum van de laatste regel uit het oogst-log, of leeg. Views per reeks kan de gebruiker zelf in Notion toevoegen.
- **Verversen, niet dupliceren.** Herken bestaande items op de combinatie Reeks plus Nummer/Datum plus Titel: bestaande pagina's bijwerken, nieuwe toevoegen. Bestaat de database nog niet, stel dan eerst voor hem op de systeempagina aan te maken en wacht op akkoord.

## Foutafhandeling

- **Bronarchief niet gekoppeld of niet gevonden**: meld het en vraag de gebruiker de map te koppelen. Bestaat er nog helemaal geen Bronarchief, verwijs dan naar de opzet-prompt uit de les Je bronarchief opzetten; deze skill bouwt geen archief vanaf nul.
- **Reeks-map zonder transcripten**: meld welke items status "transcript nodig" krijgen en verwijs naar @transcribe.
- **README of REEKS.md ontbreekt**: aanmaken volgens het juiste sjabloon, niet vragen.
- **Bestanden groter dan circa 15MB aangetroffen** (video's, zware PDF's, ongecomprimeerde exports): signaleren; daarvan hoort alleen het transcript of een verwijzing in het archief. Niet lezen, niet indexeren.
- **Reeds geïndexeerde items**: bestaande regels en oogst-logs laten staan, alleen aanvullen of corrigeren.
- **Onduidelijk reekstype**: kort vragen welk type het is, niet gokken.
- **Los transcript aangeleverd in de chat**: stel voor het in de juiste reeks-map te zetten, doe dat na akkoord, en indexeer het direct mee. Bestanden horen in de map, niet in de chat; maar dit vangnet voorkomt dat de gebruiker het zelf moet verplaatsen.
- **Notion niet beschikbaar of niet gekoppeld** (alleen relevant als de spiegel gevraagd is): meld het, sla de spiegelstap over en rond de rest van de run gewoon af; de README's zijn hoe dan ook compleet.

---

*Versie 11-11-2026: in de plugin eva-hq-extra; klanttaal oogsten naar de database Klanttaal; Notion-spiegel via de systeempagina. Eerste klantversie: 19-07-2026.*
