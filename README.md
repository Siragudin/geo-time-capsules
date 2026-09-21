# 📜 Geo-Time Capsules (Digital Time Capsules)

<p align="center">
  <img src="https://shields.io">
  <img src="https://shields.io">
  <img src="https://shields.io">
</p>

## 🌟 Overview

**Geo-Time Capsules** is a production-ready conceptual specification for a global web application featuring an interactive geo-map of human memories, hopes, and messages, anchored to real-world coordinates and time. Users bury digital "safes" containing text and media at the exact coordinates where they are physically standing, with a delayed unlock timer set years into the future.

> 💡 **Product Note:** This architectural framework is designed with maximum scalability in mind. While perfect for open-source indie development, it is natively structured to serve as an **organic feature-layer for global mapping ecosystems like Google Maps**, turning geographic navigation into a historical continuum.

---

## 🌍 Why This Matters

Humanity produces infinite digital content — but almost none of it is tied to **place** and **future time**.  
Geo-Time Capsules introduce a new digital primitive:

**Geo-temporal memory.**

A capsule is not just a file — it is a *future event anchored to a physical location*.  
This creates a new cultural layer over the world map:

- personal histories,  
- collective memories,  
- artistic messages,  
- time-delayed letters,  
- digital monuments,  
- ephemeral self-destructing stories.

This is a missing piece in today’s digital ecosystem.

---

## 🎨 1. UX / Frontend

- **Map Interface:** Full-screen interactive map centered on the user's live geolocation.
- **Marker States** (clustered automatically):
  - 🔐 **Sealed (Amber):** Locked content, countdown only.
  - 🔓 **Permanent Archive (Green):** Unlocked forever.
  - ⏳ **Self-Destruct (Blinking Red):** Unlocked with deletion timer.
- **Capsule Creation Flow:**  
  GPS → fine-tune pin → attach media → set unlock date → choose lifecycle.
- **Invisible Moderation:**  
  Capsules undergo a background safety pipeline before appearing on the map.

---

## 🕵️ 2. Identity Model: Pseudonymity

Public anonymity, internal accountability.

- Users see capsules without knowing the author.
- Platform knows the author (required account).
- Optional self-identification via content.

### Authentication
- Email/password + OAuth (Google, Apple, etc.)
- Personal dashboard
- Account-based rate limits

---

## 🔒 3. Capsule Lifecycle

Locked at creation. No edits.

1. **Permanent Archive** — unlocks, stays forever.  
2. **Self-Destruct** — unlocks, then auto-deletes after TTL.

### Removal
- Admin-only (reports or moderation flags)
- GDPR-compliant erasure path

### Unlock Window
- **Min:** 1 year  
- **Max:** 100 years (with engineering disclaimer)

---

## ⚙️ 4. Content Moderation Matrix

| Type | Method |
|---|---|
| Text | Toxicity/threat classifier |
| Photo | Image moderation + CSAM hash-matching |
| Audio | Transcription → classifier |
| Video | Frame sampling + moderation + audio transcription |

Tier affects queue priority, not safety depth.

---

## 🗺️ 5. Institutional Users

For museums, universities, archives:

- Endowment/Partner Tier  
- Sustainability Reserve Fund  
- Data transfer obligations in case of shutdown

---

## ⚙️ 6. Technical Architecture

- **Frontend:** React/Vue + Google Maps / MapLibre  
- **Backend:** FastAPI / Node.js / Go  
- **Model:** Authoritative Server  
- **Storage:** PostgreSQL + PostGIS + S3  
- **Durability:** Multi-region redundancy  
- **Abuse Prevention:** reCAPTCHA v3 + account limits

---

## 💼 7. Monetization

- Free: text + photo (5 MB)  
- Paid: video/audio (50–100 MB), gold marker  
- Ads: site chrome only  
- Institutional endowments

---

## ⚖️ License

Published under **CC BY-NC-ND 4.0**.  
Non-commercial use only.  
Commercial integration requires explicit permission.

---

## 🚀 Integration Potential with Google Maps

Geo-Time Capsules can serve as:

- a new **Time Layer** in Google Maps,  
- a cultural extension for **Google Arts & Culture**,  
- a geo-temporal experiment for **Google Labs**,  
- a showcase for **Google Cloud multi-region durability**.

---

## 🛣️ Roadmap (Conceptual)

- Phase 1 — Public Specification  
- Phase 2 — MVP Prototype  
- Phase 3 — Open-Source Community  
- Phase 4 — Institutional Partnerships  
- Phase 5 — Google Maps Integration Proposal
