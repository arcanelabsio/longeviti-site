---
title: Adding new reports
lead: The full workflow for introducing new blood work, doctor's notes, or nutritionist updates into an existing plan.
---

Your health changes. Blood work cycles every 3–6 months, medications change, new symptoms appear. Each of these is a new *report* you feed into Longeviti, and the system adapts.

## The three-step flow

### 1. Drop the report in Drive

Put it under `.app/longeviti/user/reports/`. Name it descriptively if you can:

```
2026-04-16_fasting-blood-work.pdf
2026-04-16_doctor-note.pdf
2026-04-20_nutritionist-plan-q2.pdf
```

Or use subfolders by date:

```
reports/2026-04-16/blood-work.pdf
reports/2026-04-16/doctor-note.pdf
```

Either works. ELARA will figure out the date from the file, the filename, or the folder.

### 2. Run ELARA

```bash
cd ~/longeviti-framework
claude
```

```
/elara I just added a new blood work report — prioritize it
```

ELARA will:

- Parse the new report(s)
- Compare against your previous reports (trends)
- Update `user/profile.json` with any new conditions, medications, or corrected markers
- Check whether current rules still make sense (might tighten or relax)
- Check whether current plan still makes sense (might swap meals or adjust targets)
- Call ATLAS with any plan changes
- Write a coaching session explaining what changed and why

### 3. Sync the app

Open the app, pull to refresh. You'll see:

- Updated targets on the Home tab (if they changed)
- New meals in upcoming weekly plans
- A fresh coaching session in the Coaching tab summarizing what this report means for you

Total elapsed time: ~5 minutes.

## What constitutes a "new report"?

Anything that updates your health picture:

| Input | Handling |
|---|---|
| Full blood work (lipid, glucose, CBC, etc.) | Full trend update, may trigger rule changes |
| Single-marker update (e.g., HbA1c only) | Trend point added, context-limited update |
| Doctor's visit notes | Parsed for diagnoses, med changes, recommendations |
| Nutritionist's plan | Merged with ATLAS's current plan — nutritionist preferences take priority on meals they specified |
| Pharmacy receipt (new prescription) | Medication added to profile |
| Imaging report (ultrasound, MRI) | Findings noted, rarely affects plan unless a condition is newly diagnosed |
| Subjective notes ("my sleep has been terrible") | Treated as qualitative input; may trigger a pattern check |

## When reports conflict

Sometimes two reports disagree. ELARA handles conflicts with a priority order:

1. **More recent wins** for time-variant markers (weight, glucose, BP)
2. **Clinical grade wins** for lab values (a certified lab over a home device)
3. **Specialist wins over generalist** for specialty-specific guidance
4. **Explicit override wins over all** — if you tell ELARA "ignore the 2025-Q4 report, it was contaminated", that sticks

Conflicts are surfaced in the coaching session so you know they happened.

## When a nutritionist sends a new plan

This is a common flow and worth calling out. If you have a nutritionist:

1. They send a new meal plan (PDF, WhatsApp message, typed out)
2. You save it to `user/reports/`
3. You run `/elara`

ELARA will:

- Extract the meals they specified
- Map them to entries in `nutrition-db/foods.json` (or add new entries)
- Tell ATLAS to use the nutritionist's plan as the baseline for the next N weeks
- Layer on supplement schedules and rules from your own profile (things the nutritionist didn't specify)

You end up with the nutritionist's meal discipline + Longeviti's tracking and coaching. This is the highest-signal mode to run the system in.

## Frequency

There's no minimum. More reports = better coaching; fewer reports = ELARA relies more on tracking data to infer trends.

Practical cadence:

- **Blood work**: quarterly if you have an active condition, annually otherwise
- **Weight**: tracked daily in the app; no need for separate reports
- **BP/glucose**: same — daily logs suffice
- **Doctor visits**: whenever they happen
- **Nutritionist updates**: whenever they happen

## What if you never add new reports?

The system still works — it just relies entirely on your daily tracking to infer trends. Targets will still adjust as your weight changes. But structural plan changes (new conditions, medication interactions) can't happen without a new report to trigger them.

If you go a year without new reports and your tracking is stable, ELARA will flag:

> "Your last blood work was 14 months ago. Consider a follow-up — HbA1c in particular."

You can ignore it. Longeviti suggests, you decide.
