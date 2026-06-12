# Journey Pages Plan — Your Relationship Coach

> Status: PLAN (v1, 2026-06-12). Nothing in this document is built yet.
> Companion repos: `internal-coach4u-hub` (Journey Card formula, transcripts, file notes),
> `Marketing-materials` (brand, video pipeline `video/README.md`, ElevenLabs voice).

## Outcome in one sentence

A set of membership-gated, one-page concept explainers, each with a short branded video in
Cath's voice, sent to couples after a session so the concept they just learned lands again at
home, hosted inside Your Relationship Coach so the portal becomes chargeable.

## Why here (the platform decision)

Keep building in **yourrelationshipcoach**. Reasons:

- The auth + membership gating (`users.membership_status = 'active'`) already works, so the
  content is already behind a paywall-shaped door. "I haven't nailed the login yet" is a
  polish problem, not an architecture problem.
- The couple portal spine already exists (`couple.html` Home, Planning, Strategy, Operations,
  Learn) and the Learn page already lists 23 concept resources by module. Journey pages are a
  natural extension, not a new build.
- The internal hub keeps the practitioner side (transcripts, file notes, patterns); this repo
  stays the client-facing product. Clean split, same Supabase pattern.

## What a Journey Page is

Adapted from the ThriveHQ Journey Card formula (internal hub `CLAUDE.md`), reshaped for
post-session teaching rather than onboarding steps:

1. **Header** — module colour, concept title, one-line promise ("The dance you noticed in
   session, and how to step out of it").
2. **Video first** — Vimeo embed at the top, ~90 seconds, Cath's voice. The page works
   without it, but the video is the hero.
3. **What we covered** — the Journey Card "Confirmed" section translated to coaching: 3-4
   green-tick lines anchoring what the couple already did in session ("You named your cycle",
   "You each took a turn as sender and receiver"). Always first, always present.
4. **The concept on one page** — the existing infographic-style explainer (many already exist
   in `resources/relationships/`).
5. **Try this before next session** — one small practice, not homework-as-burden.
6. **Questions / contact** — standard footer, WhatsApp as primary channel.

**Tone:** Couples sub-brand (accent `#be185d` per `Marketing-materials/brand-and-voice.md`),
warm, holds both partners, the client is the couple, Australian English, no exclamation marks.
Inside this portal the v2.2 design system (navy `#003366` / teal `#0D9488`, `css/style.css`)
stays the chrome; the pink accent is reserved for journey-page highlights so they read as a
series.

## Video pipeline (already proven)

Per `Marketing-materials/video/README.md`:

- Remotion (React → MP4) rendered inside Claude Code, square 1080×1080, ~90 s.
- ElevenLabs TTS with Cath's cloned voice (id `V50HFUKIgwPl4QEG3try`), one call per scene,
  silence-padded, ffmpeg-concatenated, timing JSON for scene sync.
- Fonts inlined as base64; source committed, MP4 not — finished video lives on Vimeo.
- Embed pattern: `https://player.vimeo.com/video/{ID}` in a 16/9 (or square) wrapper.
- One sub-folder per video in `Marketing-materials/video/<project-name>/`.

Note on privacy: Vimeo free tier is public-URL. Concept videos are generic teaching content
(no client material), so free tier is acceptable to start; revisit Plus/Pro for
domain-restricted embeds when the portal becomes paid.

## Concept backlog — ranked by what live sessions actually needed

Drawn from the session transcripts and file notes in the internal hub (client names stay in
the CRM, not in this repo). Frequency = how many current clients needed the concept taught
in-session in May-June 2026.

| Priority | Journey page (video + page) | Existing page to build on | Why first |
|---|---|---|---|
| 1 | Pursuer–Avoider: your cycle, not your characters | `pursuer-avoider-dynamics.html` | Taught in every couple file currently open; both couples named their own cycle in session |
| 2 | The 4 Relationship Killers (criticism–defensiveness focus) | `the-4-relationship-killers.html` | The criticism→defensiveness arc appeared live in both couples' sessions |
| 3 | The Imago Dialogue: sender and receiver | `communication-and-imago-dialogue.html` | First practice is always clunky; a refresher video directly protects the at-home practice |
| 4 | Your body in conflict: noticing and self-soothing | `your-body-in-conflict.html` + `nervous-system-and-conflict.html` | Somatic signs and "neither of us can help while dysregulated" was a live lesson; circuit-breakers need reinforcement at home |
| 5 | Mental load: seeing it, naming it, sharing it | new page | Named explicitly in session ("isolated"); no existing resource covers it |
| 5b | Raising an issue safely (the Issue Clarifier) | `issue-clarifier.html` | Walked through live in two files; the "notice the issue before it becomes a fight" growth edge |
| 6 | Capacity and connection (window of tolerance for couples) | `capacity-and-connection.html` | Bridges couples work and ADHD/NDIS work; stress shrinking availability was a theme in three files |
| 7 | Strengths as a communication lens | `your-strengths-as-a-couple.html` | CliftonStrengths used as a non-blaming frame (e.g. Harmony, Achiever); pairs with Gallup journey |
| 8 | Repair: coming back after a blow-up | `rebuilding-trust.html` (partial) | Repair attempts surfaced in session; the "what's the point" default is the risk to interrupt |
| 9 | ADHD and your relationship | `adhd-and-your-relationship.html` | Individual/NDIS overlap: pacing, consistency, preemptive action before the crash |
| 10 | Daily connection rituals | `daily-connection-rituals.html` | The maintenance layer once the repair concepts have landed |

Build order: ship 1-3 as the pilot (they cover every active couple), then 4-5, then the rest
as sessions demand. One video at a time, reviewed by Cath before the next is made.

## Where journey pages live in the portal

- New folder `journey/` at repo root (sibling of `resources/`), one page per concept,
  e.g. `journey/pursuer-avoider.html`. Same auth guard as every protected page.
- Each journey page links to its deeper worksheet in `resources/relationships/` — journey page
  = watch and absorb; resource page = print and work.
- Surface them in three places:
  1. A "Recently in session" strip on `couple.html` (Home) — manual order to start.
  2. The Learn page (`couple-learn.html`) gets a "Journey" badge on concepts that have a video.
  3. **The post-session send** — the real distribution channel. After a session, Cath (or the
     file-note flow in the internal hub) sends the couple one WhatsApp link: "This is the one
     page to look at before next session." The CRM's draft-email-from-session flow can suggest
     which journey page matches the session's filed themes.

## Connection to the Patterns work (internal hub)

The file-note **themes** field now holds named patterns per session (Pursuer–Avoider,
criticism–defensiveness, mental load, and so on). That gives a natural mapping:

> filed pattern → matching journey page → post-session send.

When 5+ journey pages exist, add a small lookup in the internal hub's file-note email drafter
(pattern keyword → journey URL) so the suggested follow-up email includes the right page
automatically.

## Phases

- **Phase 1 — pilot (one concept end to end):** Pursuer–Avoider. Script in Cath's voice →
  Remotion video → Vimeo → `journey/pursuer-avoider.html` → send to both active couples after
  their next session. Prove the loop before scaling.
- **Phase 2 — core trio:** add 4 Relationship Killers + Imago Dialogue pages; add the Home
  strip and Learn badges.
- **Phase 3 — pattern-driven growth:** one new journey page per recurring filed theme (mental
  load, body in conflict, capacity). Wire the CRM theme → page suggestion.
- **Phase 4 — chargeable portal:** once 8-10 journey pages exist, the portal is a sellable
  membership: tidy the login flow, decide pricing, and move video hosting to a
  domain-restricted tier if needed.

## Open decisions for Cath

1. Video length and format: stick with the square 90-second template, or 16:9 for these
   since they are watched on the portal rather than social.
2. Whether journey pages keep the portal navy/teal with pink accents (recommended) or go full
   Couples pink.
3. Pilot couple(s) for Phase 1 feedback.
