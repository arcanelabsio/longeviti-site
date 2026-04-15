---
title: Logging meals
lead: How daily meal logging works in the app — planned meals, swaps, freeform additions, and what compliance dots mean.
---

The Meals tab is where you log what you actually ate. The app pre-fills each of the five canonical slots with today's planned meal; you tap to confirm, swap, or skip.

## The five meal slots

Longeviti uses a fixed five-slot model across all plans:

1. **Breakfast**
2. **Mid-morning**
3. **Lunch**
4. **Afternoon snack**
5. **Dinner**

This rigidity is deliberate — it keeps tracking, coaching, and plan generation on the same schema. If your actual eating pattern differs (intermittent fasting, four meals, six meals), you still log into these slots and leave empty ones empty.

## The three actions per meal

For each slot, you can:

### 1. Confirm the planned meal
One tap. The meal is logged as-eaten, macros counted, compliance checked against `rules.yaml`.

### 2. Swap
Opens the food picker, which searches your `nutrition-db/foods.json`. Pick a replacement — the app recalculates macros and re-runs compliance.

### 3. Skip
Logs the slot as intentionally skipped (fasting, missed, not hungry). Skipped meals don't count against adherence in the same way missing data does.

## Compliance dots

Every logged meal gets a colored dot:

- **Green** — compliant with all active rules in `rules.yaml`
- **Amber** — violates a soft rule (e.g., "prefer whole grains" when you ate white rice)
- **Red** — violates a hard rule (e.g., allergy, hard sodium cap, diabetes carb ceiling)

Tap the dot to see which rule triggered. This is powered by the rule engine described in [Schemas]({{ '/docs/schemas/' | relative_url }}#rules).

## Freeform additions

Ate something that isn't in the catalog? Tap "add food" and either:

- **Pick from search** — the catalog has hundreds of regional items
- **Add new** — enter name and macros manually. This becomes a private entry in your local state. To make it permanent, mention it in `/elara` and it'll be added to `nutrition-db/foods.json` on the next sync.

<div class="callout">
<p><strong>Why the catalog approach?</strong> Macros-by-memory is error-prone. A shared catalog means "one cup cooked toor dal" has the same macros everywhere — in your plan, in your log, in ELARA's analysis. This eliminates a whole class of silent data quality issues.</p>
</div>

## Water and supplements

Same screen, different rings:

- **Water** — tap the glass icon to log a serving. Default serving is 250 ml; hold to customize.
- **Supplements** — check boxes against today's scheduled supplements (set by ATLAS in the plan). Taking more than scheduled? Add freeform, same as meals.

## Offline and sync

- All logging works **offline** — no connectivity required.
- Changes are saved to local JSON immediately.
- A **10-second debounced auto-sync** pushes to Drive after you stop editing (see [ADR-0009]({{ '/docs/drive-layout/' | relative_url }})).
- Conflict resolution uses `newerWins` — if you log from two devices, the most recent write survives.

## What gets sent to ELARA

ELARA reads your tracking on its next run. It sees:

- Which slots you hit vs. skipped
- Which meals you swapped and what you swapped to
- Compliance dot distribution (green / amber / red counts)
- Freeform additions outside the catalog
- Water total, supplement adherence

This is what becomes the weekly coaching insight. More honest logging → better coaching. Fudging the data to look adherent just makes ELARA's recommendations worse.

## Next

- **[Vitals and workouts]({{ '/docs/vitals-workouts/' | relative_url }})** — the other two tracking rings
- **[Reading coaching insights]({{ '/docs/coaching/' | relative_url }})** — how ELARA uses your logs
