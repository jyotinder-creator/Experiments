# 04 — Offer Architect (Marcus) — run 2026-09-25-digital-software

**Inputs:** `pmf/brief.md`, `01-demand-scout.md`, `02-gap-hunter.md`, `03-customer-psychologist.md`. There are two finalists: **A** (compliance-grade document render API, Germany first) and **B** (Shopify accessibility regression guard). Niches 3 and 4 were dropped in 03.
**Founder constraints:** digital/software only, global, bootstrapped (hundreds of $, up to $10–50k if justified), passive recurring revenue, target of $5–10k MRR in 12–18 months with under 5–10 h/week at steady state.
**Date:** 2026-09-25.

## Evidence note: read first
- **Every competitor pricing page was blocked by the proxy this session.** That includes docraptor.com, pdfshift.io, xinvoice.net, invoice-api.xhub.io, rechnungsapi.de, finax.dev, thelawin.dev, marketplace.stripe.com, testparty.ai, accessshield.net, getadacertify.com and apps.shopify.com. **No price below is "verified live".** The labels mean:
  - **snippet-verified:** the price appeared in a search-engine snippet of the vendor's page or of a third-party pricing page.
  - **assumed:** my estimate.
  - Before launch, a human should open each vendor page and confirm the numbers.
- **Two new findings change the offers relative to 02/03:**
  1. **Niche A: bare ZUGFeRD/XRechnung generation is already a €6–10/mo commodity.** Examples: rechnungsapi.de at €5.99/mo for 100 invoices (free tier 10/mo), XInvoice from €9.90/mo, and a free tier at invoice-api.xhub.io, plus InvoiceXML, thelawin.dev and finaX. This is stronger than 03's "commoditising" finding. **We cannot compete on generation. We compete on the four things around it:** fidelity to your existing template, PDF/UA, validation evidence, and billing-system connectors.
  2. **Niche B: AccessComply** (Shopify, snippet-verified: Free / Starter $49 / Shield $99 / Citadel $399) already sells source-code theme fixes with backups, monitoring, statements and PDF reports. That partly closes gap-hunter's "$50–150 real-fix tier" gap. Cheap monitors also exist: shoplab Accessibility Check at $10/mo with Flow triggers, and ADAcertify at $37/mo, which is widget-based. **The remaining wedge is narrower:** *change attribution* (which app update or theme edit broke it), a *tamper-evident evidence ledger*, and *agency multi-store*. I still think it is defensible, but less so than A. See the verdict at the end.

---

## Niche A — Compliance-grade document render API (Germany first)

**Working name:** *ValidRender*. Tagline: "Your invoice template in. A validated E-Rechnung out." Trademark and domain are not checked.

### A1. The "only" statement
> **We're the only API that turns the invoice template you already have (HTML or a Stripe/Chargebee invoice) into a ZUGFeRD/XRechnung hybrid PDF that is both PDF/A-3b and PDF/UA-1 tagged, with a veraPDF + KoSIT (EN 16931) validation report attached to every file, for B2B SaaS teams facing Germany's 2027/28 issuing deadline.**

Uniqueness status: **assumed, verify.** In the snippets I saw, no German e-invoice API (XInvoice, rechnungsapi.de, xhub, finaX, thelawin) advertises PDF/UA or faithful rendering of custom HTML templates. DocRaptor does PDF/UA and PDF/A but has no e-invoice XML. Confirm both points on the live pages before publishing the claim.

### A2. Offer stack

**Core (what the customer buys)**
1. **`POST /render`:** takes HTML+CSS or JSON invoice data and returns a PDF/A-3b file with embedded EN 16931 CII XML (ZUGFeRD 2.3/2.5 profiles EN16931/XRechnung, Factur-X), or a pure XRechnung UBL/CII.
2. **PDF/UA-1 tagging** on the same call (reading order, table headers, language, alt text for the logo). This covers the "accessible documents" request from public-sector customers (ADA Title II Apr 2027/2028, EAA) at no extra integration cost.
3. **A validation report returned with every file** (JSON + human-readable PDF): veraPDF (PDF/A-3b, PDF/UA-1) plus the KoSIT validator (EN 16931 + XRechnung CIUS schematron). The report is the product. It is the "proof to a third party" artefact 03 identified.
4. **Standards absorbed.** We track schema and profile updates (ZUGFeRD 2.5 launched May 2026; XRechnung 3.0.x) and pin versions per API key. A monthly changelog email reads: "This month we absorbed X; you changed nothing."
5. **Billing connectors.** A Stripe webhook connector (`invoice.finalized` → render → attach the PDF back to the Stripe invoice or email it), then Chargebee and Paddle. Each is a one-time mapping of Stripe fields to EN 16931 BT fields that the customer reviews once.

**Bonuses (each removes a specific objection)**
| Bonus | Objection it kills |
|---|---|
| **Free public validator** (upload any e-invoice from any tool and get the same report) | "There's a free generator": show what it fails on. It is also the top-of-funnel SEO asset. |
| **Stripe → EN 16931 field-mapping kit** (open-source, MIT) plus DE deadline checklist | "Mapping is the hard part" |
| **Template linter**: flags CSS that breaks PDF/A or UA (web fonts not embedded, transparency, missing `lang`) before production | "Will my existing design survive?" |
| **Self-host Docker licence** (Scale tier add-on) and open formats | "What if you vanish?" |
| **MCP tool + CLI** | Lets AI agents and CI pipelines use the same endpoint; cheap distribution |

**Guarantee (risk reversal): "Valid or free"**
- A document whose own attached report shows a failure (PDF/A-3b, PDF/UA-1, or EN 16931 schematron errors, excluding warnings) is **not counted or billed**.
- If a recipient's AP system rejects a file our report marked valid, for a *structural* reason, we fix it within 2 business days or **credit that month in full**.
- Tax content (VAT category, reverse charge, amounts) is the customer's responsibility and is stated in the ToS.
- Plus a 30-day money-back on the first paid month.

**Onboarding (self-serve, target: first valid file in under 10 minutes)**
1. Get an API key, with no card needed for the free tier.
2. Paste your HTML or connect Stripe (OAuth).
3. Get a sample ZUGFeRD file with its report.
4. Flip to live.

SDKs: Node, Python, PHP (big in DE: Shopware/Laravel) and .NET. There is a Postman collection. The docs are in English and German.

**Explicitly excluded (scope control keeps it passive)**
- Transmission: Peppol access point, French PA, KSeF clearance. We generate files; customers transmit or email them. We document hand-off to Billit/PAs.
- Tax determination or advice; inbound invoice parsing/OCR; remediation of legacy PDFs.
- Template design services and custom integrations beyond Stripe, Chargebee and Paddle.
- GoBD *procedural documentation* (Verfahrensdokumentation). The archive stores files; it does not certify the customer's process.
- Phone support and SLAs below the Scale tier. Support is email/docs only, with 1 business-day response.

### A3. Differentiation vs. top 3 competitors
| Dimension the buyer cares about (from 02/03) | **ValidRender** | Commodity e-invoice APIs (XInvoice €9.90+, rechnungsapi.de €5.99/100 docs, xhub, finaX) | DocRaptor (Prince) | Stripe Marketplace apps (Billit, "E-Invoice - E-Rechnung", MiracleBill) |
|---|---|---|---|---|
| **Keeps my existing invoice design** (HTML/CSS fidelity) | Yes: Chromium-grade layout, then PDF/A-3 post-processing | Mostly generate their own layout from JSON *(assumed; verify)* | Yes (Prince CSS; strong print CSS) | App's own template; limited customisation *(assumed)* |
| **Validation evidence per file** (veraPDF + EN 16931) | Report attached to every file; failures not billed | Business-rule validation on some (rechnungsapi.de mentions "Geschäftsregel-Validierung"); per-file report not seen | PDF/A validity; **no e-invoice XML at all** | Not seen |
| **PDF/UA accessible output** | Included from Growth | Not advertised *(snippet scan)* | Yes | No |
| **Works with non-Stripe billing / home-grown code** | Yes (generic API) plus Stripe/Chargebee/Paddle connectors | Yes (API) | Yes (API) | Stripe-only |
| Price for ~2k docs/mo | €99 (includes UA + archive) | ~€10–40 *(snippet/assumed)* | $75 for 1,250 docs, $149 Max *(snippet-verified)* | Unknown *(page blocked)* |

**Positioning in one line:** "cheaper than DocRaptor and e-invoice-aware; more than a €10 XML generator because it keeps your design, adds PDF/UA, and proves every file is valid." Don't fight the €6–10 tier. Let it be the anchor that makes the free validator's findings look alarming.

### A4. Recurring model and why it fits
**Usage-tiered subscription (documents per month) with an accumulating archive.** Invoices go out in a monthly billing run, forever, so the need recurs on the same schedule as the subscription. That avoids ScreenshotOne's "one project, one month" churn (02 §5.2 #13). The endpoint sits in the billing code path, where nobody touches working code. Standards drift (ZUGFeRD 2.5, XRechnung updates, later PL/BE/ViDA) makes the job harder every year. The archive adds switching cost without holding data hostage: export is always free.

### A5. Pricing tiers
Prices are in EUR for EU buyers and the same number in USD elsewhere. Annual plans get 2 months free.

| | **Sandbox (entry)** | **Starter** | **Growth** (hero) | **Scale** |
|---|---|---|---|---|
| Price | €0 | **€29/mo** | **€99/mo** | **€299/mo** |
| Validated documents / mo | 50 (sandbox watermark on live use: no) | 500 | 5,000 | 25,000 |
| Overage | n/a | €0.03/doc | €0.015/doc | €0.008/doc |
| ZUGFeRD / Factur-X / XRechnung, PDF/A-3b | Yes | Yes | Yes | Yes |
| Validation report per file | Yes | Yes | Yes | Yes |
| PDF/UA-1 tagging | No | No | **Yes** | Yes |
| Stripe connector | Yes | Yes | Yes | Yes |
| Chargebee/Paddle connectors | No | No | Yes | Yes |
| Archive (validated original + report, EU region) | 7 days | 90 days | **10 years** | 10 years + export API |
| Standards version pinning, changelog | Yes | Yes | Yes | Yes |
| Sub-accounts / white-label (Persona B app builders) | No | No | No | **Yes** |
| Uptime commitment | None | None | None | 99.9% SLA, status webhooks |
| Self-host Docker licence | No | No | No | Add-on €4,900/yr *(assumed; tests the "you might vanish" objection)* |

**Why these numbers**
- **Sandbox at 50 docs:** big enough for a real integration, small enough that any live SaaS outgrows it in month 1. 03's anti-persona (the "one invoice" freelancer) stays free and costs nearly nothing to serve.
- **Starter at €29, not the €19 03 suggested:** at €19 we look like a pricier €10 XML generator. €29 signals a different category and is still a card swipe, well below the ~€300 approval threshold (03 §1.1).
- **Growth at €99 (the hero):** anchored to the cost of the problem. A DIY build with its validation failures costs €1.2k–8k one-off plus maintenance (03 §1.3), so €99/mo pays back within a year before counting maintenance. It sits under DocRaptor Max ($149, snippet-verified) while adding e-invoice XML. PDF/UA and the 10-year archive live here on purpose, to pull buyers up from Starter.
- **Scale at €299:** built for Persona B (app builders reselling to merchants) and DocRaptor Bronze-level buyers ($399, snippet-verified). It comes in just under the ~€300 finance-approval line.
- **Documents, not MB credits,** which avoids PDFShift's "extra credits over 5 MB" complaint (02 #9).

**Entry offer:** the **free public validator** plus the Sandbox tier. There is no paid tripwire, because developers buy after they have integrated, not before.
- *Seasonal urgency (real):* in Q4 2026, offer an annual prepay with 2 months free that locks 2027 pricing. The DE Jan-2027 deadline for sellers over €800k turnover is real.

### A6. Unit economics (conservative; EUR)
| Line | Value | Status |
|---|---|---|
| Tier mix | 55% Starter / 35% Growth / 10% Scale | assumed |
| Blended ARPU (after annual discounts; overage ignored) | **€75/mo** | assumed (list mix = €80.5) |
| Payments / merchant of record (Paddle-type, handles EU VAT) | ~5% + €0.50 ≈ €4.25 | assumed (typical MoR rates) |
| Compute + storage per customer (Chromium render + veraPDF + KoSIT, about 2k docs/mo, about 1–3 CPU-s/doc; archive on object storage) | ~€0.75 | assumed |
| Allocated fixed infra (2-region servers, monitoring, status page ≈ €250/mo) at 100 customers | €2.50 | assumed |
| **COGS per customer** | **≈ €7.50** | assumed |
| **Gross margin** | **≈ 90%** | derived |
| Monthly churn | **3%**. Billing-path infra; conservative vs "nobody touches billing code" | assumed |
| **LTV** (ARPU × GM ÷ churn) | 75 × 0.90 ÷ 0.03 = **€2,250** | derived |
| **Target CAC** | **≤ €250** blended (SEO/free validator, near €0 cash; Google Ads on "ZUGFeRD API"/"XRechnung API" as a test only) | assumed |
| LTV:CAC | **9:1** at target (ceiling CAC for 3:1 = €750) | derived |
| **CAC payback** | 250 ÷ (75 × 0.9) = **3.7 months** | derived |
| Fixed costs/mo | ~€750 (infra €250, tooling/email/docs €100, reserve for a commercial PDF library licence if open-source tagging falls short ≈ €400) | assumed |
| **Break-even customers** | 750 ÷ 67.5 ≈ **12 customers** | derived |
| Founder target ($5–10k MRR) | ≈ 70–135 customers | derived |
| **MRR at 100 / 500 / 1,000 customers** | **€7.5k / €37.5k / €75k** | derived |

**Reality check on scale:** ScreenshotOne needed years to reach 800+ customers (01). 100–150 customers inside 18 months is ambitious but consistent with a deadline-driven market (DE 2027/2028). 500+ probably needs FR/PL/BE formats and the app-builder channel.

**Key risks to the economics**
- Stripe ships native EN 16931. Mitigation: be generic-API first, and keep Chargebee, Paddle, home-grown billing and app builders at about 50%+ of the mix.
- Chromium output → PDF/UA-1 tagging fidelity is an **engineering risk** (assumed solvable with Chromium tagged-PDF plus post-processing). The €400/mo licence reserve covers a fallback.
- Licence-check veraPDF (GPL/MPL) and KoSIT/Mustang (Apache 2.0) for SaaS use.

### A7. Pricing experiments to run first
1. **Validation-only fake door (week 1–3, ~€0):** on the free validator's result page, add "Get this report automatically for every invoice your current tool makes: €19/mo" next to "Render with ValidRender: from €29". **Metric:** click-through and waitlist signups per SKU. **Decision:** if validation-only wins more than 2:1, launch a Validate tier at €19 (priced at the e-rechnung-validator.de level of €39, snippet) as the wedge into users of free generators.
2. **Hero-tier price test (weeks 4–8, ~€300–500 ads):** Google Ads on DE/EN "ZUGFeRD API", "XRechnung API" and "E-Rechnung Stripe" split to two pricing pages, €29/€99/€299 vs €19/€79/€249. **Metric:** sandbox → paid within 30 days and the tier chosen. **Decision:** keep the higher ladder unless conversion drops by more than 25%.
3. **Q4 deadline annual prepay (Nov–Dec 2026):** an in-app and email offer: "Lock 2027 pricing: annual Growth at €990 (2 months free)". **Metric:** % of paid accounts converting to annual (target ≥ 25%). It also tests how strong the deadline urgency is and funds the build.

### A8. Sales pitch (in Jonas's words)
> "Stripe won't make your ZUGFeRD invoices, and from January 2027 your German customers can refuse a plain PDF. Keep the invoice template you already have: send it to one endpoint and get back a ZUGFeRD/XRechnung PDF/A-3 that's also accessible (PDF/UA), with a veraPDF and EN 16931 validation report attached to every file. If our own report says a file failed, you don't pay for it. When the standards change (ZUGFeRD 2.5 landed in May), we absorb it and you change nothing. Free for 50 invoices a month; €99/mo covers most SaaS billing runs, and that's less than one day of a developer chasing validator errors."

---

## Niche B — Shopify accessibility regression guard

**Working name:** *Driftguard*. Tagline: "Know what broke, who broke it, and prove you fixed it." Trademark and domain are not checked.

### B1. The "only" statement
> **We're the only Shopify accessibility app that tells you *which app update or theme edit* introduced new accessibility problems, flags it within hours, gives you a reviewed code fix for your theme, and keeps a dated, tamper-evident record of every change and fix: no widget, and no "compliance" badge. It's built for US/EU-selling Shopify stores that have been (or fear being) hit with a demand letter.**

Uniqueness status: **assumed, verify.** AccessComply, shoplab, ADAcertify and TestParty all market monitoring and/or fixes. None of the snippets showed *attribution to a specific app/theme change* or a *tamper-evident ledger*. If a competitor ships attribution, this offer degrades toward me-too. See the verdict.

Mechanism (so the claim is buildable):
- Shopify `themes/update` and `themes/publish` webhooks trigger a rescan within an hour.
- A nightly rendered-DOM diff fingerprints injected script sources, app-embed blocks (from `settings_data.json`) and app DOM nodes against a library of the top ~300 Shopify apps' signatures. Other apps' installs have no webhook, so we detect them through the diff.
- Each new violation is mapped to the change that introduced it.

### B2. Offer stack

**Core**
1. **Baseline scan:** axe-core-class WCAG 2.2 A/AA engine plus rendered keyboard and focus checks across each representative template (home, collection, product, cart drawer, search, pages, popups).
2. **Change-triggered rescans:** on theme publish/update and on a detected app change, plus a scheduled full crawl.
3. **Attribution:** each new issue is tagged "Introduced by: *Judge.me widget v…, detected 2026-10-03*" or "*Theme edit to `product-form.liquid` by staff member X*".
4. **Code-level fixes:**
   - Liquid, CSS and ARIA patches shown as a **diff**, applied to a **duplicate theme** first, with one-click rollback.
   - Bulk AI alt-text drafts that a human approves.
   - For third-party app issues we can't patch, a pre-written **vendor bug report** plus a CSS/Liquid workaround where one exists.
5. **Evidence ledger:** every scan, change, fix and approval is timestamped and hash-chained (tamper-evident), and exports as a PDF "Accessibility Activity Record" the merchant can hand to counsel.
6. **Monthly "What changed on your store" report:** for example, "3 apps updated, 1 introduced 4 issues, fixed in 1 click; 97 days with zero unresolved critical issues". This is the retention engine for the "invisible value" risk (03 §2.7).
7. **Auto-maintained accessibility statement** (ADA/EAA style), with an honest "known limitations" section.

**Bonuses**
| Bonus | Objection it kills |
|---|---|
| **"Which of my apps are hurting me?" free report** at install | "Is this worth paying for?" Shows attribution value in the first 5 minutes |
| **Overlay removal checklist** (plaintiffs search for widget script tags) | "I already pay for an overlay" |
| **Duplicate-theme preview plus rollback on every fix** | "Will it break my theme?" |
| **"Send to my developer" export** (GitHub-style patch file plus issue cards) | "I can't read code" |
| **Partner manual-audit referral** (discounted, human-delivered by a partner; we don't deliver it) | "Automated scans catch only ~30–40%" |

**Guarantee: "Caught-first"**
- If a theme publish or detected app change introduces a new automatically detectable *critical* violation and we don't alert you within 24 hours, **that month is free**.
- Plus a 30-day money-back on the first paid month.
- **Never** "lawsuit-proof", "compliant" or "certified". The copy states: "No tool can guarantee you won't be sued. We detect and fix what automation can find, catch regressions the day they happen, and keep your record."

**Onboarding (self-serve)**
1. Install from the Shopify App Store. The baseline scan runs automatically and the free app-impact report is ready in about 10 minutes.
2. A 14-day Guard trial through Shopify Billing.
3. A guided "fix your top 10" flow on a duplicate theme.
4. The weekly digest starts.

**Explicitly excluded**
- Legal advice, demand-letter responses, expert witness work, VPATs and certificates.
- Manual audits (partner referral only).
- Shopify checkout (Shopify-controlled), headless/Hydrogen storefronts, native mobile apps.
- Editing third-party app code (report plus workaround only).
- PDFs, video captions and custom development.
- Any storefront JavaScript widget or overlay. We ship **zero** storefront JS.

### B3. Differentiation vs. top 3 competitors
| Dimension the buyer cares about | **Driftguard** | AccessComply ($0/$49/$99/$399, snippet-verified) | TestParty ($599+/mo; $1–5k for larger stores, snippet-verified) | Overlays / widget apps (accessiBe from $41/mo, ADAcertify $37/mo; snippet-verified) |
|---|---|---|---|---|
| **"Which app or edit broke it?"** (attribution) | Core feature: change-triggered rescans mapped to app/theme change | Monitoring; attribution not seen *(verify)* | Managed service; attribution possibly manual | No |
| **Real code fixes, safely** | Diff on a duplicate theme, merchant approves, rollback | Auto-writes fixes to theme with backup (claims to auto-fix 60–72% of detected violations) | Human-managed source fixes via GitHub | No (runtime overlay; the FTC fined accessiBe $1M) |
| **Evidence for counsel** | Hash-chained, dated ledger plus PDF activity record | Statements and PDF reports | Audits and certificates | Widget "certificate" (plaintiffs target these) |
| **Honest claims** | No compliance or badge claims | Uses "ADA + WCAG compliance" language | "Compliance certificates" | Compliance marketing |
| Price for a typical $50k/mo store | $99 | $49–99 | $599+ | $37–49 |

**Positioning:** "AccessComply fixes your backlog; Driftguard guards the store after that." We compete on *what changed, who did it, and proof*, not on fix count. We lead with the regression story (02's 15/20 gap), not the mid-price story, because AccessComply already occupies the mid-price slot.

### B4. Recurring model and why it fits
**A monitoring subscription (a "guard" plan) with an accumulating evidence ledger.** The need recurs naturally because the store keeps changing: apps auto-update, staff edit themes, and plaintiff crawlers never stop. The value grows with time: "24 months of dated records" is the switching cost, and export is always free, never hostage. The agency tier fits the agency's own recurring "accessibility care plan" line item ($100–300/client/mo, per 03).

### B5. Pricing tiers
Billed via Shopify Billing; annual plans get 2 months free.

| | **Watch** | **Guard** (hero) | **Pro** (anchor) | **Agency** |
|---|---|---|---|---|
| Price | **$39/mo** | **$99/mo** | **$199/mo** | **$349/mo** for 15 stores, +$19/store |
| URLs covered | 50 representative | 500 | 5,000 + Markets/languages | Guard per store |
| Change-triggered rescans | Within 24 h | **Within 1 h** | Within 1 h | Within 1 h |
| App/theme attribution | Yes | Yes | Yes | Yes |
| Fixes | Copy-paste snippets | **One-click diff patches + rollback**, bulk alt text | Same + priority fix queue | Same |
| Evidence ledger | 12 months | **Unlimited + PDF activity record** | Same + on-demand "counsel pack" | Per client |
| Monthly "what changed" report | Yes | Yes | Yes (multi-language statement) | **White-label** |
| Partner audit discount | No | No | Yes | Yes |

**Why these numbers**
- **Watch at $39:** above cheap scanners (shoplab $10) and ADAcertify ($37). The attribution and ledger justify the premium. It sits below AccessComply Starter ($49), so it is the obvious low-risk first step for the "scared but not sued" buyer (the vicarious trigger in 03, and the best long-term customer).
- **Guard at $99 (the hero):** matches 03's "likely" WTP ($79) plus a margin for one-click patches. It prices at par with AccessComply Shield ($99) but sells a different job. The anchor is the cost of the problem: $5k–25k per demand-letter settlement plus $4k–30k remediation projects, so one letter pays for 4–20 years of Guard.
- **Pro at $199:** the anchor or decoy that makes Guard look reasonable. It serves recently sued stores and multi-market EU sellers. It sits far below TestParty's $599.
- **Agency at $349 for 15 stores (~$23/store):** agencies resell at $100–300/client (03), which is a 4–13x margin for them. They are our cheapest acquisition channel.

**Entry offer:** a free install with a **"Which of my apps are hurting accessibility?"** report plus a 14-day Guard trial.
- Optional tripwire, fully automated: **"Demand Letter Response Pack" at $149 one-off.** It contains an immediate full crawl, a prioritized fix list, the evidence-ledger start date and a partner-audit referral, and is **credited in full toward an annual Guard plan** within 30 days.
- Sold as *documentation*, never legal help. It captures the highest-intent, most reactive buyer (03 trigger #1) and converts them to annual at peak fear, which 03 recommends to counter post-settlement churn.

### B6. Unit economics (conservative; USD)
| Line | Value | Status |
|---|---|---|
| Tier mix | 40% Watch / 45% Guard / 10% Pro / 5% Agency accounts | assumed |
| Blended ARPU (after annual discounts; Agency counted as 1 account) | **$90/mo** | assumed (list mix = $97.5) |
| Shopify app revenue share (0% up to $1M/yr, 15% above) + processing fee | ~3% ≈ $2.70 | assumed (verify current Partner terms) |
| Compute (headless Chromium scans; about 5–15k page renders per store per month on shared dedicated servers) + LLM (alt text, fix drafts) | ~$3.00 | assumed |
| Support contractor (theme patches create tickets; ~10 min per customer per month at $25/h) | ~$4.50 | assumed |
| Allocated fixed infra at 100 customers | ~$2.00 | assumed |
| **COGS per customer** | **≈ $12** | assumed |
| **Gross margin** | **≈ 87%** | derived |
| Monthly churn | **4%** (03 estimate 3–5%; reactive post-lawsuit buyers churn) | assumed |
| **LTV** | 90 × 0.87 ÷ 0.04 = **$1,960** | derived |
| **Target CAC** | **≤ $250** blended (App Store organic search, SEO on "shopify ADA lawsuit"/"accessibility app broke", agencies; App Store ads as a test) | assumed |
| LTV:CAC | **7.8:1** at target (3:1 ceiling = $650) | derived |
| **CAC payback** | 250 ÷ (90 × 0.87) = **3.2 months** | derived |
| Fixed costs/mo | ~$750 (infra $250, tooling $150, **E&O/cyber liability insurance ≈ $200–300**, which is mandatory in this niche; one-off legal review of all marketing claims, $1.5–3k, amortised) | assumed |
| **Break-even customers** | 750 ÷ 78 ≈ **10 customers** | derived |
| Founder target ($5–10k MRR) | ≈ 55–110 customers | derived |
| **MRR at 100 / 500 / 1,000 customers** | **$9k / $45k / $90k** (at 1,000, revenue is about $1.08M/yr, so Shopify's 15% share applies to the portion above $1M) | derived |

**Passivity warning:** B is **less passive than A**. Theme patches, "your fix broke my popup" tickets and app-signature maintenance (apps change their markup) mean roughly 3–6 h/week at 100+ customers, even with a support contractor *(assumed)*. Keep patches opt-in and diff-reviewed, and keep the app-signature library automated (cluster unknown script origins, then review weekly).

### B7. Pricing experiments to run first
1. **Attribution fake door vs. fix-count messaging (week 1–3, ~$200):** two landing pages linked from SEO posts and a small Shopify-community ad budget. Page A: "A new app broke your accessibility. We'll tell you which one." Page B: "Fix accessibility issues in your theme code." **Metric:** waitlist or install-intent conversion. **Decision:** if B wins clearly, attribution is not the wedge and we are me-too against AccessComply, so reconsider the niche. This is the kill test.
2. **Watch $39 vs $29 at App Store launch:** run each price in a 4-week window (sequential test, since Shopify has no native A/B). **Metric:** install → paid and 60-day upgrade to Guard. Hypothesis: $39 loses under 15% conversion and gains margin.
3. **Demand Letter Pack ($149) → annual conversion:** track the % of pack buyers taking annual Guard ($990) within 30 days. Target ≥ 30%. If below 15%, drop the pack and fold the content into the trial.

### B8. Sales pitch (in Dana's words)
> "Your theme was 'accessible' when you bought it. Then 30 apps and a dozen freelancer edits later, you have no idea what's broken until a demand letter tells you. Driftguard watches your store and, the moment an app update or theme change breaks something, tells you *which one did it*, shows you the exact code fix, and applies it to a copy of your theme so nothing breaks. Every scan and every fix goes into a dated record you can hand your lawyer. No widget: plaintiff firms literally search for those. No fake 'compliance' badge: nobody can honestly sell you that. $99 a month, which is less than one hour of your lawyer's time."

---

## Verdict and cross-niche notes
| | A: ValidRender | B: Driftguard |
|---|---|---|
| Defensible offer? | **Yes.** Differentiated on fidelity + PDF/UA + per-file validation evidence + connectors, all verifiable by the buyer | **Yes, conditionally.** It rests on change attribution plus the evidence ledger. Experiment B7-1 is the kill test against AccessComply |
| Blended ARPU / GM | €75 / ~90% | $90 / ~87% |
| LTV / target CAC / payback | €2,250 / €250 / 3.7 mo | $1,960 / $250 / 3.2 mo |
| Break-even | ~12 customers | ~10 customers |
| MRR at 100 / 500 / 1,000 | €7.5k / €37.5k / €75k | $9k / $45k / $90k |
| Passivity | High (API + docs + SEO) | Medium (theme-patch support) |
| Biggest risk | Stripe ships native EN 16931; PDF/UA tagging fidelity | AccessComply adds attribution; claims liability |

**Recommendation:** build **A first**. It is more passive and has a real deadline window from now through Q1 2028. Run B's kill test (B7-1) in parallel for about $200 before writing any B code. Shared psychology: both products ship a **proof artefact** with every run (a validation report or evidence ledger), and both win on honest claims.

## Sources (snippet-level; pages were blocked for direct fetch)
- rechnungsapi.de pricing (€5.99/100 invoices, free 10): https://www.rechnungsapi.de/pricing
- XInvoice from €9.90/mo: https://www.xinvoice.net/en
- e-rechnung-validator.de API from €39/mo: https://e-rechnung-validator.de/xrechnung-validator-api/
- invoice-api.xhub (pay-as-you-go, free tier): https://invoice-api.xhub.io/en
- InvoiceXML ZUGFeRD API (100 free credits): https://www.invoicexml.com/blog/zugferd-api-toolkit
- thelawin.dev: https://thelawin.dev/ ; finaX: https://finax.dev/
- DocRaptor tiers ($15/$29/$75 for 1,250 docs/$149/$399/$1,000): https://www.softwareadvice.com/api-management/docraptor-profile/ , https://apis.io/plans/docraptor/docraptor-plans-pricing/
- Stripe e-invoicing requires an app (no native EN 16931): https://wavect.io/blog/stripe-billing-e-invoicing-2027/ , https://marketplace.stripe.com/apps/e-invoice-e-rechnung , https://marketplace.stripe.com/apps/e-rechnung-e-invoice-miraclebill
- ZUGFeRD 2.5 launch (May 2026): https://www.vatupdate.com/2026/05/03/zugferd-2-5-launch-en-16931-alignment-and-gross-invoicing-for-germany-and-france/
- AccessComply ($0/$49/$99/$399, source-code fixes): https://accesscomply.com/
- TestParty ($599+; $1–5k/mo): https://testparty.ai/blog/cheapest-ada-compliance-tools-shopify
- shoplab Accessibility Check ($10/mo, Flow): https://apps.shopify.com/shoplab-accessibility-check
- ADAcertify ($444/yr ≈ $37/mo, widget-based): https://getadacertify.com/best-shopify-accessibility-app-in-2026/
- accessiBe from $41/mo: https://frontdeskreview.com/software/web-accessibility/accessibe/
- AccessShield (pricing not retrieved): https://accessshield.net/
