---
title: What is Longeviti?
lead: A personal longevity system where your health reports drive an adaptive meal, supplement, and coaching plan — stored entirely in your own Google Drive.
---

Longeviti is built on a simple premise: **you already have the data a nutritionist would use — blood work, doctor's notes, prior meal plans.** Most of it sits unused in folders and photo rolls. Longeviti reads that data and turns it into a living plan that adapts as your health changes.

## The two-repo model

Longeviti has two moving parts you'll interact with:

| Part | What it is | Where it lives |
|---|---|---|
| **The app** | A Flutter mobile app that renders your plan, logs daily tracking, and shows coaching insights | On your phone (Android / iOS) |
| **The framework** | Claude Code skills (`/onboard`, `/elara`, `/atlas`) that author and evolve your plan | A repo you clone locally |

Between them sits the contract — a set of JSON schemas and a folder on your Google Drive. The app only *reads* plans; the framework only *writes* them. Tracking data flows the other way: the app writes it, the framework reads it to produce coaching.

<div class="callout">
<p><strong>Why two repos?</strong> Separating the intelligence layer (Claude skills, which change as prompting evolves) from the rendering layer (a signed mobile app, which ships slowly through app stores) lets each side evolve independently. You can upgrade your coaching the moment a new skill lands — no app update needed.</p>
</div>

## The three skills

All plan intelligence runs as Claude Code skills you invoke from your terminal:

- <span class="cmd">/onboard</span> — first-time setup. Reads your health reports, builds your profile, and generates an initial plan.
- <span class="cmd">/elara</span> — your weekly coach. Reviews new reports and your tracking data, derives insights, and tells ATLAS what to change.
- <span class="cmd">/atlas</span> — the plan author. Takes directives from ELARA and writes new meal plans, supplement schedules, and rules to Drive.

ELARA is the intelligence. ATLAS is the hands. The app is the window.

## What lives on Google Drive

Everything. Longeviti has **no backend servers**. Your data is in a single folder on your Drive:

```
.app/longeviti/
├── user/              ← profile.json, health reports you drop in
├── plans/             ← weekly plan JSONs, rules.yaml
├── tracking/          ← daily meal/vitals logs (app writes here)
├── elara/             ← coaching session outputs
└── nutrition-db/      ← food catalog with macros and rules
```

The app uses OAuth to read and write inside *this folder only*. The framework runs locally on your machine and reads/writes the same folder using `rclone` or Drive API credentials you configure once.

## What's not negotiable

- **No cloud inference on your health data.** Everything the skills do runs on your machine, in your Claude Code session.
- **No proprietary formats.** Every file is JSON, Markdown, or YAML. You can read and edit them in any text editor.
- **No vendor lock-in.** Delete the app and the framework — your Drive folder is still yours, fully readable.

## Who is this for?

- People whose nutritionist gives them PDF meal plans but no way to track adherence.
- People *without* a nutritionist who want a real plan from their blood work.
- Engineers who want to read the schemas and bend the system to their own protocols.

If that's you, head to [Getting started]({{ '/docs/getting-started/' | relative_url }}).
