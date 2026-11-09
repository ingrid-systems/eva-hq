---
name: inspire
description: Content creatie-inspiratie op basis van zoektermen en optioneel eigen creators. Zoektermen-scan op Instagram (hashtags) en YouTube (keywords) naar trending formats en hooks, plus een optionele scan van vooraf vastgelegde creators. Verzamelt en schrijft naar een staging-bestand dat daarna met @ideate verwerkt wordt. TRIGGERS op @inspire, "content inspiratie", "inspiratie zoeken", "wat werkt op Instagram", "trending content", "content ideeën", "wat posten anderen", "content research", "hashtag research", "scan mijn creators", "creators bekijken", of elke vraag over het verzamelen van content-inspiratie.
version: "1.2"
---

# inspire

Verzamelskill. Haalt content-inspiratie binnen en schrijft die naar een staging-bestand. Slaat zelf niets op in de inspiratiedatabase; dat doet de `ideate`-skill. Bestaat uit deel A (zoektermen-scan, draait altijd) en deel B (creator-scan, conditioneel).

## Inputs

Lees uit `Resources/Input/` van de werkmap van je Content Manager (de les "Je content map inrichten"). Herken de bestanden aan hun naam, ook als het nummer ervoor afwijkt:

- `11 - Inspire zoektermen.md` — Instagram-hashtags en YouTube-zoektermen. Ontbreekt het bestand of is het leeg: meld dat er geen zoektermen zijn en vraag de gebruiker ze aan te leveren. Verzin zelf geen zoektermen.
- `09 - Inspirerende creators.md` — creators in de vorm `- [Naam] - [platform]: [URL]`.

Chat-overrides hebben voorrang op de bestanden:
- `@inspire "term1" "term2"` → gebruik alleen deze zoektermen voor deel A.
- `@inspire zoektermen` → draai alleen deel A.
- `@inspire creators` → draai alleen deel B.

## Output

Schrijf alle vondsten naar:
```
Output/Inspiratie-staging/inspire-JJJJ-MM-DD.md
```
Maak de map `Output/Inspiratie-staging/` aan als die niet bestaat. Bestaat het dagbestand al, append in plaats van overschrijven. Toon in de chat alleen een korte samenvatting en de regel: "Draai @ideate om dit te verwerken."

Format van het staging-bestand:
```
# Inspire — JJJJ-MM-DD

## Zoektermen-scan
[per item: hook/titel | type | engagement | link | 1 regel waarom het werkt]

## Creator-scan
### [Creator] — [platform]
[per post: hook | format | engagement | link]
```

## Kostenbewaking

Elke Apify-actor rekent per opgehaald resultaat, sommige ook per start. Controleer daarom vóór elke actor-call dat het limietveld de naam heeft die hieronder staat en gevuld is; een verkeerde of lege limiet betekent bij de meeste actors "onbeperkt" en scrapet dan de volledige geschiedenis. Haal niet meer posts op dan de skill voorschrijft.

## Jouw werkwijze

Lees vóór je begint `Resources/Fundament/werkwijze.md` in de werkmap van je Content Manager, als dat bestand er is, en daarin het kopje `## @inspire`. Wat daar staat is hoe de gebruiker wil dat deze skill werkt, en gaat voor op de standaard in deze skill. Zegt de gebruiker dat deze skill voortaan iets anders moet doen, stel dan één regel voor onder dat kopje, schrijf hem na akkoord weg (maak het bestand met de kop `# Werkwijze`, of het kopje, aan als het er nog niet is) en werk vanaf dat moment zo. Pas de skill zelf nooit aan: een update van de plugin overschrijft hem.

## Procedure

### Deel A — Zoektermen-scan

Draait altijd, tenzij de chat-override `creators` is gebruikt. Gebruik alle hashtags uit bestand 11 (of de chat-override) in één Instagram-call, met 10 resultaten per hashtag: één actor-start en zo min mogelijk posts.

1. Bepaal de dedupe-set: zoek het meest recente eerdere `inspire-*.md` in `Output/Inspiratie-staging/` (ook `*-verwerkt.md`) en verzamel de post-URL's die daarin staan. Geen eerder bestand: lege set. Dit is het rollende geheugen; je vergelijkt alleen met de vorige run, niet met alle history.
2. Instagram: roep `apify/instagram-hashtag-scraper` aan met alle hashtags in één call:
   ```json
   { "hashtags": ["<term1>", "<term2>", "..."], "resultsType": "posts", "resultsLimit": 10 }
   ```
   Velden: `caption`, `url`, `likesCount`, `commentsCount`, `type`, `videoViewCount`, `timestamp`, `alt`, `ownerUsername`.
   `resultsLimit` is het aantal posts per hashtag. Zet dit op 10, niet hoger, tenzij de gebruiker om meer vraagt.
   Let op: op een gratis Apify-plan levert deze actor alleen de eerste pagina resultaten. 10 per hashtag valt daar normaal binnen; controleer bij de eerste run of elke hashtag posts teruggeeft en meld het als een hashtag leeg blijft.
3. YouTube: roep `grow_media/youtube-search-api` aan, één call per zoekterm:
   ```json
   { "q": "<term>", "maxResults": 5, "order": "viewCount", "publishedAfter": "<30 dagen geleden, RFC 3339>" }
   ```
   Velden: `title`, `text`, `url`, `channelName`, `viewCount`, `likes`, `commentsCount`, `duration`, `date`.
   Let op: deze actor respecteert `publishedAfter` niet altijd strikt; filter zelf op `date` en laat resultaten ouder dan 30 dagen vallen.
4. Laat posts vallen waarvan de URL al in de dedupe-set staat.
5. Bepaal per overgebleven item het format, de hook (eerste regel/titel) en de engagement. Schrijf onder `## Zoektermen-scan`.

### Deel B — Creator-scan

Voorwaarde: draai dit deel alleen als `Resources/Input/09 - Inspirerende creators.md` bestaat en minstens één creator-regel bevat. Anders: sla deel B over en meld in één regel in de chat dat er nog geen creators in bestand 09 staan, zodat de gebruiker ze kan toevoegen.

Scrape incrementeel: per creator alleen posts die nieuwer zijn dan de vorige run, zodat geen post twee keer wordt gereviewd. Houd de voortgang bij in `Output/Inspiratie-staging/_creator-state.json`, met per creator-URL het veld `laatste_post_timestamp` (ISO-timestamp van de nieuwste post die de vorige run is opgehaald). Maak het bestand aan als het niet bestaat.

Voer het zware werk uit in subagents zodat de hoofdcontext schoon blijft. Per creator uit bestand 09, op basis van het platform in de regel. Eerste run voor een creator (geen entry in `_creator-state.json`): nieuwste 15 tot 25 posts als basislijn. Vervolgrun: alleen posts nieuwer dan `laatste_post_timestamp`, dedupe op shortCode/URL, begrens op 25 nieuwe posts. Is die grens geraakt, meld dan bij die creator dat er mogelijk meer nieuwe posts waren en dat de oudste daarvan niet zijn meegenomen.

1. Scrape, per platform met de bijbehorende actor. Vul altijd het limietveld met de naam die hier staat, anders scrapet de actor onbeperkt:
   - **Instagram** — `apify/instagram-scraper`: `{ "directUrls": ["<profiel-URL>"], "resultsType": "posts", "resultsLimit": 25 }`. Voeg bij een vervolgrun `"onlyPostsNewerThan": "<laatste_post_timestamp>"` toe, zodat de actor alleen nieuwere posts ophaalt. Filter daarna alsnog op `timestamp` nieuwer dan `laatste_post_timestamp`; vastgepinde posts kunnen ondanks het filter meekomen. Haal daarna per reel de on-screen hook op via OCR: `zen-studio/google-lens-ocr` met `{ "imageUrl": "<displayUrl>", "language": "<taal van de creator, bv. en of nl>", "outputDetail": "text_only" }`, in batches van maximaal 10, binnen de subagent.
   - **LinkedIn** — `supreme_coder/linkedin-post`: `{ "urls": ["<profiel- of bedrijfs-URL>"], "limitPerSource": 25 }`. Het limietveld heet `limitPerSource`, niet `maxPosts`; met een verkeerde naam of leeg veld haalt de actor de volledige profielgeschiedenis op (kan honderden posts zijn). Bij een vervolgrun: voeg `"scrapeUntil": "<laatste_post_timestamp>"` toe, zodat de actor alleen nieuwere posts ophaalt in plaats van alles op te halen en achteraf te filteren. Filter daarna alsnog op `postedAtISO` nieuwer dan `laatste_post_timestamp`. De volledige `text` is de hook, geen OCR.
   - **YouTube** — `streamers/youtube-scraper`: `{ "startUrls": [{ "url": "<kanaal-URL>" }], "sortVideosBy": "NEWEST", "maxResults": 25, "downloadSubtitles": false }`. Voeg bij een vervolgrun `"oldestPostDate": "<laatste_post_timestamp>"` toe om alleen nieuwere video's te halen. `title` en de omschrijving (`text`) zijn de hook, geen OCR. Transcript komt pas in @ideate op de keepers.
2. Retry: posts die niet binnenkomen opnieuw ophalen, maximaal twee pogingen per post. Log in het staging-bestand wat na twee pogingen ontbreekt.
3. De subagent retourneert compact (per post: shortCode/URL, type, engagement, hook, datum), geen ruwe JSON.
4. Update `_creator-state.json`: zet `laatste_post_timestamp` voor deze creator op de timestamp van de nieuwste opgehaalde post.
5. Levert deze run geen nieuwe posts voor een creator, schrijf dan de regel "geen nieuwe posts sinds [laatste_post_timestamp]" en ga door naar de volgende creator.
6. Schrijf per creator een blok met alleen de nieuwe posts onder `## Creator-scan`.

### Afronden

Toon in de chat: aantal items uit deel A, aantal per creator uit deel B, en eventuele gaten. Sluit af met het staging-pad en "Draai @ideate om dit te verwerken."

## Foutafhandeling

- Bestand 11 ontbreekt of is leeg: meld het en vraag om zoektermen. Draai niet door op verzonnen termen.
- Bestand 09 ontbreekt of bevat geen creators: sla deel B over en meld in één regel dat er nog geen creators zijn ingevuld.
- Een actor faalt: ga door met de overige bronnen, log de gemiste bron in het staging-bestand.
- Begrens deel B op 15 tot 25 posts per creator bij de eerste run, en op 25 nieuwe posts per creator bij vervolgruns.
- Mist een post een bruikbare `timestamp`, dedupliceer dan op shortCode/URL zodat hij niet alsnog dubbel wordt opgehaald.
