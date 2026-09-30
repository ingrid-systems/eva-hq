---
name: inspire
description: Content creatie-inspiratie op basis van zoektermen en optioneel eigen creators. Zoektermen-scan op Instagram (hashtags) en YouTube (keywords) naar trending formats en hooks, plus een optionele scan van vooraf vastgelegde creators. Verzamelt en schrijft naar een staging-bestand dat daarna met @ideate verwerkt wordt. TRIGGERS op @inspire, "content inspiratie", "inspiratie zoeken", "wat werkt op Instagram", "trending content", "content ideeën", "wat posten anderen", "content research", "hashtag research", "scan mijn creators", "creators bekijken", of elke vraag over het verzamelen van content-inspiratie.
version: "1.0"
---

# inspire

Verzamelskill. Haalt content-inspiratie binnen en schrijft die naar een staging-bestand. Slaat zelf niets op in de inspiratiedatabase; dat doet de `ideate`-skill. Bestaat uit deel A (zoektermen-scan, draait altijd) en deel B (creator-scan, conditioneel).

## Inputs

Lees uit `Resources/Input/` van de actieve Content Creator-projectmap:

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

## Procedure

### Deel A — Zoektermen-scan

Draait altijd, tenzij de chat-override `creators` is gebruikt. Maximaal 3 tot 4 hashtags per run.

1. Bepaal de dedupe-set: zoek het meest recente eerdere `inspire-*.md` in `Output/Inspiratie-staging/` (ook `*-verwerkt.md`) en verzamel de post-URL's die daarin staan. Geen eerder bestand: lege set. Dit is het rollende geheugen; je vergelijkt alleen met de vorige run, niet met alle history.
2. Instagram: roep `burbn/instagram-hashtag-posts-scraper` aan, één call per hashtag:
   ```json
   { "hashtag": "<term>", "maxResults": 10 }
   ```
   Velden: `caption`, `postUrl`, `likeCount`, `commentCount`, `mediaType`, `isVideo`, `postedAt`, `accessibilityCaption`.
3. YouTube: roep `grow_media/youtube-search-api` aan, één call per zoekterm:
   ```json
   { "q": "<term>", "maxResults": 5, "order": "viewCount", "publishedAfter": "<30 dagen geleden, RFC 3339>" }
   ```
   Velden: `title`, `text`, `url`, `channelName`, `viewCount`, `likes`, `commentsCount`, `duration`, `date`.
4. Laat posts vallen waarvan de URL al in de dedupe-set staat.
5. Bepaal per overgebleven item het format, de hook (eerste regel/titel) en de engagement. Schrijf onder `## Zoektermen-scan`.

### Deel B — Creator-scan

Voorwaarde: draai dit deel alleen als `Resources/Input/09 - Inspirerende creators.md` bestaat en minstens één creator-regel bevat. Anders: sla deel B over en meld in één regel in de chat dat er nog geen creators in bestand 09 staan, zodat de gebruiker ze kan toevoegen.

Scrape incrementeel: per creator alleen posts die nieuwer zijn dan de vorige run, zodat geen post twee keer wordt gereviewd. Houd de voortgang bij in `Output/Inspiratie-staging/_creator-state.json`, met per creator-URL het veld `laatste_post_timestamp` (ISO-timestamp van de nieuwste post die de vorige run is opgehaald). Maak het bestand aan als het niet bestaat.

Voer het zware werk uit in subagents zodat de hoofdcontext schoon blijft. Per creator uit bestand 09, op basis van het platform in de regel. Eerste run voor een creator (geen entry in `_creator-state.json`): nieuwste 15 tot 25 posts als basislijn. Vervolgrun: alleen posts nieuwer dan `laatste_post_timestamp`, dedupe op shortCode/URL, begrens op 50 nieuwe posts en meld als die grens is geraakt.

1. Scrape, per platform met de bijbehorende actor:
   - **Instagram** — `apify/instagram-scraper`: `{ "directUrls": ["<profiel-URL>"], "resultsType": "posts", "resultsLimit": 25 }`. Filter op `timestamp` nieuwer dan `laatste_post_timestamp`. Haal daarna per reel de on-screen hook op via OCR: `zen-studio/google-lens-ocr` met `{ "imageUrl": "<displayUrl>", "language": "<taal van de creator, bv. en of nl>", "outputDetail": "text_only" }`, in batches van maximaal 10, binnen de subagent.
   - **LinkedIn** — `supreme_coder/linkedin-post`: `{ "urls": ["<profiel- of bedrijfs-URL>"], "maxPosts": 25 }`. Filter op `postedAtISO` nieuwer dan `laatste_post_timestamp`. De volledige `text` is de hook, geen OCR.
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
- Begrens deel B op 15 tot 25 posts per creator bij de eerste run, en op 50 nieuwe posts per creator bij vervolgruns.
- Mist een post een bruikbare `timestamp`, dedupliceer dan op shortCode/URL zodat hij niet alsnog dubbel wordt opgehaald.
