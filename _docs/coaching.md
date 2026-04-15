---
title: Reading coaching insights
lead: What ELARA produces each week, where it shows up in the app, and how to act on it.
---

After every `/elara` run, a coaching session JSON lands in your Drive under `elara/`. The app syncs it, and you see the output in the **Coaching** tab.

## What a session contains

Each coaching session has six sections:

### 1. Summary
One paragraph. "Good week on supplements, slipping on water, two dinners above the carb ceiling."

### 2. Adherence metrics
Numerical summary of the week:

- Meals logged vs. planned
- Calorie adherence (within / above / below target)
- Macro adherence per macro
- Supplement adherence
- Water adherence
- Sleep adherence
- Compliance dot distribution

### 3. Patterns
Observations across multiple days:

> "Wednesday and Friday both had post-dinner glucose spikes above 180. Both days included white rice at dinner. Worth switching to millet or reducing portion."

### 4. Directives to ATLAS
Concrete plan changes ELARA is requesting:

- Meal swaps ("Replace Wednesday dinner with quinoa bowl")
- Rule changes ("Tighten sodium to 1.8g/day")
- Target changes ("Bump protein to 135g — you've been hitting 125g consistently")

These are executed automatically if you approved autonomous mode during `/onboard`, or queued for review otherwise.

### 5. Red flags
Anything that warrants attention:

- A vital trending the wrong way
- A blood marker due for re-testing
- A medication interaction risk
- A goal at risk of being missed

### 6. Next week preview
One-liner on what to expect in next week's plan and what to focus on.

## Where it shows up in the app

- **Home tab** — a single-line summary card ("This week: adherence 84% · 2 red flags")
- **Coaching tab** — full session, scrollable, with per-section expand/collapse
- **Notifications** — optional, opt-in, fires when a new session lands on Drive

## How to act on it

The Coaching tab has three buttons per directive:

- **Accept** — mark as acknowledged, no action needed (ATLAS has already applied it)
- **Defer** — acknowledge but don't apply yet (useful for life-event weeks — travel, illness)
- **Question** — adds a note that will be fed back into the next `/elara` run ("Why switch to millet?")

The Question flow is the most underused and the most valuable. ELARA's reasoning is available — ask for it.

## Frequency

| Cadence | When to pick it |
|---|---|
| Weekly (recommended) | Stable health, on a plan that's working |
| Bi-weekly | Stable + low data change rate |
| Monthly | Maintenance mode — plan is working, just confirming |
| On-demand | New report arrived, or a pattern you want checked now |

Set the cadence during `/onboard` or tell ELARA directly: "Switch me to bi-weekly reviews."

## What ELARA can and can't do

**Can:**
- Read reports, tracking, and the current plan
- Derive trends and correlations
- Produce directives ATLAS executes
- Flag values outside reference ranges

**Can't:**
- Diagnose conditions (it flags, you confirm with a doctor)
- Prescribe or change medications (it notes what's in your reports, nothing more)
- Override your stated preferences (if you've said "no dairy", ELARA won't recommend dairy even if it would be optimal)

<div class="callout callout--warn">
<p><strong>ELARA is not your doctor.</strong> It's a well-informed assistant that reads your data and keeps your plan current. Any medical decision — medication, major dietary shift, exercise program around an existing condition — should involve an actual clinician. Longeviti is designed to make that conversation more data-rich, not to replace it.</p>
</div>

## Next

- **[Running ELARA]({{ '/docs/running-elara/' | relative_url }})** — the mechanics of the weekly run
- **[Running ATLAS]({{ '/docs/running-atlas/' | relative_url }})** — when and how ATLAS executes ELARA's directives
