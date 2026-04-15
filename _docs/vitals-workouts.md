---
title: Vitals and workouts
lead: Logging body metrics (weight, BP, glucose) and exercise — what's tracked, what's optional, and how ELARA uses each.
---

Outside of meals, Longeviti tracks two other daily inputs: **vitals** (body metrics that reflect health state) and **workouts** (activity).

## Vitals

The Vitals tab captures:

| Vital | Frequency | Why ELARA cares |
|---|---|---|
| **Weight** | Daily preferred, weekly min | Drives target recalculation |
| **Blood pressure** | Daily if hypertensive, else weekly | Validates sodium-reduction rules |
| **Fasting glucose** | Daily if diabetic/prediabetic | Validates carb ceiling effectiveness |
| **Post-meal glucose** | Ad-hoc | Identifies problem foods |
| **Heart rate (resting)** | Weekly | Cardiovascular trend |
| **Waist circumference** | Weekly | Better than weight alone for metabolic trend |
| **Sleep hours** | Daily | Fourth ring on the dashboard |
| **Mood** | Daily (1–5 scale) | Surfaces correlations with diet/sleep |

### What's optional

Everything except the one or two vitals tied to your active conditions. If you're not prediabetic, don't bother logging fasting glucose. ELARA only complains about missing data for vitals directly relevant to your goals.

### Units

Configured in `profile.json` under `preferences.units`. Defaults are metric (kg, mg/dL, mmHg, cm). Switch at any time — historical data is stored raw, unit conversion happens in display.

## Workouts

The Workouts tab logs:

- **Type** — cardio, strength, yoga, walk, etc.
- **Duration** — minutes
- **Intensity** — low / moderate / high (self-reported, no heart-rate required)
- **Notes** — freeform

That's it. Longeviti does not try to be a workout app. No set/rep tracking, no heart-rate graphs, no VO2 estimation. If you want that, use a dedicated app — the goal here is to feed ELARA enough signal to adjust calorie targets and protein needs.

### How workouts affect your plan

- **Duration × intensity** bumps your daily calorie target (via activity factor).
- **Strength sessions** in the last 7 days bump protein target.
- **Zero activity for > 5 days** triggers a coaching flag.

You'll see these adjustments surface in ELARA's weekly output.

## The four rings

The Home tab renders four concentric activity rings, drawing from all of the above:

| Ring | Color | Source |
|---|---|---|
| Diet | Hot coral | Calorie adherence (within 10% of target = full ring) |
| Supplements | Amber | % of scheduled supplements taken |
| Water | Cyan | Water ml vs. target |
| Sleep | Lavender | Sleep hours vs. target |

Rings are a snapshot of *today*. Your weekly trend is shown in the Coaching tab.

## Optional integrations

Longeviti doesn't integrate with HealthKit, Google Fit, or wearables today. This is deliberate:

- Most wearable data is noisy (step counts vary 20% between devices, heart rate estimates lag reality, sleep staging is guesswork).
- The signals ELARA needs — weight, BP, fasting glucose — are almost all *medical-grade inputs* that you take manually anyway.
- Adding integrations expands the surface area for data leaks.

If this changes in the future, it will be opt-in per integration and documented here.

## Next

- **[Reading coaching insights]({{ '/docs/coaching/' | relative_url }})** — how ELARA synthesizes vitals + workouts + meals
