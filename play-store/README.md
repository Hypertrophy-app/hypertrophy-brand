# Play Store submission

Everything Play Console asks for, and where this repository answers it.

Assets live in [`assets/`](assets). Copy lives in [`listing.md`](listing.md).
Data safety answers live in [`data-safety.md`](data-safety.md).

## Assets

| Asset | Requirement | File |
| --- | --- | --- |
| App icon | 512 x 512 PNG, 32-bit, no transparency, under 1 MB | `assets/icon-512.png` |
| Feature graphic | 1024 x 500 PNG or JPEG, no transparency | `assets/feature-graphic-1024x500.png` |
| Phone screenshots | 2 to 8, portrait, 16:9 or 9:16, 320 to 3840 px on the long edge | not yet captured |
| Tablet screenshots | 7-inch and 10-inch, recommended | not yet captured |
| Promo video | YouTube URL, optional | none |

Screenshots must show the shipping UI. Capture on a device at 1080 x 2400 in
both themes, then pick one theme for the listing so the set reads as a single
product. Do not mock up a screen that the app does not render.

## Blockers

These stop a submission today. They are listed in the order Play Console
raises them.

- [ ] **Privacy policy URL.** Required for every app. Blocked on
  `hypertrophy-site` being published. The policy is drafted there; it needs a
  legal read before it is public.
- [ ] **Account deletion.** The Worker exposes no delete-account endpoint, and
  the client has no deletion flow. Play requires both an in-app path and a web
  URL for any app that creates accounts. This is a product change in
  `hypertrophy-android`, not a paperwork item.
- [ ] **App access.** Every screen is gated behind sign-in. Reviewers need a
  working demo account and written steps, or the app is rejected as
  inaccessible.
- [ ] **Target API level.** This build targets API 35. Play raises the floor
  for new apps periodically; confirm the current minimum before submission and
  bump `targetSdk` if it has moved.
- [ ] **Health apps declaration.** The app reads Health Connect and writes
  session exports. Play requires the health declaration and a matching privacy
  policy section.
- [ ] **Play App Signing.** Enroll and upload the upload key. The release
  workflow signs with `ANDROID_KEYSTORE_BASE64`; that keystore is the upload
  key, not the app signing key.

## Checklist

- [ ] App name, 30 characters. `Hypertrophy` is 11.
- [ ] Short description, 80 characters. Drafted in `listing.md`.
- [ ] Full description, 4000 characters. Drafted in `listing.md`.
- [ ] Icon and feature graphic uploaded.
- [ ] Screenshots uploaded for phone, and tablet if shipping tablet layouts.
- [ ] Content rating questionnaire completed (IARC). Expect "Everyone" with a
  health and fitness note; no user-generated content beyond squad chat, which
  is moderated by removal only.
- [ ] Target audience set to 13+ or 18+. The app handles health data and squad
  chat, so do not declare it child-directed.
- [ ] Ads declaration: no ads.
- [ ] News app declaration: not a news app.
- [ ] Government app declaration: no.
- [ ] Financial features declaration: none.
- [ ] Data safety form completed from `data-safety.md`.
- [ ] Privacy policy URL live and reachable without sign-in.
- [ ] App access instructions and demo credentials supplied.
- [ ] Store listing contact email and website set.
- [ ] Internal testing track smoke-tested on a clean install.
- [ ] Closed testing at 12+ testers for 14 days, if the account is new. Play
  gates production access behind this; start it early.
- [ ] Production rollout, staged at 20 percent, then full.

## Release tracks

Internal, then closed, then production. Every promotion should be a signed
bundle from the release workflow, never a locally built artifact. The workflow
writes a draft GitHub release with generated notes; review those notes before
publishing, because they become the "What's new" text users read.
