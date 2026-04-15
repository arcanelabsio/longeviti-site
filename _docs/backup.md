---
title: Backup and export
lead: How to keep a copy of your Longeviti data outside Drive — and how to read it later, with or without the app.
---

Your data is already on Google Drive, which has versioning and high durability. That's usually enough. But for medical data, "usually" is a hedge you might not want. Here's how to belt-and-suspenders it.

## Option 1 — Drive's built-in versioning

For every JSON file, Drive keeps 30 days of version history automatically. To see it:

1. Right-click the file on drive.google.com
2. "File information" → "Version history"

Restore any version with one click. This covers accidental deletions, app bugs, or you fat-fingering a manual edit.

**Limitation:** only 30 days and Drive must still exist (account-level loss isn't covered).

## Option 2 — Drive backup via Google Takeout

Quarterly is enough for most users.

1. Go to [takeout.google.com](https://takeout.google.com)
2. Deselect everything except Drive
3. Click "Select drive content" and scope to `.app/longeviti/` only
4. Request the export, download when ready, store offline

Takeout gives you a zip of every file as-is. Restoring means unzipping, dropping back into Drive, waiting for the app to re-sync.

## Option 3 — rclone to local disk or another cloud

If you already use `rclone` for the framework, you have the tools:

```bash
# Local copy
rclone sync gdrive:.app/longeviti ~/longeviti-backup

# To another cloud
rclone sync gdrive:.app/longeviti b2:my-longeviti-bucket
```

Add a cron entry to do this weekly:

```
0 3 * * 0 rclone sync gdrive:.app/longeviti ~/longeviti-backup --log-file ~/rclone.log
```

## Option 4 — Git-backed history

If you want every change recorded with a commit message, use a git repo as your backup target:

```bash
cd ~/longeviti-backup
git init
rclone sync gdrive:.app/longeviti .
git add .
git commit -m "Backup 2026-04-16"
```

Run this weekly. You'll have a full git log of how your plans and tracking evolved, which is surprisingly useful when looking back at why a particular rule was added.

<div class="callout callout--warn">
<p><strong>Do not push this git repo to a public host.</strong> It contains your full health history. Private GitHub / GitLab / self-hosted Gitea only.</p>
</div>

## Reading your data without the app

Every file Longeviti produces is human-readable:

- **JSON files** — open in any editor or `jq`
- **Markdown plans** — open in any editor
- **YAML rules** — open in any editor
- **PDF reports** — they're your originals

No proprietary formats. No encryption you don't control. If Longeviti the project vanishes tomorrow, your data keeps working.

## Exporting a specific slice

### Your tracking as CSV

```bash
jq -r '[.date, .meals | length, .water_ml, .sleep_hours] | @csv' \
  ~/longeviti-backup/tracking/*.json > tracking.csv
```

### Your coaching history as a single document

```bash
cat ~/longeviti-backup/elara/session_*.json \
  | jq -r '"\n# Session " + .date + "\n\n" + .summary + "\n\n" + (.patterns | join("\n"))' \
  > coaching-log.md
```

### Your food catalog

```bash
jq '.[] | {name, calories, protein_g}' ~/longeviti-backup/nutrition-db/foods.json \
  > foods-summary.json
```

These recipes are starting points — `jq` gives you everything else.

## Sharing with a doctor or coach

The lowest-friction way:

1. Right-click `.app/longeviti/user/` on Drive → Share → give them "Viewer" access
2. Revoke when the consultation is done

If you want to give them a snapshot rather than live access, run a Takeout export and email the zip (encrypted, preferably).

## What you cannot recover

- **Local-only tracking that never synced.** If you logged meals offline and the app was deleted before syncing, those logs are gone. The 10-second debounce means this is an edge case — don't uninstall mid-write.
- **Reports you deleted.** There's no trash for `user/reports/` beyond Drive's 30-day trash. Empty the trash and it's gone.

## Restoring from backup

1. Copy your backup back to `.app/longeviti/` on Drive (any method above, reversed)
2. Open the app — it'll re-sync automatically on next launch
3. Run `/elara` once to regenerate the current coaching session if you want it fresh

Restoration is idempotent — you can overwrite an existing Drive folder with a backup safely.

## Next

- **[Schemas]({{ '/docs/schemas/' | relative_url }})** — the formats every backed-up file conforms to
