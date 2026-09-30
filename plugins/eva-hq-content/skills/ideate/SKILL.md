---
name: ideate
description: De brug tussen verzamelen en creëren. Pakt wat @briefing en @inspire naar staging hebben geschreven, synthetiseert daar content-ideeën uit, presenteert een aanbevolen voorselectie (top 3 tot 5 op volgorde, met één aanrader), laat de gebruiker selecteren, stelt per gekozen idee een intentie voor, haalt op gekozen creator-items de volledige copy op, en zet alleen de selectie in de inspiratiedatabase. TRIGGERS op @ideate, "ideate", "verwerk mijn inspiratie", "wat heb je gevonden", "presenteer de inspiratie", "content-ideeën maken", "wat kan ik hiermee", of elke vraag om de verzamelde briefing- en inspire-input om te zetten naar bruikbare ideeën.
version: "1.2"
---

# ideate

Convergentieskill. Leest de onverwerkte staging-bestanden van `briefing` en `inspire`, synthetiseert content-ideeën, rangschikt ze intern op sterkte, presenteert een aanbevolen voorselectie, laat de gebruiker kiezen, stelt per gekozen idee een intentie voor, verrijkt gekozen creator-items met de volledige copy en schrijft alleen de selectie naar de inspiratiedatabase. Markeert verwerkte staging-bestanden.

## Inputs

1. Staging: alle bestanden in `Output/Inspiratie-staging/` die NIET op `-verwerkt.md` eindigen (`briefing-*.md` en `inspire-*.md`). Sla `*-verwerkt.md` over. Is er geen onverwerkt bestand: meld dit en stel voor eerst `briefing` of `inspire` te draaien. Stop daarna.
2. Context (lezen, niet wijzigen), om ideeën af te stemmen:
   - het topic-pillars document in `Resources/Input/`
   - het villain/anti-visie document in `Resources/Input/`
   - de Brand Voice Guide uit About Me

## Procedure

1. Lees alle onverwerkte staging-bestanden. Registreer per item de bron (creator + post, of briefing-topic) en de link voor provenance.
2. Synthetiseer content-ideeën door de staging-input te kruisen met de topic-pillars, de villain en de Brand Voice. Combineer, herhaal niet: bijvoorbeeld een trending creator-hook toegepast op een briefing-topic onder de passende pillar. Rangschik daarna alle gesynthetiseerde ideeën intern op sterkte. Wegingsvolgorde: fit met een pillar en met de villain/anti-visie, kracht en specificiteit van de hook (micro-specifiek boven algemeen), en spreiding over de pillars zodat de top niet uit één rol bestaat. Dit rangschikken doe je stil; de gebruiker ziet alleen de uitkomst.
3. Presenteer een voorselectie, geen platte lijst. Toon de sterkste 3 tot 5 ideeën op volgorde, met het bovenste idee expliciet als aanrader ("deze raad ik aan, want ..."). Presenteer er nooit meer dan 5 in één keer, ook niet bij een grote staging-batch; de rest houd je achter de hand. Sluit af met de vraag of de gebruiker de overige ideeën ook wil zien (en hoeveel dat er zijn). Per gepresenteerd idee:
   - idee (1 regel)
   - pillar
   - bron + link
   - hook/angle
   - `copy nog ophalen: ja/nee` (ja bij een creator-reel of -carrousel waarvan de volledige copy nog in de video of slides zit)
4. Wacht op de selectie van de gebruiker. Sla niets op zonder expliciete selectie. Vraagt de gebruiker om de rest, toon dan de volgende 3 tot 5 op dezelfde manier.
5. Intentie bepalen. Stel per geselecteerd idee een intentie van één regel voor: wat de post moet bereiken, afgeleid uit de pillar, het CTA-niveau en de angle (bijvoorbeeld awareness opbouwen, een comment-trigger naar een weggever, of een CTA naar een aanbod). Leg de voorgestelde intenties in één keer voor en vraag of ze kloppen of aangepast moeten worden. Pas aan op basis van de reactie. Schrijf niets weg vóór deze bevestiging.
6. Diepe fase, alleen op geselecteerde items met `copy nog ophalen: ja`:
   - Instagram reel: `apify/instagram-reel-scraper` met `{ "username": ["<schone reel-URL>"], "resultsLimit": 1, "includeTranscript": true }`. Strip query-params als `?igsh=` uit de URL.
   - Instagram carrousel: `zen-studio/google-lens-ocr` per slide (`childPosts[n].displayUrl`), met `outputDetail: "text_only"`.
   - LinkedIn: de volledige posttekst zit al in de staging, geen extra call.
   - YouTube: gebruik standaard de omschrijving. Wil de gebruiker de volledige uitwerking, haal dan het transcript via `streamers/youtube-scraper` met `{ "startUrls": [{ "url": "<video-URL>" }], "maxResults": 1, "downloadSubtitles": true, "subtitlesFormat": "plaintext" }`.
   Meld vooraf het aantal items en de geschatte kosten (transcripts rekenen per minuut) en wacht op akkoord. Voer de diepe fase uit in een subagent.
7. Schrijf elk geselecteerd item naar de inspiratiedatabase via de `save`-skill (route: Inspiratie). Vul het Notities-veld in het vaste format: de kern van het idee op de eerste regel, dan een lege regel, dan `INTENTIE: <de bevestigde intentie uit stap 5>`. Voeg daaronder de ideate-metadata toe: pillar, hook/angle, format, bron + link, en de opgehaalde copy indien de diepe fase is gedraaid.
8. Markeren als verwerkt: hernoem een staging-bestand naar `...-verwerkt.md` (bijvoorbeeld `inspire-2026-06-28.md` → `inspire-2026-06-28-verwerkt.md`) pas NADAT de gebruiker bevestigt dat het bestand volledig is doorgenomen. Een bestand is pas volledig doorgenomen als ook de achtergehouden ideeën zijn getoond en afgehandeld. Bij een gedeeltelijk doorgenomen bestand: laat het ongewijzigd staan. Bij twijfel: vraag het.
9. Vat af: aantal gepresenteerde ideeën, hoeveel er nog achter de hand zijn, aantal opgeslagen, welke bestanden als verwerkt gemarkeerd, en wat nog onverwerkt openstaat.

## Foutafhandeling

- Geen onverwerkte staging: meld het, stel voor eerst `briefing` of `inspire` te draaien, stop.
- Een diepe-fase-call faalt: sla het idee alsnog op met de bestaande hook en markeer in het database-item "copy nog ophalen".
- Ontbrekende context (geen pillars of Brand Voice): ga door, maar meld dat de ideeën algemener zijn en dat de rangschikking dus grover is.
- Gebruiker bevestigt geen intentie of slaat het over: schrijf de kern zonder INTENTIE weg en meld dat het idee zonder intentie is opgeslagen.
- Markeer nooit een bestand als verwerkt zonder expliciete bevestiging van de gebruiker.
