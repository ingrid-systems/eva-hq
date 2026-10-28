# eva-hq-planning

De Personal Assistant van EVA HQ, module Planning. Eén skill, drie aanroepen.

| Aanroep | Wanneer | Wat het doet |
|---|---|---|
| `@planning week` | In je weekplanningsblok | Oogst je bronnen, rolt terugkerende taken en draaiboeken uit, ruimt je Inbox op en vult je week in je Focus- en Losse-taakjes-blokken. Legt voor, schrijft na akkoord. |
| `@planning dag` | Bij je dagstart | Het anker van de dag, je taken per blok, en een nieuwe plek voor wat blijft liggen. |
| `@planning review` | Aan het eind van de week | Wat af is en wat niet, wat wacht, wat al lang open staat, en wat er de komende twee weken aankomt. |

## Wat je nodig hebt

- De drie voorbereidingslessen van de module Planning gedaan: de werkmap Operations/Planning met `Resources/Fundament/planning.md`, de database Taken op je Business HQ-pagina, en je weekindeling als blokken in een eigen Google-agenda.
- De les "Je bronnen aansluiten" gedaan: sectie 6 Bronnen in planning.md.
- De save-skill, geïnstalleerd als losse skill (de les "Skill inrichten: Save All (@save)"). Die bevat de link van je Business HQ-pagina; @planning vindt je Taken-database via die ene link.
- De koppelingen: Notion en Google Agenda. Gmail als je mails als taak binnenhaalt; de connector van je eigen planningstool als je die in de les "Externe bronnen aansluiten" hebt aangesloten.

## Wat de skill niet doet

- Geen afspraken per taak in je agenda: taken krijgen een Do-datum in Notion, je blokken staan in je weekindelingsagenda.
- Niets schrijven in een andere planningstool: die taken worden alleen getoond.
- Geen contentplanning: dat doet `@planner` uit eva-hq-content.
