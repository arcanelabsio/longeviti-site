---
title: First-time setup (onboarding)
lead: A deep dive into what /onboard does, what it asks, and what lands on your Drive when it's done.
---

The `/onboard` skill is the one-time bootstrap. You'll run it once when you first set up Longeviti, and rarely after that — day-to-day plan updates go through `/elara` instead.

## What /onboard does, in order

1. **Configures your Drive path.** Confirms `.app/longeviti/` as the root and creates the subfolders if they don't exist.
2. **Reads your health reports.** Parses PDFs, images, and text notes. Extracts blood markers, conditions, medications, and any existing meal plans.
3. **Builds your profile.** Writes `user/profile.json` with targets (calories, protein, carbs, fat, fiber, water), conditions, meds, and demographics.
4. **Seeds the nutrition database.** Copies a regional food catalog into `nutrition-db/foods.json`. You'll pick the region during the interview (South Indian, North Indian, Mediterranean, Standard American, etc.).
5. **Adds regional composite foods.** Things like "dosa + sambar + chutney" as one entry, with combined macros — the app can log these with a single tap.
6. **Generates weekly plan MDs.** One Markdown file per week in `plans/`, human-readable.
7. **Generates weekly plan JSONs.** The machine-readable form the app consumes.
8. **Writes rules.yaml.** Food rules derived from your conditions — e.g., "no added sugar" if you're prediabetic, "sodium < 2g/day" if hypertensive.
9. **Syncs everything to Drive.** Uses the Drive API credentials you configured.

## The interview

`/onboard` is conversational. It will ask, in this order:

### 1. Drive root
> "Where should your Longeviti data live? Default is `.app/longeviti/`."

Accept the default unless you have a reason to change it. This path is also hard-coded in the app, so changing it means the app won't find your data.

### 2. Reports
> "Point me to your health reports. You can paste paths, drop files in `user/reports/` on Drive, or share a folder."

You can mix sources. For each report Claude will summarize what it extracted before moving on — check these summaries. If a marker is wrong, say so and Claude will re-parse.

### 3. Goals
> "What are you optimizing for? Weight, biomarkers, energy, longevity in general?"

Be specific. "Drop HbA1c from 6.2 to below 5.7" produces a very different plan from "eat healthier." If you have multiple goals, rank them.

### 4. Regional preference
> "Which food catalog should I use as your base?"

This determines the default meals and the shopping list. You can add cuisines later, but picking the right base saves rework.

### 5. Constraints
> "Any foods you can't or won't eat? Allergies, dietary patterns (vegetarian, pescatarian, halal, kosher), cultural preferences?"

These become hard rules in `rules.yaml`. The rule engine will flag any food that violates them.

## What you'll see on Drive when it's done

```
.app/longeviti/
├── user/
│   ├── profile.json                 ← your targets, conditions, meds
│   └── reports/                     ← the files you handed in
├── plans/
│   ├── week_2026_04_13.md           ← human-readable
│   ├── week_2026_04_13.json         ← app reads this
│   ├── week_2026_04_20.md
│   ├── week_2026_04_20.json
│   ├── week_2026_04_27.md
│   ├── week_2026_04_27.json
│   ├── week_2026_05_04.md
│   ├── week_2026_05_04.json
│   └── rules.yaml                   ← active food/lifestyle rules
├── nutrition-db/
│   └── foods.json                   ← food catalog with macros + rule tags
└── tracking/                        ← empty until you log your first day
```

## Re-running /onboard

Usually you don't. If your goals or region change significantly, prefer:

- **`/elara`** — for plan updates driven by new reports or patterns
- **`/atlas`** — directly, if you know exactly what rule or meal to change

Re-running `/onboard` will overwrite your profile and rules. It will *not* touch your tracking data, but it will regenerate plans from week-N forward.

<div class="callout callout--warn">
<p><strong>Back up before re-onboarding.</strong> Copy the entire <span class="mono">.app/longeviti/</span> folder somewhere safe. Onboarding is a bulk rewrite — if something surprises you, the backup is your escape hatch.</p>
</div>

## Next

- **[Uploading health reports]({{ '/docs/health-reports/' | relative_url }})** — formats, OCR quality, multi-report timelines
- **[Profile and targets]({{ '/docs/profile/' | relative_url }})** — what lives in `profile.json` and when to edit it by hand
