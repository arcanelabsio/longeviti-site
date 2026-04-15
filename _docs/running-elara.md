---
title: Running ELARA
lead: The weekly rhythm for coaching — how to invoke it, what it reads, and what it writes.
---

ELARA is the weekly coach. You invoke it once a week (or on-demand when something changes) and it produces a coaching session + any necessary plan updates.

## How to run it

From the framework directory:

```bash
cd ~/longeviti-framework
claude
```

Then in the Claude session:

```
/elara
```

That's the entire interaction for a default run. ELARA will:

1. Pull the latest tracking data from Drive
2. Check `user/reports/` for any new reports since last run
3. Load current profile, active plan, and rules
4. Analyze patterns across the past 7 days
5. Write a coaching session to `elara/session_YYYY-MM-DD.json`
6. If plan changes are warranted, call `/atlas` with specific directives
7. Report back with a summary

Expect 2–5 minutes end to end.

## Variations

You can give ELARA narrower instructions:

```
/elara review only the last 3 days
/elara focus on my glucose data
/elara I just added a new blood work report — prioritize that
/elara dry run — tell me what you would change but don't update plans
```

Dry-run mode is especially useful when you want to see ELARA's reasoning before committing to plan changes.

## When to run it

### Scheduled (default)
Pick a day and stick to it. Sundays work well — you've got a full Mon–Sun tracking week and a fresh plan starts Monday.

### On-demand triggers
Run `/elara` immediately when:

- **New blood work arrives.** ELARA should see it before next week's plan is used.
- **You've had > 3 days of tracking gaps.** Less data means weaker signal; catching up soon matters.
- **You notice a pattern** — fatigue, weight change, glucose behavior. Let ELARA check whether the data agrees.
- **Life changes** — travel, illness, new medication. The plan needs context.

## What ELARA reads

On every run, in this order:

1. **`user/profile.json`** — current profile
2. **`user/reports/`** — all reports, newest first; stops when it hits ones it has read before
3. **`plans/week_*.json`** — the current week's plan
4. **`plans/rules.yaml`** — active rules
5. **`tracking/*.json`** — the last 14 days (7 primary + 7 context)
6. **`elara/session_*.json`** — the previous 4 coaching sessions (for continuity)
7. **`nutrition-db/foods.json`** — food macros and rule tags

## What ELARA writes

- **One session JSON** per run in `elara/` — this is the coaching output the app renders
- **Directives to ATLAS** — logged in the session and executed in the same run

ELARA does not directly write to `plans/` or `rules.yaml`. Plan changes always go through ATLAS. This separation is architectural — see [ADR-002 in the framework](https://github.com/arcanelabs/longeviti-framework/blob/main/docs/adr/002-atlas-elara-pipeline-separation.md).

## Autonomy levels

During `/onboard` you picked one of three modes:

| Mode | Behavior |
|---|---|
| **Autonomous** | ELARA calls ATLAS automatically; plan updates appear in the app next sync |
| **Supervised** | ELARA proposes changes; you approve each before ATLAS runs |
| **Read-only** | ELARA coaches but never calls ATLAS; you run `/atlas` manually when ready |

Change it anytime:

```
/elara switch me to supervised mode
```

Most users start in supervised for the first few weeks, then move to autonomous once they trust the directives.

## Token cost

A typical ELARA run costs roughly 50–150k tokens (input + output) against your Claude quota. The biggest drivers are:

- Number of reports in `user/reports/` (read on each run until cached)
- Length of the current `foods.json`
- Number of tracking days analyzed

Pro tier handles weekly runs comfortably. If you're running daily, consider Team.

## Next

- **[Running ATLAS]({{ '/docs/running-atlas/' | relative_url }})** — the execution side of the pipeline
- **[Adding new reports]({{ '/docs/new-reports/' | relative_url }})** — introducing new health data mid-cycle
