---
title: Uploading health reports
lead: How to get your blood work, nutritionist plans, and medical notes into Longeviti in a form ELARA can actually use.
---

Longeviti treats reports as **append-only timeline events**. Each new report becomes part of your health history, and ELARA reads them chronologically to understand where you were, where you are, and where you're trending.

## Supported formats

| Format | Works? | Notes |
|---|---|---|
| Searchable PDF (most lab PDFs) | Excellent | Directly extracted |
| Scanned PDF | Good | OCR runs automatically |
| Phone photo of a printed report | Good | Keep it in focus and well-lit |
| Handwritten doctor's notes | Mixed | OCR struggles with cursive — type them up if accuracy matters |
| `.txt` / `.md` notes | Perfect | Fastest path when you already have structured info |
| `.docx` nutrition plan | Good | Extracted as text |
| HL7 / FHIR exports | Good | Parsed for structured fields |
| Excel / CSV | Good | Great for longitudinal blood work |

## Where to put them

Any of these work — pick whichever is easiest:

- **Drive folder** `.app/longeviti/user/reports/` — drop files here, ELARA picks them up automatically
- **Local paths** — tell Claude during `/elara` or `/onboard` and they'll be copied to Drive
- **Subfolders by date** — e.g. `reports/2026-04-16/blood-work.pdf` — ELARA honors folder structure as a hint about timing

<div class="callout">
<p><strong>Naming convention that helps:</strong> <span class="mono">YYYY-MM-DD_type.pdf</span> — e.g. <span class="mono">2026-04-16_lipid-panel.pdf</span>. ELARA uses this when the report itself is ambiguous about the date.</p>
</div>

## What ELARA looks for

On a first read, ELARA extracts:

- **Identity** — name on the report (matched against your profile to catch mis-filed reports)
- **Date** — report date, not file upload date
- **Numeric markers** — anything with a value and unit (glucose, HbA1c, LDL, HDL, TG, TSH, vitamin D, B12, creatinine, eGFR, ALT, AST, etc.)
- **Textual findings** — "mild fatty liver", "grade 1 hypertension", etc.
- **Medications** — drug names, doses, frequencies
- **Recommendations** — any diet/lifestyle advice the doctor or nutritionist wrote down

## Multi-report timelines

Drop five years of blood work in at once — ELARA will read them in date order and track trends. This is especially useful for:

- **Directional change** — is your HbA1c climbing or falling?
- **Response to interventions** — did LDL respond to statin therapy?
- **Seasonal patterns** — does vitamin D dip every winter?

Trend observations show up in the coaching output and often become rules (e.g., "maintain vitamin D supplementation year-round" once a pattern is confirmed).

## Sensitive information

All report reading happens **locally** in your Claude Code session. Reports:

- Are uploaded to your own Drive (where they already were, effectively)
- Are never sent to a Longeviti server (there isn't one)
- Are read by Claude — the same LLM you're talking to — and nothing else

If a report contains information you'd rather ELARA not use (insurance details, SSN, diagnoses you want excluded), redact it before uploading. A Sharpie on the printed copy, then a phone photo, is the most foolproof method.

## When a report is out of date

Old reports are still valuable — they establish baselines and trends. ELARA weighs recency automatically. You don't need to delete old reports, and doing so can hurt plan quality.

If a *specific finding* is no longer relevant (e.g., an acute infection from two years ago), tell ELARA in plain language: "Ignore the elevated WBC from the 2024-03 report — that was a one-off." ELARA will record the override in your profile.

## Next

- **[Profile and targets]({{ '/docs/profile/' | relative_url }})** — how profile fields get populated from reports
- **[Running ELARA]({{ '/docs/running-elara/' | relative_url }})** — the weekly rhythm for incorporating new data
