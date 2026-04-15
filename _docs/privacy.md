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

- **No AI inference on our side.** The app does zero LLM calls. There's no Longeviti inference API, no on-device ML model, no "AI middleware" between you and your assistant. See [Bring your own AI]({{ '/docs/bring-your-own-ai/' | relative_url }}).
- **No telemetry.** The app does not phone home. We collect zero usage data.
- **No analytics.** No Firebase, no Mixpanel, no Google Analytics in the app or on this website.
- **No accounts.** There is no "Longeviti account" to create. Your identity is your Google account, which you already have.
- **No server-side storage of your health data.** There are no Longeviti servers. The open-source code is the entire system.
- **No ads, ever.** Not now, not later. No business model that would incentivize collecting your data.
- **No selling or sharing.** See previous point.

## What Longeviti *does* do

- Builds an Android APK that you sideload on your phone (iOS build planned).
- Hosts this documentation site and the open-source code on GitHub.
- Publishes APK releases on the [longeviti-site releases page](https://github.com/ajitgunturi/longeviti-site/releases).
- Publishes updates to the framework repo, which you pull down manually.

That's the entire operational surface.

## The trust boundaries

There are three parties that see your data:

### 1. You
Obviously.

### 2. Google
Your Drive is a Google service. Google's terms apply. If you trust Google with your email, calendar, and photos, Drive is the same trust envelope.

### 3. Your AI provider (your choice)
When you run `/elara` or `/atlas`, the contents of your reports and tracking get sent to *your own* AI provider's API — whichever one you chose:

- **Claude Code** → Anthropic's API ([privacy policy](https://www.anthropic.com/privacy), [usage policy](https://www.anthropic.com/legal/usage-policy))
- **Codex** → OpenAI's API ([privacy policy](https://openai.com/policies/privacy-policy))
- **Local model via Ollama/LM Studio** → stays on your machine, zero third-party transit

Longeviti does not select your AI for you and does not route your data through any service of ours. The trust you extend is to whichever provider you've already chosen for your CLI assistant.

<div class="callout callout--warn">
<p><strong>If you don't want <em>any</em> third party to see your reports</strong>, run the skills against a local model (Ollama, LM Studio, llama.cpp). This is not officially supported — larger models (70B+) produce valid plans, smaller ones often fail schema validation — but the skills are MIT-licensed Markdown files and work with any Claude-compatible CLI.</p>
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
