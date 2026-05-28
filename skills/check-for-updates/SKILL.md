---
description: >
  Check for updates to the Youforce PO Toolkit plugin and install the latest version with one click.
  Use this skill when the user says "check for updates", "is there a new version", "update de plugin",
  "nieuwe versie beschikbaar?", or "plugin updaten".
---

# Check for updates — Youforce PO Toolkit

**Geïnstalleerde versie:** `0.6.0`
**GitHub repo:** `chrisbergbi/youforce-po-toolkit`

---

## Stappen

### 1. Haal de laatste versie op via WebSearch

Gebruik `WebSearch` (laad via ToolSearch indien nodig) met query:
```
github chrisbergbi youforce-po-toolkit releases latest
```

Extraheer het versienummer uit de resultaten (formaat: `v0.x.x` of `0.x.x`).
Sla op als `latest_version` (zonder `v`-prefix).

### 2. Vergelijk met geïnstalleerde versie

- `installed_version` = `0.6.0` (hardcoded — wordt bijgewerkt bij elke release)
- Als `latest_version` == `installed_version`: vertel de gebruiker dat de plugin up-to-date is. Stop.
- Als `latest_version` > `installed_version`: ga naar stap 3.

### 3. Download en presenteer de nieuwe versie

Gebruik `mcp__workspace__bash` om:
1. De repo te clonen (publiek, geen token nodig):
```bash
cd /tmp && rm -rf youforce-po-toolkit-update
git clone https://github.com/chrisbergbi/youforce-po-toolkit.git youforce-po-toolkit-update
```

2. Het `.plugin` bestand te bouwen:
```bash
cd /tmp
zip -r /tmp/youforce-po-toolkit.plugin youforce-po-toolkit-update/ \
  -x "*.DS_Store" -x "*.git*" -x "*/.git/*"
```

3. Kopieer naar de workspace zodat present_files erbij kan:
```bash
cp /tmp/youforce-po-toolkit.plugin "/sessions/funny-focused-volta/mnt/CLAUDE AI/youforce-po-toolkit.plugin"
```
> **Let op voor beheerder:** het pad `/sessions/funny-focused-volta/mnt/CLAUDE AI/` is de bash-equivalent
> van de CLAUDE AI workspace. Dit pad kan per sessie anders zijn — gebruik `mcp__cowork__present_files`
> met het host-pad `/Users/chris/Downloads/CLAUDE AI/youforce-po-toolkit.plugin`.

### 4. Presenteer het bestand

Laad `mcp__cowork__present_files` via ToolSearch en roep het aan met:
```
file_path: /Users/chris/Downloads/CLAUDE AI/youforce-po-toolkit.plugin
```

De gebruiker ziet een "Save plugin" knop en hoeft alleen daarop te klikken.

### 5. Geef een samenvatting in de chat

- Bij update: "Er is een nieuwe versie: **v{latest_version}** (huidig: v{installed_version}). Klik op 'Save plugin' hierboven om te updaten."
- Bij up-to-date: "Je plugin is up-to-date (v{installed_version})."
- Bij fout in WebSearch of git clone: geef de gebruiker deze link om handmatig te downloaden:
  `https://github.com/chrisbergbi/youforce-po-toolkit/releases/latest`

---

## Notes voor beheerder (Chris)
Bij elke nieuwe release: vervang `0.6.0` (twee keer) door het nieuwe versienummer.
