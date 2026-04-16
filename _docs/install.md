---
title: Download & install
permalink: /download/
lead: The Longeviti app isn't on the Play Store yet. You'll sideload the APK directly. It takes about two minutes.
---

<div class="callout">
<p><strong>Android only for now.</strong> An iOS build is planned but not available yet. If you're on iPhone, you can still read and edit your Drive data directly — just no mobile UI.</p>
</div>

## Get the APK

Latest release:

<p>
  <a class="btn btn--solid" href="https://github.com/arcanelabsio/longeviti-site/releases/latest/download/longeviti.apk">Download longeviti.apk ↓</a>
  &nbsp;
  <a href="https://github.com/arcanelabsio/longeviti-site/releases">All releases</a>
</p>

Each release notes which app version it is, what changed, and the SHA256 for verification.

## Install on your phone

### Step 1 — Allow installs from this source

Because the APK comes from outside the Play Store, Android needs permission to install it.

- **Android 12+**: When you open the APK, Android will prompt "Allow from this source?" — tap **Settings**, toggle on **"Allow from this source"** for your browser or file manager, and go back.
- **Android 8–11**: Settings → Apps → *your browser* → **Install unknown apps** → Allow.

You only grant this once per source app (Chrome, Firefox, Files, etc.).

### Step 2 — Open the APK

Find the downloaded `longeviti.apk` in your Downloads folder and tap it. Android shows a summary of what's being installed. Tap **Install**.

### Step 3 — Launch and sign in

Open Longeviti. You'll be prompted to sign in with Google and grant access to your Drive — approve both. On first launch you'll see the sync-gate screen creating `.app/longeviti/` on your Drive.

At this point your app is ready but empty. Continue with [Getting started]({{ '/docs/getting-started/' | relative_url }}) step 2 to clone the framework and run onboarding.

## Verifying the download

Each release page publishes the APK's SHA256. To verify on macOS/Linux:

```bash
shasum -a 256 ~/Downloads/longeviti.apk
```

Compare the output with the hash on the release page. Mismatch = don't install, re-download.

## Updating

There's no in-app auto-updater yet. When a new release drops:

1. Download the new APK from [releases](https://github.com/arcanelabsio/longeviti-site/releases)
2. Open it — Android recognizes it as an upgrade and keeps your existing data
3. Your local cache migrates automatically; Drive-side data is untouched

Subscribe to releases: on the [releases page](https://github.com/arcanelabsio/longeviti-site/releases), click **Watch** → **Custom** → **Releases** — you'll get an email each time.

## Why not the Play Store?

Play Store submission is planned once the app hits a stable feature set. Until then, sideloading keeps the release cycle tight — we can ship a fix the same day without waiting on store review.

## Why not F-Droid?

F-Droid submission is under consideration. The blocker is reproducible builds and some transitive dependencies that use Google Play Services (Drive SDK) — F-Droid's guidelines around these are strict. If you'd like to help with this, see the framework's [CONTRIBUTING](https://github.com/arcanelabsio/longeviti-framework/blob/main/CONTRIBUTING.md).

## Uninstalling

Standard Android: long-press the app icon → **Uninstall**. Local cache is wiped; your Drive data is unaffected. Reinstall anytime and everything syncs back down.

## What about iOS?

On iPhone, there's no sideload path equivalent without Apple Developer mode. TestFlight distribution requires an Apple Developer Program account, which we'll set up closer to a stable release. Until then, iOS users can:

- Read and edit Drive data via the web ([drive.google.com](https://drive.google.com))
- Run the Claude Code skills (`/onboard`, `/elara`, `/atlas`) on a Mac — that handles plans, coaching, and reports
- Wait for the TestFlight build (no firm ETA — follow [releases](https://github.com/arcanelabsio/longeviti-site/releases))
