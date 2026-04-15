---
title: Profile and targets
lead: What lives in profile.json, when ELARA updates it, and when you should edit it by hand.
---

`user/profile.json` is the single source of truth for *who you are* in Longeviti's eyes. Everything — meal generation, compliance rules, coaching thresholds — reads from it.

## What's in profile.json

```json
{
  "schema_version": 1,
  "user": {
    "name": "Ajit",
    "dob": "1987-05-12",
    "sex": "male",
    "height_cm": 175,
    "weight_kg": 78
  },
  "targets": {
    "calories_kcal": 2100,
    "protein_g": 130,
    "carbs_g": 210,
    "fat_g": 75,
    "fiber_g": 35,
    "water_ml": 3000,
    "sleep_hours": 7.5
  },
  "conditions": ["prediabetes", "hypertension-stage-1"],
  "medications": [
    {"name": "Metformin", "dose_mg": 500, "frequency": "2x/day"}
  ],
  "preferences": {
    "cuisine": ["south_indian", "mediterranean"],
    "vegetarian": true,
    "allergies": ["peanut"]
  },
  "goals": [
    {"description": "HbA1c < 5.7", "target_date": "2026-10-01"}
  ]
}
```

The live schema is in [`schemas/profile.schema.json`]({{ '/docs/schemas/' | relative_url }}).

## Who writes each field

| Field | Written by | When |
|---|---|---|
| `user.*` | `/onboard` | At setup; rarely changes |
| `targets.*` | ELARA | Every plan refresh, based on current weight + goals |
| `conditions` | ELARA | When a new report adds or removes a condition |
| `medications` | ELARA | When a new report mentions a med change |
| `preferences.*` | You | Edit directly, or tell ELARA in prose |
| `goals` | You + ELARA | You set them; ELARA updates target dates and status |

## When to edit by hand

Most changes flow through `/elara`. But three cases are faster to edit directly:

1. **Typos in `user.name` or a wrong DOB.** Open `profile.json` in any editor, fix, save. The app will re-sync on next open.
2. **Adding an allergy you just discovered.** Append to `preferences.allergies`. The rule engine picks it up on next app launch.
3. **Removing a goal you've hit.** Delete the goal object from the array. ELARA will re-read and adjust the plan.

<div class="callout callout--warn">
<p><strong>Don't edit <span class="mono">targets.*</span> by hand.</strong> Calorie and macro targets are computed from your weight, activity level, goals, and conditions. Hand-editing them will drift them out of sync — ELARA will silently recalculate them on the next run and your edit is lost.</p>
</div>

## When ELARA updates targets

Target recalculation happens automatically when:

- **Weight changes by ≥ 2 kg** since last plan
- **A new condition appears** in the latest report
- **A medication change** alters caloric or macro needs (e.g., starting Ozempic drops the calorie floor)
- **A goal deadline passes** — targets tighten or loosen based on progress

If you want to force a recalculation, run `/elara` and say "review my profile targets against the latest weigh-in."

## Conditions and rules

`conditions` is the most load-bearing field after `targets`. Each entry maps to one or more default rules in `rules.yaml`. Example mappings:

| Condition | Default rules |
|---|---|
| `prediabetes` | No added sugar; carbs < 40% kcal; glycemic index < 55 preferred |
| `hypertension` | Sodium < 2g/day; potassium target ≥ 3.5g/day |
| `hypothyroidism` | Iodine target ≥ 150 µg; separate thyroid meds from calcium/iron by 4h |
| `ckd-stage-2` | Protein 0.6–0.8 g/kg body weight; phosphorus-aware |

Rules are *additive* — having two conditions applies both rule sets, with conflicts resolved toward the stricter rule. See [Schemas]({{ '/docs/schemas/' | relative_url }}) for the rules schema.

## Privacy note

`profile.json` contains your full identity and health conditions. It sits only in your Drive folder — it's never transmitted to a Longeviti server (there isn't one). If you share your Drive folder with anyone (partner, doctor, coach), they'll see this file.
