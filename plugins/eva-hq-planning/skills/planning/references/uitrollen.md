# Uitrollen: van sjabloon naar echte taak

Hoort bij stap 2 van `@planning week`. Sjablonen staan in Taken met Soort Terugkerend of Draaiboekstap (ingericht in de les "Wat terugkomt"). Ze zijn zelf nooit een taak die gedaan wordt: ze beschrijven wanneer er een taak moet ontstaan. Deze stap maakt die taken aan voor de gekozen week, en voor draaiboeken over de horizon uit planning.md sectie 3.

## Wat een uitgerolde taak meekrijgt

- Naam: de Naam van het sjabloon (bij een draaiboekstap: de stapnaam plus de titel van het agenda-item, bijvoorbeeld "Bevestiging sturen · Intake Marieke")
- Soort: Los
- Do-datum: de berekende datum (zie hieronder)
- Status: Gepland. De datum volgt uit een regel die de klant zelf heeft vastgelegd, dus hier is geen akkoord voor nodig; in stap 4 kan de taak na akkoord nog binnen de week schuiven
- Duur, Energie: overnemen van het sjabloon
- Deadline: bij een draaiboekstap vooraf de datum van het agenda-item; bij een terugkerende taak alleen als het sjabloon een Deadline-regel heeft
- Sjabloon: relatie naar het sjabloon
- Agenda-item: de link van het agenda-item (alleen draaiboekstappen)
- Naam begint met "Check:" als Loopt automatisch aan staat

## Terugkerende taken

Lees de weergave "Terugkerend en draaiboeken" en pak de sjablonen met Soort Terugkerend. Per sjabloon bepaal je of hij in de gekozen week valt:

| Eenheid | Valt in de week als |
|---|---|
| Dag | elke werkdag van de week (of de Dagen die zijn aangevinkt); Interval 2 = om de dag |
| Week | de week past in het Interval, gerekend vanaf de laatst uitgerolde taak van dit sjabloon (of vanaf deze week als er nog geen is); op de aangevinkte Dagen, anders de eerste werkdag |
| Maand | de dag van de maand die het sjabloon noemt valt in deze week; zonder dag: de eerste werkdag van de maand; Interval 3 = elk kwartaal |
| Laatste werkdag van de maand | de laatste werkdag van de maand valt in deze week |
| Jaar | de datum van het sjabloon valt in deze week |

"Laatst uitgerolde taak" vind je via de relatie Sjabloon: zoek de taken die naar dit sjabloon wijzen en pak de hoogste Do-datum. Staat er al een taak met deze relatie en een Do-datum in de gekozen week, dan is het sjabloon al uitgerold: overslaan en niet melden, behalve in het telwerk aan het eind ("4 terugkerende taken, waarvan 1 stond er al").

Kun je uit Interval, Eenheid en Dagen niet afleiden wanneer het sjabloon valt (velden leeg of tegenstrijdig), rol hem dan niet uit en noem hem in het overzicht met de vraag wat de bedoeling is. Gokken levert een taak op die niemand herkent.

## Draaiboeken

Pak de sjablonen met Soort Draaiboekstap en groepeer ze op het veld Draaiboek (bijvoorbeeld "Klantafspraak", "Workshop"). Lees dan de werk-agenda's (plannen: ja) over de horizon uit sectie 3, week voor week, en herken de agenda-items: het woord uit het veld Draaiboek komt voor in de titel, en als het sjabloon ook een agenda noemt, staat het item in die agenda.

Per herkend agenda-item en per stap:

- Offset lezen zoals de klant hem schreef: "1 dag vooraf", "2 weken achteraf", "1 uur vooraf". Vooraf telt terug vanaf de datum van het item, achteraf telt vooruit. Een offset in uren valt op de dag van het item zelf.
- Valt de berekende Do-datum op een dag met soort Niet werken (volgens de weekindelingsagenda) of in het weekend, schuif dan naar de dichtstbijzijnde werkdag ervoor (bij vooraf) of erna (bij achteraf).
- Bestaat er al een taak met relatie Sjabloon naar deze stap én Agenda-item naar dit item, dan is hij al uitgerold: overslaan.
- Items die al voorbij zijn op het moment van uitrollen: alleen stappen achteraf die nog in de toekomst of in de gekozen week vallen.

Draaiboekstappen worden uitgerold over de hele horizon, niet alleen voor de gekozen week, zodat een stap "2 weken vooraf" niet pas opduikt als het te laat is. In stap 4 van de weekplanning komen alleen de stappen met een Do-datum in de gekozen week aan bod.

Herken je in de agenda een item dat op een draaiboek lijkt maar niet matcht (zelfde soort afspraak, ander woord in de titel), noem het één keer in het overzicht met de vraag of het draaiboek erbij hoort. Leg dat niet zelf vast; het veld Draaiboek is van de klant.

## Overzicht aan het eind van stap 2

Eén kort blok:
- Terugkerend: n taken uitgerold voor week X (en hoeveel er al stonden)
- Draaiboeken: per draaiboek het aantal herkende agenda-items en het aantal stappen dat is aangemaakt, met de verste datum
- Checks: n (stappen met Loopt automatisch)
- Vragen: sjablonen die je niet kon plaatsen, agenda-items die misschien bij een draaiboek horen

Met de klant erbij wacht je hier niet apart op akkoord: uitrollen is het uitvoeren van afspraken die de klant in de les "Wat terugkomt" al heeft bevestigd. De vragen neem je mee naar het voorstel van stap 4.
