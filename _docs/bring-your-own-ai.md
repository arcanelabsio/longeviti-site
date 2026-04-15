---
title: Bring your own AI assistant
lead: The Longeviti app does zero AI compute. All plan generation, coaching, and reasoning runs through your own AI assistant — invoked on your machine, billed to your account, with data you control.
---

Most health apps with an "AI" label send your data to their servers, run inference on their models, and send results back. You pay for that inference implicitly (in your subscription or in your data). Longeviti inverts this: **the intelligence is yours, not ours.**

## What the app does

- Renders your plan (meals, supplements, shopping list)
- Logs your daily tracking (meals, vitals, workouts)
- Evaluates your food log against a deterministic rule engine (no AI)
- Syncs everything to your Google Drive

That's it. No LLM calls. No inference servers. No hidden ML model on-device.

## What your AI assistant does

Everything that actually requires reasoning:

- Reads your health reports (blood work, doctor's notes, nutritionist plans)
- Builds your profile and targets
- Generates weekly meal plans and supplement schedules
- Derives food rules from your conditions
- Analyzes your tracking data weekly and produces coaching
- Evolves your plan as new reports arrive

This all happens in **your AI assistant's session**, on your machine, paid for out of your own subscription — not ours (we don't have one).

## The contract: schemas

How can "your AI" produce plans that "our app" can render? Because the app publishes a **formal contract** — JSON Schemas in [`data-framework/schemas/`](https://github.com/ajitgunturi/longeviti-framework/tree/main/schemas):

| Schema | Contract |
|---|---|
| `weekly_plan.schema.json` | What a weekly plan must contain to be renderable |
| `profile.schema.json` | What user profile fields the app reads |
| `coaching_session.schema.json` | What coaching output the app displays |
| `rules.schema.json` | What the rule engine understands |

As long as your AI writes files conforming to these schemas into your Drive's `.app/longeviti/` folder, the app will render them. The reasoning model behind those files is **your choice**.

## Supported AI assistants

### First-class — Claude Code
The framework ships with [Claude Code skills](https://claude.com/claude-code) (`/onboard`, `/elara`, `/atlas`) that encapsulate best-practice prompts, schema validation, and the orchestration logic. This is the fastest path.

Cost: your Claude Pro subscription (~$20/mo handles weekly use).

### First-class — Codex
The same skills are available as [Codex](https://github.com/openai/codex) variants (`$onboard`, `$elara`, `$atlas`) for users who prefer OpenAI's stack.

Cost: your OpenAI plan.

### Any Claude-compatible CLI
If you use a different AI CLI that can read files, write files, and invoke skills/tools, the framework's skill definitions are plain Markdown — adapt them.

### Local models (experimental)
Longeviti's skills run in any agentic CLI that wires a capable LLM to tool calls. In practice, plan generation needs strong reasoning (think Claude Sonnet, GPT-4, or comparable). Smaller local models (7B–13B class) produce plans that often fail schema validation. Community experiments with larger local models (70B+) via Ollama or LM Studio exist but aren't officially supported.

The bar is: **your model must produce valid JSON conforming to the schemas, every time.** If it can't, the ATLAS layer will reject its output and the app won't see malformed plans.

## Why this matters

### Privacy
Your reports never transit a Longeviti server (there isn't one). They go from your Drive → your AI assistant's API (Anthropic, OpenAI, or local) → back to your Drive. Longeviti is not in that loop.

### Cost
You're not paying us for inference we claim as our own. You pay your AI provider directly, at their published rates, with full visibility.

### Flexibility
If Anthropic ships a better model next month, you use it — no waiting for us to upgrade. If you prefer OpenAI's approach to health reasoning, switch. Your plan portability isn't contingent on our roadmap.

### Auditability
Every prompt used by the framework is a Markdown file in the public repo. You can read exactly what your AI was told to do. Compare that with opaque server-side prompts in most "AI health" products.

### Portability
The app, the schemas, and the skills are open source and MIT-licensed. If we disappeared tomorrow, you'd still have:
- Your data (on your Drive)
- Your plans (readable JSON)
- Your skill prompts (Markdown files)
- The app binary (already on your phone)

Your AI assistant doesn't care if Longeviti exists.

## The responsibility trade-off

This model gives you power and expects competence. You're responsible for:

- **Having an AI subscription.** If you cancel Claude, the system stops evolving your plan.
- **Running skills on a cadence.** No one else is going to run `/elara` for you. If you stop, coaching stops.
- **Reading the schemas** if you want to extend or customize. We document them clearly, but they're a technical artifact.

If you'd rather pay a service to do all of this invisibly, Longeviti is probably not the right fit. If you want to *see* how the sausage is made and *own* the machine, it's the right fit.

## What the app literally cannot do

Even if we wanted to, the app cannot:

- **Call an LLM.** There's no SDK for Anthropic, OpenAI, or any inference provider linked into the app binary.
- **Run inference locally.** No on-device ML model ships in the APK.
- **Route your data through a "helpful" intermediate service.** There is no intermediate service.

You can verify this — the source is at [github.com/ajitgunturi/longeviti](https://github.com/ajitgunturi/longeviti). Grep for `anthropic`, `openai`, `tflite`, `onnx` — you'll find nothing that does inference.

## The bottom line

Longeviti is a **renderer and logger**. Your AI assistant is the **planner and coach**. Google Drive is the **filesystem**. The schemas are the **API**. You own each piece.

## Next

- **[Schemas]({{ '/docs/schemas/' | relative_url }})** — the exact contract your AI must produce
- **[Running ELARA]({{ '/docs/running-elara/' | relative_url }})** — the weekly coaching run in Claude Code
- **[Privacy model]({{ '/docs/privacy/' | relative_url }})** — the complete data-flow picture
