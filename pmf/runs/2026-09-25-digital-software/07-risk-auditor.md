# 07 — Risk Auditor (Victor) — run 2026-09-25-digital-software

**Inputs:** `pmf/brief.md`, 01–06 in this run.
**Finalists:** A. *ValidRender* (e-invoice-grade render API, Germany first). B. *Driftguard* (Shopify accessibility regression guard).
**Evidence note:** the proxy blocked docs.stripe.com, invopop.com, apps.shopify.com and every competitor pricing page. Everything marked "snippet" comes from search-result text, not from a live page. Date: 2026-09-25.

## Bottom line

| | A. ValidRender | B. Driftguard |
|---|---|---|
| **Verdict** | **YELLOW.** Proceed only after the tests in §A6 pass. The offer has to be narrowed first. | **RED.** Drop it as a standalone product. The attribution/scorecard engine could come back as a year-2 content asset. |
| Biggest finding | The German issuing mandate applies **only when both seller and buyer are established in Germany**. Paddle-billed SaaS is out of scope entirely (Paddle is the seller of record). Chargebee already has Invopop and (since May 2026) Avalara. So the addressable market is "German-established B2B sellers on Stripe or home-grown billing who haven't already solved this". That is much narrower than 04 and 06 assume. | AccessifyAI (launched 30 Mar 2026) sells regression monitoring, one-click AI code fixes and an "ADA & EAA evidence pack" for **$29.99/mo**. AccessComply sells monitoring, backed-up theme fixes and remediation records from $49. Driftguard's hero tier is $99. Only "which app broke it" is still unclaimed, and Passivity 4/10 already breaks the brief. |

---

## Niche A — ValidRender

### A1. Verdict: YELLOW

The core facts hold up. From 1 Jan 2027, German businesses with more than €800k turnover in 2026 must issue EN 16931 e-invoices; from 2028, all must. Email is an allowed channel, and a ZUGFeRD hybrid is a valid e-invoice. Stripe still has no native e-invoicing: its docs send users to partner apps.

The problem is the size and shape of the wedge. Four things shrink it:
1. The obligation only applies when **both parties are established in Germany**. Non-German SaaS selling into Germany has no obligation.
2. Paddle and Lemon Squeezy customers aren't the issuer, because the merchant of record is. Paddle is UK-established, so its invoices aren't in scope at all.
3. Chargebee has Invopop (ZUGFeRD/XRechnung) and, since May 2026, Avalara as its preferred e-invoicing partner.
4. The Stripe Marketplace already has at least Billit, Invopop, "E-Invoice - E-Rechnung", MiracleBill, peppol.sh and GoRoute, and there are 8+ direct APIs from €9.90/mo.

Two of 04's four differentiators also weaken under scrutiny. In a hybrid invoice **the XML is legally the leading part**, so pixel-perfect template fidelity has low compliance value. PDF/UA on WeasyPrint has open table-tagging failures. Nobody in the German B2B/B2G invoice flow requires PDF/UA: B2G uses XRechnung, which is pure XML.

What's left is still a real, low-churn, passive-shaped business. But it will be a Starter-heavy, €10–30-anchored market, not a €99-hero market, unless the tests show otherwise. The timing is also tight: the MVP lands around week 12–17, which is **after** the Jan 2027 wave. The realistic target is the Jan 2028 all-business wave.

### A2. Claim verification

| # | Claim (source agent) | Result | Evidence |
|---|---|---|---|
| 1 | Germany requires *issuing* e-invoices: over €800k from 2027, everyone from 2028 (04, 06) | **Held up** | [Stripe](https://stripe.com/resources/more/e-invoice-in-germany): in 2027, other formats are allowed only for businesses under €800k revenue in 2026. [ecovis](https://ecovis-kso.com/blog/e-rechnungs-pflicht-ab-2027/) and [ihp-media](https://www.ihp-media.com/ratgeber/e-rechnung-pflicht-2027-unternehmen-800000-euro/) say the same. |
| 1a | "If you invoice German companies above €800k turnover…" (06 outreach hook). "From January 2027 your German customers can refuse a plain PDF" (04 pitch) | **Refuted / misleading** | The €800k threshold applies to the **issuer's** prior-year turnover, not the customer's. The obligation sits with the seller. Sellers under €800k may keep sending PDFs through 2027. Rewrite both pitches before any outreach, or the first German CTO who reads them will spot the error. |
| 1b | Scope: "B2B SaaS invoicing German/French businesses", "global" | **Refuted (materially)** | BMF letter of 15 Oct 2025 ([BMF PDF](https://www.bundesfinanzministerium.de/Content/DE/Downloads/BMF_Schreiben/Steuerarten/Umsatzsteuer/Umsatzsteuer-Anwendungserlass/2025-10-15-einfuehrung-obligatorische-e-rechnung.pdf?__blob=publicationFile&v=5), summarised by [IHK Dresden](https://www.ihk.de/dresden/hauptnavigation/recht-steuern/erechnung-6232764)): the obligation applies only if **supplier and recipient are both established in Germany**. A VAT registration without an establishment doesn't trigger it. US, UK and other-EU SaaS invoicing German firms are **out of scope**. |
| 2 | A ZUGFeRD hybrid sent by email is sufficient (04) | **Held up, with caveats** | [Handwerksblatt](https://www.handwerksblatt.de/themen-specials/die-e-rechnung-wird-pflicht-tipps-fuer-handwerksbetriebe/bundesregierung-bestaetigt-e-mail-postfach-reicht-fuer-e-rechnungen) and the [BMF FAQ](https://www.bundesfinanzministerium.de/Content/DE/FAQ/e-rechnung.html) confirm email is a valid channel. Caveat 1: in a hybrid, **the XML is the leading part** when it differs from the PDF ([lexware](https://www.lexware.de/wissen/faktura-warenwirtschaft/zugferd/)), which undercuts "keep your design" as a compliance value. Caveat 2: the MINIMUM and BASIC-WL profiles don't qualify, so default to EN16931 or higher. |
| 3 | Stripe has no native ZUGFeRD/XRechnung (04, 06) | **Held up (for now)** | [Stripe resources](https://stripe.com/resources/more/zugferd-x-invoice-germany): Stripe Invoicing/Billing "cannot create or send electronic invoices without an additional app". No native launch found for 2026. **But** the Stripe channel is crowded: [Invopop on Stripe Marketplace](https://marketplace.stripe.com/apps/invopop), Billit, [E-Invoice - E-Rechnung](https://marketplace.stripe.com/apps/e-invoice-e-rechnung), [peppol.sh](https://peppol.sh/for/stripe), [GoRoute](https://goroute.ai/stripe.html), plus fizard and miraclesync pages. |
| 3a | Chargebee/Paddle connectors are a gap (04 Growth tier) | **Refuted** | Chargebee already generates ZUGFeRD hybrids and XRechnung via [Invopop](https://docs.invopop.com/guides/chargebee), and named Avalara its preferred global e-invoicing partner on [21 May 2026](https://newsroom.avalara.com/2026-05-21-Avalara-and-Chargebee-Launch-New-Integration-to-Automate-Global-E-Invoicing-and-Live-Reporting). With Paddle, **Paddle is the seller** and issues the invoice ([MoR explainer](https://fintechspecs.com/blog/stripe-vs-paddle-vs-lemon-squeezy-vs-polar-merchant-of-record-b2b-saas/)). Paddle is UK-established, so there's no German issuing obligation, and a Paddle connector has no buyer. **Delete both connectors from the roadmap.** |
| 4 | About 8 small German APIs at €6–10/mo (04, 06) | **Held up (partly re-verified)** | XInvoice "from 9.90 €/month" ([xinvoice.net](https://www.xinvoice.net/en), snippet). I couldn't re-confirm rechnungsapi.de's €5.99 (the snippet had no price). **New:** Invopop (YC + Target Global seed, [Tracxn](https://tracxn.com/d/companies/invopop/__TVkpAtNVejltpB2BmhKeregiFcNT1iQZ0-gmwaH6gc8)) moved its Stripe/Chargebee connectors to self-serve fixed-price plans in 2026 ([Invopop blog](https://www.invopop.com/blog/new-pricing-2026), snippet). This competitor is better funded and multi-country. |
| 5 | WeasyPrint gives PDF/A-3b + PDF/UA-1 (05) | **Weakened** | PDF/A-3b plus Factur-X attachment is native ([WeasyPrint docs](https://doc.courtbouillon.org/weasyprint/stable/common_use_cases.html)). PDF/UA has open veraPDF failures on tagged tables: [#2726](https://github.com/Kozea/WeasyPrint/issues/2726) (Apr 2026) and [#1784](https://github.com/Kozea/WeasyPrint/issues/1784). Invoices are tables. |
| 6 | "PDF/UA covers public-sector demand (ADA Title II, EAA)" (04) | **Weakened** | German B2G requires XRechnung (pure XML, no PDF). ADA Title II covers a public entity's *own* web content and documents, not vendor invoices it receives. I found no evidence of any buyer requiring PDF/UA invoices. |

### A3. Risk register

| Risk | Category | L (1–5) | I (1–5) | Mitigation |
|---|---|---|---|---|
| **Addressable market is much smaller than modelled.** Only German-established sellers are in scope; Paddle/LS customers are out; Chargebee is served; most German SMBs use lexoffice, sevDesk or DATEV, which already output ZUGFeRD | Market | 4 | 5 | Qualify every lead on "established in DE? billing system?" Size the Stripe + home-grown DE B2B segment before building (see test). Target app/plugin builders (Persona B) as the multiplier. |
| **Commodity price anchor.** €9.90 APIs + free tiers + Invopop + open-source libraries (mustangproject, factur-x, horstoeko/zugferd) + LLM-written mappers | Market / Disruption | 5 | 4 | Don't sell generation. Sell *validated + versioned + archived*. Plan for a Starter-heavy mix (see stress test). |
| **DIY with AI.** Claude/ChatGPT now writes a Stripe→CII mapper with Mustang in an afternoon; KoSIT validator is free | Disruption | 4 | 4 | The value is ongoing standards absorption plus the evidence trail, not the first file. Price and position it as maintenance you don't have to own. |
| **Stripe ships native EN 16931**, or makes Invopop/Billit a default | Platform | 2 (12 mo) / 3 (24 mo) | 5 | Keep the core API platform-agnostic. Don't make the Stripe App >50% of acquisition. |
| **Timing miss.** Jan 2027 buyers (over €800k) are solving this *now*; MVP lands about week 12–17 | Market | 4 | 3 | Treat 2027 as a design-partner year and Jan 2028 (all businesses) as the real wave. A validator-first launch in Q4 2026 captures leads. |
| **False "valid" report liability.** A file passes our three validators, then the recipient's ERP or tax auditor rejects it. Or it is schematron-valid but wrong on tax content (§14 UStG), and the customer treats our report as proof of legal compliance | Legal | 3 | 4 | Report wording: "Passed KoSIT validator vX / config Y, veraPDF vZ on <date>. Not a tax or legal opinion." Cap liability at 12 months' fees. E&O insurance. Never say "legally compliant/rechtssicher" (competitors do; don't copy them). The "valid or free" guarantee is fine because it caps exposure at fees. |
| **10-year archive promise** (Growth tier) outlives the business; GoBD expectations | Legal / Ops | 2 | 4 | Rename it "copy archive (not your GoBD system of record)", with a guaranteed export window on shutdown. Or drop it from €99. |
| **Standards drift and founder single point of failure** (ZUGFeRD 2.4→2.5, KoSIT config 2026-08-31) | Passivity | 5 | 3 | As in 05: golden-file CI, mapper as data. Budget 2–4 spikes a year of about 10 h each. |
| **WeasyPrint PDF/UA table failures** | Technical | 4 | 2 | Drop PDF/UA from the launch pitch. Offer it later as a Prince-backed add-on only if a buyer asks. |
| **Month-start on-call** on a billing-critical path | Passivity | 4 | 3 | Async queue, burst workers, runbook (05). |
| **France/Poland expansion assumed for scale** (04: "500+ needs FR/PL/BE") | Regulatory | 4 | 3 | France requires transmission through certified platforms (PA), so a file generator can't serve FR alone. Poland is KSeF clearance. The easy expansion path is thinner than implied. |

### A4. Unit-economics stress test (EUR, per 04's model)

| Case | ARPU | GM/cust | Monthly churn | CAC | LTV | LTV:CAC | Payback | Customers for €5k MRR |
|---|---|---|---|---|---|---|---|---|
| Base (04) | 75 | 67.5 | 3% | 250 | 2,250 | 9.0 | 3.7 mo | 67 |
| CAC ×2 | 75 | 67.5 | 3% | 500 | 2,250 | 4.5 | 7.4 mo | 67 |
| Churn +50% | 75 | 67.5 | 4.5% | 250 | 1,500 | 6.0 | 3.7 mo | 67 |
| Price −30% | 52.5 | 45 | 3% | 250 | 1,500 | 6.0 | 5.6 mo | 95 |
| **Realistic mix** (80% Starter / 17% Growth / 3% Scale: most DE B2B SaaS send <500 invoices/mo) | 49 | 42 | 3% | 250 | 1,400 | 5.6 | 6.0 mo | 102 |
| **All stresses combined** (realistic mix −30%, churn 4.5%, CAC 500) | 34 | 28 | 4.5% | 500 | 620 | **1.2** | **18 mo** | **147** |

**Read:** gross margin survives everything because infra is about 1% of revenue. The business still works on the "price −30%" and "realistic mix" cases. It **fails only if CAC doubles as well**, meaning paid acquisition is needed because SEO or the marketplace doesn't deliver. The load-bearing number is therefore **organic/marketplace CAC**, and the *customer count* (100–150 German-established Stripe/home-grown sellers) is the real feasibility question, not margin.

### A5. Pre-mortem (March 2028)

> We shipped the MVP in late January 2027, after the €800k firms had already picked Billit, Invopop or their accountant's tool. The error-code SEO pages ranked, but most searchers were developers at Paddle-billed or non-German companies with no obligation, or at German SMBs that switched on the ZUGFeRD toggle in lexoffice. We cut the Chargebee connector after Avalara landed. PDF/UA never passed on multi-page tables, so we quietly dropped it, and the "hero" Growth tier lost its reason to exist. 70% of payers sat on €29 Starter. The Jan 2028 wave brought a spike, but at €29 against €9.90 competitors, and a few customers left when Stripe announced a native e-invoice beta. We plateaued at 60 customers and €2.2k MRR, with the founder still doing month-start on-call and two standards updates a year. It's not a disaster, but it's under half the brief's target.

The most likely failure isn't technical. It's **a market that's smaller and cheaper than modelled**, reached **one wave late**.

### A6. Kill criteria and validation test

**Before any MVP code: 4 weeks, €500 cash cap, about 40 founder hours.** All thresholds are fixed now.

1. **Technical gate (≤40 h, as in 05 Phase 0):** WeasyPrint + own CII mapper produce 20 sample invoices that pass **veraPDF PDF/A-3b + KoSIT (XRechnung config 2026-08-31) + Mustang** on the EN16931 and XRECHNUNG profiles.
   - **Fail = kill.** PDF/UA isn't part of the gate; it's descoped from launch.
2. **Demand gate (weeks 1–4):** launch the free validator plus a landing page with the **corrected** pitch (issuer-side €800k threshold). Add two qualifying questions: "Is your company established in Germany?" and "Billing: Stripe / Chargebee / Paddle / home-grown / other". Drive traffic with outreach to 60–80 GitHub/dev.to problem-havers and 50 German-established SaaS CTOs, plus €300 of Google Ads (DE exact match).
   - **Pass (all required):**
     - ≥40 waitlist sign-ups, of which **≥25 are German-established on Stripe or home-grown billing**.
     - ≥8 switch interviews, of which **≥5 have not already solved it** with a Stripe app, Invopop, lexoffice/sevDesk or an accountant tool, and say they would pay ≥€29/mo.
     - **≥5 paid pre-commitments** (Founding plan €49/mo, card charged, or an annual prepay) by day 45.
   - **Kill:** <3 paid pre-commitments by day 45, *or* <15 qualified (DE-established, Stripe/home-grown) sign-ups, *or* >60% of interviewees already solved.
   - **Pivot signal:** the validation-only SKU (€19) beats render by more than 2:1 on the fake door. In that case, launch as a validator/monitoring API and not a renderer.
3. **Price gate (at launch):** if more than 75% of the first 20 payers choose Starter, re-model at ARPU €45–50 and confirm that the ~100-customer path still exists before building connectors or the archive.

**Scope changes required before build:** delete the Paddle connector; defer Chargebee; drop PDF/UA and the 10-year archive from the hero tier; fix the €800k pitch; build a Stripe connector + generic API only.

---

## Niche B — Driftguard

### B1. Verdict: RED

Three independent reasons, each backed by evidence:

1. **The wedge is mostly occupied at a third of the price.** AccessifyAI Pro at **$29.99/mo** (launched 30 Mar 2026) already bundles daily scans up to 500 pages, one-click AI code fixes, **regression monitoring** and an **"ADA & EAA evidence pack"** ([Shopify listing](https://apps.shopify.com/accessifyai) and [fudge.ai](https://www.fudge.ai/blog/best-shopify-accessibility-apps/), snippet). AccessComply (from $49) already "monitor[s] for regressions … keeping records of the remediation work" and writes backed-up theme fixes with one-click rollback ([accesscomply.com](https://accesscomply.com/about/)). Driftguard's $99 Guard differs only by *attribution to the specific app/theme change* and hash-chaining. The first is a feature a competitor can copy in a quarter. The second is a detail merchants won't pay $70/mo extra for.
2. **The hero feature is gated by Shopify.** Theme-file writes need a write_themes exemption. Shopify's eligible use cases are backup/restore, ratings/badges, SEO, content locking and developer tooling; accessibility fixes aren't listed. A 2026 request for "targeted, merchant-approved theme-file fixes" was **denied** with App Embeds offered instead ([Shopify dev forum](https://community.shopify.dev/t/theme-api-write-exemption-denied-is-single-file-per-change-merchant-approved-writing-ever-approvable-or-is-app-embeds-the-only-path/35406)). Incumbents that already hold exemptions keep one-click fixes. A new entrant may not get them.
3. **It breaks the founder brief.** Ops scores it at Passivity 4/10 with 7–12 founder h/week, anxious non-technical users, Liquid-level support, and "I got sued" tickets. The brief's ceiling is <5–10 h/week. Add the liability surface (merchants sued while subscribed; FTC scrutiny after the $1M accessiBe fine), and B is the wrong shape for passive income even if it sells.

### B2. Claim verification

| # | Claim | Result | Evidence |
|---|---|---|---|
| 1 | AccessifyAI is a "$9.99/mo" scanner with regression monitoring (06) | **Weakened, and it's worse for B** | Basic is $9.99, but **Pro at $29.99** adds one-click AI code fixes, regression monitoring and ADA/EAA evidence packs. It has 0 reviews so far (launched Mar 2026), so its traction is unproven. |
| 2 | AccessComply ($0/$49/$99/$399) sells source-code fixes; attribution not seen (04) | **Held up** | Fixes are written to theme files with backups, plus regression monitoring and remediation records. It explicitly *flags* third-party app issues for manual review instead of attributing them, so the attribution gap is real but narrow. |
| 3 | write_themes exemption risk (05) | **Held up** | See the forum links above. There are also 2026 threads where `themeFilesUpsert`/`themeFilesCopy` returned ACCESS_DENIED even after an exemption was approved. |
| 4 | Shopify rev share "0% up to $1M/yr" (04) vs "lifetime" (05) | **05 correct; 04 refuted** | [Shopify changelog](https://shopify.dev/changelog/update-to-shopifys-app-developer-revenue-share): 0% on the first $1M **lifetime** (from 1 Jan 2025, aggregated at partner level), then 15%. It doesn't matter at this scale. |
| 5 | Support cost ~$4.50/cust (04) vs $6–12 (05) | **05 more credible** | Non-technical, fear-driven buyers plus Liquid-level tickets. 05's per-ticket assumptions are explicit; 04's 10 min/month isn't justified. Use $6–12. |

### B3. Risk register

| Risk | Category | L | I | Mitigation |
|---|---|---|---|---|
| Competitors (AccessifyAI $29.99, AccessComply $49) cover monitoring, fixes and evidence at ⅓–½ the price | Market | 5 | 5 | Would need to compete on attribution alone at ≤$39. |
| write_themes exemption denied → no one-click fixes | Platform | 4 | 4 | Diffs + "send to developer" only, which weakens the offer vs incumbents that have one-click fixes. |
| Incumbent ships "introduced by app X" attribution | Market / Disruption | 3 | 5 | None durable; the only candidate is the scorecard data moat. |
| Support becomes a service; founder at 7–12 h/week | Passivity | 5 | 4 | Scope limits help but don't fix buyer psychology. |
| Merchant sued while subscribed → reviews, claims, FTC-style scrutiny | Legal | 3 | 4 | Lawyer-reviewed claims, E&O insurance ($150–300/mo). |
| Post-settlement churn (reactive buyers) | Economic | 4 | 3 | Annual-at-peak-fear offer (03). |
| Single platform: API versions, app review, ranking algorithm, 25/28 of the category are overlays with marketing budgets | Platform | 5 | 3 | WordPress port in year 2 (more build). |

### B4. Unit-economics stress test (USD)

| Case | ARPU | GM/cust | Churn | CAC | LTV | LTV:CAC | Payback |
|---|---|---|---|---|---|---|---|
| Base (04) | 90 | 78 | 4% | 250 | 1,960 | 7.8 | 3.2 mo |
| 05 support cost (COGS $18) | 90 | 72 | 4% | 250 | 1,800 | 7.2 | 3.5 mo |
| CAC ×2 | 90 | 72 | 4% | 500 | 1,800 | 3.6 | 6.9 mo |
| Churn +50% | 90 | 72 | 6% | 250 | 1,200 | 4.8 | 3.5 mo |
| **Price −30%** (still ~2× AccessifyAI Pro) | 63 | 48 | 4% | 250 | 1,200 | 4.8 | 5.2 mo |
| **Combined** | 63 | 48 | 6% | 500 | **800** | **1.6** | **10.4 mo** |

**Read:** it's viable on paper until CAC and price both move. Competitive evidence says **price will move**: the market anchor is $29.99–49, not $99. At an ARPU of $39 you need about 130 stores for $5k MRR, with support scaling per store.

### B5. Pre-mortem

> Driftguard launched in May 2027 without the theme-write exemption. The "diff for your developer" flow converted poorly against AccessComply's one-click fixes and AccessifyAI's $29.99 evidence pack. We cut Guard to $49, then $39. Attribution was right about 75% of the time, and every miss generated a ticket from an anxious merchant. Two customers who received demand letters left 1-star reviews saying "the app didn't protect us". By month 12 we had 70 stores and $2.9k MRR, with the founder spending 10+ h/week on tickets and quarterly Shopify API upgrades.

### B6. Kill criteria and validation test (only if the founder insists on testing it)

**3 weeks, ≤$250, before any app code:**
- Submit the write_themes exemption request with the accessibility-fix use case.
- Run 04's B7-1 fake door, **adding a third page:** "Regression monitoring + evidence pack, $29" to mimic AccessifyAI.
- Scan 30 public stores and measure attribution accuracy manually.

**Reverse RED to YELLOW only if all four hold:**
1. The exemption is **granted**.
2. The attribution page beats both the fix-count page and the $29 me-too page by ≥1.5× on sign-up rate, with ≥300 visitors per arm.
3. Attribution accuracy is ≥80% on the 30 stores.
4. ≥3 agencies commit in writing to a paid pilot at ≥$199/mo.

**Any single miss confirms RED.**

---

## Adjudication of 04 vs 05 disagreements

| Topic | 04 | 05 | Ruling |
|---|---|---|---|
| A render engine | Chromium + post-processing | WeasyPrint primary, Prince fallback | **05.** Chromium doesn't produce PDF/A. But WeasyPrint's PDF/UA table bugs (#1784, #2726) mean PDF/UA leaves the launch scope. |
| A template fidelity promise | Core differentiator | Best-effort only | **05, and stronger.** Because the XML is legally leading, fidelity is a nice-to-have. Don't make it the headline. |
| A fixed costs | ~€750/mo | ~$90–150/mo | **05** for launch. It doesn't change break-even materially. |
| A connectors | Stripe, Chargebee, Paddle | Stripe first, Chargebee at ≥10 asks | **Stricter than both:** Stripe + generic API only. Paddle has no obligated buyer; Chargebee is served by Invopop and Avalara. |
| B support cost | ~$4.50/cust | $6–12/cust in year 1 | **05.** |
| B Shopify rev share | 0% to $1M/yr | 0% to $1M lifetime | **05** (Shopify changelog). |
| B one-click fixes in the hero tier | Assumed | Exemption-gated | **05.** The 2026 denial precedent makes it a gate, not an assumption. |

## Sources
- BMF letter of 15 Oct 2025: https://www.bundesfinanzministerium.de/Content/DE/Downloads/BMF_Schreiben/Steuerarten/Umsatzsteuer/Umsatzsteuer-Anwendungserlass/2025-10-15-einfuehrung-obligatorische-e-rechnung.pdf?__blob=publicationFile&v=5
- IHK Dresden (both parties must be established in DE): https://www.ihk.de/dresden/hauptnavigation/recht-steuern/erechnung-6232764
- BMF FAQ: https://www.bundesfinanzministerium.de/Content/DE/FAQ/e-rechnung.html
- Email channel confirmed: https://www.handwerksblatt.de/themen-specials/die-e-rechnung-wird-pflicht-tipps-fuer-handwerksbetriebe/bundesregierung-bestaetigt-e-mail-postfach-reicht-fuer-e-rechnungen
- Hybrid XML is the leading part: https://www.lexware.de/wissen/faktura-warenwirtschaft/zugferd/
- 2027/2028 thresholds: https://ecovis-kso.com/blog/e-rechnungs-pflicht-ab-2027/ , https://stripe.com/resources/more/e-invoice-in-germany
- Stripe needs an app: https://stripe.com/resources/more/zugferd-x-invoice-germany , https://wavect.io/blog/stripe-billing-e-invoicing-2027/
- Stripe Marketplace competitors: https://marketplace.stripe.com/apps/invopop , https://marketplace.stripe.com/apps/e-invoice-e-rechnung , https://peppol.sh/for/stripe , https://goroute.ai/stripe.html
- Chargebee + Invopop: https://docs.invopop.com/guides/chargebee ; Chargebee + Avalara (21 May 2026): https://newsroom.avalara.com/2026-05-21-Avalara-and-Chargebee-Launch-New-Integration-to-Automate-Global-E-Invoicing-and-Live-Reporting
- Invopop pricing 2026 and funding: https://www.invopop.com/blog/new-pricing-2026 , https://tracxn.com/d/companies/invopop/__TVkpAtNVejltpB2BmhKeregiFcNT1iQZ0-gmwaH6gc8
- MoR is the seller: https://fintechspecs.com/blog/stripe-vs-paddle-vs-lemon-squeezy-vs-polar-merchant-of-record-b2b-saas/
- XInvoice pricing: https://www.xinvoice.net/en
- WeasyPrint PDF/UA issues: https://github.com/Kozea/WeasyPrint/issues/2726 , https://github.com/Kozea/WeasyPrint/issues/1784
- AccessifyAI: https://apps.shopify.com/accessifyai , https://www.fudge.ai/blog/best-shopify-accessibility-apps/
- AccessComply: https://accesscomply.com/about/ , https://accesscomply.com/
- write_themes exemption: https://community.shopify.dev/t/theme-api-write-exemption-denied-is-single-file-per-change-merchant-approved-writing-ever-approvable-or-is-app-embeds-the-only-path/35406 , https://shopify.dev/docs/apps/build/online-store/asset-legacy , https://community.shopify.dev/t/issue-themefilesupsert-returns-access-denied-despite-write-themes-scope-and-approved-exemption/35003
- Shopify revenue share: https://shopify.dev/changelog/update-to-shopifys-app-developer-revenue-share
