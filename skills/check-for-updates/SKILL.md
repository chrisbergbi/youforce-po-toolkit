---
description: >
  Check for updates to the Youforce PO Toolkit plugin and install the latest version with one click.
  Use this skill when the user says "check for updates", "is there a new version", "update de plugin",
  "nieuwe versie beschikbaar?", or "plugin updaten".
---

# Check for updates — Youforce PO Toolkit

Check the GitHub releases API for the latest version of this plugin, compare it to the installed version, and present the new `.plugin` file if an update is available.

## Steps

1. Read the installed version from the plugin manifest:
   - The current version is defined in `.claude-plugin/plugin.json` as `"version": "0.1.0"`
   - Store this as `installed_version`

2. Fetch the latest release from GitHub:
   ```
   GET https://api.github.com/repos/GITHUB_OWNER/youforce-po-toolkit/releases/latest
   ```
   - Replace `GITHUB_OWNER` with the actual GitHub username or org (update this when the repo is created)
   - Parse `tag_name` (e.g. `v0.2.0`) and strip the leading `v` → `latest_version`
   - Find the asset where `name` ends in `.plugin` and store its `browser_download_url`

3. Compare versions:
   - If `latest_version` == `installed_version`: tell the user the plugin is up to date. Done.
   - If `latest_version` > `installed_version`: proceed to step 4.

4. Download the new plugin:
   - Use `mcp__workspace__bash` to download:
     ```bash
     curl -L "{browser_download_url}" -o "/tmp/youforce-po-toolkit-{latest_version}.plugin"
     cp "/tmp/youforce-po-toolkit-{latest_version}.plugin" "/sessions/.../mnt/CLAUDE AI/youforce-po-toolkit-{latest_version}.plugin"
     ```
   - Then call `mcp__cowork__present_files` with the downloaded file path.

5. Tell the user:
   > "Er is een nieuwe versie beschikbaar: **v{latest_version}** (geïnstalleerd: v{installed_version}). Klik op 'Save plugin' om te updaten."

## Notes

- If the GitHub API returns a rate-limit error (403), ask the user to try again later.
- If the repo is private, the user needs a GitHub token. Ask them to provide one and pass it as `Authorization: Bearer {token}` in the curl call.
- After installation, the user may need to restart Cowork for the new skill versions to activate.
