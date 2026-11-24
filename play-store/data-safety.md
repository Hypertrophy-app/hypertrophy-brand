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
| Do you have a privacy policy? | Yes, once `hypertrophy-site` is published |
| Is data shared with third parties? | No |

## Collected data

Everything below is optional in the sense that a lifter who never signs in and
never joins a squad keeps all of it on the device.

| Play data type | Detail | Collected | Shared | Purpose | Optional |
| --- | --- | --- | --- | --- | --- |
| Personal info, Email | Account identity through Clerk | Yes | No | Account management | No, if syncing |
| Personal info, User IDs | Clerk subject, stored as `userId` | Yes | No | Account management, app functionality | No, if syncing |
| Personal info, Name | Display name, shown to squad members | Yes | No | App functionality | Yes |
| Health and fitness, Fitness info | Sessions, sets, weight, reps, volume, personal records, estimated 1RM | Yes | No | App functionality | Yes |
| Health and fitness, Health info | Health Connect reads and user-triggered session export | Yes | No | App functionality | Yes |
| Messages, Other in-app messages | Squad chat bodies | Yes | No | App functionality | Yes |
| Photos, Photos | Avatar image, if the user sets one | Yes | No | App functionality | Yes |
| App activity, Other user-generated content | Workout titles, squad names, session recaps | Yes | No | App functionality | Yes |

## Not collected

Declare these as not collected. They are the claims most likely to be audited,
so keep them true.

| Play data type | Why not |
| --- | --- |
| Camera and photos, live camera | Frames are read by MediaPipe in memory, closed immediately, and never persisted or transmitted. The camera is used, not collected. |
| Location | Never requested. |
| Contacts, calendar, SMS, call logs | Never requested. |
| Financial info | No purchases, no subscriptions, no payment data. |
| Crash logs, diagnostics, other performance data | No crash or analytics SDK is linked. Verify with `./gradlew :app:dependencies` before submitting. |
| Device or other IDs | No advertising ID, no install ID. |
| Web browsing history | None. |

## Deletion

Play requires an in-app deletion path and a publicly reachable web URL for any
app that lets people create accounts. Neither exists yet.

When it is built, it must remove:

- The Clerk user record.
- Rows in `users`, `workout_sessions`, `workout_templates`, `personal_records`,
  and `squad_members`.
- Squad ownership transfer or squad deletion where the user is the owner.
- Chat messages authored by the user, or an anonymization policy documented in
  the privacy policy.
- R2 objects with `customMetadata.ownerId` equal to the user.
- The `session:<squadId>` state held by `SquadRoomDO`.
- Local Room tables, the outbox, and the device-local account.

## Notes for the reviewer

If asked how on-device pose estimation works: the camera stream is consumed by
MediaPipe `PoseLandmarker` in `LIVE_STREAM` mode, and only compact derived
metrics such as rep counts leave the analyzer. No frame, landmark stream, or
video is written to disk or sent over the network.
