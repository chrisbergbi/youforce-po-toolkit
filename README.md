# Youforce PO Toolkit

Een Claude Cowork plugin voor het Youforce PO-team met skills voor klantcommunicatie en community-management.

## Skills

| Skill | Gebruik |
|-------|---------|
| **Klantreactie** | Schrijft professionele reacties op klantvragen, mails en community-posts |
| **Ideeën assistent** | Reageert op klantideeën bij statuswijzigingen (backlog, gepland, afgewezen) |
| **Check for updates** | Checkt of er een nieuwe versie van de plugin beschikbaar is |

## Installatie (collega's)

1. Ga naar [Releases](../../releases) en download `youforce-po-toolkit.plugin`
2. Open Claude Cowork
3. Klik op het plugin-icoon → "Install plugin"
4. Selecteer het gedownloade bestand

Of gebruik de directe download-link (vraag Chris om de exacte URL):
```
https://github.com/OWNER/youforce-po-toolkit/releases/latest/download/youforce-po-toolkit.plugin
```

## Updates

Je kunt op twee manieren controleren of er een nieuwe versie is:

**Via de skill (aanbevolen):**
Typ in Cowork: *"check for updates"* — de skill checkt automatisch GitHub en presenteert een installatieknop als er een nieuwe versie is.

**Handmatig:**
Ga naar [Releases](../../releases), download de nieuwste versie en installeer opnieuw.

Na het installeren van een update kan het nodig zijn Cowork opnieuw te starten.

## Voor de beheerder (Chris)

### Skill aanpassen
Pas de `SKILL.md` van de gewenste skill aan in deze repo — rechtstreeks via GitHub of via Claude Cowork.

### Nieuwe versie publiceren
1. Verhoog `version` in `.claude-plugin/plugin.json` (bijv. `0.1.0` → `0.2.0`)
2. Commit en push
3. Ga naar *Releases → Create a new release* op GitHub
4. Gebruik het versienummer als tag (bijv. `v0.2.0`) en klik "Publish release"
5. GitHub Actions bouwt automatisch het `.plugin` bestand — klaar binnen ~30 seconden

### Wijzigingen via Claude Cowork pushen
Je kunt skills aanpassen in een Cowork-sessie en Claude vragen de wijzigingen naar GitHub te pushen. Zorg dat je een GitHub Personal Access Token (PAT) bij de hand hebt met `repo`-scope.

## Versiegeschiedenis

| Versie | Datum | Wijzigingen |
|--------|-------|-------------|
| 0.2.0 | 2026-05-28 | Update-checker skill toegevoegd |
| 0.1.0 | 2026-05-28 | Eerste versie: klantreactie en ideeën-assistent |
