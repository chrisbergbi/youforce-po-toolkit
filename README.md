# Youforce PO Toolkit

Een Claude Cowork plugin voor het Youforce PO-team. De plugin bevat skills voor klantcommunicatie, community-management, onboarding en release-beheer.

---

## Installeren (collega's)

Open Claude Cowork en stuur dit bericht:

> **Installeer deze plugin: https://github.com/chrisbergbi/youforce-po-toolkit/releases/latest/download/youforce-po-toolkit.plugin**

Claude downloadt de plugin automatisch en toont een installatieknop — één klik en je bent klaar.

---

## Updates

Typ in Cowork: *"check for updates"* — de skill checkt GitHub en toont een installatieknop als er een nieuwe versie beschikbaar is.

---

## Skills

### 💬 Klantreactie

**Waarvoor:** Schrijft een professionele, warme reactie op klantvragen, mails of community-posts — in de Youforce communicatiestijl.

**Wanneer gebruiken:**
- Je hebt een e-mail of communitypost van een klant en wilt snel een goed antwoord opstellen
- Je weet niet precies hoe je iets wilt formuleren
- Je wil de klant doorsturen naar het juiste kanaal (community, Jira, support)

**Hoe triggeren:** Plak de klantvraag in de chat en schrijf erbij: *"kun je hier op reageren"* of *"stel een antwoord op"*

**Voorbeelden:**
> "Stel een antwoord op voor deze mail van een klant die vraagt waarom zijn rapport leeg is"

> "Reactie opstellen voor dit community-bericht over een feature die ze missen"

---

### 💡 Youforce Ideeën Assistent

**Waarvoor:** Schrijft een professionele reactie op klantideeën ingediend via het Youforce community-platform, bij statuswijzigingen.

**Wanneer gebruiken:**
- Een klantidee krijgt een nieuwe status (backlog, gepland, afgewezen, beschikbaar)
- Je wil de klant professioneel informeren over wat er met hun idee gebeurt
- Je wil uitleggen waarom een idee niet wordt opgepakt, zonder de deur dicht te gooien

**Hoe triggeren:** Kopieer het idee uit de community en geef de nieuwe status mee.

**Voorbeelden:**
> "Reageer op dit idee — het gaat naar de backlog" [plak het idee erbij]

> "Schrijf een reactie voor dit afgewezen idee, reden: buiten scope van ons platform"

---

### 🚀 HR-Cycle Onboarding

**Waarvoor:** Maakt een kant-en-klaar Slack-bericht in `#yfo-pd-yf-talent-private` voor het onboarden van een nieuwe HR-cyclus klant.

**Wanneer gebruiken:**
- Er is een nieuw Jira-ticket voor het activeren van HR-cyclus bij een klant
- Je wil het team informeren dat er een nieuwe klant klaar staat om geonboard te worden

**Hoe triggeren:** Stuur een screenshot van het Jira-ticket of geef de klantgegevens door.

**Voorbeelden:**
> "Onboard deze klant voor HR-cyclus" [screenshot van Jira-ticket]

> "Maak een onboarding bericht voor TenantId 6528938, klant HTM Personenvervoer"

---

### 📝 Release Notes

**Waarvoor:** Genereert gebruiksvriendelijke release notes op basis van tickets, changelogs of een beschrijving van wat er is opgeleverd.

**Wanneer gebruiken:**
- Je hebt een lijst met Jira-tickets of een interne changelog en wil dit omzetten naar leesbare communicatie
- Je wil een sprint-oplevering communiceren naar klanten of stakeholders
- Je wil een professionele changelog schrijven voor een nieuwe versie

**Hoe triggeren:** Geef de tickets of wijzigingen mee en vraag om release notes.

**Voorbeelden:**
> "Schrijf release notes voor deze sprint" [lijst met tickets]

> "Maak een changelog op basis van deze Jira-tickets: [...]"

---

### 📧 Gmail Drafts

**Waarvoor:** Maakt een of meerdere opgemaakte Gmail-conceptmails aan op basis van een e-mailtekst en ontvangerslijst, inclusief HTML-opmaak.

**Wanneer gebruiken:**
- Je wil een e-mail klaarzetten zonder hem meteen te versturen
- Je hebt een tekst met opmaak (vet, bullets, kopjes) die correct in Gmail moet staan
- Je wil een mail sturen naar meerdere losse ontvangers

**Hoe triggeren:** Geef de e-mailtekst en ontvanger(s) mee.

**Voorbeelden:**
> "Zet een mail klaar naar jan@voorbeeld.nl met deze tekst: [...]"

> "Maak een concept-mail aan met onderwerp 'Update Q2'"

---

### 📮 Klant Mailing Drafts

**Waarvoor:** Maakt gepersonaliseerde Gmail-conceptmails aan voor klantmailings — één per organisatie, met automatische aanhef op basis van contactnamen.

**Wanneer gebruiken:**
- Je wil dezelfde mail sturen naar meerdere klantorganisaties, maar gepersonaliseerd per organisatie
- Je hebt een klantenlijst (als screenshot of geplakte tekst) en een e-mailtemplate
- Je wil mailings klaarzetten zonder ze meteen te versturen

**Hoe triggeren:** Geef de klantenlijst en de e-mailtemplate mee.

**Voorbeelden:**
> "Zet drafts klaar voor deze klanten" [screenshot klantenlijst + mailtekst]

> "Stuur deze update naar alle klanten in deze lijst" [klantenlijst + template]

---

### 🔄 Check for Updates

**Waarvoor:** Controleert of er een nieuwe versie van de Youforce PO Toolkit beschikbaar is op GitHub en biedt een directe installatieknop aan.

**Wanneer gebruiken:**
- Je wil controleren of je de nieuwste versie van de plugin hebt
- Je hebt gehoord dat er een nieuwe skill is toegevoegd

**Hoe triggeren:** Typ gewoon: *"check for updates"*

---

### 🛠 Publish Plugin *(alleen voor beheerder)*

**Waarvoor:** Pusht wijzigingen in een skill vanuit de workspace naar GitHub en maakt automatisch een nieuwe release aan met het bijgewerkte plugin-bestand. Bijwerkt ook de README als er een nieuwe skill is toegevoegd of een bestaande skill is veranderd.

**Wanneer gebruiken:**
- Je hebt een SKILL.md aangepast in de workspace en wil dit publiceren
- Je wil een nieuwe versie uitbrengen van de plugin

**Hoe triggeren:** *"publiceer"* of *"release maken"* — de skill vraagt daarna welke skill je wil publiceren en wat de changelog is.

> ⚠️ Vereist een GitHub token opgeslagen in `CLAUDE AI/.youforce-plugin-config.json`

---

## Voor de beheerder (Chris)

### Nieuwe versie publiceren

Vraag Claude in Cowork: *"publiceer"* of *"release maken"* — de `publish-plugin` skill regelt de rest.

### GitHub token

Sla je GitHub PAT op in `CLAUDE AI/.youforce-plugin-config.json`:

```json
{
  "github_token": "ghp_...",
  "owner": "chrisbergbi",
  "repo": "youforce-po-toolkit",
  "workspace_path_bash": "/sessions/.../mnt/CLAUDE AI",
  "workspace_path_host": "/Users/.../Downloads/CLAUDE AI"
}
```

---

## Versiegeschiedenis

| Versie | Datum | Wijzigingen |
|--------|-------|-------------|
| 0.12.0 | 2026-05-29 | check-for-updates toont release notes in chat; publish-plugin genereert uitgebreidere release beschrijvingen |
| 0.11.0 | 2026-05-29 | README uitgebreid met skill-beschrijvingen per skill; publish-plugin bijgewerkt met README-check |
| 0.10.0 | 2026-05-29 | publish-plugin uploadt nu .plugin als release asset |
| 0.9.0 | 2026-05-28 | publish-plugin skill toegevoegd |
| 0.8.0 | 2026-05-28 | HR-cycle onboarding skill toegevoegd |
| 0.3.0 | 2026-05-28 | Release-notes skill toegevoegd |
| 0.2.0 | 2026-05-28 | Update-checker skill toegevoegd |
| 0.1.0 | 2026-05-28 | Eerste versie: klantreactie en ideeën-assistent |
