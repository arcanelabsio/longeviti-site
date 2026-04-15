---
title: Prerequisites
lead: What you need — in software, hardware, and input data — to get a useful plan out of Longeviti.
---

## Software

| What | Why | How |
|---|---|---|
| Android 10+ | App target SDK (iOS build not available yet) | Phone settings |
| Google account | Drive sync | Any Gmail account |
| Git | Clone the framework | `brew install git`, `apt install git`, or [git-scm.com](https://git-scm.com) |
| Claude Code | Runs the skills | [claude.com/claude-code](https://claude.com/claude-code) |
| Python 3.11+ *(optional)* | Running schema validators locally | `brew install python` or equivalent |

## Input data

The quality of your plan scales with the quality of inputs. In priority order:

### Tier 1 — ideal
- **Recent blood work** (within 6 months). HbA1c, fasting glucose, lipid panel, CBC, thyroid, vitamin D, B12, ferritin are the most useful markers.
- **A current nutritionist's plan**, if you have one — even an old PDF gives structure.
- **Medication list** with doses.

### Tier 2 — sufficient
- **Doctor's discharge summary** or annual physical notes.
- **Self-reported conditions** (diabetes, PCOS, hypertension, etc.).
- **A dietary history** — what you typically eat on a weekday and weekend.

### Tier 3 — minimum viable
- **Just your goals and body metrics.** Height, weight, age, activity level, and what you want to achieve. ATLAS will generate a conservative general-longevity plan you can refine later.

<div class="callout">
<p><strong>Format doesn't matter.</strong> PDFs, scanned images, phone photos of a printed report, a paragraph you typed — Claude handles all of them. Prefer searchable PDFs where you have the choice; OCR accuracy drops for handwritten notes.</p>
</div>

## Hardware

- **~2 GB free** on your phone (app + a year of tracking JSON is tiny — plans and nutrition-db eat most of this).
- **~500 MB free** on your computer for the cloned framework.
- **Stable internet** during onboarding. Daily use is offline-first; you only need connectivity when the app syncs to Drive.

## Accounts

- **Google** — required. The entire data layer is Google Drive.
- **GitHub** — only if you want to fork the framework or contribute.
- **Claude** — Pro tier is sufficient for onboarding and weekly ELARA runs. Team or higher helps if you run ELARA daily.

## What you do *not* need

- A nutritionist (but you can use one — their plans become input).
- A paid account on Longeviti (there isn't one).
- A backend account or API key from us (there's no backend).
- HealthKit / Google Fit integration — Longeviti tracks its own four rings independently.
