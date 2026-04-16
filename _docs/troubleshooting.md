---
title: Troubleshooting
lead: Common problems, fast fixes.
---

## App doesn't show a plan after onboarding

**Check in order:**

1. **Is the Drive folder populated?** Open Drive in a browser, go to `.app/longeviti/plans/`. You should see four `week_*.json` files and a `rules.yaml`. If not, onboarding didn't complete — re-run `/onboard`.
2. **Are you signed into the same Google account?** The app uses OAuth scoped to the signed-in account. If you onboarded on one account and signed into the app with another, there's nothing to sync.
3. **Pull to refresh.** The sync isn't always instant. A manual refresh on the Home tab forces a resync.
4. **Check the sync-gate screen.** Kill the app and relaunch. You'll see sync progress — any step that fails will say so.
5. **Clear app cache.** Settings → Apps → Longeviti → Storage → Clear cache. Relaunch. The app will re-download everything from Drive.

## `/elara` says "no new data to analyze"

Usually means one of:

- **You haven't logged anything this week.** ELARA needs tracking data to analyze. Log at least 3 days.
- **Previous session was < 12 hours ago.** ELARA rate-limits itself to avoid thrashing. Wait, or pass `--force`.
- **Drive is out of sync.** Check that your local rclone or service account can see the latest tracking JSONs.

## `/atlas` refuses a directive

ATLAS rejects directives that would:

- Violate a hard rule in `rules.yaml`
- Produce a plan that fails schema validation
- Write outside its allowed paths

The error message will name the specific violation. Fix the directive (or the rule) and retry.

## Schema validation error

```
ValidationError: 'meals' is a required property at #/diet/sample_days/0
```

A plan file is missing a required field. This almost always means an old ATLAS run produced a file before a schema change. Fix:

```bash
cd ~/longeviti-framework
python scripts/validate_schemas.py plans/
# See which file(s) fail, then regenerate:
```

In Claude: `/atlas regenerate week_2026_04_13.json from scratch`.

## The app is showing yesterday's plan

Plans are keyed by week-start date (Monday). The app reads the plan whose `week_start` covers today. If it's showing stale data:

- **Check your device date.** Off-clock devices pick the wrong week.
- **Check if this week's plan exists.** If ATLAS only generated 4 weeks and you've run past that, you're out of plan. Run `/elara` — it will call ATLAS to roll the window forward.

## "Food not found" when trying to swap

Your swap candidate isn't in `nutrition-db/foods.json`. Options:

1. **Use "Add new"** in the app's food picker. Enter macros manually. Logged as a private entry.
2. **Add it to the shared catalog** via ATLAS:
   ```
   /atlas add to nutrition-db: "paneer tikka" — macros per 100g: 265 kcal, 18g protein, 8g carbs, 19g fat
   ```
   ATLAS appends it, app picks it up on next sync.

## Drive sync is slow

The Tracking sync is bidirectional with 10-second debounce. If you're logging rapid-fire, you'll see a delay. It's not broken — it's debouncing.

If sync has been stuck for > 5 minutes:

1. Check network
2. Check Drive storage quota (free tier is 15 GB; a full Longeviti folder is tiny, but you might be out for other reasons)
3. Force-resync: Settings → Data → Force full sync

## ELARA produces a coaching session with conflicting recommendations

Two flavors of this:

**"ELARA says eat more carbs but my rule says carbs < 40%."**
The rule wins. ELARA should respect hard rules — if it's violating one, that's a bug. Report it at [github.com/arcanelabsio/longeviti-framework/issues](https://github.com/arcanelabsio/longeviti-framework/issues).

**"ELARA says swap dinner A for dinner B, but I don't like dinner B."**
Preferences aren't rules unless they're in `profile.json`. Run:
```
/elara add "I don't like quinoa" to my preferences and retry
```

## Can't authenticate Claude Code

```bash
claude logout
claude login
```

If that fails, check your Claude subscription status. Pro tier is required for the skills to run at realistic token volumes.

## The site itself is broken

You're reading this on [longeviti.app](https://longeviti.app). If the site is misbehaving (404s, broken styles), report it at [github.com/arcanelabsio/longeviti-site/issues](https://github.com/arcanelabsio/longeviti-site/issues). This site is independent from the app, so a site outage doesn't affect your data or the app itself.

## Something else

File an issue:

- **App bugs & feature requests** → [github.com/arcanelabsio/longeviti-site/issues](https://github.com/arcanelabsio/longeviti-site/issues) (label: `app`)
- **Framework / skill bugs** → [github.com/arcanelabsio/longeviti-framework/issues](https://github.com/arcanelabsio/longeviti-framework/issues)
- **Docs issues** → [github.com/arcanelabsio/longeviti-site/issues](https://github.com/arcanelabsio/longeviti-site/issues)
