---
title: FAQ
lead: Short answers to questions that don't need their own page.
---

## Why two repos instead of one app?

The intelligence (Claude skills) evolves on a different cadence from the app binary. Skill improvements can ship the moment a new prompt is better; app releases are blocked on app store review. Separation lets both move at their natural speed.

## Do I need a nutritionist?

No. If you have one, their reports and plans become high-quality input and ELARA coordinates with them. If you don't, ELARA works from your blood work and goals alone.

## Do I need a doctor?

For Longeviti itself, no. For the clinical decisions Longeviti surfaces — "your HbA1c is trending up, consider metformin discussion" — yes. Longeviti is a tool that makes the conversation data-rich; it doesn't replace clinical judgment.

## Is this a medical device?

No. Longeviti is a personal tracking and planning tool. It does not diagnose, treat, cure, or prevent any disease. It is not FDA-cleared, CE-marked, or regulated as a medical device in any jurisdiction.

## What does it cost?

- **Longeviti itself**: free, open source.
- **Google Drive**: free tier (15 GB) is plenty; all your Longeviti data combined is under 50 MB.
- **Claude Code**: Pro tier (currently $20/month) is sufficient for weekly ELARA runs.

## Can I use this without Google Drive?

Not currently. The whole sync model is Drive-based. Forks that swap in S3, iCloud, or self-hosted storage would be technically possible but aren't officially supported.

## Can I use it without Claude?

You can use the *app* standalone — it'll track your meals against whatever plan you put in Drive manually. You lose automated plan generation and coaching, which is most of the value.

Community forks have experimented with local LLMs (Ollama, LM Studio). Quality drops significantly — the skills are tuned for Claude's reasoning.

## Can multiple people use it on the same Drive?

Not recommended. The folder layout is per-person. If two people share a Drive account, their tracking will clash.

For couples / families, each person gets their own Google account and their own `.app/longeviti/` folder.

## Is there a web version of the app?

Not currently. The mobile app is the primary client. You can read and edit any of the underlying JSON/Markdown files in Drive directly from the web, which covers the "I need to check my plan from a computer" case.

## Can I export my data to Apple Health / Google Fit?

Not automatically. The JSON files are trivially convertible with a small script — the schemas are documented — but we haven't built a first-party export.

## How do I know Longeviti won't quit?

You don't, for sure. Mitigation: everything is open source. If the project stops, the app keeps working (it's your local binary), the schemas are yours, and your data is in a format you can read with any editor. The worst case is "no more plan updates", not "data loss".

## Can I contribute?

Yes. The framework and schemas welcome PRs. See the CONTRIBUTING guides in each repo.

## Why "Longeviti"?

Same etymology as longevity, trimmed. We like it.

## Does it work offline?

- **The app**: yes, for logging and viewing. It syncs when connected.
- **The skills**: no. ELARA and ATLAS need Claude, which needs the API.

## What data does the website itself collect?

None. This site has no tracking, no cookies, no analytics. GitHub Pages serves the HTML; your browser renders it.

## My question isn't here

File an issue on whichever repo is most relevant:

- App → [github.com/ajitgunturi/longeviti](https://github.com/ajitgunturi/longeviti)
- Framework / skills → [github.com/ajitgunturi/longeviti-framework](https://github.com/ajitgunturi/longeviti-framework)
- Docs site → [github.com/ajitgunturi/longeviti-site](https://github.com/ajitgunturi/longeviti-site)
