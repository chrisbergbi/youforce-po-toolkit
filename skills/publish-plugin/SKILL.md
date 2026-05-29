---
description: >
  Push skill changes from the workspace to GitHub and create a new release.
  Use this skill when the user says "push to GitHub", "publiceer", "release maken",
  "deploy", "zet door naar GitHub", "nieuwe versie uitbrengen", or names a specific skill
  they want to publish.
---

# Publish to GitHub — Youforce PO Toolkit

Verwerk een gewijzigde skill vanuit de workspace naar GitHub en maak een nieuwe release aan.

**Config:** `CLAUDE AI/.youforce-plugin-config.json`
**Skills in workspace:** `CLAUDE AI/<skill-naam>/SKILL.md`

---

## Stappen

### 1. Lees de configuratie

```bash
cat "/sessions/funny-focused-volta/mnt/CLAUDE AI/.youforce-plugin-config.json"
```

Extraheer: `github_token`, `owner`, `repo`, `workspace_path_bash`, `workspace_path_host`.

Als het bestand niet bestaat: vraag de gebruiker om het aan te maken met hun GitHub token en de gegevens hierboven.

### 2. Bepaal welke skill(s) gepubliceerd worden

Als de gebruiker het niet expliciet heeft gezegd, vraag dan via AskUserQuestion welke skill ze willen pushen. Bekende skills:
- `klantreactie`
- `youforce-ideeen-assistent`
- `hr-cycle-onboarding`
- `release-notes`
- `gmail-drafts`
- `klant-mailing-drafts`
- `check-for-updates`
- `publish-plugin`

### 3. Haal het huidige versienummer op en stel het nieuwe in

```bash
curl -s -H "Authorization: Bearer {github_token}" \
  "https://api.github.com/repos/{owner}/{repo}/releases/latest" \
  | python3 -c "import sys,json; print(json.load(sys.stdin)['tag_name'].lstrip('v'))"
```

Stel het nieuwe versienummer voor als huidige versie + 1 patch (bijv. 0.11.0 → 0.12.0).
Vraag via AskUserQuestion om bevestiging van het versienummer én een korte changelog-tekst (één zin is genoeg — de volledige release notes schrijf jij zelf in stap 3b).

### 3b. Stel de GitHub release notes op

Schrijf zelfstandig een uitgebreide release beschrijving op basis van:
- Welke skill(s) zijn gewijzigd
- Wat er concreet is veranderd (nieuwe functionaliteit, verbeteringen, bugfixes)
- Wat de gebruiker er in de praktijk van merkt

Gebruik dit format:

```
## Wat is er nieuw in v{nieuwe_versie}

### ✨ Nieuwe skills
- **[Skill naam]** — [Wat doet het, in één zin voor een collega die het nog niet kent]

### 🔧 Verbeteringen
- **[Skill naam]**: [Wat is er verbeterd en waarom merk je dat]

### 🐛 Fixes
- **[Skill naam]**: [Wat was er mis en wat werkt nu wel]
```

Laat lege secties weg. Schrijf elke bullet zo dat een collega zonder context begrijpt wat er is veranderd en wat ze ermee kunnen. Vermijd technisch jargon.

### 4. Clone de repo en kopieer de gewijzigde skill(s)

```bash
cd /tmp && rm -rf toolkit-release
git clone https://{github_token}@github.com/{owner}/{repo}.git toolkit-release
git -C /tmp/toolkit-release config user.email "cb.vdbrg@gmail.com"
git -C /tmp/toolkit-release config user.name "Chris van den Berg"
```

Kopieer de gewijzigde skill vanuit de workspace:
```bash
cp "{workspace_path_bash}/{skill-naam}/SKILL.md" \
   "/tmp/toolkit-release/skills/{skill-naam}/SKILL.md"
```

Als er een `references/` map is:
```bash
cp -r "{workspace_path_bash}/{skill-naam}/references/." \
      "/tmp/toolkit-release/skills/{skill-naam}/references/"
```

### 5. README-check — bijwerken indien relevant

Lees de huidige README:
```bash
cat /tmp/toolkit-release/README.md
```

Beoordeel of de README bijgewerkt moet worden op basis van de wijziging:

**Bijwerken als:**
- Er is een **nieuwe skill toegevoegd** → voeg een sectie toe onder "Skills" met hetzelfde format (emoji, naam, Waarvoor, Wanneer gebruiken, Hoe triggeren, Voorbeelden) én voeg een regel toe aan de versiegeschiedenstabel
- Een skill heeft een **nieuwe functionaliteit gekregen** → pas de beschrijving of voorbeelden aan
- De **skill-naam of trigger-zinnen** zijn gewijzigd → pas de voorbeelden aan
- Er is een skill **verwijderd** → verwijder de sectie

**Niet bijwerken als:**
- Alleen interne logica of prompts zijn aangepast zonder zichtbaar verschil voor de gebruiker
- Alleen bugfixes zonder functionele wijziging

Doe de README-update zelfstandig — vraag de gebruiker er niet apart om. Vermeld in de samenvatting wat je hebt bijgewerkt.

### 6. Hoog het versienummer op in plugin.json en check-for-updates

```bash
python3 -c "
import json
p = json.load(open('/tmp/toolkit-release/.claude-plugin/plugin.json'))
p['version'] = '{nieuwe_versie}'
json.dump(p, open('/tmp/toolkit-release/.claude-plugin/plugin.json','w'), indent=2, ensure_ascii=False)
"
sed -i 's/{huidige_versie}/{nieuwe_versie}/g' \
  /tmp/toolkit-release/skills/check-for-updates/SKILL.md
```

### 7. Commit en push

```bash
cd /tmp/toolkit-release
git add -A
git commit -m "feat: {skill-naam} bijgewerkt — v{nieuwe_versie}"
git push https://{github_token}@github.com/{owner}/{repo}.git main
```

### 8. Maak een GitHub release aan

Gebruik de release notes uit stap 3b als `body`. Escape newlines als `\n` in de JSON.

```bash
RELEASE_RESPONSE=$(curl -s -X POST \
  -H "Authorization: Bearer {github_token}" \
  -H "Content-Type: application/json" \
  "https://api.github.com/repos/{owner}/{repo}/releases" \
  -d "{\"tag_name\":\"v{nieuwe_versie}\",\"name\":\"v{nieuwe_versie} — {korte titel}\",\"body\":\"{release_notes_escaped}\",\"draft\":false,\"prerelease\":false}")
RELEASE_ID=$(echo "$RELEASE_RESPONSE" | python3 -c "import sys,json; print(json.load(sys.stdin)['id'])")
echo "Release ID: $RELEASE_ID"
```

Tip: gebruik python3 om de body veilig te escapen:
```bash
python3 -c "
import json, subprocess
body = '''[release notes hier]'''
payload = {
  'tag_name': 'v{nieuwe_versie}',
  'name': 'v{nieuwe_versie} — {korte titel}',
  'body': body,
  'draft': False,
  'prerelease': False
}
print(json.dumps(payload))
" | curl -s -X POST \
  -H "Authorization: Bearer {github_token}" \
  -H "Content-Type: application/json" \
  "https://api.github.com/repos/{owner}/{repo}/releases" \
  -d @-
```

### 9. Bouw het .plugin bestand en upload als release asset

```bash
cd /tmp/toolkit-release
zip -r /tmp/youforce-po-toolkit.plugin .claude-plugin skills -x "*.DS_Store" -x "*/.git/*"

curl -s -X POST \
  -H "Authorization: Bearer {github_token}" \
  -H "Content-Type: application/zip" \
  "https://uploads.github.com/repos/{owner}/{repo}/releases/$RELEASE_ID/assets?name=youforce-po-toolkit.plugin" \
  --data-binary @/tmp/youforce-po-toolkit.plugin \
  | python3 -c "import sys,json; r=json.load(sys.stdin); print('Asset upload:', r.get('state')); print('Download URL:', r.get('browser_download_url'))"
```

### 10. Geef een samenvatting

- Skill gepubliceerd: `{skill-naam}`
- Versie: `v{nieuwe_versie}`
- README bijgewerkt: ja/nee (en wat er is gewijzigd)
- Release URL (uit de API response)
- Download URL: `https://github.com/{owner}/{repo}/releases/latest/download/youforce-po-toolkit.plugin`
- "Collega's kunnen nu `check for updates` typen om de nieuwe versie te installeren."

---

## Notes voor beheerder
- Vervang de token in `.youforce-plugin-config.json` als hij verloopt
- Check token geldigheid: `curl -s -o /dev/null -w "%{http_code}" -H "Authorization: Bearer {token}" https://api.github.com/user` → moet 200 zijn
- De clone-map heet `/tmp/toolkit-release`
