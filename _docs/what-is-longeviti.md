---
title: What is Longeviti?
lead: A personal longevity system where your health reports drive an adaptive meal, supplement, and coaching plan — stored entirely in your own Google Drive.
---

Longeviti is built on two premises:

1. **You already have the data a nutritionist would use** — blood work, doctor's notes, prior meal plans. Most of it sits unused in folders and photo rolls.
2. **You already pay for a powerful AI assistant** — Claude, Codex, or similar. That same assistant can read your health data and produce a plan, if given the right schema to write to.

Longeviti is the bridge. The app renders and logs. Your AI reasons and plans. Google Drive is the filesystem. We wrote the schemas — the contract that lets your AI produce files our app understands.

## The zero-compute app

The Longeviti app is a **renderer and logger** — nothing more. It:

- Displays your weekly plan
- Logs your daily meals, vitals, and workouts
- Checks food compliance against a deterministic rule engine (no AI)
- Syncs everything to your Drive

It does **no AI inference of any kind**. No LLM SDKs are linked into the binary. See [Bring your own AI]({{ '/docs/bring-your-own-ai/' | relative_url }}) for the full architectural rationale.

## The two-repo model

Longeviti has two moving parts you'll interact with:

| Part | What it is | Where it lives |
|---|---|---|
| **The app** | A Flutter mobile app — renderer and logger only. Zero AI compute. | Your Android phone (sideloaded — iOS planned) |
| **The framework** | Skills (`/onboard`, `/elara`, `/atlas`) for Claude Code or Codex that author and evolve your plan | A repo you clone locally; runs in *your* AI CLI |

Between them sits the contract — a set of JSON schemas and a folder on your Google Drive. The app only *reads* plans; the skills (running in your AI) *write* them. Tracking data flows the other way: the app writes it, your AI reads it to produce coaching.

<div class="callout">
<p><strong>Why two repos?</strong> Separating the intelligence layer (Claude skills, which change as prompting evolves) from the rendering layer (a signed mobile app, which ships slowly through app stores) lets each side evolve independently. You can upgrade your coaching the moment a new skill lands — no app update needed.</p>
</div>

## The three skills

All plan intelligence runs as skills you invoke from *your* AI CLI (Claude Code, Codex, or compatible). The framework ships both `/command` (Claude) and `$command` (Codex) variants:

- <span class="cmd">/onboard</span> — first-time setup. Reads your health reports, builds your profile, and generates an initial plan.
- <span class="cmd">/elara</span> — your weekly coach. Reviews new reports and your tracking data, derives insights, and tells ATLAS what to change.
- <span class="cmd">/atlas</span> — the plan author. Takes directives from ELARA and writes new meal plans, supplement schedules, and rules to Drive.

ELARA is the intelligence. ATLAS is the hands. The app is the window. Your AI assistant is the one actually doing the thinking — we just told it what to do and what schemas to write to.

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

- **No Longeviti inference.** We don't run an AI service. We don't have servers. All inference happens in *your* AI assistant's session — see [Bring your own AI]({{ '/docs/bring-your-own-ai/' | relative_url }}).
- **No proprietary formats.** Every file is JSON, Markdown, or YAML. You can read and edit them in any text editor.
- **No vendor lock-in.** Delete the app and the framework — your Drive folder is still yours, fully readable. Switch AI assistants anytime; the schemas don't care.

## Who is this for?

- People whose nutritionist gives them PDF meal plans but no way to track adherence.
- People *without* a nutritionist who want a real plan from their blood work.
- Engineers who want to read the schemas and bend the system to their own protocols.

If that's you, grab the [APK]({{ '/download/' | relative_url }}) and head to [Getting started]({{ '/docs/getting-started/' | relative_url }}).
