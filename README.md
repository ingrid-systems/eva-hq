# EVA HQ

Deze repo is de plugin-marketplace voor EVA HQ van Ingrid Staal. Je voegt de marketplace één keer toe, daarna voeg je per module de plugin toe die je nodig hebt en komen updates automatisch mee.

| Plugin | Onderdeel van EVA HQ | Wat het is |
|---|---|---|
| `eva-hq-inspiration` | Inspiration & Ideas (Catch All) | De email-processor (@mail), van Gmail-label naar Notion-database. |
| `eva-hq-content` | Content Creation | Het contentsysteem: briefing, inspire, ideate, content-creator, repurpose en planner. |
| `eva-hq-planning` | Planning (agenda en to-do's) | De Personal Assistant (@planning): weekplanning, dagstart en weekreview vanuit Taken in Notion en je Google-agenda's. |
| `eva-hq-data` | Data-analyse | De utm-skill (@utm), getagde links in je linkregister UTM Links. |

## Toevoegen (claude.ai en de Claude-app)

1. Open **Customize** in de linker zijbalk en ga naar de tab **Plugins**.
2. Klik rechtsboven op **+ Add** en kies **Add marketplace**.
3. Kies **Add from a repository**, plak `https://github.com/ingrid-systems/eva-hq` in het veld URL en klik op **Sync**.
4. De plugins staan nu onder **New plugins** op de Plugins-pagina (of zoek op de naam; ze staan dan onder Yours met het label Disabled). Klik op de plugin van de module waar je mee bezig bent en klik rechtsboven op **Add**. Elke plugin voeg je apart toe.

## Installeren (Claude Code)

```
/plugin marketplace add ingrid-systems/eva-hq
/plugin install eva-hq-inspiration@ingrid-staal-eva-hq
/plugin install eva-hq-content@ingrid-staal-eva-hq
/plugin install eva-hq-planning@ingrid-staal-eva-hq
/plugin install eva-hq-data@ingrid-staal-eva-hq
```

## Gebruiken

De skills reageren op hun eigen aanroep: `@mail`, `@briefing`, `@inspire`, `@ideate`, `@planner`, `@content`, `@repurpose`, `@planning`, `@utm`. Wat elke skill doet en wat je ervoor nodig hebt staat in de README van de plugin (`plugins/<plugin>/README.md`). De save-skill blijft een losse skill met jouw eigen link erin; die zit bewust in geen enkele plugin, omdat een plugin-update die link zou overschrijven.

## Een update publiceren

1. Pas het SKILL.md-bestand aan in `plugins/<plugin>/skills/<skill-naam>/`.
2. Hoog het versienummer op in **twee** bestanden, met dezelfde waarde:
   - `plugins/<plugin>/.claude-plugin/plugin.json`
   - `.claude-plugin/marketplace.json` (het `version`-veld bij de plugin-entry)
3. Commit en push.

Iedereen die de marketplace heeft toegevoegd, krijgt de nieuwe versie zonder iets te downloaden of opnieuw te uploaden.
