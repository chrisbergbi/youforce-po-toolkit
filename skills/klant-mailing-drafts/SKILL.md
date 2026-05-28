---
name: klant-mailing-drafts
description: >
  Stelt opgemaakte Gmail drafts op voor klantmailings — één draft per organisatie,
  met gepersonaliseerde aanhef op basis van voornamen, HTML-opmaak en automatische CC.
  Gebruik deze skill wanneer Chris een mailing wil sturen naar een groep klanten of
  organisaties op basis van een e-mailtemplate en een klantenlijst.

  De klantenlijst kan worden aangeleverd als screenshot/afbeelding van een spreadsheet
  of als geplakte tekst/CSV. De skill groepeert contacten per organisatie, bouwt een
  gepersonaliseerde aanhef, converteert de opmaak naar HTML (vet, cursief, bullets),
  voegt CC-adressen toe waar relevant, en biedt na het aanmaken de optie om de mails
  direct te versturen na expliciete bevestiging.

  Trigger bij: "zet drafts klaar voor klanten", "stuur een mail naar deze organisaties",
  "maak concept-mails aan op basis van deze lijst", "klantenlijst + mailtekst → Gmail",
  of wanneer er tegelijkertijd een e-mailtemplate én een klantenlijst worden aangeleverd.
---

# Skill: Klant Mailing Drafts

Je helpt de gebruiker om een gepersonaliseerde mailing klaar te zetten voor meerdere
klantorganisaties. Het eindresultaat zijn opgemaakte Gmail drafts — één per organisatie —
die de gebruiker alleen nog maar hoeft te versturen (of jij verzendt ze na bevestiging).

## Wat je nodig hebt

Zorg dat je deze drie zaken hebt voordat je begint. Vraag wat ontbreekt:

1. **E-mailtemplate** — de basistekst van de mail, eventueel met placeholders zoals `[Naam]` of `[DATUM]`
2. **Klantenlijst** — als screenshot/afbeelding of als geplakte tekst, met minimaal: organisatienaam, naam contactpersoon, e-mailadres
3. **Extra context** (optioneel) — onderwerpregel, CC-adressen, specifieke tijdsloten of andere inhoudelijke invulling van placeholders

## Stap 1: Klantenlijst verwerken

Lees de klantenlijst zorgvuldig in:

- **Groepeer per organisatie** — alle contacten van dezelfde organisatie gaan in één draft
- **Identificeer voor elke organisatie:**
  - Primaire ontvangers (To): e-mailadressen van alle contacten
  - CC-adressen: adressen die los staan in een extra kolom of expliciet als CC zijn aangeduid
  - Voornamen: extraheer de voornaam van elk contact voor de aanhef
- **Omgaan met ontbrekende voornamen:** als je alleen een initiaal hebt (bv. M. Jansen), gebruik die initiaal in de aanhef. Meld dit aan de gebruiker zodat hij het eventueel kan corrigeren.

**Aanhef samenstellen:**
- 1 persoon: `Goedemiddag [Voornaam],`
- 2 personen: `Goedemiddag [Naam1] en [Naam2],`
- 3+ personen: `Goedemiddag [Naam1], [Naam2] en [Naam3],`

## Stap 2: Template invullen

Vul alle placeholders in de template in:

- `[Naam]` of `[NAAM]` → vervang door de samengestelde aanhef (zonder "Goedemiddag", dat staat al in de aanhef-regel)
- `[DATUM 1]`, `[DATUM 2]`, etc. → vervang door de concrete tijdsloten als die bekend zijn
- Andere placeholders → vraag de gebruiker om invulling als ze niet uit de context af te leiden zijn

## Stap 3: HTML-opmaak genereren

Converteer de plain-text template naar nette HTML die goed werkt in Gmail:

```
Vetgedrukte tekst     → <strong>...</strong>
Cursieve tekst        → <em>...</em>
Bullet-lijsten        → <ul><li>...</li></ul>
Genummerde lijsten    → <ol><li>...</li></ol>
Alinea's              → <p>...</p>
Lege regels           → nieuwe <p> (geen <br>)
```

Bewaar de exacte structuur en toon van de originele tekst. Voeg geen inhoud toe en laat
niets weg.

## Stap 4: Gmail drafts aanmaken

Maak voor elke organisatie een draft aan via de Gmail-tool (`create_draft`) met:

- `to`: lijst van alle primaire ontvangers van die organisatie
- `cc`: CC-adressen (eigen collega's + eventuele externe CC uit de klantenlijst)
- `subject`: de opgegeven onderwerpregel
- `htmlBody`: de HTML-versie van de ingevulde template

Maak alle drafts zo veel mogelijk tegelijk aan (parallel) voor snelheid.

## Stap 5: Bevestig en bied versturen aan

Na het aanmaken, geef een overzicht:

| Organisatie | Aan | CC | Status |
|---|---|---|---|
| KvK | Margot, Ingeborg, ... | Niels | ✅ Draft |
| ... | | | |

Vraag vervolgens:
> "Alle drafts staan klaar. Wil je dat ik ze ook direct verstuur, of doe je dat zelf vanuit Gmail?"

Als de gebruiker bevestigt: verstuur alle drafts één voor één. Meld na afloop welke verstuurd zijn.

## Aandachtspunten

- Gmail-handtekening wordt **niet** automatisch toegevoegd via de API — meld dit aan de gebruiker
- Controleer altijd of CC-adressen in de klantenlijst echt bedoeld zijn als CC (sommige spreadsheets hebben extra kolommen met andere info)
- Als de gebruiker achteraf een draft wil aanpassen (bv. voornaam corrigeren): de bestaande draft kan niet worden bijgewerkt via de API — maak een nieuwe aan en adviseer de gebruiker de oude te verwijderen
