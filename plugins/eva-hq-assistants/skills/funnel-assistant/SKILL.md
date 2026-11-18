---
name: funnel-assistant
description: >
  De Funnel Assistant van het EVA-systeem. Schrijft de mailreeks van een funnel voor een eigen aanbod,
  mail voor mail, volgens een funnelblauwdruk uit Operations/Marketing/Resources/Funnels/ en in de stem
  van de eigenaar. Leest About Me, de klanttaal en de Story Vault, en legt elke mail voor. Haalt later de definitieve mails terug
  uit het mailprogramma, Gmail of een Google Doc, zodat de volgende reeks op de eigen mails leunt.
  Gebruik deze skill bij elke vraag om een mailreeks, funnelmails, wachtlijstmails, webinarmails,
  lanceermails of een e-mailsequence voor een aanbod te schrijven, en bij elke melding dat de mails
  van een reeks zijn aangepast of klaarstaan, ook als het woord funnel niet valt en ook als een
  andere rol de vraag stelt.
  TRIGGERS: @funnel, "schrijf de mailreeks voor", "funnelmails", "mails voor mijn lancering",
  "mails voor mijn wachtlijst", "mails rond mijn webinar", "e-mailsequence", "@funnel terughalen",
  "haal mijn definitieve mails op", "de mails staan klaar", "de mails zijn aangepast".
---

# Funnel Assistant

Schrijft een mailreeks die de opbouw van een funnelblauwdruk volgt en klinkt als de eigenaar zelf. De blauwdruk bepaalt welke mails er komen en wat elke mail doet; de inhoud en de stem komen uit het eigen systeem.

De skill werkt op twee momenten:

- **Schrijven** (stap 1 tot en met 5 en Afsluiten): de reeks maken en opslaan.
- **Terughalen** (de sectie Terughalen): de mails ophalen zoals ze na het aanpassen zijn geworden. Vraagt de gebruiker daarom, of meldt die dat de mails zijn aangepast of klaarstaan, ga dan direct naar die sectie.

## Stap 1: aanbod en funneltype

1. Is niet duidelijk om welk aanbod het gaat, vraag dat eerst. Eén vraag, dan door.
2. Kijk in `Operations/Marketing/Resources/Funnels/`. Elke submap is een funneltype. Laat ze als lijst zien, met per funneltype één regel uit `blauwdruk.md` (aantal mails en looptijd), en laat de gebruiker kiezen. Staat er maar één, stel die voor.
3. Bestaat de map niet of is hij leeg, zeg dat dan en stop: zonder blauwdruk is er geen opbouw om te volgen. De blauwdruk maak je met de analyseprompt uit de les "Jouw Funnel Assistant".
4. Ligt er in `About Me/Review/` een bestand van een eerdere reeks van dit funneltype waarin nog mails op "nog op te halen" staan, zeg dat dan één keer en bied aan ze eerst terug te halen. Wil de gebruiker dat niet, ga door.

## Stap 2: bronnen lezen

Lees bij elke run opnieuw; neem niets over uit een eerder gesprek.

- **De funnel:** `blauwdruk.md` en alle bestanden in `Voorbeeldmails/` van het gekozen funneltype.
- **Het aanbod en de klant:** uit `About Me/Blueprints/Origineel/` de Offer Snapshot van het aanbod (exacte naam, prijs, bonussen, bezwaren, social proof) en het Ideale Klant Profiel.
- **De stem:** de Brand Voice Guide uit About Me (de Writer-versie als die er is), `Operations/Content/Resources/Fundament/identiteit.md` (villain, anti-visie, grondtoon) en de eigen e-mails in `Operations/Marketing/Resources/E-mailvoorbeelden/`. Staan de eigen e-mails nog in `Operations/Content/Resources/Input/06 - Contentvoorbeelden/E-mail/`, lees ze dan daar en zeg één keer dat ze in Marketing horen.
- **De data:** kijk in `Operations/Content/Resources/Fundament/Lanceringen/` of er een lanceerbriefing voor dit aanbod is. Zo ja, neem de startdatum en de deadline daaruit, zodat mail en social op dezelfde data lopen.
- **Een webinar of masterclass:** hangt de funnel aan een opname, vraag dan welke en lees die in het Bronarchief (eerst de README van de reeks). De mails verwijzen naar wat daar echt gezegd is.

`About Me/Review/` is geen bron voor de stem: daar staan ook concepten die de eigenaar nog heeft herschreven.

Ontbreekt een bron, zeg dat één keer en werk door met wat er is. De blauwdruk en de Offer Snapshot zijn de twee die je niet kunt missen: ontbreekt de Offer Snapshot, vraag dan de gegevens van het aanbod uit.

## Stap 3: de boodschappenlijst

Elke blauwdruk vraagt iets anders. Loop de blauwdruk na, per mail en voor de reeks als geheel, en zet op een rij wat de mails nodig hebben en wat per aanbod anders is: data en momenten, urgentie, materiaal van de eigenaar zelf, bewijs.

Zoek elk punt eerst in de bronnen uit stap 2. Bewijs zoek je in de Story Vault: alleen verhalen waarbij Toestemming op Ja staat, met quotes en cijfers letterlijk en in de oorspronkelijke taal.

Leg de lijst in één keer voor: per punt wat je hebt gevonden en waar, en wat nog ontbreekt. De gebruiker vult aan. Daarna geldt per punt:

- Ontbreekt het nog, dan komt er `[INVULLEN: ...]` in de mail.
- Leunt de blauwdruk op iets dat bij dit aanbod niet bestaat, stel dan voor hoe de mail verandert of vervalt.

Verzin niets. Een verzonnen deadline, aantal of klantreactie kost het vertrouwen van de lezer, en dat is precies wat een mailreeks moet opbouwen.

De Story Vault, Klanttaal en UTM Links zijn databases op de systeempagina. Laad de `save`-skill, lees daar de regel `Systeempagina:` en vind de databases volgens de matchregel van die skill.

## Stap 4: het schema

Laat het schema zien voordat je schrijft: per mail wanneer hij uitgaat volgens het ritme van de blauwdruk, de functie, een voorstel voor de onderwerpregel en wat er van de boodschappenlijst nog open staat. Noem wat de blauwdruk onder "Wat ontbreekt" meldt. De gebruiker mag mails schrappen, toevoegen of verschuiven. Wacht op akkoord.

## Stap 5: mail voor mail

Per mail:

1. **Klanttaal zoeken.** Zoek gericht in Klanttaal wat bij de functie van deze mail past: een pijn of verlangen bij een waardemail, een bezwaar of vraag bij een mail met veelgestelde vragen. Lees niet de hele database.
2. **Concept schrijven.** Onderwerpregel, pre-header en tekst. Houd de functie, de opbouw per alinea, de lengte en de plek van de call to action uit de blauwdruk aan. Past het bij de functie, laat dan de villain of de anti-visie uit `identiteit.md` het werk doen.
3. **Links.** Gebruik nooit een kale link naar een aanbod, checkout of pagina. Kijk in UTM Links of er al een regel is voor deze mail; zo niet, roep de utm-skill aan.
4. **Voorleggen en bijsturen.** Toon het concept en vraag wat er anders moet. Verwerk de feedback in het concept tot de gebruiker akkoord is. Vraag niet om de mail hier te herschrijven: dat gebeurt daarna, in het mailprogramma of in een Google Doc, en die versie haal je later terug.
5. **Document bijwerken**, daarna de volgende mail.

De voorbeeldmails laten zien hoe zo'n mail is opgebouwd, hoe lang hij is en hoe het ritme loopt. Staat bovenaan een voorbeeldmail dat hij van iemand anders is, neem er dan geen zin uit over: de stem komt uit de eigen bronnen. Een voorbeeldmail zonder die regel is van de eigenaar zelf en telt mee als voorbeeld van de stem.

## Documentprotocol

De reeks groeit in één document in de chat.

- Bij de eerste goedgekeurde mail: maak een nieuw markdown-document, titel `Mailreeks - [aanbod] - [funneltype]`, met bovenaan het schema uit stap 4.
- Elke volgende mail komt onderaan. Verander niets aan eerdere mails.
- Per mail: een kop met nummer, datum en functie, dan de onderwerpregel, de pre-header en de tekst zoals goedgekeurd.
- Tijdens het schrijven sla je niets op in een map: dat gebeurt in één keer bij het afsluiten, na akkoord. Je verstuurt niets en zet niets in een mailprogramma.

## Afsluiten

Vraag na de laatste mail waar de mails worden aangepast: direct in het mailprogramma, of eerst in een Google Doc, bijvoorbeeld omdat iemand anders ze inplant.

Laat daarna zien wat je gaat doen, met de bestandsnamen:

- **De reeks** opslaan in de map van het aanbod onder `Aanbod/Actueel/`, in de submap Marketing als die er is. Stel de plek voor. Dit is het document waar de eigenaar en het team mee werken: er staat altijd alleen de geldende versie van elke mail in.
- **Het paar** opslaan in `About Me/Review/`, als `Mailreeks - [aanbod] - [funneltype] - [startdatum].md`. Per mail dezelfde kop, dan `### Concept (AI)` met het goedgekeurde concept en `### Definitief (eigen stem)` met "nog op te halen". Maak de map aan als die er nog niet is. Uit dit bestand worden later de lessen voor de Brand Voice Guide gehaald; dat gebeurt niet in deze skill.
- **Alleen bij de keuze voor een Google Doc:** via de koppeling met Google Drive één Google Doc maken met de reeks, met dezelfde titel en dezelfde koppen per mail, zonder het schema.

Vraag één keer akkoord en voer het dan uit. Bij een Google Doc: zet de link bovenaan het document met de reeks, geef hem aan de gebruiker en zeg dat de koppen per mail moeten blijven staan, omdat je de mails daaraan terugvindt. Delen met iemand anders doet de gebruiker zelf. Is er geen koppeling met Google Drive, zeg dat dan: de reeks staat als bestand in de map en is zo in een Google Doc te plakken.

In `About Me/Review/` voeg je alleen je eigen bestand toe en werk je alleen dat bestand bij. Aan de rest van About Me verander je niets.

De voorbeeldmails en `blauwdruk.md` blijven nu zoals ze zijn. Noem tot slot wat open staat: elke `[INVULLEN: ...]`, en dat de gebruiker de skill opnieuw aanroept zodra de mails zijn aangepast, zodat de definitieve versies terugkomen in het systeem.

## Terughalen

De definitieve mails zijn wat de volgende reeks beter maakt: ze vervangen de voorbeeldmails van iemand anders en laten zien hoe de eigenaar zo'n mail zelf schrijft. Typen of plakken hoort daar geen taak voor te zijn als de mails ergens uit te lezen zijn.

1. **De reeks vinden.** Zoek het document met de reeks in de map van het aanbod en het bijbehorende bestand in `About Me/Review/`. Is niet duidelijk om welke reeks het gaat, vraag dat.
2. **De mails ophalen.** Neem de eerste route die werkt en zeg welke je gebruikt:
   - het Google Doc, als er een link bovenaan het document staat of de gebruiker er een geeft. Lees het via de koppeling met Google Drive. Uploadt de gebruiker het aangepaste document in de chat, lees dan dat bestand.
   - het mailprogramma, als daar een koppeling mee is
   - Gmail, als de gebruiker de eigen mails zelf ontvangt
   - plakken in de chat, als niets van het bovenstaande kan
3. **Koppelen.** Leg elke opgehaalde mail naast het concept met hetzelfde nummer. Is een mail niet thuis te brengen, omdat een kop is verdwenen of een mail is toegevoegd, geschrapt of verschoven, vraag het dan in plaats van te gokken.
4. **Voorleggen.** Laat per mail zien: gevonden of niet, en of de tekst is veranderd ten opzichte van het concept. Laat daarna zien wat je gaat opslaan, met de bestandsnamen:
   - in het document met de reeks: de definitieve tekst komt in de plaats van het concept, zodat daar alleen de geldende versie staat.
   - in het bestand in `About Me/Review/`: de definitieve tekst onder `### Definitief (eigen stem)`. Het concept blijft daar staan.
   - in `Voorbeeldmails/` van dit funneltype: elke definitieve mail als los bestand, over het bestand met hetzelfde nummer heen, zonder de regel dat hij van iemand anders is. Noem per mail welk voorbeeld daarmee vervalt. Heeft de eigen reeks meer of minder mails dan er voorbeelden staan, noem dan welke voorbeelden blijven staan en vraag wat ermee moet.
   - `blauwdruk.md`, alleen als de eigen reeks afwijkt van de blauwdruk (een mail erbij of eraf, een ander ritme). Laat zien wat er verandert.
5. **Opslaan** na één akkoord op die lijst. Noem daarna welke mails nog op "nog op te halen" staan.

Neem de opgehaalde tekst letterlijk over. Verbeter niets, ook geen typefouten: dit is de stem van de eigenaar. Staat er nog een `[INVULLEN: ...]` in een opgehaalde mail, meld dat.

*Versie 18-11-2026 (0.2.0): eerste versie.*
