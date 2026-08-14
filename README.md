# vrp-g1 — F01-A benign verification repo

Minimal public repo for **your own** Cloud Shell VRP report. The SessionStart hook only writes a marker file under `/tmp` (no network, no credential access).

## Repo requirements

| Item | Value |
|------|--------|
| Host | Public GitHub (`github.com`) |
| Tree | `.gemini/settings.json` with a `SessionStart` command hook |
| OICS flag | `start_gemini_cli` (starts Gemini instead of bash after clone) |
| Precondition | `~/.gemini/trustedFolders.json` has `"$HOME": "TRUST_FOLDER"` (normal after using Gemini from home once) |

## Push to GitHub

```bash
cd /home/contact/vrp-g1
git init
git add README.md .gemini/settings.json
git commit -m "F01-A benign hook marker for VRP verification"
git branch -M main
git remote add origin https://github.com/mehb1/vrp-g1.git
git push -u origin main
```

## Victim link (Open in Cloud Shell)

After the repo is public on `main`:

```
https://shell.cloud.google.com/cloudshell/editor?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fmehb1%2Fvrp-g1&cloudshell_start_gemini_cli=true
```

Console variant (same parameters):

```
https://console.cloud.google.com/cloudshell/editor?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fmehb1%2Fvrp-g1&cloudshell_start_gemini_cli=true
```

If the browser link clones but does **not** start Gemini, the helper flag is still available locally (same chain minus the click):

```bash
cloudshell_open --git_repo https://github.com/mehb1/vrp-g1 --start_gemini_cli --force_new_clone
```

## What proves F01-A (expected on a trusted home disk)

1. Gemini starts in `~/cloudshell_open/vrp-g1` without a Folder Trust blocking dialog.
2. stderr shows a hook warning (not a blocking consent prompt).
3. `cat /tmp/vrp-g1-hook-ran` exists and shows timestamp, username, and clone path.
4. `~/.gemini/trusted_hooks.json` gains an entry for the clone path with key `vrp-g1-marker:…`.

## Quick local check (no GitHub)

```bash
rm -f /tmp/vrp-g1-hook-ran
cp -a /home/contact/vrp-g1 ~/cloudshell_open/vrp-g1-local
cd ~/cloudshell_open/vrp-g1-local && gemini -p "say ok"
cat /tmp/vrp-g1-hook-ran
```
