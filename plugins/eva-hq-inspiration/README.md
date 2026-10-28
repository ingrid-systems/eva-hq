# eva-hq-inspiration

Inspiration & Ideas (Catch All) van EVA HQ. Eén skill: de email-processor. Mails die je in Gmail een label onder `CLAUDE/` geeft, zet @mail in de Notion-database met dezelfde naam, met jouw intentie in de Notities.

| Skill | Aanroep | Wat het doet |
|---|---|---|
| email-processor | `@mail` of `@email` | Haalt alle mails met een CLAUDE-label op en zet ze in de database die bij het label hoort |

## Wat je nodig hebt

- Claude met Notion en Gmail gekoppeld.
- Je CLAUDE-labels in Gmail (les 'Gmail voorbereiden'). Geef een label dezelfde naam als de database: `CLAUDE/Ideas`, `CLAUDE/Links`, `CLAUDE/Stories`, `CLAUDE/Content`.
- De save-skill, geïnstalleerd en ingericht als losse skill (de les "Skill inrichten: Save All (@save)"). Die bevat de link van je Business HQ-pagina; @mail leest die regel en vindt daar je databases. Zonder save-skill doet @mail niets.

## Waarom de save-skill niet in deze plugin zit

De save-skill is de enige skill met iets persoonlijks erin (de link van je Business HQ-pagina). Een plugin wordt bij elke update overschreven, dus die ene regel zou dan verdwijnen. Daarom blijft save een losse download en zit alles wat universeel is in de plugins.

## Let op bij overstappen

Had je de email-processor al als losse skill geïnstalleerd (de versie met Data Source ID's)? Verwijder die losse versie na het installeren van deze plugin, anders staat @mail twee keer in je lijst. Je labels en databases blijven zoals ze zijn.
