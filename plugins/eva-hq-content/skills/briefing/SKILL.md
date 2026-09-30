---
name: briefing
description: Nieuwsbriefing op basis van keywords uit het vakgebied van de gebruiker. Zoekt over Substack, YouTube, LinkedIn en Google News, filtert op relevantie en schrijft naar een staging-bestand dat daarna met @ideate verwerkt wordt. TRIGGERS op @briefing, "AI briefing", "nieuws briefing", "wat is er nieuw", "nieuws vandaag", "briefing maken", "topic briefing", "zoek nieuws", "wat heb ik gemist", of elke vraag over het ophalen en samenvatten van recent vaknieuws op onderwerp.
version: "1.0"
---

# briefing

Verzamelskill. Zoekt recent vaknieuws op basis van keywords over meerdere platforms, filtert op relevantie en schrijft naar een staging-bestand. Slaat zelf niets op in de inspiratiedatabase; dat doet de `ideate`-skill.

## Inputs

Lees uit `Resources/Input/` van de actieve Content Creator-projectmap:

- `10 - Briefing keywords.md` — vakinhoudelijke keywords (komma-gescheiden of bullets). Ontbreekt het bestand of is het leeg: meld dat er geen keywords zijn en vraag de gebruiker ze aan te leveren. Verzin zelf geen keywords.

Chat-overrides hebben voorrang:
- `@briefing "term1" "term2"` → gebruik alleen deze keywords.
- `@briefing + "term"` → voeg toe aan de keywords uit bestand 10.
- `@briefing substack youtube` → beperk tot de genoemde bronnen.
- `@briefing filter:"..."` → vervang de filterinstructie.
- `@briefing max:N` → N resultaten per bron.

Filterinstructie (override via `filter:"..."`):
```
Filter op relevantie voor het vakgebied en de doelgroep van de gebruiker, af te leiden uit de keywords in bestand 10 en de projectcontext (About Me, pillars). Gebruik geen vaste niche en verzin geen relevantiecriteria die niet uit die context volgen. Is er geen eigen filter: vraag de gebruiker waarop gefilterd moet worden.
```

## Output

Schrijf de briefing naar:
```
Output/Inspiratie-staging/briefing-JJJJ-MM-DD.md
```
Maak de map `Output/Inspiratie-staging/` aan als die niet bestaat. Bestaat het dagbestand al, append in plaats van overschrijven. Toon de briefing daarnaast in de chat en sluit af met "Draai @ideate om dit te verwerken."

Format van het staging-bestand:
```
# Briefing — JJJJ-MM-DD
Keywords: [...]
Bronnen: [...]

## Top inzichten
[2 tot 3 zinnen over de belangrijkste trend]

## Items
[per item: titel | bron/auteur | datum | 2 tot 3 zinnen | actie-inzicht | link]
```

## Procedure

1. Bepaal keywords, bronnen, max-resultaten en filterinstructie uit bestand 10 plus chat-overrides. Standaard: alle vier bronnen, 5 resultaten per bron. Bepaal daarnaast de laatste-run-datum uit de datum in de bestandsnaam van het meest recente eerdere `briefing-*.md` in `Output/Inspiratie-staging/` (ook `*-verwerkt.md`). Geen eerder bestand: gebruik alleen het 7-daagse venster.
2. Haal per bron op (één call per keyword-set per bron):
   - Substack: `easyapi/substack-posts-scraper` → `{ "keywords": [...], "maxItems": 10 }`. Velden: `title`, `subtitle`, `description`, `truncated_body_text`, `canonical_url`, `post_date`, `publishedBylines[0]`, `reaction_count`, `comment_count`.
   - YouTube: `grow_media/youtube-search-api`, één call per keyword → `{ "q": "<term>", "maxResults": 5, "order": "date", "publishedAfter": "<7 dagen geleden, RFC 3339>", "relevanceLanguage": "en" }`. Velden: `title`, `text`, `url`, `channelName`, `date`, `viewCount`, `likes`, `duration`.
   - LinkedIn: `apimaestro/linkedin-posts-search-scraper-no-cookies` → `{ "keyword": "<term>", "totalPostsToScrape": 5, "sort_by": "date_posted" }`. Velden: `text`, `post_url`, `author`, `stats`, `posted_at.date`, `hashtags`.
   - Google News: `web_fetch` op `https://news.google.com/rss/search?q=<term>&hl=en&gl=US&ceid=US:en`, parse `<title>`, `<link>`, `<pubDate>`, `<source>`. Alleen voor de 2 tot 3 meest relevante keywords.
3. Filter items met een publicatiedatum op of vóór de laatste-run-datum weg. Dedupliceer, scoor elk resterend item tegen de filterinstructie, prioriteer op laatste 7 dagen, sorteer op relevantie en datum, selecteer top 10 tot 15.
4. Stel de briefing samen, schrijf naar het staging-bestand, toon in de chat en sluit af met de @ideate-verwijzing.

## Foutafhandeling

- Bestand 10 ontbreekt of is leeg: meld het en vraag om keywords. Draai niet door op verzonnen termen.
- Een actor of de RSS-feed faalt: ga door met de overige bronnen, meld welke ontbreekt.
- Weinig resultaten: stel voor de keywords te verbreden. Te veel: verscherp de filterinstructie.
