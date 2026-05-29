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
- `check-for-updates`
- `publish-plugin`

### 3. Haal het huidige versienummer op en stel het nieuwe in

```bash
curl -s -H "Authorization: Bearer {github_token}" \
  "https://api.github.com/repos/{owner}/{repo}/releases/latest" \
  | python3 -c "import sys,json; print(json.load(sys.stdin)['tag_name'].lstrip('v'))"
```

Stel het nieuwe versienummer voor als huidige versie + 1 patch (bijv. 0.8.0 → 0.9.0).
Vraag via AskUserQuestion om bevestiging van het versienummer én een korte changelog-tekst.

### 4. Clone de repo en kopieer de gewijzigde skill(s)

```bash
cd /tmp && rm -rf po-toolkit-publish
git clone https://{github_token}@github.com/{owner}/{repo}.git po-toolkit-publish
git -C po-toolkit-publish config user.email "cb.vdbrg@gmail.com"
git -C po-toolkit-publish config user.name "Chris van den Berg"
```

Kopieer de gewijzigde skill vanuit de workspace:
```bash
cp "{workspace_path_bash}/{skill-naam}/SKILL.md" \
   "/tmp/po-toolkit-publish/skills/{skill-naam}/SKILL.md"
```

Als er een `references/` map is:
```bash
cp -r "{workspace_path_bash}/{skill-naam}/references/." \
      "/tmp/po-toolkit-publish/skills/{skill-naam}/references/"
```

### 5. Hoog het versienummer op in plugin.json en check-for-updates

```bash
python3 -c "
import json
p = json.load(open('/tmp/po-toolkit-publish/.claude-plugin/plugin.json'))
p['version'] = '{nieuwe_versie}'
json.dump(p, open('/tmp/po-toolkit-publish/.claude-plugin/plugin.json','w'), indent=2, ensure_ascii=False)
"
sed -i 's/{huidige_versie}/{nieuwe_versie}/g' \
  /tmp/po-toolkit-publish/skills/check-for-updates/SKILL.md
```

### 6. Commit en push

```bash
cd /tmp/po-toolkit-publish
git add -A
git commit -m "feat: {skill-naam} bijgewerkt — v{nieuwe_versie}"
git push https://{github_token}@github.com/{owner}/{repo}.git main
```

### 7. Maak een GitHub release aan

```bash
RELEASE_RESPONSE=$(curl -s -X POST \
  -H "Authorization: Bearer {github_token}" \
  -H "Content-Type: application/json" \
  "https://api.github.com/repos/{owner}/{repo}/releases" \
  -d "{\"tag_name\":\"v{nieuwe_versie}\",\"name\":\"v{nieuwe_versie} — {skill-naam} bijgewerkt\",\"body\":\"{changelog}\",\"draft\":false,\"prerelease\":false}")
RELEASE_ID=$(echo "$RELEASE_RESPONSE" | python3 -c "import sys,json; print(json.load(sys.stdin)['id'])")
echo "Release ID: $RELEASE_ID"
```

### 8. Bouw het .plugin bestand en upload als release asset

```bash
cd /tmp/po-toolkit-publish
zip -r /tmp/youforce-po-toolkit.plugin .claude-plugin skills

curl -s -X POST \
  -H "Authorization: Bearer {github_token}" \
  -H "Content-Type: application/zip" \
  "https://uploads.github.com/repos/{owner}/{repo}/releases/$RELEASE_ID/assets?name=youforce-po-toolkit.plugin" \
  --data-binary @/tmp/youforce-po-toolkit.plugin \
  | python3 -c "import sys,json; r=json.load(sys.stdin); print('Asset upload:', r.get('state')); print('Download URL:', r.get('browser_download_url'))"
```

### 9. Geef een samenvatting

- Skill gepubliceerd: `{skill-naam}`
- Versie: `v{nieuwe_versie}`
- Release URL (uit de API response)
- Download URL: `https://github.com/{owner}/{repo}/releases/latest/download/youforce-po-toolkit.plugin`
- "Collega's kunnen nu deze URL gebruiken om de plugin te installeren, of `check for updates` typen."

---

## Notes voor beheerder
- Vervang de token in `.youforce-plugin-config.json` als hij verloopt
- Check token geldigheid: `curl -s -o /dev/null -w "%{http_code}" -H "Authorization: Bearer {token}" https://api.github.com/user` → moet 200 zijn
