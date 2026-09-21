# Project Spec: Geo Time Capsules

## Overview

A global web app — an interactive geo-map of human memories, hopes, and messages, anchored to real-world coordinates and a real-world date. Users bury digital "safes" containing text and media at the exact coordinates where they're physically standing, with a delayed unlock timer set years into the future.

---

## 1. UX / Frontend

- **Map interface:** full-screen interactive map, centered on the user's current geolocation.
- **Marker states**, clustered automatically so the map doesn't choke at scale:
  - 🔐 **Sealed (amber marker):** content locked server-side. Clicking shows only a countdown timer and the author's hint text.
  - 🔓 **Permanent archive (green marker):** unlocked capsule that stays on the map indefinitely as a historical marker. Content readable by anyone.
  - ⏳ **Self-destruct (blinking red marker):** unlocked capsule with an active deletion timer.
- **Capsule creation flow:** app requests GPS coordinates, user can fine-tune pin placement, attach media, set the unlock date, and pick a lifecycle mode.
- Capsules go through moderation before they're published to the map — no unmoderated content ever hits the public layer. We don't expose the moderation pipeline to the end user; they just see a brief "processing" state and then the pin drops.

---

## 2. Identity model: pseudonymity, not anonymity

Important distinction we're baking in from day one: **anonymity is public-facing only, not system-wide.**

- Any user can view another user's unlocked capsule without knowing who authored it.
- The platform always knows the author, tied to a required account.
- The author stays unidentified to other users unless they choose to self-identify in the content itself (face on camera, voice on audio, signed text, etc.).

### Auth
- Standard email/password + OAuth (Google, Yandex, etc.) — nothing exotic here, off-the-shelf integration (Auth0 / Firebase Auth / NextAuth).
- Per-user dashboard listing their own capsules (visible only to them).
- Rate limits on capsule creation tied to account ID, not IP — more resilient against abuse than IP-based throttling.

---

## 3. Capsule lifecycle

Locked in at creation time. No post-creation edits to content.

1. **Permanent archive mode** — unlocks on the target date, stays on the map forever as a digital record of that location. No delete button exposed to the author.
2. **Self-destruct mode** — author sets a TTL after unlock (e.g., 30 days). A scheduled job wipes the record from the DB and object storage on expiry. No recovery path.

### Content removal
- Removal is admin-side only — triggered by a user report or by moderation flagging content post-publication.
- Public "Report" action on every unlocked capsule.
- A right-to-erasure (GDPR) request path exists, gated behind identity verification for the claimant.

### Unlock window
- **Minimum: 1 year.** This isn't arbitrary — it filters out the "reminder app" use case and kills same-day drop-and-detonate abuse vectors.
- **Maximum: 100 years**, shipped with an honest disclaimer, not a marketing promise:
  > "Capsules are stored across multiple independent geographic regions with redundant backups. We can't guarantee 100-year uptime against force-majeure events, but we're architected and committed to maximizing the odds."
- MIN/MAX are config values on the backend, not hardcoded in the client.

---

## 4. Content moderation

Design principle: **users need to know moderation exists, not how it works.** Surfacing the mechanism just gives bad actors a target to route around.

### Pipeline by content type

| Content type | Check |
|---|---|
| Text | Lightweight toxicity/threat classifier |
| Photo | Standard image moderation API + mandatory hash-matching against CSAM databases (PhotoDNA/NCMEC) |
| Audio | Transcription → text classifier |
| Video | Frame sampling every N seconds → image moderation + hash-matching, plus separate audio-track transcription → text classifier |

### Flow

1. Capsule created → status `pending`, not rendered on the map.
2. Automated content scan runs.
3. Clean → capsule publishes to the map.
4. Ambiguous result → routed to human review queue; rejected by default if not cleared.
5. Reports on unlocked capsules remain a second-layer defense — automated moderation won't catch everything.
6. Rejected capsules get a generic reason ("didn't meet community guidelines") with a link to the policy page — no algorithm internals disclosed.

Tier (free vs. paid) affects queue priority only, not moderation rigor. Every capsule — regardless of plan — gets the same depth of check, including mandatory CSAM hash-matching on photo content.

---

## 5. Institutional users

Museums, universities, schools, and archives will use this differently than individual users — they're thinking in terms of "will this platform still exist when the capsule unlocks," not megabytes.

- Endowment/partner tier: a one-time or recurring contribution to a sustainability fund, not a storage-size upsell.
- A slice of revenue (premium capsules, institutional contributions, donations) routes into a reserve fund that keeps baseline hosting costs covered even if the core business fails — similar in spirit to how the Internet Archive is structured.
- Terms of service spell out, up front, what happens to the capsule archive if the company shuts down (data transfer obligation to a partner nonprofit/foundation).

---

## 6. Technical architecture

- **Frontend:** SPA (React/Vue), map layer via Google Maps JS API or MapLibre/OpenStreetMap. Client-side media compression before upload, capped to the free-tier limit.
- **Backend:** FastAPI / Node.js / Go. Authoritative server model — sealed capsule content never gets sent to the client before unlock; server enforces `currentTime >= unlockAt` on every read, so poking at the network tab doesn't get you anywhere.
- **Storage:** PostgreSQL + PostGIS for geo-indexed records. Media in an S3-compatible object store, access-gated behind short-lived presigned URLs (15-minute TTL) for unlocked capsules only.
- **Durability:** multi-region server redundancy across independent jurisdictions.
- **Abuse prevention:** reCAPTCHA v3 + account-scoped creation limits.

---

## 7. Monetization

- **Free tier:** text + photo, capped at 5 MB.
- **Paid tier:** video/audio support, expanded storage (50–100 MB), distinct gold marker icon on the map.
- **Ads:** site chrome only (sidebar, footer, landing page, stats pages). Zero ads inside capsule cards or the unlock modal — that moment stays clean.
- **Institutional/endowment tier:** for museums, universities, archives.
- **Donation button:** for individual supporters and institutions alike.
