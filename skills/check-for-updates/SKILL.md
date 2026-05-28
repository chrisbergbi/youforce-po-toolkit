---
description: >
  Check for updates to the Youforce PO Toolkit plugin and install the latest version with one click.
  Use this skill when the user says "check for updates", "is there a new version", "update de plugin",
  "nieuwe versie beschikbaar?", or "plugin updaten".
---

# Check for updates — Youforce PO Toolkit

**Geïnstalleerde versie:** `0.10.0`
**GitHub repo:** `chrisbergbi/youforce-po-toolkit`

---

## Stappen

### 1. Haal de laatste versie op via bash

```bash
curl -s "https://api.github.com/repos/chrisbergbi/youforce-po-toolkit/releases/latest"
```

Extraheer uit de JSON-response:
- `tag_name` → strip `v`-prefix → `latest_version`
- Zoek in `assets[]` het object waarvan `name` eindigt op `.plugin` → sla `browser_download_url` op

### 2. Vergelijk versies

- `installed_version` = `0.10.0`
- Als `latest_version` == `installed_version`: zeg "Je plugin is up-to-date (v0.10.0)." Stop.
- Als `latest_version` > `installed_version`: ga naar stap 3.

### 3. Download en presenteer

```bash
cd /tmp && rm -rf youforce-po-toolkit-update
git clone https://github.com/chrisbergbi/youforce-po-toolkit.git youforce-po-toolkit-update
zip -r /tmp/youforce-po-toolkit.plugin youforce-po-toolkit-update/ \
  -x "*.DS_Store" -x "*.git*" -x "*/.git/*"
cp /tmp/youforce-po-toolkit.plugin "/sessions/funny-focused-volta/mnt/CLAUDE AI/youforce-po-toolkit.plugin"
```

Laad daarna `mcp__cowork__present_files` via ToolSearch en presenteer:
```
/Users/chris/Downloads/CLAUDE AI/youforce-po-toolkit.plugin
```

### 4. Samenvatting in chat

- Update: "Er is een nieuwe versie: **v{latest_version}** (huidig: v{installed_version}). Klik op 'Save plugin' om te updaten."
- Up-to-date: "Je plugin is up-to-date (v{installed_version})."
- Bij fout: geef de gebruiker `https://github.com/chrisbergbi/youforce-po-toolkit/releases/latest`

---

## Notes voor beheerder (Chris)
Bij elke nieuwe release: vervang `0.10.0` (twee keer) door het nieuwe versienummer.
