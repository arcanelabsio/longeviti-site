---
title: Google Drive layout
lead: Every file Longeviti creates, where it lives, who writes it, and who reads it.
---

Longeviti's entire data layer is a single folder on your Google Drive: `.app/longeviti/`. This page is the map.

## The full tree

```
.app/longeviti/
├── user/
│   ├── profile.json                  ← your identity, targets, conditions, meds
│   └── reports/                      ← health reports you drop in
│       ├── 2026-04-16_blood-work.pdf
│       └── 2026-04-20/
│           ├── doctor-note.pdf
│           └── lipid-panel.pdf
├── plans/
│   ├── week_2026_04_13.md            ← human-readable plan
│   ├── week_2026_04_13.json          ← app reads this
│   ├── week_2026_04_20.md
│   ├── week_2026_04_20.json
│   ├── week_2026_04_27.md
│   ├── week_2026_04_27.json
│   ├── week_2026_05_04.md
│   ├── week_2026_05_04.json
│   └── rules.yaml                    ← active food/lifestyle rules
├── tracking/
│   ├── day_2026-04-13.json
│   ├── day_2026-04-14.json
│   └── …                             ← one file per day, forever
├── elara/
│   ├── session_2026-04-07.json
│   ├── session_2026-04-14.json
│   └── …                             ← one per coaching run
└── nutrition-db/
    └── foods.json                    ← food catalog with macros + rule tags
```

## Ownership matrix

| Path | Written by | Read by | Notes |
|---|---|---|---|
| `user/profile.json` | ELARA + you | App (read-only), ELARA, ATLAS | Single source of truth |
| `user/reports/` | You | ELARA only | Reports are input-only |
| `plans/week_*.md` | ATLAS | Humans | Not read by app |
| `plans/week_*.json` | ATLAS | App, ELARA | Schema-validated |
| `plans/rules.yaml` | ATLAS | App, ELARA | Rule engine source |
| `tracking/day_*.json` | App | App, ELARA | Never overwritten by skills |
| `elara/session_*.json` | ELARA | App, ELARA | Append-only |
| `nutrition-db/foods.json` | ATLAS | App, ELARA | Shared catalog |

<div class="callout">
<p><strong>One folder means one mental model.</strong> You can back up Longeviti by copying <span class="mono">.app/longeviti/</span>. You can share it with a doctor by sharing that one folder. You can delete everything by deleting that folder. There are no databases, no auxiliary services, no hidden state.</p>
</div>

## Sync directions

The app syncs with Drive using five separate sync clients, each scoped to a subfolder:

| Client | Path | Direction | Conflict resolution |
|---|---|---|---|
| Plans | `plans/` | Pull (Drive → app) | remoteWins |
| User | `user/` | Pull | remoteWins |
| Tracking | `tracking/` | Bidirectional | newerWins |
| Coaching | `elara/` | Pull | remoteWins |
| Nutrition | `nutrition-db/` | Pull | remoteWins |

**Pull-only** means the app will overwrite its local copy if Drive is newer, but never pushes. That guarantees the app can never corrupt plan or profile data.

**Bidirectional + newerWins** on `tracking/` means you can log from multiple devices — whichever wrote last survives per-day. You don't usually have two-device conflicts because each day is a separate file.

## File permissions

The app requests Drive access scoped to `.app/` (the parent folder). Google's OAuth grants:

- Read and write **inside `.app/longeviti/`**
- No access to the rest of your Drive
- No access to other Google services (Gmail, Calendar, Photos)

You can revoke access anytime at [myaccount.google.com/permissions](https://myaccount.google.com/permissions). The app will stop syncing; your data stays on Drive.

## Framework access

The Claude Code skills access Drive via one of:

- **`rclone`** with a Drive remote configured once during `/onboard`
- **A Service Account JSON** stored at `~/.config/longeviti/credentials.json`

Both methods respect the same folder scoping. You can inspect and revoke these independently from the app's access.

## Mobile storage

On your phone, the app keeps a cache of downloaded plans, nutrition, and coaching so daily use works offline. This cache is in the app's sandboxed storage (not Drive-visible) and is wiped if you uninstall. Tracking is *also* cached there and debounce-synced back to Drive every 10 seconds after a write.

## Migration paths

### Moving to a new phone
Install the app, sign in with the same Google account, grant Drive access. Everything syncs back down. Your tracking history is intact.

### Moving to a new Google account
Copy `.app/longeviti/` from old Drive to new Drive (share folder to yourself, then "make a copy"). Sign into app with new account. Point your framework tooling at the new Drive.

### Going offline permanently
Download the whole folder as a zip. You lose syncing, coaching, and plan updates — but your data is yours, readable with any tool that opens JSON.

## Next

- **[Privacy model]({{ '/docs/privacy/' | relative_url }})** — what leaves your device and what doesn't
- **[Backup and export]({{ '/docs/backup/' | relative_url }})** — safeguarding your data
