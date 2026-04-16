---
title: Schemas
lead: The formal contracts between the app, ELARA, and ATLAS. Read these if you're extending the system or debugging a plan.
---

Every file Longeviti produces conforms to a JSON Schema (or YAML equivalent). The schemas live in the framework repo at [`schemas/`](https://github.com/arcanelabsio/longeviti-framework/tree/main/schemas).

## The four schemas

| Schema | Validates | Written by | Read by |
|---|---|---|---|
| `weekly_plan.schema.json` | `plans/week_*.json` | ATLAS | App, ELARA |
| `profile.schema.json` | `user/profile.json` | ELARA | App, ATLAS |
| `coaching_session.schema.json` | `elara/session_*.json` | ELARA | App |
| `rules.schema.json` | `plans/rules.yaml` | ATLAS | App, ELARA |

## Why schemas matter

Three jobs:

### 1. Contract between pipelines
The app never talks to ELARA or ATLAS directly. They communicate via these files. Schemas are the API definition.

### 2. Validation gate
ATLAS validates every write against its schema before saving. A malformed plan never lands on your Drive.

### 3. Forward compatibility
Every schema has `schema_version`. When a field changes, the version bumps, and the app can reject files it doesn't understand rather than silently misrendering them.

## weekly_plan.schema.json

The largest and most-edited schema. Key sections:

```json
{
  "schema_version": 1,
  "week_start": "2026-04-13",
  "diet": {
    "daily_targets": {
      "calories_kcal": 2100,
      "protein_g": 130,
      "carbs_g": 210,
      "fat_g": 75
    },
    "sample_days": [
      {
        "day_name": "Monday",
        "meals": {
          "breakfast": { … },
          "mid_morning": { … },
          "lunch": { … },
          "afternoon_snack": { … },
          "dinner": { … }
        }
      }
    ]
  },
  "supplements": { "daily": [ … ] },
  "shopping": { "categories": [ … ] }
}
```

The five-slot meal model is baked in. Adding a sixth slot means a schema version bump and coordinated rollouts across app + ATLAS + ELARA.

## profile.schema.json

Covered in detail in [Profile and targets]({{ '/docs/profile/' | relative_url }}). Key rule: optional fields are truly optional — the app handles missing `medications` or `goals` arrays gracefully.

## coaching_session.schema.json

```json
{
  "schema_version": 1,
  "date": "2026-04-14",
  "summary": "…",
  "adherence": { … },
  "patterns": [ … ],
  "directives": [ … ],
  "red_flags": [ … ],
  "next_week": "…"
}
```

`directives` is the interesting field — it's a list of structured changes ELARA is telling ATLAS to make. The app displays a human summary; ATLAS consumes the structured form.

## rules.schema.json

Rules are YAML because they're hand-edited sometimes. The schema:

```yaml
schema_version: 1
food_rules:
  - id: no_added_sugar
    severity: hard
    applies_to:
      tags: [added_sugar]
    message: "Contains added sugar — conflicts with prediabetes management"
  - id: sodium_ceiling
    severity: soft
    threshold:
      field: sodium_mg
      op: "<="
      value: 2000
      per: day
lifestyle_rules:
  - id: hydration_floor
    severity: soft
    threshold: { … }
```

`severity: hard` → red dot, blocked from auto-planning.
`severity: soft` → amber dot, allowed but flagged.

## The rule engine

The app's `lib/services/rules_engine.dart` parses `rules.yaml` at startup and evaluates every logged meal against every rule. The evaluation is deterministic and runs in ~1ms per meal on current hardware.

Rules can reference:

- **Food tags** (`added_sugar`, `ultra_processed`, `contains_dairy`) — stored on each food in `foods.json`
- **Macros** (`carbs_g`, `sodium_mg`, `fiber_g`)
- **Meal slot** (only flag dinner carbs, not breakfast)
- **Day total** (sodium across all meals)

See the rule engine tests in the app repo for examples.

## Schema evolution

When a schema needs a new field:

1. **Additive change** — new optional field, same `schema_version`. App ignores it, framework writes it.
2. **Breaking change** — bump `schema_version`. App gains a loader for the new version. ATLAS starts emitting the new version. Migration tool handles old files.

Breaking changes are rare. ADR-0002 in the framework documents the process.

## Running validation yourself

If you want to validate files in your Drive folder:

```bash
cd ~/longeviti-framework
python scripts/validate_schemas.py path/to/plans/
```

Or via the makefile:

```bash
make validate
```

These are the same validators that CI runs on every framework commit.

## Next

- **[Troubleshooting]({{ '/docs/troubleshooting/' | relative_url }})** — what to do when something fails to validate
