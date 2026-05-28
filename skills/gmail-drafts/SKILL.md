---
name: gmail-drafts
description: >
  Maakt één of meerdere opgemaakte Gmail drafts aan op basis van een e-mailtekst en
  ontvangerslijst — met HTML-opmaak (vet, cursief, bullets), CC/BCC-ondersteuning,
  en de optie om na bevestiging direct te versturen.

  Gebruik deze skill wanneer de gebruiker een e-mail wil klaarzetten of versturen,
  en daarvoor een kant-en-klare tekst aanlevert met een of meer ontvangers.
  De skill behoudt de opmaak van de originele tekst en converteert die naar correcte
  HTML voor Gmail.

  Trigger bij: "zet een mail klaar", "maak een concept email aan", "stuur een mail naar",
  "draft aanmaken in Gmail", "email opstellen voor", of wanneer er een e-mailtekst én
  een of meer e-mailadressen worden aangeleverd. Ook triggeren bij "stuur dit door naar"
  of "mail dit naar [naam/adres]".
---

# Skill: Gmail Drafts

Je helpt de gebruiker om één of meerdere nette, opgemaakte Gmail drafts klaar te zetten.
Je doel is een draft die er in Gmail uitziet zoals de gebruiker het bedoelde — met correcte
opmaak, juiste ontvangers, en klaar om te versturen.

## Wat je nodig hebt

Zorg dat je deze zaken hebt voordat je begint:

1. **E-mailtekst** — de inhoud van de mail, eventueel met opmaak-indicaties (vet, bullets, etc.)
2. **Ontvanger(s)** — één of meerdere e-mailadressen (To)
3. **Onderwerpregel** — vraag ernaar als die ontbreekt en niet af te leiden is
4. **CC/BCC** (optioneel) — extra ontvangers die niet als primaire ontvanger staan

## Stap 1: Ontvangers en structuur vaststellen

- Meerdere ontvangers voor **dezelfde mail** → één draft met alle adressen in `to`
- **Verschillende mails** per ontvanger (bv. gepersonaliseerde versies) → aparte drafts; gebruik de `klant-mailing-drafts` skill als er een klantenlijst + template bij betrokken is

## Stap 2: HTML-opmaak genereren

Converteer de tekst naar nette HTML voor Gmail:

```
Vetgedrukte tekst     → <strong>...</strong>
Cursieve tekst        → <em>...</em>
Bullet-lijsten        → <ul><li>...</li></ul>
Genummerde lijsten    → <ol><li>...</li></ol>
Alinea's              → <p>...</p>
Koppen/titels         → <h3>...</h3> (gebruik spaarzaam)
```

Bewaar de toon en structuur van de originele tekst precies. Voeg niets toe, laat niets weg.

## Stap 3: Draft aanmaken

Maak de draft(s) aan via de Gmail-tool (`create_draft`) met:

- `to`: primaire ontvanger(s)
- `cc`: CC-adressen indien opgegeven
- `bcc`: BCC-adressen indien opgegeven
- `subject`: onderwerpregel
- `htmlBody`: HTML-versie van de tekst

## Stap 4: Bevestig en bied versturen aan

Na het aanmaken, bevestig kort wat er is klaargezet en vraag:

> "De draft staat klaar. Wil je dat ik hem ook direct verstuur?"

Wacht op expliciete bevestiging ("ja", "verstuur maar") voordat je verzendt.
Een neutrale reactie of stilte is géén bevestiging.

## Aandachtspunten

- Gmail-handtekening wordt **niet** automatisch toegevoegd via de API — meld dit kort aan de gebruiker de eerste keer
- Als er een bestaande draft moet worden **bijgewerkt**: de API ondersteunt geen updates — maak een nieuwe draft aan en adviseer de gebruiker de oude te verwijderen
- Bij twijfel over de onderwerpregel: stel er één voor op basis van de inhoud en vraag bevestiging
