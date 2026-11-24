# Brand

## Positioning

Hypertrophy is offline-first strength training. The promise is that the app
works in a basement gym with no signal, and that nothing leaves the device
unless the lifter chooses to sync it.

One line: **Strength training that works without a signal.**

## Logo

`wordmark.png` is the primary mark: "Hyper" in white, "trophy" in a crimson to
ember gradient. It is built for dark ground.

| Rule | Value |
| --- | --- |
| Minimum width | 160 px |
| Clear space | Half the cap height on every side |
| Backgrounds | Obsidian Forge `#0D0D0F` or any surface at least as dark as `#18181C` |
| Never | Recolour, outline, add shadow, stretch, or place on white |

On light surfaces use the app icon, or place the wordmark inside a dark tile.
There is no light-ground wordmark, and one should not be improvised by
inverting the white.

The icon is the letter H in Space Grotesk Bold, filled with the brand gradient,
on an Obsidian Forge square. It is legible at 48 px, which is the only size
that matters for a launcher.

## Colour

Tokens are the Phoenix UI palette. Do not introduce colours outside this table.

### Dark theme, Obsidian Forge

| Token | Hex |
| --- | --- |
| Canvas | `#0D0D0F` |
| Surface | `#18181C` |
| Elevated | `#222228` |
| Text primary | `#F5F5F7` |
| Text secondary | `#9E9EA8` |

### Light theme, Daylight Ash

| Token | Hex |
| --- | --- |
| Canvas | `#F8F7F4` |
| Surface | `#FFFFFF` |
| Elevated | `#EFECE6` |
| Text primary | `#111114` |
| Text secondary | `#5C5C66` |

### Brand and semantic

| Token | Hex | Use |
| --- | --- | --- |
| Crimson primary | `#D91E36` | Primary action, brand gradient start |
| Ember orange | `#FF6B1A` | Brand gradient end, streaks |
| Solar gold | `#FFB800` | Personal records, trophies |
| Form error crimson | `#FF2A4B` | Form faults, destructive actions |

The gradient runs left to right, crimson to ember. It is reserved for the
wordmark, the icon, and the ember ring timer. It is not a background wash.

## Typography

All three families are bundled in the app under the SIL Open Font License.

| Family | Role |
| --- | --- |
| Space Grotesk | Screen headers, workout titles, streak counters, wordmark |
| Plus Jakarta Sans | Body copy, instructions, chat, buttons |
| JetBrains Mono | Rep counters, rest timers, weights, tempo ratios, tabular figures |

Weights in use: Space Grotesk Bold, SemiBold, Medium. Plus Jakarta Sans
Regular, Medium, SemiBold. JetBrains Mono Medium, Bold.

Do not set numerals in a proportional font anywhere a number changes at
runtime. Rep counters that jitter are the fastest way to look amateur.

## Voice

Declarative, specific, and quiet. The product does hard things; the copy does
not need to.

- Sentence case for headings. No exclamation marks.
- Numbers over adjectives: "syncs in under a second", not "lightning fast".
- Say what is local and what is not. Privacy claims must be checkable against
  the code.
- Never imply medical advice, diagnosis, or guaranteed results.
- No emojis. No rocket, fire, or muscle emoji in store copy, notifications, or
  UI strings.

## Motion

Shared element transitions carry a card into the live workout. Timers use an
animated ember ring. Rep completion and PR detection fire haptics. Motion is
purposeful and short; nothing bounces for decoration.
