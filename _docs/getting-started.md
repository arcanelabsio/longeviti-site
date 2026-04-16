---
title: Getting started
lead: A 20-minute walkthrough from a fresh install to your first plan showing up in the app.
---

By the end of this guide, you'll have:

1. The Longeviti app installed on your phone
2. The `longeviti-framework` repo cloned locally
3. Claude Code set up and authenticated
4. Your first plan generated from a health report
5. The app showing today's meals, supplements, and shopping list

## Before you start

You'll need:

- An Android 10+ device (iOS coming later — see [Download & install]({{ '/download/' | relative_url }}))
- A Google account (for Drive)
- A computer (Mac, Linux, or Windows) with terminal access
- At least one health input — a blood work report, a nutritionist's plan, or even just a doctor's note in a PDF or photo. Don't have one? See [Prerequisites]({{ '/docs/prerequisites/' | relative_url }}) for what works as a minimum.
- A [Claude Code](https://claude.com/claude-code) subscription (Pro is enough for onboarding)

<div class="callout">
<p><strong>Budget:</strong> ~20 minutes for the first run. Most of that is waiting for Claude to read your reports and generate plans.</p>
</div>

## Step 1 — Install the app

We're not on the Play Store yet, so you'll sideload the APK. It's a two-minute flow — see [Download & install]({{ '/download/' | relative_url }}) for the full walkthrough (allow-unknown-sources, SHA256 verification, update path).

TL;DR:

1. Download `longeviti.apk` from our [latest release](https://github.com/arcanelabsio/longeviti-site/releases/latest).
2. Open it on your phone; approve "Install unknown apps" when prompted.
3. Launch Longeviti, sign in with Google, grant Drive access.

You'll land on a "no plan yet" empty state — that's expected. The framework will fill it in.

## Step 2 — Clone the framework

On your computer:

```bash
git clone https://github.com/arcanelabsio/longeviti-framework.git ~/longeviti-framework
cd ~/longeviti-framework
```

This repo contains the three skills Claude will use. You don't run any code in this folder directly — Claude does.

## Step 3 — Install Claude Code

If you don't already have it:

```bash
# macOS / Linux
curl -fsSL https://claude.com/install.sh | sh
```

Then authenticate:

```bash
claude login
```

Follow the browser flow. Once you're in, navigate back to the framework folder and start a Claude session:

```bash
cd ~/longeviti-framework
claude
```

## Step 4 — Run onboarding

Inside the Claude session, type:

```
/onboard
```

Claude will ask you three things:

1. **Where should it put your health reports on Drive?** Default is `.app/longeviti/user/reports/`. Accept it.
2. **Which reports do you want to include?** Drop your PDFs / photos into `~/Downloads` and tell Claude the paths, or upload them directly to the Drive folder and ask Claude to read from there.
3. **What are your goals?** "Lose 4 kg", "reverse prediabetes", "hit 130g protein a day" — anything concrete.

The `/onboard` skill will then:

- Parse your reports (blood markers, existing plans, medications)
- Write `user/profile.json` with your targets and conditions
- Seed `nutrition-db/foods.json` with a regional food catalog
- Generate four weekly plan JSONs in `plans/`
- Generate `rules.yaml` with food/lifestyle rules derived from your reports
- Upload everything to Drive

You'll see a progress stream in the terminal. Expect ~3–5 minutes depending on how many reports you provided.

## Step 5 — Sync the app

Open the Longeviti app, pull-to-refresh on the Home tab (or just wait — it auto-syncs every few minutes).

You should now see:

- **Home** — today's plan, four-ring progress tracker (diet, supplements, water, sleep)
- **Meals** — breakfast, mid-morning, lunch, afternoon snack, dinner for today
- **Supplements** — your schedule with dose and timing
- **Shopping** — a week's grocery list with categories

Tap any meal to see its ingredients, macros, and compliance dots.

## What's next?

- **[Logging meals]({{ '/docs/logging-meals/' | relative_url }})** — how daily tracking works
- **[Running ELARA]({{ '/docs/running-elara/' | relative_url }})** — weekly coaching rhythm
- **[Adding new reports]({{ '/docs/new-reports/' | relative_url }})** — evolving your plan when new blood work arrives

Stuck? See [Troubleshooting]({{ '/docs/troubleshooting/' | relative_url }}) or [FAQ]({{ '/docs/faq/' | relative_url }}).
