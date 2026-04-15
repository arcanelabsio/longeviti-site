---
title: Privacy model
lead: A plain-language description of what data Longeviti handles, where it goes, and what it doesn't do.
---

Longeviti is designed around one principle: **your health data is yours, on infrastructure you already trust.** This page describes that model in concrete terms, with no marketing softening.

## What data exists

- **Profile** — name, DOB, sex, height, weight, targets, conditions, medications
- **Health reports** — whatever you upload: blood work PDFs, doctor notes, nutritionist plans
- **Tracking** — daily meals, vitals, workouts, water, supplements, sleep
- **Coaching sessions** — ELARA's analysis output
- **Plan** — meals, supplement schedule, rules

## Where each piece lives

| Data | Storage location | Transmitted to |
|---|---|---|
| Profile | Your Google Drive | Your Claude session (when you run ELARA/ATLAS) |
| Health reports | Your Google Drive | Your Claude session (when you run ELARA) |
| Tracking | Phone local storage → your Google Drive | Your Claude session (when you run ELARA) |
| Coaching sessions | Your Google Drive | Your Claude session |
| Plan + rules | Your Google Drive | Your Claude session (when ATLAS writes) |

The phrase "your Claude session" is important. When you run `/elara`, Claude reads your files and runs inference on them. That inference:

- Happens via Anthropic's API, which Claude Code uses
- Is governed by [Anthropic's privacy policy](https://www.anthropic.com/privacy)
- Is *not* sent to Longeviti (there is no Longeviti backend)

## What Longeviti does *not* do

- **No telemetry.** The app does not phone home. We collect zero usage data.
- **No analytics.** No Firebase, no Mixpanel, no Google Analytics in the app or on this website.
- **No accounts.** There is no "Longeviti account" to create. Your identity is your Google account, which you already have.
- **No server-side storage of your health data.** There are no Longeviti servers. The open-source code is the entire system.
- **No ads, ever.** Not now, not later. No business model that would incentivize collecting your data.
- **No selling or sharing.** See previous point.

## What Longeviti *does* do

- Builds an Android APK and iOS TestFlight binary that you install on your phone.
- Hosts this documentation site and the open-source code on GitHub.
- Publishes updates to the framework repo, which you pull down manually.

That's the entire operational surface.

## The trust boundaries

There are three parties that see your data:

### 1. You
Obviously.

### 2. Google
Your Drive is a Google service. Google's terms apply. If you trust Google with your email, calendar, and photos, Drive is the same trust envelope.

### 3. Anthropic
When you run `/elara` or `/atlas`, the contents of your reports and tracking get sent to Anthropic's API as part of the Claude conversation. Anthropic's [privacy policy](https://www.anthropic.com/privacy) and [usage policies](https://www.anthropic.com/legal/usage-policy) govern what happens to that data.

<div class="callout callout--warn">
<p><strong>If you don't want Anthropic to see your reports</strong>, the skills won't work — they need to reason over your data. You can still use the app standalone for tracking, but you'd lose automated plan generation and coaching. Some users run a local LLM instead; this is not officially supported but the skills are MIT-licensed and can be adapted.</p>
</div>

## Data you can remove

At any time:

- **Uninstall the app** → local cache gone; Drive data stays
- **Delete `.app/longeviti/` on Drive** → all plan, tracking, coaching, profile data gone
- **Delete the framework repo** → skill definitions gone locally
- **Revoke OAuth in Google** → app can no longer read or write Drive

To fully remove yourself, do all four.

## GDPR, HIPAA, and jurisdictions

Longeviti is personal software, not a covered entity. It doesn't provide medical advice, store PHI on third-party servers, or act as a medical device. That said:

- **GDPR** — you are the data controller of your own data. The app is the data processor on your own device. There is no third-party processor.
- **HIPAA** — Longeviti is not HIPAA-covered. If you're sharing data with a HIPAA-covered healthcare provider, that provider's BAA with their own infrastructure applies, not this app.
- **Jurisdiction of your data** — wherever your Drive region is. Same as your Drive's existing residency.

## Children

Longeviti is not intended for or directed at anyone under 16. It's not designed for pediatric nutrition or pediatric conditions.

## Security

The app is open source; the framework is open source; the schemas are open source. Security researchers are welcome to audit. Report vulnerabilities via the [SECURITY.md in the framework repo](https://github.com/ajitgunturi/longeviti-framework/blob/main/SECURITY.md).

## Next

- **[Backup and export]({{ '/docs/backup/' | relative_url }})** — keeping copies of your data outside Drive
