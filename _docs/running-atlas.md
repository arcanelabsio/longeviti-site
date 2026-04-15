---
title: Updating plans via ATLAS
lead: ATLAS is the plan author. It takes directives from ELARA (or from you) and writes weekly plans, rules, and food catalog changes to Drive.
---

ATLAS is the execution layer. Unlike ELARA, it never reads your health reports directly — it only takes *directives* (specific, scoped instructions) and writes output.

## Two ways ATLAS runs

### 1. Called by ELARA (the common path)

This is what happens 95% of the time. ELARA analyzes data, decides "dinners need 5g more protein", and calls ATLAS with that directive. You don't invoke ATLAS directly — it runs inside the same Claude session.

### 2. Called directly by you

When you know exactly what you want changed and don't need ELARA's analysis:

```
/atlas
```

Use cases:

- **Manual rule change** — "add 'no alcohol' to my rules"
- **Meal swap** — "replace all Sunday breakfasts with eggs bhurji"
- **Adding a regional food** — "add chole bhature to nutrition-db"
- **Rolling forward** — "generate plans for weeks 5–8; we only have 1–4"
- **Reverting** — "undo ELARA's last directive set"

## What ATLAS can write

Only these files:

- `plans/week_*.md` and `plans/week_*.json`
- `plans/rules.yaml`
- `nutrition-db/foods.json`

It never touches:

- `user/profile.json` (ELARA owns this)
- `user/reports/` (read-only inputs)
- `tracking/` (app owns this)
- `elara/session_*.json` (ELARA owns this)

This boundary is enforced architecturally — ATLAS's skill definition only grants it write access to these paths. See the framework's [ADR-002](https://github.com/ajitgunturi/longeviti-framework/blob/main/docs/adr/002-atlas-elara-pipeline-separation.md).

## The directive language

ATLAS accepts structured directives:

```
- type: meal_swap
  when: "monday lunch"
  replace_with: "quinoa + grilled fish bowl"
  reason: "increase omega-3"

- type: rule_add
  rule: "sodium_max"
  value: 1800
  units: "mg/day"
  reason: "BP trending up"

- type: target_update
  target: "protein_g"
  old: 130
  new: 135
  reason: "consistent over-delivery, room to raise"
```

You can write directives in plain English and ATLAS will structure them. Or be explicit if you want tight control.

## Why a separate skill?

Three reasons:

### 1. Determinism where it matters
ATLAS is prompted to be *mechanical* — apply the directive, validate against schemas, write, done. No reinterpretation. This makes plan changes reproducible: the same directive always produces the same plan.

### 2. Validation as a hard gate
ATLAS runs every output through the JSON schemas in `schemas/`. If a meal swap produces a plan that violates the schema, ATLAS rejects it before writing. You'll never see a malformed plan on Drive.

### 3. Scripted edits for bulk changes
Bulk operations — "add sodium tags to all foods containing soy sauce" — are faster and safer as scripted edits than as LLM freeform writes. ATLAS invokes Python scripts in the framework for these cases.

## Safety rails

ATLAS will refuse to:

- **Write invalid JSON.** Every file is schema-validated before save.
- **Violate a hard rule.** If your rules say "no peanuts" and a directive would add a peanut-containing meal, ATLAS rejects it and asks ELARA to pick an alternative.
- **Delete tracking data.** Tracking is append-only from ATLAS's perspective.
- **Change `user/profile.json`.** Only ELARA can modify the profile.

## Reviewing what ATLAS changed

After any run, check the diff:

```bash
cd ~/longeviti-framework
rclone sync remote:.app/longeviti/plans ./plans-snapshot
git -C ./plans-snapshot diff HEAD~1 HEAD
```

Or just open the files on Drive — the Markdown versions (`week_*.md`) are human-readable and show changes clearly.

## When to use ATLAS directly

If you find yourself using `/atlas` more than `/elara`, that's a signal. Either:

- You've become your own coach and don't need ELARA's analysis anymore (fine)
- ELARA's directives aren't matching your intent (worth a conversation with ELARA about why)

For most users, ATLAS stays invisible — called by ELARA, doing its job, never needing your direct attention.

## Next

- **[Adding new reports]({{ '/docs/new-reports/' | relative_url }})** — the full workflow for onboarding new data
- **[Schemas]({{ '/docs/schemas/' | relative_url }})** — what ATLAS validates against
