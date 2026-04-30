# Marquise Jones

Founder of [Workforce Radar](https://workforceradar.com) — a career intelligence platform that diagnoses how the modern hiring system reads or misreads skilled professionals.

I build production AI systems at the intersection of structured data, behavioral classification, and decision support. Most of my current work is on Radar's diagnostic engine, three-portal architecture (professional, coach, hiring), and the standalone Decode entry-tier product. The dataset of 250+ classified IT, cybersecurity, and finance professionals is what powers the classification.

This profile holds the public artifacts from that work — build notes, design-system documentation, and engineering patterns. The proprietary core stays private.

---

### Live products

- **[The Radar Decode](https://radar-decode.netlify.app)** — $47 one-page operator brief, written by hand, anchored to a buyer's actual LinkedIn and resume. Entry tier of the Radar line.
- **[Workforce Radar](https://workforceradar.com)** — career intelligence platform with paid diagnostic ($97 RDS) and full engagement ($1,497 Full System).

### Recent build artifacts

- **[V41 build log](https://github.com/jonesmarquise31/Radar-Platform/blob/main/build-log/v41-radar-decode.md)** — the Decode launch: standalone deploy on a separate domain, post-payment intake flow, real-time buy alerts via Stripe webhook to Telegram
- **[V40 build log](https://github.com/jonesmarquise31/Radar-Platform/blob/main/build-log/v40-funnel-friction.md)** — funnel friction reduction: name-only entry, results gated behind account creation, phone removal across paid flows
- **[Brand system](https://github.com/jonesmarquise31/Radar-Platform/blob/main/design/brand-system.md)** — typography hero, four-color palette, V39.1 stat block treatment, the deliberate rejection of SaaS chrome
- **[Engineering patterns](https://github.com/jonesmarquise31/Radar-Platform/tree/main/patterns)** — verify-before-write with Stripe, idempotent submissions, real-time buy alerts

### Stack

TypeScript, Node.js, Supabase (Postgres + Auth + RLS, private storage with deny-all-except-service-role), Netlify (functions, hosting, edge), Stripe (Checkout + webhooks), Anthropic API (Claude), Telegram Bot API for operational alerts, n8n for orchestration, Python where it earns its place.

### Frameworks

- **3C Framework (Career Chessboard)** — tactical positioning: CEO of Your Own Career, Consultant Mindset, Control the Narrative
- **Abundance Mindset Reframe (AMR)** — psychological operating system for career operators

Frameworks are delivered to Full System buyers as part of the Radar engagement.

### Elsewhere

- [workforceradar.com](https://workforceradar.com) — the platform
- [radar-decode.netlify.app](https://radar-decode.netlify.app) — the Decode
- [LinkedIn](https://www.linkedin.com/in/marquise-jones-aa72a2117) — long-form thinking on hiring systems and career intelligence
