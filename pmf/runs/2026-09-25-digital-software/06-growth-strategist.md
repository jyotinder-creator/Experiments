# 06 — Growth Strategist (Jordan) — run 2026-09-25-digital-software

**Scope:** the two finalists that survived 01–03.
- **A. Compliance-grade document render API.** Germany first: ZUGFeRD/XRechnung, PDF/A-3, PDF/UA and a validation report with every file. Buyer: developers at B2B SaaS companies invoicing German or French businesses. Secondary buyer: builders of invoicing plugins.
- **B. Shopify accessibility regression guard.** Buyer: Shopify stores doing $20k–$300k/mo with many apps installed. Secondary buyer: Shopify agencies.

**Founder constraints** (`pmf/brief.md`): digital only, global, bootstrapped, passive recurring revenue, a target of $5–10k MRR in 12–18 months, and under 5–10 h/week at steady state.

**Motto applied:** rent attention (outreach, small ad tests) in month 1 to validate. Own attention (search assets, marketplace listings, integrations, data pages) from month 2 onward.

**Evidence quality:** Reddit, G2, apps.shopify.com and ahrefs.com were blocked by the proxy. Channel evidence below comes from search-result snippets and competitor pages, and the URLs are listed at the end. **No keyword-volume or CPC data was retrievable.** Every CAC, CPC and conversion figure here is an *estimate* built from typical dev-tool and Shopify-app benchmarks, and each one is labelled as an estimate. 04 and 05 were not present when I finished, so this file does not reflect them.

---

## Headline

| | A. Render API (DE e-invoicing) | B. Shopify a11y regression guard |
|---|---|---|
| Primary compounding channels | (1) Search moat built on a **free validator plus programmatic "error-code" and stack pages**; (2) **integration marketplaces**: a Stripe App, n8n/Make/Zapier nodes, an MCP registry listing | (1) **Shopify App Store** listing and reviews; (2) **programmatic SEO data pages**: accessibility scorecards per app and per theme, plus lawsuit-data pages |
| Blended CAC, months 6–12 (estimate) | **$40–120** cash-equivalent | **$30–90** |
| Time to first 10 paying customers | 30–60 days (deadline-driven; Q4 2026 is ideal timing) | 30–45 days |
| Passive / active once running | **Passive** (≈3–5 h/wk: standards updates, content refresh) | **Semi-passive** (≈5–8 h/wk: support tickets on theme patches, review requests, agency relationships) |
| Biggest growth risk | **The head terms are already contested.** At least 8 small ZUGFeRD API vendors are fighting for "ZUGFeRD API" and "E-Rechnung API", so you must win the long tail and the "validated + looks right + PDF/UA" angle | **Price anchoring.** AccessifyAI is a no-overlay scanner with regression monitoring at **$9.99/mo**, and 25 of the 28 category apps are overlays, so you must differentiate on app attribution plus the evidence ledger |

---

## Niche A — Compliance-grade render API (Germany first)

### A1. Where the buyer looks (ranked, with evidence)

1. **Google search, German and English, developer-intent long tail.** Competitors are clearly buying and ranking here, which proves the channel converts:
   - rechnungsapi.de has a dedicated Stripe landing page ("Stripe-Rechnungen in XRechnung & ZUGFeRD wandeln") plus API docs.
   - zugferdapi.dev ("Die Entwickler-API für die E-Rechnungspflicht 2027").
   - invoice-api.xhub.io, faktoora's `/lp/erechnung-api/` paid-landing page, and InvoiceXML's "ZUGFeRD API toolkit" blog.
   - Stripe-specific pages from fizard.com and miraclesync.de.
   - thelawin.dev.
   - Stripe itself ranks with explainer pages (zugferd-germany, xrechnung, e-invoice-in-germany) that send readers to Marketplace partners.
   - **Read:** the head terms are contested. The long tail (validation errors, stacks, HTML fidelity, PDF/UA) is not.
2. **Stripe App Marketplace.** Stripe's own guide, "Send e-invoices on Stripe Billing through App Marketplace Partners", routes German users to apps. Listed today: **Billit**, **Pennylane** and a small **"E-Invoice - E-Rechnung"** app. Stripe's docs actively *push* traffic here because Stripe won't build the feature natively. This is borrowed distribution with high intent.
3. **Workflow-automation marketplaces (n8n, Make, Zapier).** Evidence:
   - invoice-api.xhub ships `n8n-nodes-invoice-api-xhub` and has an n8n.io integration page.
   - Community nodes `n8n-nodes-einvoice` and `n8n-nodes-einvoice-de` exist.
   - An n8n Community thread describes "a 22-node workflow, and the 7 mistakes I made" to produce ZUGFeRD/XRechnung. That is DIY pain in public.
   - rechnungsapi.de markets "webhook, n8n/Make/Zapier".
4. **GitHub and dev.to (where developers hit the wall).** Where to look:
   - Issues on `ZUGFeRD/mustangproject`, `akretion/factur-x` and the `e-rechnung` GitHub topic.
   - dev.to posts: "…actually pass veraPDF and Mustang" and "Stripe invoice JSON → EN 16931 BT Germany 2026".
   - These are the recruiting grounds for the first 10 customers and for backlinks.
5. **German developer and SaaS communities.** heise forum, r/de_EDV and r/SaaS (blocked, not verified), LinkedIn posts by German SaaS CTOs, and the OMR / t3n newsletters. These give reach but they are an *active* channel.
6. **Invoicing-plugin builders (Persona B, white-label partners).** WooCommerce Germanized ecosystem vendors, Shopware plugin vendors, and the 4+ German ZUGFeRD Shopify apps (Rechna, InvoPass, Docufacture, Clever Invoice, per 02). Each one is a potential *customer* for the backend.

### A2. First 10 customers (30 days, manual and scrappy)

**Offer:** a "Founding 10" plan at **$49/mo locked for life** (list price $79–99), with a white-glove migration of your existing invoice HTML template done by the founder, plus a written validation report. The ask in return is a logo, a 20-minute switch interview, and permission to write a case study.

| Week | Tactics |
|---|---|
| **1** | Ship a **free web validator**: upload a PDF, get veraPDF PDF/A-3 + EN 16931/KoSIT results in plain English plus a "fix it with one API call" CTA. Publish in English and German. Ship a landing page with the message "Stripe won't make your ZUGFeRD invoices. One API call will. Validated, report attached." Set up the docs, the Node/Python/PHP snippets and a free tier of 50 docs/mo. |
| **2** | **Warm outreach to 60–80 developers who have publicly hit the problem.** Sources: authors of GitHub issues on mustangproject, factur-x and the node/php ZUGFeRD libraries from the last 12 months; the dev.to authors above; the n8n "22-node" thread author and commenters. Message: *"Saw your issue about [BR-DE-xx / PDF/A-3 embedding failing veraPDF]. I built an API that returns a validated ZUGFeRD PDF/A-3 from your existing HTML and attaches the veraPDF + KoSIT report. Want me to run your current invoice through it and send you the report? Free, no signup."* Aim for 15–25% replies and 8–12 free report runs. |
| **2–3** | **LinkedIn / email to 50 German-market B2B SaaS founders and CTOs** (billing on Stripe, found through Stripe customer lists on BuiltWith or stack pages). Hook: *"From 1 Jan 2027, if you invoice German companies above €800k turnover, a PDF from Stripe is no longer enough. We generate the ZUGFeRD file from the invoice you already send."* Submit the Stripe App to Marketplace review now, because review can take weeks. |
| **3** | **Show HN / dev.to launch post:** "We ran 5 free Factur-X generators through veraPDF and the KoSIT validator. Here's what failed." This is the validator-comparison objection-killer from 03, used as launch content. Post in the n8n community with a ready-made workflow template. |
| **4** | Convert free-report users with a 15-minute call plus template migration. Offer 3 invoicing-plugin builders a **white-label pilot** (free for 60 days, then per-document wholesale pricing). |
| **Validation budget (optional)** | $300–500 on Google Ads (DE and EN) on exact-match terms: "zugferd api", "xrechnung api", "stripe e-rechnung", "factur-x api", "e-rechnung erstellen api". The purpose is to read signup rate and CPC, not to scale. *Estimated CPC €3–10; not verified.* |

**Target by day 30:** 150 validator uses, 30 free-tier signups, 5–10 paying customers (each Founding-10 customer is worth $49–99 MRR). If fewer than 3 are paying by day 45 despite the Q4 deadline memo season, that is a strong negative signal.

### A3. Compounding engine (months 2–12)

**Primary channel 1: a search moat built on a free tool plus programmatic pages (passive).**
- **Free tools as lead magnets:** the validator (DE/EN/FR), a "Stripe invoice → ZUGFeRD preview" tool (paste a Stripe invoice ID or JSON), and a "Does my invoice need to be an e-invoice in 2027?" decision tool (turnover × customer type × country).
- **Programmatic "error-code" pages (the core long-tail asset).** Build one page per KoSIT/EN 16931 business rule and veraPDF failure, e.g. "BR-DE-15 Leitweg-ID fehlt", "BR-CO-10 sum of line net amounts", "PDF/A-3 embedded file AFRelationship missing". Developers paste validator errors straight into Google, so each page gets an explanation, a fix and the "our API never produces this" CTA. There are roughly 200–400 rules and failure messages, all written once. The content is only refreshed when the standards change, which you have to track anyway.
- **Stack pages:** "ZUGFeRD in Node.js / Python / PHP-Laravel / Ruby-Rails / .NET / Go", "Factur-X with WeasyPrint", "Puppeteer PDF → PDF/A-3", "Stripe Billing e-Rechnung", "Chargebee / Paddle / Lemon Squeezy XRechnung". That is about 30–40 pages.
- **Comparison and "alternative to" pages.** These capture people switching away from a competitor:
  - "DocRaptor alternative for PDF/A and PDF/UA"
  - "PDFShift / Api2Pdf PDF/A-3"
  - "Mustang vs API"
  - "rechnungsapi vs … / invoice-api.xhub vs …" (factual and honest)
  - "wkhtmltopdf ZUGFeRD".
- **Content volume:** about 60 hand-written pages in months 1–4 (≈6 h/wk), then about 250 programmatic error pages generated from the rules corpus. After that, a monthly "standards changelog" post (XRechnung 3.x/4.0, ZUGFeRD 2.3.x) that doubles as the retention email.
- **Expected ramp (estimate):** 500–1,500 organic visits/mo by month 6 and 3–6k by month 12. At 3–5% visit → free signup and 4–8% free → paid (typical dev-tool range), that is **8–20 new paid customers a month by month 12**.

**Primary channel 2: integration marketplaces (passive once listed).**
- **Stripe App:** "Validated ZUGFeRD/XRechnung for every Stripe invoice". It uses your existing Stripe invoice branding, attaches the PDF/A-3 to the invoice email and keeps the validation report. Stripe's docs already point German users to the Marketplace, and there are currently only 3 or so relevant listings. This is the highest-intent borrowed channel, and it makes Stripe's native-feature risk (03 §1.7) explicit: if Stripe ships the feature natively, this channel shrinks. **Hedge:** keep the core API platform-agnostic.
- **n8n community node plus Make and Zapier apps**, each with published workflow templates ("Stripe invoice.finalized → ZUGFeRD → email", "Lexoffice/sevDesk export → XRechnung"). The competitor xhub already has one, which proves demand. Yours wins on the validation report and on keeping the design from your HTML.
- **MCP registry / agent tool listings** (a cheap, low-signal lottery ticket).

**Secondary (semi-passive): white-label for invoicing-plugin builders.** Each signed plugin vendor brings hundreds to thousands of merchants at wholesale per-document pricing. Expect 1–3 signed in year 1. The work is front-loaded (integration support), then it runs passively.

**Avoid long-term:** outbound cold email as the main channel, conference sponsorships (DMS/EXPO, E-Rechnungs-Gipfel) and paid search beyond validation. German B2B CPCs rise sharply into the Jan 2027 and Jan 2028 deadlines (estimate), and with ARPU around $79 paid search only pencils out at ≥8% free → paid.

### A4. Channel economics (estimates, not measured)

| Channel | Est. CAC (cash + founder-time at $50/h) | Time to results | Once running | Notes |
|---|---|---|---|---|
| Warm outreach to GitHub, dev.to and n8n "problem-havers" | $100–250 (time only) | 1–4 weeks | **Active** | Best for validation and interviews; doesn't scale |
| LinkedIn / email to DE SaaS CTOs | $150–400 | 2–6 weeks | **Active** | Deadline hook works in Q4 2026 and Q4 2027 |
| Google Ads (validation only) | $150–400 per paid customer | Days | Active (spend) | CPC €3–10 (est.); stop after reading the signal |
| Free validator + error-code/stack SEO | $20–60 at maturity | 4–9 months | **Passive** | Compounds; content refreshes as standards change |
| Comparison / alternative pages | $20–50 | 3–6 months | **Passive** | Captures DocRaptor, PDFShift and DIY switchers |
| Stripe App Marketplace | $10–40 | 1–3 months (review) | **Passive** | Platform risk if Stripe goes native |
| n8n / Make / Zapier nodes + templates | $10–40 | 2–4 months | **Passive** | Competitor precedent (xhub) |
| White-label plugin builders | $200–600 per partner (but each partner brings N merchants) | 2–6 months | **Semi-passive** | Front-loaded integration work |
| Show HN / dev.to launch posts | ~$0 | Spike | One-off | Backlinks feed SEO |

**Funnel math to $5k MRR** (ARPU ≈ $79, estimate): about 65 paying customers. At 5% free → paid that means about 1,300 free signups, which at 4% visit → signup is about 32k cumulative visits. Realistic by **month 12–15** if the error-code pages rank. Churn should be low (<2%/mo, because the API sits in billing code), so MRR compounds.

### A5. Moat-building moves
- **Validation data moat:** anonymised aggregate stats on "which rules fail most, by generator or library". Publish a quarterly "State of German e-invoice validity" report. It is citation-bait, it earns backlinks, and it reinforces the "validated" brand.
- **Standards-velocity brand:** a public changelog and status page ("we absorbed XRechnung 4.0 on day X"). Being the first to support each new version makes you the default recommendation.
- **Integrations as switching cost:** Stripe App, n8n node, SDKs in 5 languages, and templates stored on your side (exportable, but convenient to leave where they are).
- **Distribution partnerships:** white-label plugin builders, and referral deals with German Steuerberater software communities (DATEV marketplace later; heavy, so year 2).
- **Adjacent reuse:** the same engine for PDF/UA documents in the US ADA Title II Apr 2027 window, and FR Factur-X in Sep 2027. Each adds a new cluster of search pages to the same domain.

### A6. Leading indicators (weekly)
- Validator runs per week, and the % of runs that FAIL (the pain signal)
- Free signups; % making their first API call within 24h (activation)
- Free → paid conversion by cohort; docs/month per paid account (a proxy for expansion)
- Organic sessions and impressions on error-code pages (GSC); number of ranking pages in the top 10
- Stripe App installs; n8n node downloads (npm weekly)
- Outbound reply rate (month 1 only); number of switch interviews completed
- Support tickets per 100 accounts (the passivity guard: target <3/wk)

---

## Niche B — Shopify accessibility regression guard

### B1. Where the buyer looks (ranked, with evidence)

1. **The Shopify App Store accessibility category.** This is the default place merchants search. A dev.to author who scanned thousands of themes says **25 of the 28 apps in the category are overlays**. Non-overlay scanners are few:
   - **AccessifyAI** (free, and $9.99/mo for weekly scans, fix suggestions and "continuous monitoring catches regressions from theme updates")
   - Accessify ADA Audit
   - Scanify
   - Accesimo.

   **The channel is proven to exist, but the $9.99 anchor matters.** Do not compete on "scanner". Compete on "*which app broke it* + evidence ledger + on-change webhook scan". (The listing pages themselves were blocked, so the details come from snippets.)
2. **Google search for lawsuit and compliance intent.** The SERP for "Shopify ADA compliance" is dense with guides from TestParty, accessible.org, wcagsafe, ratedwithai, adascanner, 216digital, Appify Commerce, AudioEye and fudge.ai, plus roundups like "best Shopify accessibility apps 2026". Competitors investing this heavily shows the traffic converts. **EcomBack** built a whole content brand on monthly and quarterly lawsuit reports. Its Q1 2026 report: 1,037 suits, **Shopify 459 (44%)**, **265 involving widgets**, and the top 10 law firms filing 82.5% of cases. Lawsuit data is an owned-media format that already works in this niche.
3. **Shopify agencies (Persona B).** TestParty runs a white-label agency program ("agency increased retention 34%"), and AccessPro sells white-label to agencies. Agencies are clearly a channel that competitors are paying to reach. Find them through the Shopify Partner Directory and Storetasker/Experts marketplaces.
4. **Shopify Community forums and founder groups.** The ADA threads cited in 02 and 03 ("ADA accessibility lawsuits", "Legal Scam re ADA compliance") exist, and an ADA/privacy-lawsuit video sits in the Google Merchant Center Community. There are also DTC Facebook groups and r/shopify (blocked, not verified). Useful for reach and recruiting; *active*.
5. **Podcasts and newsletters** (2X eCommerce, DTC Pod, Retail Tech). Awareness only, one-off.

### B2. First 10 customers (30 days)

**Offer:** a "Founding stores" plan at **$49/mo locked** (list price $79), with the founder personally reviewing the first scan and a 30-minute walkthrough of the fix list. For agencies: **3 client stores free for 60 days**, then $199/mo for 10 stores.

| Week | Tactics |
|---|---|
| **1** | Submit the app to the Shopify App Store (review takes 1–3 weeks, so start on day 1). At the same time, build a **free public "Plaintiff-crawler check"** at `yourdomain/check?store=…`. It runs axe-core-class checks on the homepage, a product page, the cart and search, detects overlay script tags ("plaintiff firms search for these"), and lists the installed third-party app scripts it can see. It exports a PDF. |
| **1–2** | **Build a target list of 300 stores** using StoreCensus or BuiltWith: Shopify, apparel/beauty/home/food, overlay widget detected (accessiBe, UserWay, Accessibly), $20k–$300k/mo estimated revenue. Overlay users are pre-qualified: they already pay for accessibility and are more likely to be sued (EcomBack: 25.5% of Q1 2026 suits involved widgets). |
| **2–3** | **Cold email the 300 with their store's free report attached** (personalised, no scare tactics, no compliance claims). Hook: *"Your store runs [UserWay]. In Q1 2026, 265 ADA suits targeted sites with widgets. We scanned your product page: 14 issues a plaintiff crawler would flag, 9 from theme code, 3 introduced by [Klaviyo form / reviews app]. Here's the report. The fixes are code, not a widget."* Expect 3–6% replies (estimate), about 10–15 conversations and 4–7 installs. |
| **2–4** | **Agencies:** personally email 40 Shopify agencies from the Partner Directory (5–30 staff, 20+ retainer clients) offering a white-label "accessibility care plan" line item. Close 1–2 design partners; each brings 3–10 stores. |
| **3–4** | Post a useful, non-promotional reply (the free check link, no pitch) in the active Shopify Community ADA threads. Publish "We scanned the top 100 Shopify apps' storefront widgets for WCAG failures", seeded to r/shopify, DTC Twitter/LinkedIn and dev.to. |
| **Validation budget (optional)** | $300 of Shopify App Store search ads on "ada", "accessibility", "wcag", "eaa" once the listing is live. It reads install → paid rate. *CPC est. $1–4; not verified.* |

**Target by day 30:** 200 free checks, 15 installs, **5–10 paying stores** (including agency stores). Kill or pivot signal: installs happen but fewer than 20% convert to paid after the trial. That would mean the $9.99 scanners are "good enough" and the attribution and ledger wedge isn't landing.

### B3. Compounding engine (months 2–12)

**Primary channel 1: the Shopify App Store (passive once ranked).**
- The ranking drivers are review count and velocity, install → retention, and keyword-rich listing copy ("ADA", "WCAG 2.2", "EAA", "accessibility audit", "no overlay", "BFSG"). Localise the listing into DE, FR and NL for the EAA angle (the German BFSG keyword is already used by competitors).
- **Review engine:** an in-app prompt fires right after the first "regression caught and fixed" event, which is the peak-value moment. Aim for 30 reviews by month 6 and 80+ by month 12.
- **"Built for Shopify" badge:** aim for it by month 6 (it helps category ranking).
- **Expected ramp (estimate):** 3–8 organic installs/day by month 9 at 25–35% trial → paid, which is **25–60 new paid stores a month**. That is the upside case. The conservative case is a third of that.

**Primary channel 2: programmatic SEO data pages (passive, and hard to copy).**
- **"Is [App] accessible?" scorecards for the top 300–500 storefront-facing Shopify apps** (reviews widgets, pop-ups, chat, upsell, loyalty). You scan the injected widget on a test store and publish WCAG failures, the date, and a changelog when the app updates. This matches the exact job ("which app broke it"), and no competitor can copy the data without doing the scanning. The pages also create a loop with app developers: they share when they fix their score, which earns backlinks.
- **"Is [Theme] accessible?" scorecards** for every Theme Store theme and version (about 200 pages), re-scanned whenever a theme version ships.
- **Monthly "Shopify ADA lawsuit report"** built on public filings (the EcomBack format with a Shopify focus): factual and non-defamatory, covering counts, industries and the most common failure patterns. It becomes a newsletter plus press and backlink magnet.
- **Evergreen intent pages:** "received ADA demand letter Shopify what to do", "Shopify accessibility statement generator" (free tool), "accessiBe/UserWay alternative Shopify", "remove accessibility overlay Shopify", "EAA Shopify checklist", "BFSG Shopify".
- **Content volume:** about 30 hand-written pages, 500–700 programmatic scorecards (automated re-scans) and 1 monthly report (≈4 h/month once templated).

**Secondary (semi-passive): agency partner program.** A multi-store dashboard, white-label PDFs, and a 20–30% recurring rev share or wholesale pricing. It compounds through agency retention, but it needs relationship upkeep, so expect ≈2 h/wk.

**Avoid long-term:** fear-based cold email at scale (reputational and spam risk in a category tarnished by overlays), Meta ads to "scared merchants" (FTC-claims risk plus weak economics at an ARPU of $49–79), and doing manual audits yourself (refer them to a partner instead).

### B4. Channel economics (estimates, not measured)

| Channel | Est. CAC | Time to results | Once running | Notes |
|---|---|---|---|---|
| Cold email to overlay-using stores + free report | $60–150 (tools + time) | 2–4 weeks | **Active** | Validation only; keep it personalised and low-volume |
| Agency design partners | $100–300 per agency (brings 3–10 stores) | 3–8 weeks | **Semi-active** | Best LTV; relationship upkeep |
| Shopify App Store organic | $10–40 | 1–3 months to index, 6+ months to rank | **Passive** | Listing + reviews = the compounding asset |
| Shopify App Store search ads | $80–200 per paid store | Days | Active (spend) | Profitable only if 12-month LTV >$600 (≈$79 × ~10 months) |
| App/theme scorecards (programmatic SEO) | $15–50 at maturity | 4–9 months | **Passive** | Automated re-scans; unique data |
| Monthly lawsuit report / newsletter | $20–60 | 3–6 months | **Semi-passive** | ≈4 h/month |
| Shopify Community / founder groups | Time only | Weeks | **Active** | Recruiting and interviews |

**Funnel math to $5k MRR** (blended ARPU ≈ $70 incl. agency seats, estimate): about 72 paying stores. Churn is 3–5%/mo (03), so you need **about 3–4 new stores a month just to stand still at 72**. The App Store has to deliver 10+ paid stores a month by month 9 for the business to grow. That is plausible but not certain given the $9.99 scanners.

### B5. Moat-building moves
- **App-and-theme accessibility index:** the dataset of scan results per app version and theme version. It is the attribution engine and the SEO asset at once. It gets more valuable every month, and a competitor starting later cannot reproduce the history.
- **Evidence ledger as switching cost:** dated scans, fixes and statement versions. "Cancelling means losing 24 months of monitoring records" (03), and it is exportable, not held hostage.
- **Honesty brand:** "No widget. No badge. We tell you what we don't cover." This is the anti-overlay position, and it earns citations from accessibility advocates and lawyers.
- **App-developer network effect:** invite app developers to claim their scorecard and get notified of regressions. Some will embed "tested with [you]" badges, which is free distribution.
- **Partner referrals:** a manual-audit partner (you refer up and they refer SMB monitoring down) and ADA-defence law firms (a "what to do after a demand letter" co-authored guide).

### B6. Leading indicators (weekly)
- Free checks run; % of checks from stores with overlays detected
- App installs; install → first full scan completion; trial → paid %
- Regressions caught per store per month (the core value event), and % of stores with ≥1 caught regression in the first 30 days
- Reviews added per week; App Store keyword rank for "accessibility", "ada", "wcag"
- Scorecard pages indexed and their organic clicks; app-developer claims per week
- Monthly churn by acquisition source (reactive "demand-letter" buyers vs proactive)
- Support tickets per 10 stores (passivity guard: target <1/wk per 10 stores)

---

## Cross-niche recommendation (growth lens)

- **A fits "passive" better.** Its compounding assets (error-code pages, validator, integrations) are built once and refreshed rarely. Customers sit in billing code, so churn is low. The risk is crowding at the head terms (8+ small ZUGFeRD APIs), so **the growth thesis only works if you own the long tail and the "validated + your own design + PDF/UA" position.** The deadline calendar (DE Jan 2027 and Jan 2028, FR Sep 2027) gives two or three natural demand spikes.
- **B has faster early traction** (the App Store plus a hot fear trigger) and a better unique-data SEO asset (scorecards), but churn is higher and the support load is real. A $9.99 incumbent already claims regression monitoring.
- **Sequencing if the founder can only do one:** A, with the Stripe App and the validator shipped in Q4 2026 to catch the Jan 2027 wave. B's scorecard engine could be a year-2 second product: same "verifiable output" thesis, different buyer.

## Sources
- https://www.rechnungsapi.de/ and https://www.rechnungsapi.de/loesungen/startups
- https://www.zugferdapi.dev/
- https://invoice-api.xhub.io/en and https://invoice-api.xhub.io/en/docs/integrations/n8n
- https://n8n.io/integrations/invoice-apixhub/
- https://faktoora.com/lp/erechnung-api/
- https://www.invoicexml.com/blog/zugferd-api-toolkit
- https://thelawin.dev/
- https://fizard.com/stripe-e-rechnung
- https://www.miraclesync.de/blog/stripe-e-rechnung
- https://stripe.com/guides/send-e-invoices-on-stripe-billing-through-app-marketplace-partners
- https://marketplace.stripe.com/apps/e-invoice-e-rechnung
- https://marketplace.stripe.com/apps/billit-e-invoicing
- https://stripe.com/resources/more/zugferd-germany
- https://community.n8n.io/t/turning-pdf-invoices-into-zugferd-xrechnung-a-22-node-workflow-and-the-7-mistakes-i-made/310891
- https://github.com/geckse/n8n-nodes-einvoice
- https://github.com/topics/e-rechnung
- https://apps.shopify.com/accessifyai (via snippet; direct fetch blocked)
- https://dev.to/_abbdc2/what-i-learned-scanning-thousands-of-shopify-themes-for-wcag-violations-5d29
- https://www.fudge.ai/blog/best-shopify-accessibility-apps/
- https://testparty.ai/blog/best-shopify-accessibility-apps-2026
- https://testparty.ai/blog/how-one-shopify-agency-increased-retention-34-with-white-label-accessibility
- https://accesspro.io/accessibility-widget/shopify-app/
- https://www.ecomback.com/ada-website-lawsuits-recap-report/q1-2026
- https://accessible.org/shopify-ada-compliance/
- https://wcagsafe.com/blog/shopify-ada-compliance
- https://support.google.com/merchants/community-video/411964495/ada-privacy-lawsuits-targeting-shopify-stores-how-to-avoid-a-lawsuit
- Plus sources carried from 02-gap-hunter.md and 03-customer-psychologist.md
