# Marquise Jones

I build complete AI products because I want them to exist. Not
prototypes and not demos: a model in production generating deliverables that
people paid for, money moving through Stripe, customer data under row level
security, and an on call rotation of one.

What that actually took is below.

---

## The technical work

Most of it comes out of [Workforce Radar](https://workforceradar.com), a career
intelligence platform I built and run alone, schema to checkout to deploy.

**Production LLM integration.** Claude generates paid deliverables inside
background functions, behind Stripe checkout, for customers who paid for them.
The scoring underneath is deliberately not generated. I chose deterministic
classification over model classification and documented the reasoning, so
results are reproducible and auditable, the expensive path stays bounded, and a
model outage degrades the product instead of breaking it. Model output is
treated as untrusted input: parsed defensively, shape checked against required
fields, and failures marked terminally so no record sits pending with nothing
watching it.

**Payments and the paths money moves through.** Stripe in live mode: checkout
sessions, subscriptions, customer portal, and webhook handling with HMAC
signature verification and a replay tolerance window. When two products share a
price point, the router refuses to write rather than guessing which one was
bought, alerts an operator, and returns a status that stops the retry storm.
Nineteen test files sit on those branches specifically, because a payment
written to the wrong product is found weeks later by the customer.

**Data modeling and access control.** Roughly forty Postgres tables under row
level security. Private storage set to deny all except the service role.
Scheduled aggregate refreshes on pg_cron every six hours. Where the hosted admin
API proved unreliable, a SECURITY DEFINER Postgres function over PostgREST
instead, with the reason recorded next to it.

**Operating it after shipping.** Rollback runbooks, smoke tests, post deploy
monitoring notes, secrets rotation, scheduled backups, health and disk checks.
CI that runs the test suite and then greps the codebase for retired terminology,
so a finished migration cannot silently regress.

**Native applications.** A pywebview desktop shell over a Python engine, bundled
with PyInstaller into a macOS .app that a non technical user installs by
dragging it. Playwright drives real Chrome for session capture, and no password
is ever typed or read by the program. Credentials resolve to Application Support
when the app is frozen, because a bundled app launches with its working
directory at root and a relative path silently loses the file.

**Acquisition and hot paths.** Playwright, Browserbase and AgentQL against live
sites. In the draft engine, player availability is derived by set arithmetic
against a cached universe rather than by polling the free agent endpoint,
because that endpoint lags the live board by a pick or two at exactly the moment
the lag is most expensive.

Node 20, Python, Postgres, Netlify Functions, Supabase, Stripe, the Anthropic
API, Playwright, Twilio, PyInstaller.

---

## What is here

**[Radar-Platform](https://github.com/jonesmarquise31/Radar-Platform)** is the
architecture record for the platform above. Build logs, numbered decision
records, named engineering patterns, and the reasoning behind each. Start here
if you want to see how I think about a system before writing it.

**[radar-patterns](https://github.com/jonesmarquise31/radar-patterns)** is five
production patterns extracted from that platform: webhook signature
verification, verify before write payment routing, idempotent writes under at
least once delivery, deferred model generation, and request auth. Sixty tests,
zero dependencies, runs with `npm test` in about a second. This is the
implementation half of Radar-Platform's architecture half.

**[fantasy-football-intelligence-engine](https://github.com/jonesmarquise31/fantasy-football-intelligence-engine)**
is Underwriter v1, a native macOS draft application. A pywebview shell over a
Python engine that prices player scarcity by value over replacement rather than
projected points, and adapts every recommendation to the shape of a specific
draft slot. Different domain, same habit: find where the conventional number
misleads, and build the one that does not.

The Radar diagnostic engine, the classification core, and the dataset of 250+
classified IT, cybersecurity and finance professionals stay private. The
architecture is public. The proprietary core is not.

---

## What is coming

More in both directions. Tooling for the IT and security work I do during the
day, and projects for the things I care about outside it: sports analytics,
fitness, whatever problem is currently annoying me enough to build for. The
domain changes and the approach does not. I would rather ship a working system
in an unfamiliar area than write a clean abstraction over a problem I have never
actually had.

---

## Elsewhere

- [workforceradar.com](https://workforceradar.com), the platform
- [LinkedIn](https://www.linkedin.com/in/marquise-jones-aa72a2117) and Substack, where I write about the extinction of the W2 layer and what replaces it

None of this was commissioned. I build these because I want them to exist, and
they tend to end up with customers anyway. If your team is working on something
shaped like it, LinkedIn is the fastest way to reach me.
