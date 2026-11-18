# eva-hq-assistants

Plugin van EVA HQ voor de module HQ Assistants. Assistenten voeren uit voor elke medewerker die ze oproept en schrijven het resultaat na akkoord weg op de plek waar het hoort.

## Skills

- `salespage-assistant` (@salespage, @loes): LOES AI B*tch, de Ideale Salespagina Generator van Marloes, ingericht voor het EVA-systeem. Leest de Offer Snapshot van je aanbod, je Ideale Klant Profiel en je Brand Voice Guide uit About Me en stelt alleen nog de vragen die daar niet in staan. Schrijft de pagina blok voor blok in een document in de chat en zet die na akkoord in de map van je aanbod.
- `funnel-assistant` (@funnel): schrijft de mailreeks van een funnel voor je aanbod, mail voor mail. Volgt een funnelblauwdruk uit `Operations/Marketing/Resources/Funnels/` (welke mails, in welke volgorde, met welke functie en in welk ritme) en haalt de inhoud en de stem uit About Me, je klanttaal, je Story Vault en je eigen e-mails. Legt elke mail voor en slaat de reeks na akkoord op in de map van je aanbod. Zijn de mails aangepast, dan haalt hij je definitieve versies terug; die worden de voorbeelden voor de volgende reeks.

## Koppeling

Bij installatie wordt de koppeling met de server van Marloes meegeleverd (`.mcp.json`). Die koppeling levert per blok de vragen en schrijfinstructies; zonder die koppeling kan de assistent de blokken niet schrijven. Activeer hem na het installeren via de tab Connectors van de plugin; de klikstappen staan in het stappenplan op de pagina van je programma.
