# Dagstart en weekreview

Twee korte rituelen rond de weekplanning. De dagstart maakt de dag klein genoeg om te beginnen; de weekreview sluit de week af en voedt de volgende weekplanning. Allebei leggen ze voor en schrijven ze pas na akkoord. In de situatie zonder klant (geplande taak) zet je het resultaat klaar als bericht en schrijf je niets.

## Dagstart (`@planning dag`)

### Eerst licht oogsten

Door de week komen er taken bij: een to-do via @save, een mail met het label, de actiepunten uit een call van vanochtend. Die zijn vaak voor vandaag of morgen, dus de dagstart wacht daar niet mee tot de weekplanning. `@planning dag` mag daarom op elk moment van de dag draaien, ook een tweede keer na een call. Oogst alleen wat nieuw is sinds de vorige aanroep van `@planning dag` of `@planning week` (datum en tijd staan in MEMORY.md van de werkmap; is er geen regel, neem dan gisteren), met dezelfde bronnen als stap 1 van de weekplanning (alleen die sectie 6 noemt):

- Mail: zijn er mails met het label `CLAUDE/Taken`, laad de `email-processor`-skill en verwerk alleen dat label.
- Bronarchief: per ja-reeks de calls zonder regel "taken" in het oogst-log; per call de eigen actiepunten van de klant voorstellen, na akkoord naar de Inbox met het transcript als Bron en een regel in het oogst-log. In de situatie zonder klant: voorstellen klaarzetten, niets wegschrijven.
- Externe takenbron: alleen de open taken met datum vandaag lezen, om ze in de dag te tonen.

### Wat je leest

- Taken met Do-datum vandaag en Status Gepland of Bezig (via de weergave Deze week of Planbord · gepland).
- Open taken met een Do-datum vóór vandaag, Status Gepland of Bezig: de blijvers (via Planbord · blijven liggen als die weergave bestaat).
- Taken in de Inbox die zijn aangemaakt na de laatste weekplanning (de datum staat in MEMORY.md van de werkmap; created_time van Notion): de nieuwkomers. Wat ouder is blijft voor de weekplanning, anders herkauwt de dagstart elke ochtend de hele Inbox. Nieuwkomers die bij een eerdere dagstart vandaag al zijn voorgelegd en bewust zijn blijven liggen, sla je over.
- De weekindelingsagenda en de werk-agenda's voor vandaag: welke blokken er zijn, welke afspraken erin vallen, hoeveel ruimte er dus echt is. Draai je later op de dag, dan tellen alleen de blokken die nog komen; wat voorbij is heeft geen ruimte meer.
- planning.md sectie 3 (grens klein taakje, maximale vulling) en sectie 4 (voorkeuren).

### Wat je voorlegt

Eén bericht, in deze volgorde:

1. **Het anker.** De ene taak die vandaag af moet. Kies hem op Deadline (vandaag of morgen eerst), daarna op wat sectie 4 belangrijk noemt, daarna de grootste Focus-taak van de dag. Eén zin waarom. De klant kan een andere kiezen; dan is dat het anker. Is er vandaag al een dagstart geweest (regel in MEMORY.md), dan blijft dat anker staan en noem je het alleen; de dag zelf leg je dan niet opnieuw voor, alleen blijvers en nieuwkomers.
2. **De dag.** Per blok (alleen Focus en Losse taakjes) de taken die erin staan met hun Duur, en de vulling in procenten. Afspraken uit de werk-agenda's als context ertussen, zodat de klant ziet waar de dag strak is.
3. **Blijven liggen.** Per blijver één voorstel voor een nieuwe plek, met de reden erbij:
   - vandaag, in een blok dat nog ruimte heeft en bij de Duur past;
   - een andere dag deze week, als vandaag vol is of de taak een Deadline later deze week heeft;
   - terug naar de Inbox, als er deze week geen plek is en er geen Deadline dringt.
   Geen teller hoe vaak een taak al is doorgeschoven, geen waarschuwing. Een blijver is informatie over de week, geen oordeel over de klant. Een blijver met een Deadline die vandaag of gisteren was, zet je bovenaan met alleen de vraag wat de klant ermee wil.
4. **Nieuw in de Inbox.** Per nieuwkomer één voorstel, Deadline deze week eerst: vandaag in een blok dat nog ruimte heeft en bij de Duur past; morgen; een andere dag deze week; of laten liggen tot de weekplanning. Kleine taakjes (onder de grens uit sectie 3) stel je voor in het eerstvolgende Losse-taakjes-blok. Is het geen taak maar een idee, zeg dat; de klant beslist, jij zet hem dan via de save-skill in Ideas & Decisions en de taak op Vervallen. Meer dan tien nieuwkomers: stel voor ze nu kort door te lopen of de weekplanning naar voren te halen.
5. **Rest van de Inbox.** Eén regel: "n oudere taken in de Inbox, die komen bij de weekplanning aan bod." 

### Wat je schrijft na akkoord

Alleen de Do-datum van de blijvers die de klant verschuift, Status Inbox voor wat terug naar de Inbox gaat (de Do-datum maak je dan leeg), en Do-datum plus Status Gepland voor de nieuwkomers die de klant inplant. Wat de klant laat liggen blijft Inbox. Zet in MEMORY.md van de werkmap één regel met datum, tijd en het anker ("dagstart 2026-11-03 09:10 · anker: offerte Jansen"); de volgende aanroep rekent vanaf dat moment. Het anker krijgt geen apart veld: het staat in het bericht. Niets schuift vanzelf door; een blijver waar de klant niets over zegt, blijft staan waar hij staat en komt morgen opnieuw in Blijven liggen. Buiten de dagstart om blijft het planbord de snelle route voor een losse taak: de Inbox daar heeft Vandaag, Kies dag en Weg.

## Weekreview (`@planning review`)

Aan het eind van de week, vóór de volgende weekplanning. Doel: afsluiten wat af is, opruimen wat blijft hangen, en zien wat er de komende twee weken aankomt. De review is geen beoordeling van de klant; het is de plek waar taken die steeds blijven liggen één keer een eerlijke vraag krijgen.

### Wat je leest

- Taken met Status Klaar en een Do-datum in de afgelopen week: wat gedaan is.
- Taken met Status Gepland of Bezig en een Do-datum in de afgelopen week: wat niet gedaan is.
- Taken met Status Wacht: met het veld Wacht op.
- Open taken (Inbox, Gepland, Bezig) die langer open staan dan de horizon in weken uit planning.md sectie 3, gerekend met de created_time van Notion: de lang-openstaanders. Staat er in sectie 4 een eigen termijn voor, gebruik die. Verzin geen eigen termijn.
- Deadlines in de komende twee weken (Planbord · deadlines, of een query op Deadline).
- De werk-agenda's voor de komende twee weken, per agenda per week: wat komt eraan, en horen daar draaiboekstappen bij die nog niet zijn uitgerold (dan doet de volgende weekplanning dat, zeg het alleen).

### Wat je voorlegt

Eén bericht, in deze volgorde:

1. **Afgelopen week.** In twee regels: hoeveel gepland, hoeveel op Klaar, hoeveel niet. Daarna de niet-gedane taken als lijst met per taak een voorstel: mee naar de volgende week, terug naar de Inbox, of Vervallen. Draaiboekstappen van een item dat al voorbij is: Vervallen, tenzij het een stap achteraf is.
2. **Wacht op.** Per taak de naam en op wie of wat hij wacht, met de vraag of daar deze week iets mee moet (een herinnering sturen is dan een nieuwe taak in de Inbox) of dat hij nog even blijft wachten.
3. **Blijft al lang open.** Per lang-openstaander de vraag, zonder omhaal: nog steeds belangrijk? Ja: deze week inplannen (de weekplanning pakt hem op). Nee: Vervallen. Noem de aanmaakdatum, niet het aantal keren dat hij is verschoven.
4. **Komende twee weken.** De Deadlines met hun datum, de afspraken die opvallen (meerdaags, buiten de deur, met een draaiboek), en of de weekindeling van die weken afwijkt van normaal (een Niet werken-blok dat ontbreekt of extra is). Eén regel per punt; dit is kijken, niet plannen.

### Wat je schrijft na akkoord

Statuswijzigingen (Vervallen, Inbox, Wacht) en Do-datums voor wat mee naar volgende week gaat. Nieuwe taken uit de Wacht op-stap gaan naar de Inbox. Zet in MEMORY.md van de werkmap één regel met datum: "weekreview week X gedaan, n klaar van m, k vervallen". Ga daarna door met `@planning week` voor de komende week, tenzij de klant dat op het vaste moment in het weekplanningsblok wil doen.
