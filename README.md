# Marquise Jones

DoD IT by day. I build AI systems because I like working out how they behave
when they actually have to hold: when money moves through them, when the data
belongs to somebody, when a person is going to read the output and act on it.

Most of that has gone into one platform. Some of it has gone into compliance
tooling for the environments I work in. The rest is whatever I am curious about
at the time.

---

## Compliance automation

Three tools for the paperwork side of hardening, each with tests and CI. They
cover the documentation and evidence spine of RMF: Implement, Assess, and the
POA&M side of Monitor. Categorize and Authorize are judgment calls with
signatures on them, not automation targets.

| Tool | RMF step | What it produces |
|---|---|---|
| ssp-cis-builder | 3, Implement | SSP narrative and eMASS CIS matrix from a versioned manifest |
| stig-delta-engine | 4, Assess | Remediation scoring between two XCCDF scans, plus a POA&M for what is open |
| acas-poam-generator | 4 and 6, Assess and Monitor | ACAS or Nessus findings collapsed into an importable POA&M workbook |

None of this was planned as a suite. It is what happened after automating the
parts I kept doing by hand.

**What they do.** XCCDF 1.2 results parsing straight out of OpenSCAP and SCC,
CCI normalisation across the four formats ACAS exports them in, CCI to NIST SP
800-53 Rev 5 correlation, DoD CAT rating derivation, and eMASS artifacts whose
column order matches the import template. A PPS pre-flight takes a read only
census of listening services before hardening severs the ones the enclave
exists to run.

**The system description lives in one file.** ssp-cis-builder renders both
documents from a YAML manifest under version control, so changing the boundary
firewall once updates every control statement that references it. The SSP stops
being a document somebody edits and starts being a build artifact.

**Nothing is silently dropped or silently guessed.** An unresolvable CCI is
emitted as UNMAPPED and counted, never defaulted to a plausible control,
because a wrong control on a POA&M item misleads the authorizing official. A
port that cannot be probed is UNKNOWN, never CLOSED, because treating an
unreachable host as quiet would green light a hardening run nobody inspected.
Both rules exist because the opposite behaviour was in the code and I found
what it did.

**Deployability is a design constraint.** The STIG engine has zero third party
dependencies. An air gapped enclave rarely has a reachable package index, and
a compliance tool that cannot be installed where it is needed is not a
compliance tool.

**Reproducibility is enforced, not assumed.** The POA&M generator's output was
varying between runs on identical input because host lists came out of a set.
CI now fails if two runs under different hash seeds disagree. An audit artifact
that changes when you regenerate it cannot be defended.

**An incomplete document says so.** A manifest missing a field renders
NOT SPECIFIED rather than the string None, and a control template naming a
component nobody declared falls back to readable text instead of raising a
KeyError halfway through the document. CI fails if the word None reaches a
generated SSP. A visibly incomplete artifact is fixable; a quietly wrong one
gets signed.

---

## Production AI systems

Most of this comes out of [Workforce Radar](https://workforceradar.com), a
career intelligence platform I built and run alone, schema to checkout to
deploy.

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

**Delivery and infrastructure.** Custom domain on Netlify with the DNS to match:
verification records, SPF, and mail routing, so transactional email lands in
inboxes instead of spam. Edge routing, redirects, and per deploy preview URLs
through `netlify.toml`, with function timeouts tuned individually from 26
seconds up to a 900 second background window. Bundling is declared explicitly
rather than inferred: esbuild cannot statically trace a runtime
`require(path.join(...))`, so shared config and scoring libraries have to be
force included or the function 502s at cold start with a module import error.
I found that the way everyone does, from a failed checkout in production. A
second product ships to its own separate deploy target, and the scheduled
generation worker runs on a Hostinger VPS with cron, health checks, and its own
backup script.

**Native applications.** A pywebview desktop shell over a Python engine, bundled
with PyInstaller into a macOS .app that a non technical user installs by
dragging it. Playwright drives real Chrome for session capture, and no password
is ever typed or read by the program. Credentials resolve to Application Support
when the app is frozen, because a bundled app launches with its working
directory at root and a relative path silently loses the file.

**Design systems.** A documented system rather than a stylesheet: palette, type
scale, a 4px spacing unit, radius and elevation rules marked locked, and a page
type matrix saying which motion is permitted where. Tokens live in one file
whose header states the contract, that the design doc is the canonical source
and tokens cannot change without updating it first. Semantic state colors are
kept separate from decorative ones so an error never renders as an accent.
Contrast ratios are documented against the background, and there is an explicit
reduced motion policy. Constraint is the point: the rules exist so that pages
built months apart by different methods still look like one product.

**Acquisition and hot paths.** Playwright, Browserbase and AgentQL against live
sites. In the draft engine, player availability is derived by set arithmetic
against a cached universe rather than by polling the free agent endpoint,
because that endpoint lags the live board by a pick or two at exactly the moment
the lag is most expensive.

Node 20, Python, Postgres, Netlify (Functions, edge, DNS), Supabase, Stripe,
the Anthropic API, Playwright, Twilio, GSAP, PyInstaller, GitHub Actions.

---

## What is here

**[ssp-cis-builder](https://github.com/jonesmarquise31/ssp-cis-builder)**
renders an SSP summary and an eMASS CIS matrix from one YAML manifest. Fifteen
Rev 5 controls across nine families, per control status and designation
overrides validated against the eMASS enums. 96 tests at 100 percent coverage,
enforced in CI.

**[stig-delta-engine](https://github.com/jonesmarquise31/stig-delta-engine)**
scores DISA STIG remediation between two XCCDF scans and generates the eMASS
POA&M for whatever is still open. Standard library only. 45 tests, CI on
Python 3.9 through 3.13.

**[acas-poam-generator](https://github.com/jonesmarquise31/acas-poam-generator)**
turns a raw ACAS or Nessus export into a styled, importable POA&M workbook.
Per host rows collapse to one record per finding, CCIs resolve to NIST
controls, output is byte stable across runs. 61 tests, CI.

**[radar-patterns](https://github.com/jonesmarquise31/radar-patterns)** is five
production patterns pulled out of the Radar platform: webhook signature
verification, verify before write payment routing, idempotent writes under at
least once delivery, deferred model generation, and request auth. Sixty tests,
zero dependencies, runs in about a second.

**[Radar-Platform](https://github.com/jonesmarquise31/Radar-Platform)** is the
architecture record behind that platform. Build logs, numbered decision
records, named patterns, and the reasoning for each. Start here if you want to
see how I think about a system before writing it.

**[fantasy-football-intelligence-engine](https://github.com/jonesmarquise31/fantasy-football-intelligence-engine)**
is a native macOS draft application. A pywebview shell over a Python engine
that prices player scarcity by value over replacement rather than projected
points. This one is for fun, and it still got shipped as a signed installable
app rather than a script.

The Radar diagnostic engine, the classification core, and the dataset of 250+
classified IT, cybersecurity and finance professionals stay private. The
architecture is public. The proprietary core is not.

---

## Side projects

I write on LinkedIn and Substack about the extinction of the W2 layer, and I
build tools for the people I write for. Past that it is sports analytics,
fitness, and whatever else is annoying me enough to be worth a weekend. The
domain changes and the approach does not. I would rather ship a working system
in an unfamiliar area than write a clean abstraction over a problem I have
never actually had.

---

## Elsewhere

- [workforceradar.com](https://workforceradar.com), the platform
- [LinkedIn](https://www.linkedin.com/in/marquise-jones-aa72a2117), long form on hiring systems and career strategy

None of this was commissioned. I build them because I want them to exist.
