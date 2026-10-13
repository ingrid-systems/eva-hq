# EVA HQ

Deze repo is de plugin-marketplace voor EVA HQ van Ingrid Staal. Je voegt de marketplace één keer toe, daarna installeer je per module de plugin die je nodig hebt en komen updates automatisch mee.

| Plugin | Module | Wat het is |
|---|---|---|
| `eva-hq-inspiration` | Module 1 | Inspiration & Ideas (Catch All): de email-processor (@mail), van Gmail-label naar Notion-database. |
| `eva-hq-content` | Module 2 | Het contentsysteem: briefing, inspire, ideate, content-creator, repurpose en planner. |
| `eva-hq-data` | Module 3 | Data-analyse: de utm-skill (@utm), getagde links in je linkregister UTM Links. |

## Installeren (Claude chat, Claude Desktop, Cowork)

1. Open **Customize** in de linker sidebar en ga naar de tab **Plugins**.
2. Klik bij **Personal plugins** op de "+" en kies **Add marketplace**.
3. Kies **Add from a repository** en plak: `https://github.com/ingrid-systems/eva-hq`
4. Klik op **Install** bij de plugin van de module waar je mee bezig bent (`eva-hq-inspiration` voor Module 1, `eva-hq-content` voor Module 2, `eva-hq-data` voor Module 3).

In Cowork open je eerst de Cowork-tab en daarna Customize.

## Installeren (Claude Code)

```
/plugin marketplace add ingrid-systems/eva-hq
/plugin install eva-hq-inspiration@ingrid-staal-eva-hq
/plugin install eva-hq-content@ingrid-staal-eva-hq
/plugin install eva-hq-data@ingrid-staal-eva-hq
```

## Gebruiken

De skills reageren op hun eigen aanroep: `@mail` (Module 1), `@briefing`, `@inspire`, `@ideate`, `@planner`, `@content`, `@repurpose` (Module 2), `@utm` (Module 3). Wat elke skill doet en wat je ervoor nodig hebt staat in de README van de plugin (`plugins/<plugin>/README.md`). De save-skill uit Module 1 blijft een losse skill met jouw eigen link erin; die zit bewust in geen enkele plugin, omdat een plugin-update die link zou overschrijven.

## Een update publiceren

1. Pas het SKILL.md-bestand aan in `plugins/<plugin>/skills/<skill-naam>/`.
2. Hoog het versienummer op in **twee** bestanden, met dezelfde waarde:
   - `plugins/<plugin>/.claude-plugin/plugin.json`
   - `.claude-plugin/marketplace.json` (het `version`-veld bij de plugin-entry)
3. Commit en push.

Iedereen die de marketplace heeft toegevoegd, krijgt de nieuwe versie zonder iets te downloaden of opnieuw te uploaden.
