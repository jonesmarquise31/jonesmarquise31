# Marquise Jones

Systems engineer at the Naval Health Research Center. I write about the
extinction of the W2 layer and prepare IT professionals for what replaces it.

The argument is straightforward. The employment relationship that organized
technical careers for two generations is being unbundled, and the people most
exposed are the ones doing the most work: the systems administrator carrying
engineer scope on a sysadmin title, the security analyst running architecture
decisions without the pay band, the infrastructure lead whose contribution is
invisible to the systems that price them. They are not underperforming. They
are being read incorrectly by a hiring apparatus that has not been rebuilt for
what they actually do.

I write about that on [LinkedIn](https://www.linkedin.com/in/marquise-jones-aa72a2117)
and on Substack. This profile is the other half: the technical work behind the
argument.

---

## What's here

**[Radar-Platform](https://github.com/jonesmarquise31/Radar-Platform)** — the
architecture record for [Workforce Radar](https://workforceradar.com), a career
intelligence platform I built and run. Build logs, numbered decision records,
named engineering patterns, and the reasoning behind each. Start here if you
want to see how I think about a system before I write it.

**[radar-patterns](https://github.com/jonesmarquise31/radar-patterns)** — five
production patterns extracted from that platform: webhook signature
verification, verify-before-write payment routing, idempotent writes under
at-least-once delivery, deferred model generation, and request auth. Sixty
tests, no dependencies, runs with `npm test`. This is the implementation half of
Radar-Platform's architecture half.

**[fantasy-football-intelligence-engine](https://github.com/jonesmarquise31/fantasy-football-intelligence-engine)**
— Underwriter v1, a native macOS draft application. A pywebview shell over a
Python engine that prices player scarcity by value over replacement rather than
projected points, and adapts its recommendations to the shape of a specific
draft slot. Different domain, same habit: find where the conventional number is
misleading, and build the one that isn't.

The Workforce Radar platform itself — the diagnostic engine, the classification
core, the dataset of 250+ classified IT, cybersecurity, and finance
professionals — stays private. The architecture is public; the proprietary core
is not.

---

## What I build with

Python, JavaScript, and Node on the serverless side. Postgres with row-level
security. Stripe for payments. The Anthropic API where generation earns its
place, and deterministic logic everywhere it does not. Playwright for
acquisition. PyInstaller when something needs to become an application someone
can double-click.

The pattern across all of it: understand the domain first, keep the expensive
path bounded, and make the system explain its own reasoning so a person can
disagree with it.

---

## What's coming

More of the same in both directions. Tooling for the IT and security work I do
during the day, and projects for the things I care about outside it — sports
analytics, fitness, whatever problem is currently annoying me enough to build
for. The domain changes; the approach does not. I would rather ship a working
system in an unfamiliar area than write a clean abstraction over a problem I
have not actually had.

---

## Elsewhere

- [workforceradar.com](https://workforceradar.com) — the platform
- [LinkedIn](https://www.linkedin.com/in/marquise-jones-aa72a2117) — long-form on hiring systems and career strategy

Open to contract, partnership, and full-time work. Reach me through LinkedIn.
