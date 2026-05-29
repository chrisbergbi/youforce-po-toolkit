# Youforce PO Toolkit

Een Claude Cowork plugin voor het Youforce PO-team met skills voor klantcommunicatie en community-management.

## Skills

| Skill | Gebruik |
|-------|---------|
| **Klantreactie** | Schrijft professionele reacties op klantvragen, mails en community-posts |
| **Ideeën assistent** | Reageert op klantideeën bij statuswijzigingen (backlog, gepland, afgewezen) |
| **HR Cycle Onboarding** | Stuurt Slack-onboardingbericht voor nieuwe HR-cyclus klanten |
| **Release Notes** | Genereert release notes vanuit tickets of changelogs |
| **Check for updates** | Checkt of er een nieuwe versie van de plugin beschikbaar is |
| **Publish plugin** | Pusht skill-wijzigingen naar GitHub en maakt een nieuwe release aan |

## Installatie (collega's)

Open Cowork en stuur dit bericht:

> **Installeer deze plugin: https://github.com/chrisbergbi/youforce-po-toolkit/releases/latest/download/youforce-po-toolkit.plugin**

Claude downloadt het bestand en presenteert het met een installatieknop — klaar.

## Updates

Typ in Cowork: *"check for updates"* — de skill checkt automatisch GitHub en presenteert een installatieknop als er een nieuwe versie beschikbaar is.

## Voor de beheerder (Chris)

### Nieuwe versie publiceren

Vraag Claude in Cowork: *"publiceer"* of *"release maken"* — de `publish-plugin` skill regelt de rest: wijzigingen committen, versienummer ophogen, release aanmaken én het `.plugin` bestand uploaden als asset.

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

## Versiegeschiedenis

| Versie | Datum | Wijzigingen |
|--------|-------|-------------|
| 0.10.0 | 2026-05-29 | publish-plugin uploadt nu .plugin als release asset |
| 0.3.0 | 2026-05-28 | Release-notes skill toegevoegd |
| 0.2.0 | 2026-05-28 | Update-checker skill toegevoegd |
| 0.1.0 | 2026-05-28 | Eerste versie: klantreactie en ideeën-assistent |
