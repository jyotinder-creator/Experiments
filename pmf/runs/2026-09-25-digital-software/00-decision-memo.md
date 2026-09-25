# 00 — Decision Memo (Helen, Council Chair) — run 2026-09-25-digital-software

**Inputs:** `pmf/brief.md`, `pmf/scorecard.md`, reports 01–07 in this run. **Date:** 2026-09-25.
**Main assumption, since the brief leaves it blank:** the founder can code in Python or TypeScript and has 15–20 h/week. If that is wrong, see §8, question 1. The decision changes.

---

## 1. The decision

**Build ValidRender, a validated e-invoice API for German B2B companies that bill through Stripe or their own code.** Its buyers must issue ZUGFeRD/XRechnung e-invoices from 1 Jan 2027 (if 2026 turnover was over €800k) or 1 Jan 2028 (everyone). **The wedge is proof, not file generation.** Every call returns an EN 16931 hybrid PDF/A-3b plus a veraPDF + KoSIT + Mustang validation report. Files that fail aren't billed, and we absorb standards updates so customers change nothing. **Pricing:** €0 (50 docs) / €29 / €99 / €299 per month. Plan on a real average of about €45–50, with a €49 "Founding" plan for the first 10 customers. **Channel:** a free public e-invoice validator plus programmatic error-code pages, seeded by direct outreach to developers who have publicly hit validator errors.

**This is a conditional go.** ValidRender scores 60.5/100, half a point above the "validate" line. Only the 45-day gates in §6 turn it into a real business.

---

## 2. Why this wins, and how thin the win is

### Evidence chain

| Link | Evidence | Strength |
|---|---|---|
| **Demand** | Germany's issuing mandate is law (BMF letter of 15 Oct 2025): over €800k turnover from 2027, everyone from 2028 (07 §A2 #1). Stripe can't produce e-invoices natively and sends users to Marketplace apps (07 #3). 8+ small German APIs and 6+ Stripe apps already charge for this, so people pay (04, 06, 07). | **Medium.** Spending is legally required, but no keyword volumes and no competitor revenue figures were retrievable. |
| **Gap** | Generating the file is a €6–10/mo commodity with free and open-source options (03, 04). What is left: a validation report with every file, "failed files aren't billed", and ongoing standards absorption. There were two releases in the last 10 months: ZUGFeRD 2.4 mandatory in Jan 2026, and a new KoSIT config on 2026-08-31 (05). | **Weak to medium.** Most of the original wedge fell away under audit (PDF/UA, template fidelity, Chargebee/Paddle connectors). |
| **Customer** | A lead developer at a German-established B2B SaaS on Stripe or home-grown billing. Triggers: a finance memo about the deadline, a customer's accounts-payable team rejecting a PDF, or a DIY attempt failing the validators (03 §1.4). Can spend €50–200/mo on a card without approval. | **Medium.** The ICP is sharp but small. No real buyer has been interviewed yet. |
| **Offer** | A differentiated offer exists (04 §A1–A5, narrowed by 07): validated output, the "valid or free" guarantee, and a free validator as the lead magnet. | **Passes the "no copycat" gate**, narrowly. |
| **Operations** | Passivity 7/10. 4–6 founder h/week at 100 customers and 5–8 at 500 (05). Infra is about 1% of revenue and gross margin 85–90%. | **Strong.** This is the main reason A wins. |
| **Growth** | Owned assets: the validator, 200–400 error-code pages (one per KoSIT/EN 16931 rule), stack pages, n8n node, Stripe App (06). Head terms are contested; the long tail isn't. | **Medium.** It depends on the SEO ranking, which is unproven. |
| **Risk** | YELLOW (07). The market is narrower and cheaper than modelled, it may arrive one wave late (MVP lands after Jan 2027), and AI makes DIY easier. Margins survive every stress case. The model fails only if price drops *and* CAC doubles. | **The load-bearing risk is customer count, not margin.** |

### The candid read

- **The evidence is weak by normal standards.** Almost all voice-of-customer text came from search snippets, because the proxy blocked Reddit, G2, the Shopify App Store, Trustpilot, Hacker News and every competitor pricing page (02, 03, 04, 07). No agent retrieved keyword volumes. No agent spoke to a buyer. Every price here is "snippet-verified" at best.
- **A wins mainly because it is the most passive option left, not because demand is proven.** Its strongest scores are recurring fit (8) and passivity (7). Its weakest are gap strength (4) and durability (4).
- **A cannot reach "millions of users".** The addressable market is companies established in Germany, selling B2B to other German companies, using Stripe or their own billing code, and not already solved by lexoffice, sevDesk, DATEV, Invopop or Billit. That is plausibly low thousands of companies worldwide, not millions. A realistic ceiling is €5–15k MRR. That meets the brief's $5–10k MRR target but not its "scale to millions" line. I optimised for the MRR and hours targets, because those are measurable and bounded (see §8, question 3).
- **The risk-auditor's pre-mortem is a plausible outcome:** 60 customers and €2.2k MRR by March 2028. The gates in §6 exist to find that out in 45 days for €500, not in 18 months.

---

## 3. Scorecard

Scoring: 0–10 per criterion × weight. The maximum total is 100.

| # | Criterion (weight) | **A. ValidRender** | **B. Driftguard** | **R. Print-grade render API** *(runner-up, provisional)* |
|---|---|---|---|---|
| 1 | Proven demand (15%) | 6 → 9.0 | 7 → 10.5 | 8 → 12.0 |
| 2 | Gap strength (15%) | 4 → 6.0 | 3 → 4.5 | 3 → 4.5 |
| 3 | Customer clarity / WTP (10%) | 6 → 6.0 | 5 → 5.0 | 4 → 4.0 |
| 4 | Recurring-revenue fit (15%) | 8 → 12.0 | 6 → 9.0 | 6 → 9.0 |
| 5 | Unit economics (10%) | 7 → 7.0 | 6 → 6.0 | 6 → 6.0 |
| 6 | Passivity (15%) | 7 → 10.5 | 4 → 6.0 | 8 → 12.0 |
| 7 | Compounding acquisition (10%) | 6 → 6.0 | 7 → 7.0 | 6 → 6.0 |
| 8 | Risk-adjusted durability (10%) | 4 → 4.0 | 2 → 2.0 | 5 → 5.0 |
| | **Total** | **60.5** | **50.0** | **58.5** (unaudited) |
| | **Gates** | YELLOW; passivity 7; differentiated offer yes → **eligible** | **RED → excluded.** Passivity 4 is at the floor. | No offer from 04 and no 07 audit, so it **fails the offer gate today** |
| | **Band** | 60–74: validate with extra caution on gap strength | Excluded (and under 60 anyway) | Under 60: parked until needed |

**Reasons behind the scores that matter**

- **A, demand 6.** The mandate is real and people pay (8+ vendors), but the obligation applies only when both parties are established in Germany (07 #1b), and there is no volume or revenue data.
- **A, gap 4.** Generation is commodity. PDF/UA and template fidelity lost their value under audit. Validation is sold elsewhere too (e-rechnung-validator.de from €39). What is left is "validated + versioned + not billed if failed".
- **A, unit economics 7.** Gross margin of about 90% and LTV:CAC of 5.6 hold even on the realistic tier mix. Payback of about 6 months misses the under-3-month bar. The combined stress case (LTV:CAC 1.2) needs paid CAC to happen.
- **A, durability 4.** YELLOW, with four live threats: AI-assisted DIY, a native Stripe feature, a better-funded Invopop, and a timing miss.
- **B:** I do **not** overrule the RED. All three of 07's reasons are evidenced:
  - AccessifyAI Pro at $29.99 already sells regression monitoring, fixes and an evidence pack.
  - The write_themes exemption was denied to a similar app in 2026.
  - Passivity is 4/10 at 7–12 h/week, which breaks the brief.
- **R is scored by me alone, without an audit.** Expect a 07-style stress test to cost it 3–8 points. Its demand score is the highest here because solo operators publish revenue in this category: ScreenshotOne at $25k+ MRR and PDFShift at about $7.2k/mo (01). Its gap score is low because 02 calls generic HTML-to-PDF a copycat trap.

---

## 4. Disagreements resolved

| # | Topic | Positions | Ruling and why |
|---|---|---|---|
| 1 | **Who is in scope** | 04/06: "B2B SaaS invoicing German/French businesses", global. 07: only seller and buyer both established in Germany. | **07 wins.** It cites the primary source (BMF letter, 15 Oct 2025, via IHK Dresden). Every lead gets two qualifying questions: "established in DE?" and "billing system?". |
| 2 | **Outreach pitch** | 04/06: "if your German customers are over €800k…" / "your customers can refuse a PDF". 07: the threshold is the issuer's turnover. | **07 wins.** The pitch is rewritten before any outreach goes out. The wrong version would lose credibility with the first German CTO who reads it. |
| 3 | **Connectors** | 04: Stripe, Chargebee, Paddle. 05: Stripe first, Chargebee at 10 requests. 07: Stripe + generic API only. | **07 wins.** Paddle is the seller of record and is UK-established, so its customers have no obligation. Chargebee is already served by Invopop and, since May 2026, Avalara. |
| 4 | **Render engine** | 04: Chromium + post-processing. 05: WeasyPrint, with Prince as fallback. | **05 wins.** Chromium doesn't produce conformant PDF/A. WeasyPrint has native PDF/A-3b and a Factur-X attachment API. |
| 5 | **PDF/UA in the hero tier** | 02/03/04: a core differentiator. 05/07: WeasyPrint has open veraPDF failures on tables, and no buyer requires PDF/UA invoices. | **07 wins. Descoped from launch.** It can return later as a Prince-backed add-on, and only if a paying customer asks for it. |
| 6 | **"Keep your template" as the headline** | 04: yes. 05: best-effort only. 07: the XML is legally the leading part of a hybrid invoice. | **07 wins.** Fidelity is a nice-to-have mentioned on the pricing page, not the headline. |
| 7 | **10-year archive in €99 Growth** | 04: yes. 05/07: an obligation that outlives the business. | **07 wins.** Launch without it. If it is added later, call it a "copy archive (not your GoBD system of record)" with a guaranteed export window on shutdown. |
| 8 | **ARPU / tier mix** | 04: €75 blended. 07: realistic €49 (80% Starter). | **Plan on €45–50.** €5k MRR then needs about 100–110 customers, not 67. The price gate in §6 checks this. |
| 9 | **Churn** | 06: under 2%. 04: 3%. 07 stress case: 4.5%. | **Plan on 3%.** The API sits in billing code, which supports low churn, but that is unmeasured. Measure it from month 4. |
| 10 | **Timing** | 06: first 10 customers in 30–60 days, Q4 2026 ideal. 07: the MVP lands after the Jan 2027 wave. | **Both are partly right.** Sell Founding pre-commitments now and deliver them concierge-style from the technical spike's code. Treat 2027 as the design-partner year and **Jan 2028 (all businesses) as the real wave.** |
| 11 | **Primary channel** | 06: SEO and the Stripe App are co-primary. 07: 6+ apps already crowd the Stripe Marketplace. | **SEO (validator + error-code pages) is primary.** The Stripe App is secondary and should never be more than 50% of acquisition. |
| 12 | **Founder hours** | 06: 3–5 h/week. 05: 4–6 h/week at 100 customers, 5–8 at 500. | **05**, because its estimate comes from a detailed task list. |
| 13 | **France** | 02/04: FR Factur-X as the expansion path. 07: France requires a certified platform (PA), and 03 found 13 free PAs. | **Germany only.** France is a later add-on at most, selling to PAs or app builders rather than merchants. |
| 14 | **B's rev share and support cost** | 04: 0% up to $1M/yr, ~$4.50 support per customer. 05/07: $1M lifetime, $6–12 support. | **05/07.** This doesn't matter, since B is excluded. |

---

## 5. Runner-up and when to switch

### The runner-up: R, a print-grade HTML→PDF API for developers and AI agents (global)

This is not B. **B is RED, sits at the passivity floor (4/10) and scores 50.** Reviving it would mean overruling three separate pieces of evidence. R is the better backup, for five reasons:

1. **It reuses about 70% of A's build.** WeasyPrint workers, the API, keys, metering, billing, the docs site and the ops runbook all carry over. If A fails on demand, the spike and MVP work are not wasted.
2. **It is global.** That fits the brief's "online, global" line far better than a Germany-only tool. It still won't reach millions of users, but it has a higher ceiling than A.
3. **Solo operators have proven the category pays.** ScreenshotOne passed $25k MRR with 800+ customers, and PDFShift makes about $7.2k/mo (01). It scored 20/25 with demand-scout and was the most passive option on its list.
4. **It has a real, if modest, gap.** 02 scored "print-grade pagination (running headers/footers, repeated table headers, page counters) without Prince pricing" at 14/20, the second-best gap in that niche. A paged-media engine like WeasyPrint handles this natively, where headless Chrome struggles. Add PDF/A-3b output by default, pricing per document rather than per MB, and an MCP tool listing for AI agents.
5. **It has no deadline.** Doing A first costs R nothing. Doing R first would miss A's 2027/2028 window.

**Honest weaknesses of R:** 15+ competitors are fighting over "wkhtmltopdf alternative" keywords. ScreenshotOne has plateaued at about $32k MRR with 9% churn. 02 explicitly calls generic HTML-to-PDF APIs a copycat trap. **R fails the "differentiated offer" gate today.** Before any R code, run offer-architect and risk-auditor on it (about 1 week). If they can't produce an "only" statement that survives the audit, park R as well and re-run the hunt.

**The first fallback is inside A:** 07's pivot signal. If the "validate-only €19" fake door beats "render from €29" by more than 2:1, relaunch A as a validation/monitoring API before switching niches.

### Kill criteria that trigger the switch (fixed now, not negotiable later)

| When | Trigger | Action |
|---|---|---|
| Week 3 (≤40 h spent) | Technical gate fails: 20 sample invoices don't pass veraPDF PDF/A-3b + KoSIT (config 2026-08-31) + Mustang on the EN16931 and XRECHNUNG profiles. | Kill A. If the failure is in the XML mapping and not the rendering, go straight to R's offer and audit pass. |
| Day 45 (~12 Nov 2026) | Fewer than 3 paid pre-commitments, **or** fewer than 15 qualified sign-ups (DE-established, Stripe/home-grown), **or** more than 60% of interviewees have already solved it. | Kill A. Switch to R (offer and audit first). |
| Day 45 | "Validate-only" beats "render" by more than 2:1 on the fake door. | Pivot A to a validator API. Don't switch niches yet. |
| First 20 payers | More than 75% choose Starter, **and** re-modelling at an ARPU of €45–50 shows no credible path to about 100 customers. | Freeze connectors and archive work. Decide at the month-6 checkpoint. |
| Month 6 (end of Mar 2027) | Fewer than 20 paying customers **or** under €1k MRR, **or** error-code pages have under 500 organic visits a month. | Switch to R. Keep A running on maintenance mode (it's cheap) through the 2028 wave. |
| Any time | Stripe announces native EN 16931 e-invoicing. | Re-run the 07 stress test within 2 weeks. If Stripe customers are more than 50% of the base, switch to R. |

---

## 6. 90-day plan

Week 1 starts **Mon 28 Sep 2026**. Assumes 15–20 h/week. **The cash cap before MVP code is €500 (07).**

### Weeks 1–4: validate (the 07 gates)

| Week | Tasks | Budget | Success metric by end of week |
|---|---|---|---|
| **1** (28 Sep) | • Start the technical spike: WeasyPrint `pdf/a-3b` + own JSON→CII mapper (EN16931 core fields), with veraPDF + KoSIT + Mustang running as one Java sidecar.<br>• Licence-check veraPDF (MPL route), KoSIT and Mustang for SaaS use.<br>• Build a landing page in EN and DE with the **corrected** pitch (issuer-side €800k threshold, "established in Germany"), two qualifying questions and a waitlist.<br>• Build the outreach list: 60–80 GitHub issue and dev.to authors (mustangproject, factur-x, the n8n "22-node" thread) plus 50 German-established SaaS CTOs on Stripe. | ~€50 (domain, one Hetzner CX33) | 5 sample invoices pass all three validators. Landing page live. List built. |
| **2** (5 Oct) | • Put the free public validator live: upload a file, get a plain-language EN/DE report, nothing stored.<br>• Add the fake door on the result page: "Validate every invoice automatically, €19" vs "Render with ValidRender, from €29".<br>• Outreach wave 1 to developers: *"Want me to run your current invoice through veraPDF + KoSIT? Free."*<br>• Start €300 of Google Ads (DE exact match: "xrechnung api", "zugferd api", "stripe e-rechnung"). | €300 ads | 50+ validator runs; 15+ waitlist sign-ups; 4+ interviews booked. |
| **3** (12 Oct) | • **Technical gate deadline:** 20 invoices pass on EN16931 + XRECHNUNG profiles within ≤40 h. PDF/UA is not part of the gate.<br>• Outreach wave 2 to CTOs.<br>• Publish on dev.to and Show HN: "We ran 5 free ZUGFeRD generators through veraPDF + KoSIT. Here's what failed."<br>• Run switch interviews (03 script). | €0–50 | **Gate 1 passed, or A is killed.** 4+ interviews done. |
| **4** (19 Oct) | • Tally the results.<br>• Open the **Founding plan: €49/mo locked**, card charged or annual prepay. It includes a white-glove template migration by the founder.<br>• Write the MVP spec with the 07 scope cuts (Stripe + generic API only; no PDF/UA, no archive, no Chargebee or Paddle). | €0 | **Soft go for MVP code:** ≥40 sign-ups with ≥25 qualified; ≥8 interviews with ≥5 unsolved and willing to pay ≥€29. Anything less means stop coding and keep selling to day 45. |

### Month 2 (weeks 5–9, 26 Oct – 29 Nov): build a narrow MVP and hit the hard gate

- **Tasks:**
  - Build a FastAPI service with API keys, JSON + HTML input, and sync plus async (queue + webhook) modes.
  - Support the EN16931 and XRECHNUNG profiles, with the validation sidecar and a JSON + HTML report.
  - Meter usage so failed documents aren't billed. Rate-limit identical failing payloads to prevent abuse.
  - Add a Sandbox tier and self-serve checkout.
  - Deliver Founding customers concierge-style, using spike code where needed.
- **Hard gate on day 45 (~12 Nov): ≥5 paid pre-commitments.** Fewer than 3 kills A (§5). 3–4 extends selling by 2 weeks, with no new code.
- **Budget:** €100–250 (infra, email).
- **Metrics:** ≥5 paying Founding customers; first valid document within 24 h of signup for 100% of them; ≥100 validator runs per week.

### Month 3 (weeks 10–13, 30 Nov – 27 Dec): first customers live, then launch

- **Tasks:**
  - Build the Stripe connector (`invoice.finalized` → render → attach) and submit the Stripe App for review.
  - Publish docs (quick-start in Node, Python and PHP; OpenAPI) and the first 20 error-code pages plus 10 stack pages.
  - Add a status page and error tracking.
  - Get a lawyer to review the ToS and DPA. Report wording: "Passed KoSIT vX / veraPDF vY on <date>. Not a tax or legal opinion." Liability capped at 12 months of fees.
  - Get E&O insurance quotes.
  - Send a Q4 annual-prepay offer: "Lock 2027 pricing".
- **Budget:** €500–1,500 (lawyer) + €60–150/mo (infra and insurance once bound).
- **Metrics:** 5–10 paying customers; ≥25% of payers on annual plans; validation pass rate above 95% on customer traffic; fewer than 3 support tickets per week.
- **Price gate from 07:** start tracking the tier choice of the first 20 payers.

**Total 90-day cash:** about €1–2.3k, well inside the "few hundred to start" brief once validation passes.

### After day 90 (month by month, summarised)

| Months | Focus | Gate / metric |
|---|---|---|
| 4–6 (Jan–Mar 2027) | Generate the remaining ~200 error-code pages; Stripe App live; n8n node; golden-file CI (about 200 invoices × each profile, nightly against pinned and latest validators) | **Month-6 checkpoint:** ≥20 paying, ≥€1k MRR, ≥500 organic visits/mo, or switch to R |
| 7–12 | Systemise: error-to-fix library, AI docs bot, month-start burst workers, runbooks, restore drill, usage-drop and dunning automations | ≥50 paying; founder ≤8 h/week |
| 13–18 (Q4 2027 – Q1 2028) | The Jan 2028 all-business wave: deadline content, annual-prepay campaign, white-label pitch to 3 invoicing-plugin builders | ~100–110 customers ≈ €5k MRR at an ARPU of €45–50 |

---

## 7. Path to passive

| Milestone | Founder h/week | What changes |
|---|---|---|
| 0–10 customers (build) | 15–20 | Founder does everything, including the concierge migrations for Founding customers. |
| ~25 customers (~€1.2k MRR) | 8–10 | Error-code-to-fix library and AI docs bot live. Stop cold outreach; SEO and validator take over. Golden-file CI catches standards drift automatically. |
| ~50 customers (~€2.4k MRR) | 6–8 | Stripe App and n8n node listed. Support macros written. Content drops to 1 post a month (the standards changelog doubles as the retention email). |
| ~100 customers (~€5k MRR) | **4–6** | Add a second hosting region. Month-start runbook tested so a contractor can restart and scale the service. This is the brief's target. |
| 150–200 customers | 5–8, mostly standards work | Hire a part-time technical support contractor for 5–10 h/week (~$800–1.5k/mo) to handle tier-1 tickets and act as month-start first responder. |
| Year 2 | 4–6 | Put a freelance ZUGFeRD specialist on a small retainer, so standards knowledge doesn't live only in the founder's head. |

**What never fully goes away:**
- 2–4 standards spikes a year of about 10 h each (KoSIT, ZUGFeRD and veraPDF releases).
- On-call around days 1–3 of each month, when invoice runs cluster.

Both are predictable and schedulable, which is why A scores 7/10 on passivity and not 9.

---

## 8. Open questions for the founder

1. **Can you code, and in what?** The plan assumes Python/TypeScript and comfort running a Java validation service. **If you can't code, don't start A.** 05 prices hiring it out at $35–55k plus a $1–2k/month specialist retainer for ever, which spends the whole budget on a 60-point idea. In that case the council should be re-run with "non-technical" in the brief.
2. **Hours per week, really?** The plan needs 15–20 h/week for about 12 months. At under 10 h/week the MVP slips past Q1 2027 and only the 2028 wave is left.
3. **Which goal wins: "$5–10k MRR at under 10 h/week" or "scale to millions of users"?** They pull in opposite directions. A serves the first and can't do the second. If "millions" is a hard requirement, none of this run's finalists fits, and the council should look at consumer or prosumer products with paid acquisition. That is a riskier and less passive profile.
4. **What is your target MRR and by when?** A realistic A outcome is €5k MRR around months 15–18 (early 2028). If you need that sooner, say so now.
5. **Will you work in German?** DE docs, DE landing pages and occasional DE support emails raise conversion in this market. English-only is possible but weaker.
6. **Will you accept month-start on-call and a liability surface?** That means sitting on customers' billing paths, E&O insurance (~$60–150/mo) and a one-off lawyer review (€500–1,500).
7. **Where is your legal entity?** This affects DPA credibility with German buyers (EU hosting is planned either way) and the choice between Paddle and Stripe for your own billing.
8. **Do you have any German SaaS or developer network?** Even five warm introductions would change the day-45 gate odds materially.
9. **Is €500 of validation spend and about 40 hours acceptable as the only commitment before the gates?** Nothing larger should be spent until day 45.
