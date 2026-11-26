# Data safety answers

Derived from `backend/src/db/schema.ts`, the Worker routes, and the Android data
layer. Re-verify against the code before each submission; this file is only
correct while the code is.

## Summary answers

| Console question | Answer |
| --- | --- |
| Does your app collect or share any of the required user data types? | Yes |
| Is all user data encrypted in transit? | Yes, HTTPS and WSS only |
| Do you provide a way for users to request that their data be deleted? | Not yet. See blockers. |
| Do you have a privacy policy? | Yes. Draft is live at `https://hypertrophy-app.pages.dev/privacy/`; custom domain pending. |
| Is data shared with third parties? | No. Clerk and Cloudflare process it on our behalf as service providers. |

## Account requirement

Clerk authentication is mandatory. Every screen is gated behind an active
session, and every local row is keyed by the Clerk user id. There is no
signed-out or local-only mode. Do not describe account data as optional.

## Collected data

A completed session uploads automatically through the outbox while the user is
signed in. Plans, templates, weekly schedules, personal records, and recaps stay
on the device.

| Play data type | Detail | Collected | Shared | Purpose | Optional |
| --- | --- | --- | --- | --- | --- |
| Personal info, Email | Account identity through Clerk | Yes | No | Account management | No |
| Personal info, User IDs | Clerk subject, stored as `userId` | Yes | No | Account management, app functionality | No |
| Device or other IDs | Clerk device token, an app-instance credential | Yes | No | Account management | No |
| Health and fitness, Fitness info | Session title, timestamps, per-set exercise id, reps, weight, range of motion, tempo, form score, total volume | Yes | No | App functionality, sync | No |
| Messages, Other in-app messages | Squad chat bodies | Yes | No | App functionality | Yes, only when using squad chat |
| App activity, In-app actions | Squad presence status updates | Yes | No | App functionality | Yes, only when using squads |
| App activity, Other user-generated content | Squad names and invite codes | Yes | No | App functionality | Yes, only when using squads |

## Not collected

Declare these as not collected. They are the claims most likely to be audited,
so keep them true.

| Play data type | Why not |
| --- | --- |
| Health and fitness, Health info | Health Connect is write-only. Session exports are written on the device and never uploaded. |
| Photos and videos | No photo picker and no avatar upload. Camera frames are analyzed in memory by MediaPipe and discarded. |
| Name | The app never asks for a name. The `display_name` column holds the Clerk user id, which is already declared as a User ID. |
| Location | Never requested. |
| Contacts, calendar, SMS, call logs | Never requested. |
| Financial info | No purchases, no subscriptions, no payment data. |
| Crash logs, diagnostics | No crash or analytics SDK is linked. Verify with `./gradlew :app:dependencies` before submitting. |
| Web browsing history | None. |

## Deletion

Play requires an in-app deletion path and a publicly reachable web URL for any
app that lets people create accounts. Neither exists yet.

Uninstalling the app clears local Room data only. Synced sessions, the `users`
row, squad membership, and chat state remain in D1, R2, and the Durable Object.

When it is built, it must remove:

- The Clerk user record.
- Rows in `users`, `workout_sessions`, `workout_templates`, `personal_records`,
  and `squad_members`.
- Squad ownership transfer or squad deletion where the user is the owner.
- Chat messages authored by the user, or an anonymization policy documented in
  the privacy policy.
- R2 objects with `customMetadata.ownerId` equal to the user.
- The `session:<squadId>` state held by `SquadRoomDO`.
- Local Room tables, the outbox, and the local Clerk state.

## Notes for the reviewer

If asked how on-device pose estimation works: the camera stream is consumed by
MediaPipe `PoseLandmarker` in `LIVE_STREAM` mode, and only compact derived
metrics such as rep counts leave the analyzer. No frame, landmark stream, or
video is written to disk or sent over the network.
