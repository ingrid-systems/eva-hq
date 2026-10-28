# eva-hq-data

Data-analyse van EVA HQ. Eén skill: de utm-skill. Met @utm bouw je een getagde link volgens vaste naamregels en zet je hem direct in je linkregister in Notion.

| Skill | Aanroep | Wat het doet |
|---|---|---|
| utm | `@utm` | Vraagt bestemming, kanaal en campagne, bouwt de UTM-link en slaat hem op als regel in UTM Links. Bestaat de link al, dan krijg je die terug |

## Wat je nodig hebt

- Claude met Notion gekoppeld.
- De save-skill, geïnstalleerd en ingericht als losse skill (de les "Skill inrichten: Save All (@save)"). Die bevat de link van je Business HQ-pagina; @utm leest die regel en vindt daar je database. Zonder save-skill doet @utm niets.
- De database "UTM Links" op je Business HQ-pagina, met een uitlegregel eronder die met de naam begint. Die zet je op in de les "Werken met UTM-links".

## Waarom de save-skill niet in deze plugin zit

De save-skill is de enige skill met iets persoonlijks erin (de link van je Business HQ-pagina). Een plugin wordt bij elke update overschreven, dus die ene regel zou dan verdwijnen. Daarom blijft save een losse download en zit alles wat universeel is in de plugins.

## Let op bij overstappen

Had je de utm-skill al als losse skill geïnstalleerd? Verwijder die losse versie na het installeren van deze plugin, anders staat @utm twee keer in je lijst. Je database blijft zoals hij is; zorg alleen dat hij op je Business HQ-pagina staat met een uitlegregel eronder (zie de les "Werken met UTM-links").
