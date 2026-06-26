# 🏆 Street Play

**A sports competition management platform connecting players, organizers, and businesses through live, role-based match experiences.**

Street Play is a full-stack mobile platform built to digitize grassroots sports competitions — handling everything from player onboarding and match entry payments to live match tracking and post-match performance ratings. The backend is the core of the system: a multi-role architecture supporting four distinct user types, each with scoped permissions, workflows, and data access patterns.

This is not a CRUD app. It's a system with real financial flows, real-time state, and access control logic that had to be designed before a single screen was built.

---

## 🧠 What It Solves

Grassroots and amateur sports competitions are usually run informally — entry fees collected in cash, match results tracked on paper, no reliable way to rate player performance over time. Street Play turns that into a structured digital system: organizers can run competitions end-to-end, players get a verifiable performance history, and businesses can sponsor or monetize competitions through a built-in payment layer.

---

## 🏗 System Architecture

### Multi-Role Access Model
The platform supports 4+ distinct user types (players, organizers, verified businesses, and system admins), each with its own onboarding flow, permission scope, and data visibility rules. Role-based access is enforced at the API layer, not just the UI — meaning the same endpoint behaves differently depending on who's calling it.

### Authentication
- Email/password signup with OTP verification, login, and password reset
- Multi-step onboarding for player profile creation
- **Google OAuth via a secure code-exchange flow**: rather than passing tokens directly through redirect URLs (a common vulnerability), the backend issues an encrypted, single-use token cached server-side with a short TTL. The frontend exchanges this token for a session via a dedicated callback screen — closing off a real attack surface most OAuth integrations leave open.
- Token-based API authentication via Laravel Sanctum, with persistent session handling on the client.

### Payments
Paystack is integrated for match entry fees and payouts, built to Paystack's documented standards. Payout access is gated to verified business accounts only — a deliberate compliance and fraud-prevention decision baked into the access control layer, not bolted on afterward.

### Real-Time Match Tracking
Live match state and event updates are broadcast to connected mobile clients using **Laravel Reverb** (Laravel's native WebSocket server) paired with `pusher-js` on the client. This powers real-time score updates, match events, and live state changes across a public channel architecture — without relying on a third-party WebSocket provider.

### Player Rating Engine
A dual-layer rating system tracks player performance per match:
- **User ratings** — peer-submitted scores from other players/organizers
- **System ratings** — calculated from in-match performance statistics

These two layers are combined to produce a more reliable, harder-to-game performance signal than either source alone would give.

---

## ⚙️ Tech Stack

**Backend:** Laravel · Laravel Sanctum · Laravel Reverb (WebSockets) · MySQL · Paystack API
**Frontend:** React Native (Expo) · pusher-js
**Architecture:** Role-based access control · Encrypted single-use token exchange · Real-time broadcast channels

---

## 🔍 Engineering Challenges Solved

- **Secure OAuth without redirect token exposure** — designed a backend-issued, TTL-bound token exchange instead of passing session tokens through redirect URLs.
- **Cross-stack debugging** — resolved response payload mismatches between frontend expectations and backend output, a Laravel cache driver misconfiguration affecting token TTL behavior, Eloquent query bugs (`get()` vs `first()` returning unexpected collection vs model types), and URL encoding issues breaking redirect-based auth flows.
- **Complex relational schema** — designed the database structure to support competitions, matches, multi-role users, ratings, and payment records as interconnected but independently scalable entities.

---

## 📌 Status

Actively in development. Core authentication, payment integration, real-time match tracking, and the rating engine are built and functional; the platform is being extended with additional competition and organizer-facing features.

*Code in this repository is private during active development. Architecture details, schema design discussions, and a walkthrough are available on request.*

---

## 📬 Built by

**John Oduyebo**
Backend Engineer (Laravel/PHP) · Quantitative Data Analyst (Python)
📧 oduyebojohn123@gmail.com · 🐙 [github.com/alpha-king1](https://github.com/alpha-king1) · 💼 [LinkedIn](https://www.linkedin.com/in/john-oduyebo-514923213)
