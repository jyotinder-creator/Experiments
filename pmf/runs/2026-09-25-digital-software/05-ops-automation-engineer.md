# 05 — Ops & Automation Engineer (Priya) — run 2026-09-25-digital-software

**Scope:** the two finalists kept by `03-customer-psychologist.md`: **A. Compliance-grade document render API** (Germany-first) and **B. Shopify accessibility regression guard**. `04-offer-architect.md` did not exist when this was written, so pricing follows the ranges in 03: **A: $19 / $49–99 / $249+ per month; B: $29 / $79 / $149–199, agency $299+**. Re-check the unit economics below once 04 lands.

**Founder assumption:** a solo technical founder who can code in Python/TypeScript, working 15–20 h/week during the build. For a founder who can't code, I give the cost of hiring the work out.

**Evidence notes:** prices come from search snippets (direct fetches are mostly blocked). **[unverified]** marks anything I could not confirm through a 2026 source. Hetzner raised prices twice in 2026 (1 Apr and 15 Jun), and I use the post-June prices.

## Summary

| | A. Render API | B. Shopify a11y guard |
|---|---|---|
| **Passivity Score** | **7 / 10** | **4 / 10** (5 if Shopify grants a write_themes exemption and support is productized) |
| Build to launch | ~250–330 h, ~$300–1.5k | ~300–380 h, ~$300–2k |
| Build to "systemized" | ~450–550 h total, ~$2–4k total | ~500–650 h total, ~$3–5k total |
| Cost to hire it out (non-coder) | ~$35–55k, plus a ~$1–2k/mo standards retainer | ~$35–65k, plus a ~$1.5–3k/mo dev and support retainer |
| Monthly running cost (100 customers) | ~$90–150, plus payment fees | ~$140–200, plus the 2.9% Shopify billing fee |
| Monthly running cost (500 customers) | ~$350–500 + ~$0.8–1.5k support contractor | ~$500–700 + ~$2.5–5k support contractor(s) |
| Founder hrs/week (100 customers) | **4–6** | **7–10** |
| Founder hrs/week (500 customers) | **5–8** (with a contractor) | **8–12** (with 1–2 contractors) |
| Top risk | A silently wrong "valid" report on an invoice the customer's AP rejects. This is a trust and liability event on a billing-critical path. | Shopify gatekeeping (theme-write exemption, app review, API changes) plus non-technical merchants who turn "tool" into "do it for me" service work. |

---

## Niche A — Compliance-grade document render API

### A1. Passivity Score: **7 / 10**. Verdict

This is the most automatable business in the run. The buyer is a developer who reads docs, the product is a stateless API call, delivery is 100% software, and billing is self-serve. Nobody has to be onboarded by a human. It is **not** an 8–9, for three structural reasons. (1) **Standards drift is permanent founder work.** ZUGFeRD 2.4/Factur-X 1.08 shipped in Dec 2025 and became mandatory on 15 Jan 2026. KoSIT shipped a new XRechnung validator configuration on 2026-08-31 (CEN schematron 1.3.16) with rule-severity overrides. WeasyPrint ships roughly quarterly and still has open PDF/UA table-tagging issues. Someone has to understand these changes, and that someone is you. (2) **You sit on the customer's billing path.** Invoice runs cluster on the 1st of the month, so an outage or a bad release lands on everyone at once, and you need real on-call around month-end. (3) **Support tickets are "your validator says my invoice is invalid"**, which usually means the customer's data is wrong (missing BT fields, VAT category errors). That can be deflected with good error messages, but not fully. With a golden-file CI suite, auto-mapped error explanations and a part-time technical support contractor, the business settles at **4–8 founder hours a week**. That is within the brief.

### A2. Value-chain map

| Step | Tag | Tool / role | Notes |
|---|---|---|---|
| Acquire: SEO/docs | Automate + founder (initially) | Static docs site (Docusaurus/Starlight on free static hosting) with programmatic pages ("ZUGFeRD API Node", "XRechnung from Stripe invoice") | Write ~30 pages once, then generate the rest from templates. AI drafts, founder reviews. |
| Acquire: dev channels | Founder-only (build phase), then eliminate | dev.to posts, GitHub examples, a public "validator comparison" repo | Front-loaded. Taper to ~1 post/month. |
| Acquire: partners (app builders, Persona B) | Founder-only | Email and calls | The only sales-like activity. Keep it to ≤1 h/week and serve partners through the same self-serve API plus a volume plan. |
| Sell | Automate | Pricing page + self-serve checkout (Paddle MoR, or Stripe Billing) + free tier (50 docs/mo) | No sales calls under $499/mo. Above that, a Cal.com link, founder only. |
| Onboard | Automate | API key on signup, quick-start in 3 languages, a Postman/OpenAPI collection, SDKs generated from OpenAPI, an onboarding email sequence (Postmark) | Measure "first valid document within 24 h". Nudge automatically if it doesn't happen. |
| Deliver: render | Automate | WeasyPrint (BSD) in a Python worker pool on Hetzner, queue (Redis/Postgres), sync endpoint for ≤5 pages, async + webhook for batches | Month-start batch runs go through the async queue. |
| Deliver: XML generation | Automate | Own JSON→CII (EN 16931 / XRechnung CIUS) mapper; Mustang (Apache-2.0) as reference and cross-check | Mapping the ~160 business terms is the core IP. |
| Deliver: validation | Automate | veraPDF (MPL-2.0/GPLv3 dual) for PDF/A-3b and PDF/UA-1; KoSIT validator + XRechnung config (schematron); Mustang validator | Java sidecar service, versions pinned per release. |
| Deliver: validation report | Automate | JSON + human-readable PDF/HTML report per document; error codes mapped to plain-language fixes | The main support-deflection lever. |
| Deliver: archive (add-on) | Automate | Hetzner Object Storage with object lock / immutable bucket, retention per plan | Opt-in only. Default is zero-retention. |
| Support: tier 1 | Automate → delegate | Docs AI bot (Crisp Hugo, or an LLM over the docs), "explain this error" endpoint; part-time technical support contractor from ~200 customers | SOP: reproduce with the customer's payload in a sandbox, map to the docs, escalate if it's a validator disagreement. |
| Support: tier 2 (standards) | Founder-only | You | Validator disagreements, profile questions and new mandates. Can't be delegated cheaply. |
| Standards maintenance | Founder-only (+ automate detection) | GitHub release watch on KoSIT, veraPDF, Mustang and WeasyPrint; Renovate bot; nightly golden-file suite against pinned **and** latest validators | Detection is automated. The judgement isn't. |
| Retain | Automate | Monthly "standards absorbed" changelog email, usage-drop alerts (Postmark + a cron job), annual plan offer at month 3 | Churn signal: doc volume down >50% month on month triggers an automatic check-in email. |
| Bill / dunning | Automate | Paddle (handles VAT and dunning), or Stripe Billing + Smart Retries | Metered overages via usage records. |
| Ops / on-call | Automate + founder | Better Stack uptime + status page (free tier), Sentry, queue-depth alerts; extra worker capacity scheduled for the 1st–3rd of each month | Founder is on call. Keep a runbook so a contractor can restart and scale. |
| Legal / compliance | Founder-only (one-off) + eliminate | DPA template, ToS ("validated against X; tax treatment is the customer's responsibility"), EU hosting | One-off lawyer review. Never claim "legally compliant". |
| Finance / bookkeeping | Delegate | Accountant or bookkeeping tool | ~1 h/month. |

**Eliminated scope:** Peppol/PA transmission (send enterprise leads to Billit/Pagero), remediation of existing PDFs, tax-treatment advice, custom template design services (offer a template gallery instead), and on-prem installs until there is a paid self-host tier with no support SLA.

### A3. Recommended stack and monthly cost

**Rendering engine decision (the key technical question):**

| Option | PDF/A-3b + embedded XML | PDF/UA-1 | Modern CSS fidelity | Cost at scale | Verdict |
|---|---|---|---|---|---|
| Chromium (Puppeteer/Playwright) | No (needs heavy post-processing; output isn't PDF/A-conformant) | Partial tagging, not PDF/UA-conformant | Best | Cheap | **Don't use** as the final writer. Possible later for "visual preview" only. |
| **WeasyPrint (BSD-3)** | **Yes**: `pdf/a-3b` variant; built-in Factur-X/ZUGFeRD attachment API since v64 | **Yes, with gaps**: `pdf/ua-1` variant, but open issues on table header associations, colspan/rowspan and form widgets (veraPDF failures reported in 2026) | Good for invoice-style documents; no JS; slow on long tables (~0.6 s for a 50-row invoice in a 2026 benchmark) | ~$0 licence; CPU-bound | **Use as the primary engine.** Constrain templates: no JS, block layout where possible, a tested template gallery. Contribute or patch the PDF/UA table fixes. |
| Prince (via own licence) | Yes | Yes (mature) | Very good paged-media CSS | **$3,800 one-time per server + annual maintenance**, or a site licence from **~$2,000/yr for startups** (per princexml.com via snippet). SaaS/API resale may need an OEM agreement **[unverified]** | **Phase-3 upgrade path** for a "premium fidelity" tier once MRR is above ~$5k. Not for launch. |
| DocRaptor (Prince as a service) | Yes | Yes | Very good | $15/125 docs, $75/1,250 docs, down to ~$0.025/doc at ~$1,000/mo | **Can't be resold** at $49–99 for 2k–10k docs: the wholesale cost would exceed the price. Useful only as a benchmark. |
| iText / PDFlib | Yes | Yes | Not an HTML renderer (iText pdfHTML is an add-on) | Commercial licence, AGPL otherwise **[pricing unverified]** | Skip. AGPL/licence cost and a JVM-heavy stack. |

**Validation layer:** veraPDF (dual GPLv3+/MPLv2+; use it under MPL as an unmodified separate service, which is fine for SaaS) + KoSIT validator with the XRechnung configuration (latest release 2026-08-31) + Mustang (Apache-2.0) for ZUGFeRD profile checks. All Java, so run them as one long-lived JVM service to avoid ~1 s JVM start-up per document.

**Throughput estimate (my estimate, not benchmarked):** ~0.5–1 s of render CPU plus ~0.5–1 s of validation CPU gives **~1.5 CPU-seconds per 1–2-page invoice**. One 8-vCPU Hetzner CX43 therefore handles ~15–19k docs/hour. At 500 customers averaging 2k docs/month, you process **~1M docs/month ≈ 420 CPU-hours**. That is trivial on average, but it spikes on the 1st of the month, so you need a queue plus 2–4 extra workers scheduled for days 1–3.

| Item | Tool | 100 customers | 500 customers | Status |
|---|---|---|---|---|
| API + render workers | Hetzner Cloud CX33 (€8.49) / CX43 (€15.99) | 2× CX33 + 1× CX43 (validators) ≈ €33 | 3× CX43 always on + 2–3 burst CX43 for ~3 days/month ≈ €55–70 | Verified (post-June 2026 prices, ex-VAT, ex-IPv4) |
| Database | Postgres self-hosted on a CX33 plus Hetzner backups, or a managed Postgres | ~€10–15 | ~€25–40 | Hetzner backup = 20% of server price **[unverified post-June]** |
| Object storage (reports, optional archive) | Hetzner Object Storage | €6.49 (1 TB incl.) | €6.49–20 | Verified (Apr 2026 price) |
| Load balancer | Hetzner LB | ~€6 | ~€6–12 | **[unverified post-June price]** |
| Transactional email | Postmark Basic | $15 (10k emails) | $15–30 | Verified |
| Support chat + KB + AI bot | Crisp Free → Mini €45 → Essentials €95 | €0–45 | €95 | Verified |
| Uptime + status page + on-call | Better Stack free tier; 1 responder licence $29–34 at scale | $0 | $34 | Verified |
| Error tracking | Sentry free / Team | $0 | ~$26 | **[unverified]** |
| Docs site | Docusaurus/Starlight on Cloudflare Pages / GitHub Pages | $0 | $0 | Open source; free hosting |
| Payments | **Paddle (MoR) 5% + $0.50**, or Stripe (2.9% + $0.30) + **Stripe Billing 0.7%** + Stripe Tax | ~6–7% of revenue (Paddle) | same | Verified. Paddle removes VAT/sales-tax filing work, which is worth ~2–3 points of margin for passivity. |
| Insurance (tech E&O / cyber) | Broker | ~$60–150/mo | ~$100–250/mo | **[unverified; get quotes]** |
| LLM (docs bot / "explain this error") | Claude Haiku 4.5 ($1/$5 per M tokens) | <$10 | $20–50 | Verified |
| **Total fixed (ex-payment fees)** | | **~$90–150/mo** (drops to ~$60 before insurance and Crisp) | **~$350–500/mo** | |

**Cost per customer:** infra ~$0.50–1/month at scale, payment fees ~6–7% of ARPU, and support ~$2–3/customer/month once a contractor is hired. At an assumed ARPU of $70, gross margin is ~85–88%.

**Dogfooding bonus:** render your own customer invoices through your API. If you sell through Paddle, Paddle is the merchant of record, but a "we eat our own cooking" invoice is still a marketing asset.

### A4. Sweat-equity build plan (technical founder at 15–20 h/week)

| Phase | Weeks | Founder hours | Cash | Deliverables / exit criteria |
|---|---|---|---|---|
| **0. Spike + demand check** | 1–3 | 30–40 | ~$50 (domain, one CX33) | WeasyPrint + own CII XML produce a file that passes **veraPDF (PDF/A-3b), Mustang and KoSIT** for the EN16931 and XRECHNUNG profiles on 20 sample invoices. PDF/UA-1 passes on the invoice templates. Landing page + waitlist. 8 switch interviews (script in 03). **Kill criterion:** can't reach clean veraPDF + KoSIT passes in 40 h, or fewer than 30 waitlist sign-ups in 4 weeks. |
| **1. MVP** | 4–12 | 150–200 | ~$100–200 | FastAPI service: API keys, JSON and HTML input, sync + async (queue + webhooks), JSON→CII mapper (EN 16931 core BTs), Java validation sidecar, report (JSON + HTML), usage metering, minimal dashboard (keys, usage, recent docs), Paddle/Stripe checkout + free tier, quick-start docs, OpenAPI spec. 3 invoice templates. |
| **2. Launch** | 13–17 | 60–90 | ~$500–1,500 (lawyer review of ToS/DPA ~€500–1,500 **[estimate]**) | Docs site with ~30 SEO pages, Stripe-invoice→ZUGFeRD recipe, public validator-comparison repo, Node + Python SDKs (generated), status page, DPA, zero-retention default, dev.to/HN launch, first 10 paying customers. |
| **3. Systemize** | 18–40 | 120–160 | ~$500–1,000 (insurance start, burst infra) | Golden-file CI suite (~200 invoices × every profile, nightly against pinned and latest validators); error-code→plain-language fix library; AI docs bot; month-start autoscaling; backups + restore drill; runbooks; archive add-on; Stripe webhook connector (listens to `invoice.finalized` and renders automatically); PDF/UA hardening (table tags); usage-drop and dunning automations; KPI dashboard. |
| **4. Delegate** | 40–52 | 30–40 | ~$800–1,500/mo from ~150–200 customers | Support SOPs and macros; hire a part-time technical support contractor (reads JSON, reproduces payloads, knows HTTP APIs; ~$25–40/h, 5–10 h/week); runbook-based "first responder" for month-start incidents; founder keeps tier-2 and standards work. |
| **Total** | ~12 months | **~400–530 h** | **~$1.5–4k one-off**, plus ~$60–150/mo running | |

**Non-coder founder:** hiring this out needs a Python/Java developer who also understands PDF/A and EN 16931. That is a rare profile, at $60–120/h (Upwork seniors run $60–150/h; generic Python devs $20–40/h are **not** sufficient). ~400–450 h comes to **$35–55k** up front, plus a **$1–2k/month standards and maintenance retainer** for ever. That uses the whole budget and creates permanent dependence on one contractor's knowledge. **I don't recommend Niche A to a non-coder.**

### A5. Steady-state operating model

| Role | Does what | Hours/week at 100 customers | Hours/week at 500 customers |
|---|---|---|---|
| **Founder** | Standards maintenance (review validator/schematron releases, update the mapper, run the golden suite), tier-2 support, month-start watch, 1 content piece/month, partner emails, KPI review | **4–6** | **5–8** |
| Support contractor (part-time) | Tier-1 tickets, docs updates from tickets, first responder on the runbook | 0 (founder covers ~1.5 h/week) | 8–12 h/week (~$1–1.5k/mo) |
| Bookkeeper / accountant | Monthly books | ~0.25 | ~0.25 |
| Software | Everything else (render, validate, bill, dun, onboard, retain emails, monitoring) | — | — |

**Where the founder's hours go at 500 customers (my estimate):**
- Standards and dependency updates: ~1.5 h/week on average. It is spiky: 1–2 weeks of ~10 h around each KoSIT or ZUGFeRD release, 2–4 times a year.
- Tier-2 support: ~1.5–2 h/week. Assumes ~60–100 tickets/month overall, 70–80% handled by the bot or the contractor.
- Ops and incidents: ~1 h/week, concentrated in days 1–3 of each month.
- Growth and content: ~1–2 h/week.
- Dashboard review and contractor QA: ~0.5 h/week.

**Support load assumptions:** ~0.3–0.5 tickets per customer per month in the first 6 months, falling to ~0.1–0.2 once the error-explanation library exists. Onboarding tickets dominate: "BT-xx missing", "which profile?", "fonts".

**Churn handling (automated):** usage-drop email, cancellation survey (Paddle/Stripe portal), a "downgrade instead of cancel" offer, and win-back when a new mandate lands (e.g. the DE 1 Jan 2028 all-business obligation).

**Weekly KPI dashboard:** docs rendered, validation pass rate (by profile), p95 latency, month-start queue peak, error rate, MRR / new / churned, trial→paid, tickets opened and resolved, % deflected by the bot, validator versions in production vs latest.

### A6. Top operational risks and mitigations

| Risk | Likelihood / impact | Mitigation |
|---|---|---|
| **False "valid" report**: our validator passes a file that a customer's AP system or a stricter validator rejects | Medium / **High** (trust, churn, liability claims) | Run three independent validators (veraPDF + KoSIT + Mustang); report validator versions in every report; nightly golden suite against the latest releases; ToS limits the claim to "validated against X version Y"; E&O insurance; public changelog. |
| **Standards drift absorbed late** (new XRechnung config, CEN schematron, ZUGFeRD 2.x) | High frequency / Medium | GitHub release-watch alerts; Renovate; "latest validator" shadow run on 1% of traffic; published support policy ("new versions within 30 days"). |
| **Month-start capacity spike / outage** | Medium / High | Async queue + retries; scheduled burst workers on days 1–3; ask customers to use async for batches; status page; runbook a contractor can execute. |
| **WeasyPrint dependency** (small maintainer team, PDF/UA gaps) | Medium / Medium | Pin versions; contribute or sponsor fixes (CourtBouillon offers paid support **[unverified]**); keep templates within tested CSS; Prince licence as a fallback engine once MRR supports it (~$2k/yr site licence for startups). |
| **Founder knowledge is a single point of failure** (the standards expertise lives in your head) | High / High | Write the mapper as data (a BT→XPath table), not code paths; decision log for each profile quirk; golden files are the executable spec; after year 1, consider a small retainer with a freelance ZUGFeRD specialist. |
| **Stripe / Paddle / Chargebee ship native e-invoicing** | Medium / High (demand) | Serve non-Stripe billing, home-grown stacks and app builders; the PDF/UA-for-public-sector segment (ADA Title II, Apr 2027/2028) is a second market for the same engine. |
| **Hetzner price or availability shocks** (three price rises in 2026) | Medium / Low–Medium | Docker/Compose or Nomad deploys that are portable across providers; infra is <2% of revenue, so even a 2× increase doesn't hurt. Keep a second-provider deploy tested. |
| **Data protection** (invoice PII) | Low / High | Zero-retention default, EU-only hosting, DPA, encryption at rest for the archive, no logging of payload bodies. |

---

## Niche B — Shopify accessibility regression guard

### B1. Passivity Score: **4 / 10**. Verdict

The scanning is easy to automate. The *business around it* isn't. Four things drag the score down.

**(1) Change detection is harder than the pitch suggests.** Shopify's `themes/update` webhook **does not fire when theme files are edited**, and there is **no webhook when a merchant installs another app**. "Rescan on every app or theme change" therefore needs polling theme-file checksums (the GraphQL `theme.files` query exposes `checksumMd5` and `updatedAt`) plus a scheduled storefront crawl that diffs third-party scripts and app blocks. It's doable, but it's engineering you must maintain.

**(2) The "code-level fix" is gated by Shopify.** Since API 2023-04, writing theme files needs a **write_themes protected-scope exemption**. A June 2026 developer-forum thread reports an app that *proposes targeted theme-file fixes* being **denied**, with App Embeds offered as the path. App Embeds is effectively the overlay/injection model you're positioning against. Without the exemption, the product becomes "here is the patch, send it to your developer".

**(3) The buyers aren't technical and they are scared.** Tickets will be "does this mean I'm compliant?", "I got a demand letter, what do I do?", "your fix broke my cart drawer", "is this a false positive?". Many need someone who can read Liquid, and the natural gravity is toward done-for-you remediation. That is a service business.

**(4) Liability-adjacent context.** Customers who get sued while using your app will write to you, and some will leave reviews about it.

The category is also filling up: AccessifyAI ("find and fix WCAG theme issues in code, no overlay"), AccessComply, Patrol, TestParty and Consentmo's scanner are all active. With disciplined scope (report + patch + evidence ledger, no remediation services, strong self-serve help), it can reach **7–10 founder hours/week at 100 stores and 8–12 at 500 stores** with contractors. That is at or above the brief's ceiling, and the support is harder to delegate cheaply than Niche A's.

### B2. Value-chain map

| Step | Tag | Tool / role | Notes |
|---|---|---|---|
| Acquire: App Store | Automate (once listed) | Shopify App Store listing, keyword-optimized; free plan / trial | The main channel. Reviews drive ranking, so a review-request automation is needed after the first "fixed" event. |
| Acquire: agencies | Founder-only | Partner outreach, agency plan | Sales-like. Keep scoped. |
| Acquire: content | Automate + founder | Free "scan your store" tool, blog on demand-letter triggers | Beware: free scans with no monitoring are commodity. |
| Sell | Automate | **Shopify Billing API** (managed pricing or app subscriptions) | 0% revenue share on the first $1M lifetime, then 15%; 2.9% processing fee. No Stripe needed except for off-platform agency plans. |
| Onboard | Automate | OAuth install → baseline scan in <10 min → issue cards → onboarding email sequence | Must handle password-protected stores, locales and markets. |
| Deliver: change detection | Automate | Webhooks `themes/publish`, `themes/update`, `app/uninstalled`; **poll** `theme.files` checksums every 1–6 h; nightly storefront crawl diffing script origins and app blocks | No webhook exists for other-app installs or file edits. Polling is required. |
| Deliver: scanning | Automate | Playwright + **axe-core (MPL-2.0)** on self-hosted Chromium workers; ~20–50 template-representative URLs per store; keyboard/focus heuristics | Self-host. Browserless Scale ($350/mo, ~500k units) would be ~5–10× the cost at 500 stores. |
| Deliver: attribution | Automate | Map DOM nodes to section/block IDs (`shopify-section-*`, app block handles), theme files and script origins | The core IP. Heuristic, so some misattribution tickets are inevitable. |
| Deliver: fix suggestions | Automate (AI) + founder QA of the pattern library | Pattern library for the top ~30 violations (alt, labels, contrast, focus, ARIA); LLM (Claude Haiku 4.5) for Liquid diffs; re-scan the patched render in a sandbox before showing it | Cost ≈ <$0.01 per suggestion. |
| Deliver: applying fixes | **Blocked / founder-only decision** | If the write_themes exemption is granted: apply to a duplicate theme with one-click rollback. If not: copy-paste diff, "send to developer" export, partner-dev marketplace | Get the exemption decision before building one-click fixes. |
| Deliver: evidence ledger + statement | Automate | Append-only log (hash-chained), monthly PDF evidence pack, auto-generated accessibility statement (EN / DE / FR via LLM) | Wording must avoid any compliance claim. |
| Retain | Automate | Monthly "what changed on your store" digest, streak metrics ("97 days, 0 unresolved criticals"), annual plan offer on first scan | The key churn lever per 03. |
| Support: tier 1 | Delegate + automate | Help center, in-app AI assistant, Crisp; **Shopify-literate** support contractor | Needs Liquid literacy: more expensive than generic VA support. |
| Support: tier 2 (theme code, disputes, "sued" cases) | Founder-only → partner referral | You; referral to partner auditors/lawyers | Hard rule: no remediation services, no legal advice. |
| Platform compliance | Founder-only | GDPR mandatory webhooks, quarterly Shopify API version upgrades, app review responses, Built for Shopify criteria | ~4–8 h per quarter. |
| Bill / dunning | Automate (Shopify) | Shopify handles charges, and failed payments freeze the app | Low effort. |
| Churn handling | Automate | `app/uninstalled` → exit survey email; data retained 30 days for win-back; digest re-engagement | |

**Eliminated scope:** overlays/widgets, manual audits, VPATs, litigation support, headless/Hydrogen stores, WordPress (until year 2), and "we fix it for you" retainers (refer to a partner agency for a referral fee instead).

### B3. Recommended stack and monthly cost

**Scanning capacity estimate (my estimate):** a daily scan of ~30 URLs × ~6 s ≈ 3 CPU-minutes per store, plus change-triggered partial scans. Call it ~4 CPU-min/store/day, so **500 stores ≈ 33 CPU-hours/day ≈ 1.5 cores on average**. Chromium's memory use (~300–500 MB per concurrent page) is the binding constraint, not CPU. Two to three CX43s (8 vCPU/16 GB) cover 500 stores with headroom. Premium "full-site crawl" tiers (hundreds of URLs weekly) scale linearly, so price them accordingly.

| Item | Tool | 100 stores | 500 stores | Status |
|---|---|---|---|---|
| App server (embedded app, webhooks, API) | Shopify React Router/Remix app template on Hetzner CX33 ×2 | ~€17 | ~€32 (2× CX43) | Verified prices |
| Scanner workers | Playwright + axe-core on Hetzner CX43 | 1× ≈ €16 | 3× ≈ €48 | Verified |
| DB + queue | Postgres + Redis (self-hosted, with backups) | ~€10–15 | ~€30–45 | Backup price **[unverified post-June]** |
| Screenshot / evidence storage | Hetzner Object Storage | €6.49 | €6.49–15 | Verified |
| LLM (fix suggestions, statement translation, support bot) | Claude Haiku 4.5 ($1/$5 per M tokens; batch −50%) | ~$10–20 | ~$60–150 | Verified |
| Email (digests, alerts) | Postmark Basic | $15 | $15–35 (~2–3k digests + alerts) | Verified |
| Support + help center | Crisp Mini €45 → Essentials €95 | €45 | €95 | Verified |
| Monitoring | Better Stack free → 1 responder $29–34; Sentry | $0 | ~$60 | Better Stack verified; Sentry **[unverified]** |
| Insurance (E&O / cyber; higher for an ADA-adjacent product) | Broker | ~$100–200 | ~$150–300 | **[unverified; get quotes]** |
| Shopify fees | 0% revenue share up to $1M lifetime, then 15%; **2.9% processing** | ~3% of revenue | ~3% | Verified |
| **Total fixed** | | **~$140–200/mo** | **~$500–700/mo** | |

**Cost per customer:** infra + LLM ~$0.60–1.20/store/month, Shopify processing ~2.9%, and support at scale ~$5–10/store/month once contractors are counted. Support, not infra, is the cost that scales.

### B4. Sweat-equity build plan (technical founder at 15–20 h/week)

| Phase | Weeks | Founder hours | Cash | Deliverables / exit criteria |
|---|---|---|---|---|
| **0. Spike + gate checks** | 1–4 | 40–50 | ~$50 | Scan 30 real stores (public storefronts); measure attribution accuracy (target ≥80% of violations mapped to theme file or app); **submit the write_themes exemption question/request early** (answer within ~2 weeks per Shopify docs); 8 merchant/agency interviews. **Kill or reshape if:** attribution is below 70%, *or* the exemption is denied **and** interviewees won't pay $49+ for "diff + evidence" without one-click apply. |
| **1. MVP (unlisted/custom app)** | 5–14 | 180–230 | ~$100–200 | OAuth + embedded admin UI (Polaris); GDPR webhooks; theme webhooks + checksum polling; scan pipeline; attribution; issue cards in plain English; pattern-library fixes; evidence ledger; digest email; Shopify Billing. 5–10 design-partner stores via agencies. |
| **2. App Store launch** | 15–20 | 60–90 | ~$300–2,000 (lawyer review of ToS + marketing claims, **strongly advised** given the FTC/accessiBe precedent) | Listing, screenshots, video, review submission and fix cycles (review can take weeks **[unverified duration]**), help center (~40 articles), statement generator, onboarding emails, review-request automation. |
| **3. Systemize** | 21–44 | 150–200 | ~$500–1,500 (insurance, infra) | LLM Liquid diffs with sandbox re-scan verification; false-positive suppression and "won't fix" states; agency multi-store dashboard; PDF evidence pack; app-change detection by script diff; support macros; in-app assistant; quarterly API-upgrade checklist; KPI dashboard. |
| **4. Delegate** | 44–56 | 40–50 | ~$1.5–3k/mo from ~150 stores | Hire a **Shopify-literate** support contractor (reads Liquid/HTML; $25–50/h, 10–20 h/week); SOPs for top-20 ticket types; escalation matrix (legal → refer, theme breakage → founder); partner-agency referral program for "do it for me" requests. |
| **Total** | ~13–14 months | **~470–620 h** | **~$1.5–5k one-off**, plus ~$100–200/mo running | |

**Non-coder founder:** a senior Shopify app developer costs **$60–150/h** (Upwork 2026 ranges). ~450–550 h comes to **$35–65k**, plus an ongoing **$1.5–3k/month** retainer for API upgrades, scanner fixes and attribution tuning. That uses the whole budget, and a Shopify app with no in-house dev decays within a year of API versions. **Not recommended for a non-coder.**

### B5. Steady-state operating model

| Role | Does what | Hours/week at 100 stores | Hours/week at 500 stores |
|---|---|---|---|
| **Founder** | Tier-2 theme/code tickets, attribution and pattern-library tuning, Shopify API upgrades + app review, false-positive triage, reviews/reputation, agency relationships, KPI review | **7–10** | **8–12** |
| Support contractor #1 (Shopify-literate) | Tier-1 tickets, "explain this issue", help-center upkeep, review replies (draft) | 0–5 h/week (founder covers most) | 15–20 h/week (~$2–3.5k/mo) |
| Support contractor #2 / partner agency | Overflow; "do it for me" requests go to a partner agency for a referral fee | — | 0–10 h/week or referral-only |
| Software | Scans, change detection, digests, billing, ledger, statements | — | — |

**Support load assumptions (my estimate):** ~0.6–1.0 tickets per store per month in year 1 (non-technical, anxious users; every new violation triggers a question), falling to ~0.3–0.5 with a good help center and in-app assistant. At 500 stores that is ~150–250 tickets/month at ~15–20 min each, or **~40–80 h/month**. Roughly 20–30% need Liquid-level help. Add review management and uninstall-feedback handling (~1 h/week).

**Churn handling:** at an expected ~3–5% monthly churn (03), 500 stores means ~15–25 cancellations a month. The exit survey and win-back emails are automated. The "invisible value" digest is the main preventive lever. Annual plans pushed at the moment of highest fear.

**Weekly KPI dashboard:** installs, trial→paid, MRR / churn, scans run and failed, change events detected (webhook vs polling vs script diff), attribution rate, fix-suggestion acceptance, false-positive flags, tickets per 100 stores, time-to-first-response, App Store rating, Shopify API deprecation warnings.

### B6. Top operational risks and mitigations

| Risk | Likelihood / impact | Mitigation |
|---|---|---|
| **write_themes exemption denied**, so there are no one-click fixes | Medium–High / High | Ask in Phase 0 before building apply-flows. Design the core value around detection + attribution + evidence + copy-ready diffs. Offer a partner-dev handoff. Never fall back to an App Embed overlay (it contradicts the positioning). |
| **Support becomes services** ("just fix it for me", "help, I'm being sued") | High / High | Hard scope in ToS and UI; referral partners (auditors, agencies, ADA defense counsel) with referral fees; macros; price premium tiers to fund Liquid-literate support. |
| **Change detection misses events** (no webhook for app installs or file edits) | High / Medium | Three layers: webhooks + checksum polling + a daily storefront script diff. Display "last checked" honestly. Never promise "instant". |
| **Scanning blocked or throttled** (bot protection, password pages, geo/markets variants) | Medium / Medium | Scan via the merchant-authorized context (storefront password where provided), polite rate limits, a stable user-agent, retries; flag unscannable pages in the report. |
| **Liability and reputational events** (merchant sued while subscribed; FTC-style claim scrutiny) | Medium / High | No "compliant" or "lawsuit-proof" language anywhere (the FTC fined accessiBe $1M); lawyer-reviewed claims; E&O insurance; evidence ledger framed as "record of effort". |
| **Shopify platform dependency** (API versions quarterly, app review, policy changes, 15% share after $1M) | Certain / Medium | Quarterly upgrade checklist; stay current with Built for Shopify requirements; later, extract the scanner/ledger core so it can serve WordPress/Woo (a second platform reduces single-platform risk). |
| **Crowded category** (AccessifyAI, AccessComply, Patrol, TestParty, Consentmo) | High / Medium (CAC up, price pressure) | Compete on attribution + evidence ledger + agency multi-store, not on "scan + fix". This is a positioning question for 04. |
| **Founder as the only Liquid/attribution expert** | High / Medium | Pattern library as data; golden-store regression suite (10 popular themes × 20 popular apps); SOPs; a contractor able to add patterns by year 2. |

---

## Cross-niche recommendation (ops lens)

1. **A is operationally superior.** It has developer buyers, deterministic outputs, three free and permissive validators, cheap infra (~1% of revenue) and ~4–8 founder h/week at 500 customers. Its irreducible founder work (standards drift) is *predictable and schedulable*: a few spikes a year when KoSIT, FeRD or veraPDF release. That suits a passive-income goal.
2. **B's ceiling on passivity is set by its users and by Shopify**, not by the code. The scanning is cheap. The support is expensive and needs Liquid skills. The flagship feature ("code-level fixes") depends on a Shopify exemption that is being denied for similar apps in 2026. Only pursue B as a second product, *after* A is systemized, and only if the Phase-0 gate checks pass.
3. **Shared asset:** both products are "verifiable output + evidence artefact". The report/ledger component (hash-chained, versioned validator metadata, PDF report) can be built once and reused.
4. **Before writing code for A:** confirm that WeasyPrint's PDF/UA output passes veraPDF on your *actual* invoice templates (tables!). If it doesn't within ~40 h of work, the PDF/UA claim moves to a later tier (Prince licence), and launch is ZUGFeRD/XRechnung + PDF/A-3b only.

## Sources

- DocRaptor pricing (via snippet): https://docraptor.com/plans, https://pdfbolt.com/compare/docraptor
- Prince licensing (via snippet): https://www.princexml.com/purchase/, https://html2pdfapi.com/compare/vs-prince-xml
- WeasyPrint Factur-X / PDF/A-3b / PDF/UA-1: https://www.courtbouillon.org/blog/00055-weasyprint-64/, https://doc.courtbouillon.org/weasyprint/stable/common_use_cases.html, https://github.com/Kozea/WeasyPrint/issues/2726, https://github.com/Kozea/WeasyPrint/issues/2482
- WeasyPrint performance: https://pdf4.dev/blog/playwright-vs-weasyprint, https://pdf4.dev/blog/html-to-pdf-benchmark-2026
- veraPDF licence: https://docs.verapdf.org/develop/ ; Mustang licence: https://github.com/ZUGFeRD/mustangproject/blob/master/LICENSE
- KoSIT XRechnung validator config releases: https://github.com/itplr-kosit/validator-configuration-xrechnung/releases
- ZUGFeRD 2.4 / Factur-X 1.08: https://www.zugferd-community.net/en/blog/2025-12-05-ferd_veroeffentlicht_zugferd_2_4_factur-x_1_08, https://fnfe-mpe.org/wp-content/uploads/2025/12/2025-12-04_Factur-X_1.08_ZUGFeRD_2.4_Press_Release_EN.pdf
- Hetzner 2026 prices: https://docs.hetzner.com/general/infrastructure-and-availability/price-adjustment/, https://agentdeals.dev/hetzner-pricing-2026, https://northflank.com/blog/hetzner-cloud-server-price-increases, https://european-alternatives.eu/product/hetzner-object-storage
- Paddle / Stripe fees: https://www.stackscored.com/pricing/saas-billing/paddle/, https://stripe.com/billing/pricing, https://usagebox.com/articles/stripe-billing-fees-2026-the-07-percent-math
- Crisp: https://www.dragapp.com/blog/crisp-pricing/ ; Better Stack: https://betterstack.com/pricing ; Postmark: https://www.saaspricepulse.com/tools/postmark ; Claude Haiku 4.5: https://www.anthropic.com/claude/haiku
- Browserless: https://www.browserless.io/pricing
- axe-core licence: https://github.com/dequelabs/axe-core
- Shopify revenue share: https://shopify.dev/docs/apps/launch/distribution/revenue-share
- Shopify webhooks (themes/update doesn't fire on file edits; no app-install webhook): https://shopify.dev/docs/api/admin-graphql/latest/enums/WebhookSubscriptionTopic, https://community.shopify.com/t/webhooks-for-app-install/376367
- Shopify theme files checksum: https://shopify.dev/docs/api/admin-graphql/latest/queries/theme
- write_themes exemption: https://shopify.dev/docs/apps/build/online-store/asset-legacy, https://community.shopify.dev/t/theme-api-write-exemption-denied-is-single-file-per-change-merchant-approved-writing-ever-approvable-or-is-app-embeds-the-only-path/35406
- Shopify a11y competitors: https://apps.shopify.com/accessifyai, https://accesscomply.com/, https://www.fudge.ai/blog/best-shopify-accessibility-apps/
- Freelance rates: https://www.upwork.com/hire/shopify-developers/cost/, https://www.aalpha.net/articles/python-developer-hourly-rates/
