---
name: email-processor
description: Verwerkt doorgestuurde emails vanuit CLAUDE/* Gmail labels. TRIGGERT op @email of @mail. Routeert content naar de juiste Notion database op basis van het Gmail label. Kent geen database-ID's: haalt de databases van de systeempagina via de regel Systeempagina in de save-skill. Vereist de save-skill uit Module 1.
---

# Email Processor

Verwerkt emails die de gebruiker heeft gelabeld met CLAUDE/* labels in Gmail.

## De databases vinden

Deze skill kent geen database-ID's en zoekt zelf niets op in Notion. De `save`-skill is de ene bron voor de Notion-databases. Doe dit aan het begin van elke @mail, vóór het routeren:

1. Laad de `save`-skill (zoals `@planner` en `@ideate` doen) en lees daar de regel `Systeempagina:`.
2. Fetch die pagina en bepaal welke database welke is volgens de matchregel van de save-skill, sectie "Finding the databases". Pak per label de database uit de tabel hieronder. Let op: de naam staat in de uitlegregel onder het databaseblok, niet in de bloktitel (die kan afwijken, bijvoorbeeld "EVA Vault" boven de regel "Inspiratie").
3. Fetch vóór het aanmaken de `collection://` data source van de doeldatabase en volg het schema: de exacte veldnamen, en de select- en status-opties precies zoals het schema ze spelt.

Hier staat bewust geen kopie van die matchregel. Verandert de regel of de systeempagina, dan verandert dat op één plek in de save-skill en volgt deze skill automatisch mee.

## Gmail Label → Notion Database Routing

| Gmail Label | Notion Database |
|-------------|-----------------|
| CLAUDE/Content, en elk label dat met "Content" begint (CLAUDE/Content Marketing, CLAUDE/Content Courses, CLAUDE/Content-Marketing) | Inspiratie (of Inspiration) |
| CLAUDE/Ideas | Ideeën & Beslissingen (of Ideas & Decisions) |
| CLAUDE/Links | Praktische Links |
| CLAUDE/Stories | Story Vault |

**Content-labels gaan altijd naar Inspiratie, nooit naar de Content-database.** Content is een output-database: daar komt pas iets in als de gebruiker zelf content heeft gemaakt. Binnenkomende mail gaat naar Inspiratie. Staat er na "Content" nog een woord in het label (Marketing, Courses, Training), gebruik dat dan als hint voor het veld Bestemming: kies de optie uit het schema die daar het dichtst bij ligt (Marketing naar de marketing-optie, Courses naar de training-optie). Staat er niets, bepaal Bestemming dan uit de inhoud.

**Overslaan.** Verwerk nooit mails met het label `CLAUDE/VERWERKT` (daar staan de al verwerkte mails) of `CLAUDE/DEMO` (alleen via @demo). Het bovenliggende label `CLAUDE` zelf is geen routing. Zoek per label, niet op "alles onder CLAUDE".

**Andere CLAUDE-labels.** Elk ander label onder `CLAUDE/` routeert naar de database op de systeempagina waarvan de naam overeenkomt met het deel na de slash, volgens de matchregel hierboven: `CLAUDE/Prompts` naar een database "Prompt Bibliotheek" of "Prompts", `CLAUDE/Klanten` naar "Klantnotities". Is er geen match, vraag dan één keer welke database de gebruiker bedoelt en gebruik die in deze run. De skill hoeft dus niet aangepast te worden als er een label of database bijkomt.

## Workflow

1. Gebruiker stuurt email door naar zichzelf met CLAUDE/* label
2. Gebruiker triggert met `@email` of `@mail`
3. Claude bepaalt de databases via de systeempagina (zie hierboven)
4. Claude haalt de labellijst op, pakt elk label onder `CLAUDE/` behalve VERWERKT en DEMO, en doorzoekt Gmail per label met `label:` plus de labelnaam, waarbij `/` en spaties een streepje worden (`CLAUDE/Content Marketing` wordt `label:CLAUDE-Content-Marketing`). Zoeken op label-ID geeft niets terug.
5. Claude verwerkt op basis van label en eventuele instructie van de gebruiker

## ⚠️ KRITIEK: Verwerkingslogica

**STAP 1: LEES DE FORWARDING MESSAGE EERST**

Wanneer de gebruiker een email doorstuurt, staat er vaak een bericht bovenaan met hun intentie. Dit is de BELANGRIJKSTE context en bepaalt hoe de email verwerkt moet worden.

De forwarding message staat typisch:
- Bovenaan de doorgestuurde email
- Vóór "---------- Forwarded message ---------"
- Of in het "content" veld als eerste tekst

**Voorbeelden van intenties:**
- "Vergelijk met bestaande content"
- "Dit wil ik gebruiken voor mijn training"
- "Goed voorbeeld voor marketing"
- "Check of ik dit al heb"

**STAP 2: INTENTIE ALTIJD OPNEMEN IN NOTITIES**

De intentie uit de forwarding message MOET ALTIJD in het Notities veld komen. Format:

```
[Kern/samenvatting van de email content]

INTENTIE: [Exacte of geparafraseerde tekst van de forwarding message]
```

Als er geen duidelijke intentie is, vraag dan eerst aan de gebruiker wat de bedoeling is.

**STAP 3: Label bepaalt database, intentie bepaalt verwerking**
- Gmail label → welke Notion database
- Forwarding message → HOE te verwerken, wat te benadrukken, en wat in Notities komt

## Mail Link Regel

Bij het opslaan van emails, construeer altijd de Gmail thread URL vanuit de threadId:
`https://mail.google.com/mail/u/0/#all/{threadId}`

**Routing logica:**
- Bron Type = E-Mail → zet Gmail URL in `Mail Link`
- Als er óók een externe link is (bijv. naar een resource uit de mail) → die blijft in `Bron URL`
- Als het géén mail is → `Mail Link` leeg laten

Het veld heet in het template `Mail Link` of `Mail link`; neem de spelling uit het schema. Heeft de database zo'n veld niet, zet de Gmail-URL dan in `Bron URL` en meld dat.

---

## Database Schema's

De velden hieronder zijn de velden van het EVA-template. De opties komen altijd uit het schema van de data source; de namen tussen haakjes zijn de standaardwaarden uit het template, bedoeld om de juiste optie te herkennen, niet om in te vullen als het schema iets anders zegt. Heeft de database een veld dat hier niet staat, laat het leeg tenzij de mail er duidelijk iets voor aanlevert.

### Inspiratie

| Veld | Type | Toelichting |
|------|------|-------------|
| Naam | title | - |
| Status | status | opties uit het schema |
| Bestemming | select | opties uit het schema |
| Bron Type | select | opties uit het schema |
| Bron URL | url | Externe link uit de email content |
| Mail Link (of Mail link, zoals het schema het spelt) | url | Gmail thread URL |
| Notities | text | - |

**Verwerking:**
- Naam: Beschrijvende titel op basis van de content
- Status: de inbox-optie uit het schema (in het template 📥 Inbox)
- Bestemming: Beste match met de content, uit de opties van het schema
- Bron Type: de e-mail-optie uit het schema (in het template 📩 E-Mail)
- Mail Link: `https://mail.google.com/mail/u/0/#all/{threadId}`
- Bron URL: Externe link indien aanwezig (nieuwsbrief URL, artikel, etc.)
- Notities: `[Kern van het idee]\n\nINTENTIE: [Instructie van de gebruiker]`

---

### Ideeën & Beslissingen

| Veld | Type | Toelichting |
|------|------|-------------|
| Naam | title | - |
| Status | status | opties uit het schema |
| Type | select | opties uit het schema |
| Categorie | select | opties uit het schema |
| Notities | text | - |
| Bron URL | url | Externe link uit de email content |
| Mail Link (of Mail link, zoals het schema het spelt) | url | Gmail thread URL |

**Verwerking:**
- Naam: Duidelijke titel
- Status: de inbox-optie uit het schema (in het template 📥 Inbox)
- Type: de idea-optie uit het schema als standaard (in het template 💡 Idea), de decision-optie als de mail een besluit bevat
- Categorie: Beste match met de content, uit de opties van het schema
- Mail Link: `https://mail.google.com/mail/u/0/#all/{threadId}`
- Bron URL: Externe link indien aanwezig
- Notities: `[Context/samenvatting]\n\nINTENTIE: [Forwarding message van de gebruiker]`

---

### Praktische Links

| Veld | Type | Toelichting |
|------|------|-------------|
| Naam | title | De naam/titel van de link of resource |
| Categorie | select | opties uit het schema |
| URL | url | De daadwerkelijke link/URL |
| Code | text | Affiliate codes, kortingscodes |
| Mail Link (of Mail link, zoals het schema het spelt) | url | Gmail thread URL |
| Notities | text | Aanvullende notities |

**Verwerking:**
- Naam: Duidelijke naam van de tool/resource
- Categorie: Beste matchende categorie uit de opties van het schema
- URL: De hoofd-URL
- Code: Eventuele codes uit de email
- Mail Link: `https://mail.google.com/mail/u/0/#all/{threadId}`
- Notities: `[Wat het is / waar het voor dient]\n\nINTENTIE: [Forwarding message van de gebruiker]`

---

### Story Vault

| Veld | Type | Toelichting |
|------|------|-------------|
| Naam | title | Korte beschrijving van de review/verhaal |
| Type | select | opties uit het schema |
| Aanbod | select | opties uit het schema |
| Platform | select | opties uit het schema |
| Bron | text | Klantnaam of eigen naam voor persoonlijke verhalen |
| Inhoud | text | Volledige quote of verhaal |
| Mail Link (of Mail link, zoals het schema het spelt) | url | Gmail thread URL |
| Thema's | multi_select | opties uit het schema |

**Type definities:**
- **Case study**: Klantvoorbeeld met concrete resultaten en context
- **Review**: Testimonial of quote van klant
- **Persoonlijk verhaal**: Je eigen verhaal

**Verwerking:**
- Naam: Korte, duidelijke beschrijving
- Type: Bepaal uit context, kies de optie zoals het schema hem spelt
- Aanbod: Match met product indien vermeld, anders de algemene optie uit het schema
- Platform: de e-mail-optie uit het schema (standaard voor email-processor)
- Bron: Klantnaam uit context
- Inhoud: `[Volledige tekst van de review/verhaal]\n\nINTENTIE: [Forwarding message van de gebruiker]`
- Mail Link: `https://mail.google.com/mail/u/0/#all/{threadId}`
- Thema's: Selecteer relevante thema's uit de opties van het schema

---

## Voorbeeld API Calls

`data_source_id` is altijd de `collection://`-id van de database die je via de systeempagina hebt gematcht. Zet er nooit een vast ID in.

### Content Email → Inspiratie

```python
{
  "parent": {"type": "data_source_id", "data_source_id": "<collection-id van de gematchte database Inspiratie>"},
  "pages": [{
    "properties": {
      "Naam": "AI workflow automatisering tips",
      "Status": "📥 Inbox",
      "Bestemming": "🎓 Training",
      "Bron Type": "📩 E-Mail",
      "Mail Link": "https://mail.google.com/mail/u/0/#all/19abc12345678",
      "Notities": "Handige tips voor het automatiseren van repetitieve taken met AI.\n\nINTENTIE: Gebruiken voor eigen training content"
    }
  }]
}
```

### Links Email → Praktische Links

```python
{
  "parent": {"type": "data_source_id", "data_source_id": "<collection-id van de gematchte database Praktische Links>"},
  "pages": [{
    "properties": {
      "Naam": "Handige AI Tool",
      "Categorie": "🛠️ Tools & Software",
      "URL": "https://example.com/tool",
      "Code": "KORTING20",
      "Mail Link": "https://mail.google.com/mail/u/0/#all/19abc12345680",
      "Notities": "AI tool voor content creatie. 20% korting met code.\n\nINTENTIE: Testen voor eigen workflow"
    }
  }]
}
```

### Stories Email → Story Vault

```python
{
  "parent": {"type": "data_source_id", "data_source_id": "<collection-id van de gematchte database Story Vault>"},
  "pages": [{
    "properties": {
      "Naam": "Klant - Tijdsbesparing resultaat",
      "Type": "Case study",
      "Aanbod": "Algemeen",
      "Platform": "Email",
      "Bron": "Klantnaam",
      "Inhoud": "Door de nieuwe werkwijze bespaar ik nu 10 uur per week.\n\nINTENTIE: Gebruiken voor sales page",
      "Mail Link": "https://mail.google.com/mail/u/0/#all/19abc12345681",
      "Thema's": "[\"Tijdsbesparing\", \"Resultaat\"]"
    }
  }]
}
```

De waarden in deze voorbeelden zijn illustratief. Gebruik altijd de opties zoals het schema van de klant ze spelt.

---

## Als iets niet werkt

- **Save-skill niet geïnstalleerd, of de regel `Systeempagina:` is leeg.** Zeg dat de link van de Business HQ-pagina eerst in de save-skill moet staan, verwijs naar Module 1, de les "Skill inrichten: Save All (@save)", en verwerk geen mail.
- **De systeempagina is niet op te halen.** Meld dat, en dat de Notion-koppeling actief moet zijn. Verwerk geen mail.
- **De database die bij het label hoort staat niet op de systeempagina.** Zeg welke ontbreekt en dat hij op de Business HQ-pagina moet staan, op de manier die de save-skill beschrijft. Verwijs naar Module 1, de les "Notion-templates installeren en personaliseren". Verwerk de mails met de andere labels wel.
- **Een label hoort bij geen enkele database op de pagina.** Vraag één keer welke database bedoeld is. Krijg je geen antwoord, sla die mails over en meld welke.
- **Gmail niet gekoppeld.** Meld het en stop; er is niets op te halen.

## Na het aanmaken

Na het aanmaken van een entry, bevestig aan de gebruiker:
- Wat er is aangemaakt
- In welke database
- Link naar de entry
- Vraag of de mail naar het label `CLAUDE/VERWERKT` mag. Bij ja: dat label erop zetten en het routing-label eraf halen, zodat de mail bij de volgende @mail niet opnieuw meegaat. Bestaat `CLAUDE/VERWERKT` niet, maak het dan één keer aan (of vraag de gebruiker dat te doen) en zeg dat erbij.

---

*Versie 13-10-2026: geen database-ID's meer; de databases komen van de systeempagina via de save-skill. Content-labels met een toevoeging (Marketing, Courses) gaan naar Inspiratie met die toevoeging als Bestemming-hint. VERWERKT en DEMO worden overgeslagen; verwerkte mails gaan op verzoek naar CLAUDE/VERWERKT.*
