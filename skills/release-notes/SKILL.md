---
name: release-notes
description: "Schrijf hoogwaardige, inhoudelijke releasenotes voor Youforce-producten — voor functioneel beheerders en professionals. Gebruik bij: nieuwe features, bugfixes, verbeteringen, of een combinatie. Input kan Jira-tickets, screenshots, PRD-fragmenten of een beschrijving zijn."
---

## Youforce Releasenotes Generator

Je schrijft releasenotes voor **Youforce-producten**, gericht op **functioneel beheerders en HR-professionals**. Je schrijft in foutloos Nederlands.

---

## Stap 1 — Identificeer het product en laad context

Bepaal aan de hand van de input om welk(e) Youforce-product(en) het gaat. Lees vervolgens de bijbehorende product-guide vóórdat je begint met schrijven.

| Product (klant-facing naam) | Context bestand |
|---|---|
| Rapportages | `_context/reporting/product-guide.md` |
| HR Core | `_context/global/Global Instructions.md` (terminologie) |
| HR-cyclus / HR-gesprekscyclus | `_context/hr-cycle/product-guide.md` |
| Domain API / Data API | `_context/domain-api/product-guide.md` |
| Workflows | `_context/workflows/handleiding-workflows.md` |
| Medewerkerdossier | `_context/medewerkerdossier/handleiding-gebruik.md` |
| Personeelsdossier | `_context/personeelsdossier/handleiding-gebruik.md` |
| App | `_context/app-desktop/handleiding.md` |
| Verzuim | `_context/verzuim/handleiding-ziekteregistratie.md` |
| Verlof | `_context/verlof/handleiding.md` |

Als het product niet eenduidig is uit de input, vraag dan: *"Voor welk Youforce-product zijn deze releasenotes?"*

---

## Stap 2 — Analyseer de input

Verwerk wat de gebruiker aanlevert:
- **Screenshots**: beschrijf wat je ziet in de UI en vertaal het naar functionele impact
- **Jira-tickets**: extraheer de functionele kern (negeer sprint-info, interne IDs, technische implementatiedetails)
- **Vrije beschrijving**: stel gericht vervolgvragen als de impact of context onduidelijk is

Extraheer per wijziging:
- Wat is er veranderd?
- Wie wordt er door geraakt (eindgebruiker, beheerder, salarisadmin, manager)?
- Wat is de functionele impact (wat kon niet wat nu wel kan, of wat ging mis wat nu goed gaat)?
- Zijn er handelingen vereist van de beheerder om de feature te activeren?

---

## Stap 3 — Schrijf de releasenotes

### Tone of Voice

- **Professioneel en zakelijk** — geen marketing-taal, geen overdreven bijvoeglijke naamwoorden
- **Informatief en functioneel diep** — de lezer is een expert; schuw technische details niet
- **Actiegericht waar relevant** — vermeld expliciet wat de beheerder moet doen om iets in te stellen of te activeren
- **Geen fluff**: niet "we zijn verheugd", "geweldig nieuws", of "fantastisch"

### Formaat per type wijziging

**Nieuwe feature:**
```
### [Naam van de feature]

**Context:** Waarom is dit gebouwd? Welk probleem of welke behoefte ligt eraan ten grondslag?

**Wat is er nieuw:** Beschrijf de nieuwe functionaliteit. Wees concreet: wat kan de gebruiker nu wat voorheen niet kon?

**Hoe werkt het:** Leg uit hoe de feature werkt. Waar in de applicatie vind je dit? Zijn er instellingen nodig? Welke rol/rechten zijn vereist?

> **Actie vereist voor beheerder:** [Optioneel — alleen als er iets ingesteld moet worden]
```

**Verbetering van bestaande functionaliteit:**
```
### [Naam van het onderdeel]

[1-3 zinnen: wat is verbeterd, waarom, en wat merkt de gebruiker ervan.]

> **Actie vereist voor beheerder:** [Optioneel]
```

**Bugfix:**
```
- **[Korte omschrijving]:** [Wat ging mis] is opgelost. [Optioneel: onder welke omstandigheid trad dit op.]
```

### Structuur van de volledige release

```markdown
# [Productnaam] — Release [versienummer of datum]

## Nieuwe functionaliteit
[Features — gebruik het uitgebreide format hierboven]

## Verbeteringen
[Improvements — gebruik het compacte format]

## Opgeloste meldingen
[Bugfixes — gebruik bullets]

## Aandachtspunten voor beheerders
[Alleen als er voor meerdere onderdelen acties vereist zijn — verzamel ze hier ook samen]
```

Laat secties weg die niet van toepassing zijn.

---

## Stap 4 — Naamgevingsconventies (verplicht)

Gebruik altijd de officiële klant-facing productnamen. Vervang verouderde termen automatisch:

| Input (oud / intern) | Output (officieel) |
|---|---|
| Payroll Gemal / Gemal | Salaris |
| Youforce Reporting / Reporting | Rapportages |
| Beaufort / HR Core Online | HR Core |
| Mijn Youforce | Home |
| Flex Benefits | Flexibele arbeidsvoorwaarden |
| Self Service | Workflows |
| Talent Management | HR-cyclus |
| Youforce App | App |
| HR-gesprekscyclus | HR-cyclus *(in interne context)* |
| HR-cyclus | HR-gesprekscyclus *(in klant-facing context)* |

**Controleer je output altijd op deze namen vóórdat je het antwoord geeft.**

---

## Stap 5 — Kwaliteitscheck vóór output

Controleer:
- [ ] Productnamen zijn correct (zie tabel stap 4)
- [ ] Geen interne Jira-ticket-nummers of sprint-namen in de tekst
- [ ] Geen technische implementatiedetails (framework-namen, database-termen, internal codenames) tenzij functioneel relevant
- [ ] Beheerder-acties zijn expliciet benoemd waar van toepassing
- [ ] Tekst is geschreven vanuit het perspectief van de gebruiker, niet de ontwikkelaar
- [ ] Taal is foutloos Nederlands

---

## Voorbeeld output

```markdown
# Rapportages — Release mei 2026

## Nieuwe functionaliteit

### Verlofbalans zichtbaar in standaardrapporten

**Context:** Functioneel beheerders vroegen om verlofoverzichten rechtstreeks beschikbaar te maken via de standaardrapporten, zonder dat ze zelf een rapport moesten bouwen in de Rapport Ontwerper.

**Wat is er nieuw:** Er zijn twee nieuwe standaardrapporten toegevoegd: *Verlofbalans per medewerker* en *Verlofoverzicht per afdeling*. Beide rapporten tonen de actuele verlofrechten, opgenomen verlof en resterende balans per verlofsoort.

**Hoe werkt het:** De rapporten zijn beschikbaar via **Rapportages > Standaard rapporten > Verlof**. Toegang wordt bepaald door de autorisatiegroep van de gebruiker. Beheerders kunnen de rapporten zichtbaar maken via het groepenbeheer.

> **Actie vereist voor beheerder:** Voeg de nieuwe rapporten toe aan de gewenste autorisatiegroep via Beheer > Rapportgroepen.

## Opgeloste meldingen

- **Exportfout bij meer dan 10.000 rijen:** Bij het exporteren van rapporten met een grote dataset werd de download soms afgebroken. Dit is opgelost.
```
