---
description: >
  Check for updates to the Youforce PO Toolkit plugin and install the latest version.
  Use this skill when the user says "check for updates", "is there a new version", "update de plugin",
  "nieuwe versie beschikbaar?", or "plugin updaten".
---

# Check for updates — Youforce PO Toolkit

**Geïnstalleerde versie:** `0.5.0`
**GitHub repo:** `chrisbergbi/youforce-po-toolkit`

Maak een artifact dat via browser-side `fetch()` de nieuwste GitHub-release ophaalt en de gebruiker
een downloadknop biedt als er een update beschikbaar is.

> De bash-sandbox kan GitHub niet bereiken (proxy blokkeert het). Een artifact draait in
> Electron's webview, die wél directe internettoegang heeft.

---

## Stappen

### 1. Stel versies in
- `installed_version` = `0.5.0` (hardcoded in deze skill — wordt bijgewerkt bij elke release)

### 2. Laad de artifact-tool
Laad `mcp__cowork__create_artifact` via ToolSearch.

### 3. Maak het artifact aan

Bouw een self-contained HTML artifact met deze logica:

```
const INSTALLED = "0.5.0";
const API_URL = "https://api.github.com/repos/chrisbergbi/youforce-po-toolkit/releases/latest";

fetch(API_URL)
  → parse tag_name → strip "v" → latest_version
  → parse assets[] → zoek asset waar name eindigt op ".plugin" → browser_download_url

Als latest_version == INSTALLED:
  Toon groen vinkje: "Je hebt de laatste versie (v{INSTALLED})."

Als latest_version > INSTALLED:
  Toon: "Update beschikbaar: v{latest_version} (huidig: v{INSTALLED})"
  Toon downloadknop: <a href="{browser_download_url}" download>Download v{latest_version}</a>
  Toon instructie onder de knop:
    "Na het downloaden: open Cowork-instellingen → Plugins → Install plugin
     en selecteer het gedownloade .plugin bestand. Een herstart kan nodig zijn."

Bij fout (netwerk / rate-limit):
  Toon foutmelding met "Probeer opnieuw"-knop die fetch() herhaalt.
  Bij rate-limit (HTTP 403/429): vermeld expliciet dat GitHub tijdelijk beperkt is.
```

**Artifact-richtlijnen:**
- Toon een laadspinner terwijl de API wordt bevraagd
- Strakke, minimalistische opmaak passend bij Cowork
- Geen externe CDN-links — alles inline
- Gebruik CSS variables voor kleuren zodat het werkt in dark mode

### 4. Chat-samenvatting
Na het aanmaken van het artifact, geef een korte samenvatting:
- Update beschikbaar: "Er is een nieuwe versie: **v{latest_version}**. Download via het venster hierboven en installeer via Plugins → Install plugin."
- Up-to-date: "Je plugin is up-to-date (v{installed_version})."

---

## Fallback: als het artifact toch geen GitHub-toegang heeft

1. Gebruik `WebSearch` met query: `github chrisbergbi youforce-po-toolkit releases`
2. Extraheer het laatste versienummer
3. Vergelijk met `0.5.0`
4. Als update: geef de gebruiker deze link:
   `https://github.com/chrisbergbi/youforce-po-toolkit/releases/latest`

---

## Notes voor beheerder (Chris)
Bij elke nieuwe release: vervang het versienummer op regel 1 van de frontmatter én in de
`INSTALLED`-variabele in de stappen hierboven door het nieuwe versienummer.
