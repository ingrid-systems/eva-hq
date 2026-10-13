---
name: utm
description: Genereer UTM-getagde URLs voor checkout-links, funnels, advertenties en social posts en registreer ze in de database UTM Links op je Business HQ-pagina in Notion. Kent geen database-ID's; vindt de database via de regel Systeempagina in de save-skill uit Module 1. TRIGGERS op @utm, "utm link", "utm tag", "tracking link", "campagne link", "maak een link voor", "link met tracking", "checkout link met utm", of elke vraag over het aanmaken van trackbare links voor marketing. Gebruik deze skill ook als de gebruiker vraagt om links te taggen voor e-mail, Meta Ads, Instagram of andere kanalen.
---

# UTM Link Builder

Genereer gestructureerde UTM-links door slim vragen te stellen. De Notion-database "UTM Links" is leidend: elke regel daarin is een bestaande, bruikbare link, en elke nieuwe link wordt er direct in opgeslagen.

## De database vinden

Deze skill kent geen database-ID's en zoekt zelf niets op in Notion. De `save`-skill is de ene bron voor de Notion-databases. Doe dit aan het begin van elke @utm, vóór stap 1:

1. Laad de `save`-skill (zoals `@mail` en `@content` doen) en lees daar de regel `Systeempagina:`.
2. Fetch die pagina en pak de database "UTM Links" volgens de matchregel van de save-skill, sectie "Finding the databases". Let op: de naam staat in de uitlegregel onder het databaseblok, niet in de bloktitel.
3. Fetch de `collection://` data source van die database en volg het schema: de exacte veldnamen en de select-opties van Kanaal en UTM Campaign precies zoals het schema ze spelt.

Hier staat bewust geen kopie van die matchregel. Verandert de regel of de systeempagina, dan verandert dat op één plek in de save-skill en volgt deze skill automatisch mee.

**Save-skill niet geïnstalleerd of geen link op de regel `Systeempagina:`:** zeg dat de link van de Business HQ-pagina eerst in de save-skill moet staan (de les "Skill inrichten: Save All" in Module 1) en stop.

**Geen database "UTM Links" op de Business HQ-pagina:** meld dat en verwijs naar de les "Werken met UTM-links" in de module Data-analyse, waar de database wordt opgezet en op de Business HQ-pagina gezet. Bouw de link wel, maar zeg erbij dat hij nergens geregistreerd is. Verzin geen vervangende opslagplek.

## Proces

### Stap 1: Verzamel de basis-URL

Vraag om de bestemmings-URL (checkout-pagina, landingspagina, salespage). Als de gebruiker alleen een productnaam noemt, vraag dan om de volledige URL. Verzin nooit zelf een URL.

### Stap 2: Bepaal het kanaal

Lees de select-opties van het veld Kanaal uit het schema en bied die aan. Gebruik deze standaardtabel voor de bijbehorende source en medium:

| Kanaal | utm_source | utm_medium |
|---|---|---|
| Meta Ads (Facebook en Instagram) | `facebook` | `cpc` |
| Instagram Ads | `instagram` | `cpc` |
| Google Ads | `google` | `cpc` |
| E-mail (nieuwsbrief of funnel) | naam van het mailprogramma, bijv. `activecampaign`, `mailblue`, `mailerlite` | `email` |
| Instagram organisch (bio/stories) | `instagram` | `social` |
| LinkedIn organisch | `linkedin` | `social` |
| LinkedIn Ads | `linkedin` | `cpc` |
| Anders | vraag om source en medium | |

Noemt de gebruiker een kanaal dat nog niet als select-optie bestaat, voeg het dan toe aan de opties van de database.

### Stap 3: Bepaal de campagnenaam (utm_campaign)

Lees eerst de bestaande opties van UTM Campaign uit het schema. Past de link bij een bestaande campagne, stel die dan voor. Alleen bij een echt nieuwe actie komt er een nieuwe naam bij; voeg die dan toe als optie.

Regels voor campagnenamen:
- Altijd lowercase
- Geen spaties, gebruik streepjes (-)
- Geen datums in de naam tenzij het een eenmalige actie is
- Dezelfde campagnenaam voor alle kanalen van dezelfde actie

Voldoet een voorgestelde naam niet aan deze regels, stel dan een gecorrigeerde versie voor.

### Stap 4: Vraag het specifieke onderdeel (utm_content) - optioneel

Dit identificeert de specifieke mail, ad of link binnen de campagne. Voorbeelden: `mail-1`, `mail-reminder`, `ad-retargeting-v2`, `cta-hero`, `link-bio`, `story-1`. Niet relevant of maar één link? Sla over.

### Stap 5: Check of de link al bestaat

Nu kanaal, bestemming, campagne en content bekend zijn: zoek daarop in de database. Bestaat de regel al, lever de Volledige URL daaruit dan uit volgens stap 6 en ga door naar stap 8. Maak nooit een tweede regel voor dezelfde link.

### Stap 6: Genereer de URL

Bouw de complete URL, klaar om te kopiëren:

```
[basis-URL]?utm_source=[source]&utm_medium=[medium]&utm_campaign=[campaign]&utm_content=[content]
```

Laat utm_content weg als die niet is opgegeven. Geef een korte samenvatting: kanaal, campagne, onderdeel.

### Stap 7: Sla de link op in de database

Maak een regel aan in "UTM Links", met de veldnamen uit het schema:
- Naam: korte omschrijving (bijv. "Mail 2 welkomstfunnel")
- Volledige URL: de gegenereerde link
- Basis-URL: de URL zonder parameters
- Kanaal, UTM Source, UTM Medium, UTM Campaign, UTM Content: zoals gekozen
- Plek: waar de link komt te staan; vraag dit als het nog niet duidelijk is

### Stap 8: Vraag of er meer varianten nodig zijn

Vraag: "Wil je dezelfde campagne nog op een ander kanaal of voor een andere mail/ad?" Zo ja, terug naar stap 2 of stap 4, met behoud van de campagnenaam.

## Edge cases

- **Basis-URL bevat al een `?`**: plak de UTM-parameters erachter met `&` in plaats van een tweede `?`.
- **Diakrieten of spaties in een naam**: normaliseer naar lowercase ASCII met streepjes en leg de correctie kort voor.
- **Trailing slash**: laat de basis-URL zoals de gebruiker hem geeft, verander alleen wat na het `?` komt.
- **Een andere skill roept deze skill aan** (bijvoorbeeld de content-creator die een link in een post nodig heeft): doorloop dezelfde stappen, stel alleen de vragen waarvan het antwoord niet al uit de context volgt, en geef de Volledige URL terug.

## Belangrijke regels

- Gebruik NOOIT hoofdletters in UTM-parameters, alles lowercase
- Gebruik NOOIT spaties, altijd streepjes
- Verzin NOOIT zelf een basis-URL of campagnenaam: bij twijfel vragen, niet gokken
- utm_term wordt NIET gebruikt (dat is voor Google-zoekwoorden)
- Dezelfde campagne = dezelfde utm_campaign, ongeacht het kanaal
- Tag NOOIT links binnen de eigen website van de gebruiker; UTM is voor verkeer van buiten
- Pas bestaande regels in de database nooit ongevraagd aan

---

*Versie 13-10-2026: de database UTM Links komt van de systeempagina uit de save-skill (regel `Systeempagina:`), op naam; geen zoeken meer door heel Notion. Stap 5 (bestaat de link al) toegevoegd. Verwijzingen naar lessen op titel. Uitgeleverd als plugin eva-hq-data.*
