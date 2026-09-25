# 03 — Customer Psychologist (Dr. Lena) — run 2026-09-25-digital-software

**Scope:** the 4 finalists from `02-gap-hunter.md`, each analysed through the wedge gap-hunter recommended. The AI receptionist was dropped for lack of a gap. Founder constraints from `pmf/brief.md`: digital/software only, global, bootstrapped, passive recurring revenue, scale without more effort. Date: 2026-09-25.

## Evidence quality: read first

- **Direct page fetches were blocked almost everywhere this session.** Beyond the known list (Reddit, G2, Capterra, Trustpilot, HN, Shopify Community, wordpress.org, apps.shopify.com, ahrefs.com), fetches of thelawin.dev, nicchan.me, lamicrobyflo.fr and comparatif-facture-electronique.fr also failed. **Every quote below comes from a search-engine snippet or from gap-hunter's snippet set.** Text in quotation marks is what the snippet presented as page text. Text marked *(paraphrase)* was summarised by the search layer and is not verbatim. Anything marked *(GH #n)* is reused from `02-gap-hunter.md`, with the link given there.
- **Context quotes from real buyers are scarce** ("I just started…", "my accountant told me…"). Most of the triggers here are *structural*: legal deadlines, demand letters, a platform that stops supporting something. I say so wherever it applies. **Next step for validation:** 8–10 switch interviews per kept niche (script at the end of this file).
- **Three findings contradict earlier stages. They change the verdicts:**
  1. **Niche 3 (FR e-reporting):** as of 22 Apr 2026, **13 approved platforms (PAs) offer free plans to micro-entrepreneurs and TPEs** (Indy, Tiime, Pennylane, Qonto…). For micro-entrepreneurs, e-reporting is a **periodic summary every two months**, not per-transaction reporting. That removes the bottom of the market. [quelle-pa.fr / tiime / portail-autoentrepreneur via snippet](https://blog.tiime.fr/plateforme-agreee-facturation-electronique-gratuite)
  2. **Niche 4 (Shopify AI visibility):** gap-hunter said "none of them sees order data". In fact **at least 6 Shopify-native apps already track product-level recommendations**: Visibly AI (Pro **$9.90/mo**), TrackGPT (builds prompts from your catalogue and samples daily), ShelfSight, Mento, IndexGPT and Finseo. **Shopify Admin already shows ChatGPT-attributed orders natively**, and TrackBee sells UTM attribution. [apps.shopify.com/visibly-ai](https://apps.shopify.com/visibly-ai), [gettrackgpt.com/shopify](https://gettrackgpt.com/shopify), [apps.shopify.com/shelfsight](https://apps.shopify.com/shelfsight), [finseo.ai](https://www.finseo.ai/chatgpt-shopping-visibility), [get-ryze.ai](https://www.get-ryze.ai/blog/shopify-chatgpt-agentic-storefronts-guide)
  3. **Niche 1 (render API):** the Factur-X/ZUGFeRD *generation* layer is commoditising. There is a **free, keyless REST API (XMLBridge)**, several Apify actors, thelawin.dev and InvoiceXML, plus open-source libraries (akretion `factur-x` for Python, `@stackforge-eu/factur-x` for Node). The paid value has to be **validation, PDF/UA, HTML fidelity and an archive**, not "we embed the XML". [dev.to snippet re XMLBridge](https://dev.to/ismail183/generate-a-factur-x-pdf-invoice-in-python-free-no-library-needed-2n3o), [apify](https://apify.com/josefbednar/factur-x-zugferd-e-invoice), [thelawin.dev](https://thelawin.dev/), [akretion/factur-x](https://github.com/akretion/factur-x)

## Scoreboard

| # | Niche (wedge) | Primary buyer | WTP low / likely / premium (per mo) | Recurrence of the need | Retention engine | Verdict |
|---|---|---|---|---|---|---|
| 1 | Compliance-grade render API | Lead dev at a 5–50-person B2B SaaS on Stripe/own billing, invoicing German or French businesses | $19 / $49–99 / $249+ | Monthly (every invoice run) | **Strong** | **KEEP. Primary.** Germany first. |
| 2 | Shopify a11y regression guard | Owner or operator of a $20k–$300k/mo US/EU-selling Shopify store with 15+ apps | $29 / $79 / $149–199 (agency $299+) | Continuous (every app or theme change) | **Medium-strong**; value can become invisible | **KEEP. Secondary.** |
| 3 | FR B2C e-reporting aggregator | VAT-registered French multi-channel SME (not micro), led by their accountant | €0 (free PAs) / €19–39 / €79 | Legally recurring (every 10 days to 2 months) | Strong *once installed*, but acquisition is a one-off 2027 spike against free competitors | **DROP as a standalone.** Could be a feature on #1 later. |
| 4 | Shopify "AI recommends my products → revenue" | DTC founder or marketer at a $50k–$1M/mo brand with falling Google traffic | $9.90 / $29–49 / $99–149 | Weekly curiosity, not an obligation | **Weak-medium**: "benchmark, not source of truth" | **DROP / park.** Crowded at $9.90 and Shopify covers attribution natively. |

---

## Niche 1 — Compliance-grade document render API
*(HTML/JSON → PDF/A-3b with embedded Factur-X/ZUGFeRD/XRechnung XML, PDF/UA-1 tagging, and a veraPDF + EN 16931 validation report with every file)*

### 1.1 Primary persona cards

**Persona A: "Jonas, the reluctant e-invoicing owner"** (the main payer)
- **Snapshot:** senior or lead developer (sometimes the CTO) at a 5–50-person B2B SaaS, agency tool or vertical platform. Based in DE/AT/FR/NL, or a non-EU company with German or French business customers. Billing runs on Stripe Billing, Paddle, Chargebee or home-grown code that renders invoice PDFs from HTML templates.
- **Situation:** finance or a big customer has just told him: "our invoices have to be e-invoices." Stripe "does not emit a legally compliant German e-invoice file" and "cannot create or send electronic invoices without an additional app" [wavect.io](https://wavect.io/blog/stripe-billing-e-invoicing-2027/). He has two options: install a marketplace app (Billit), or "build a webhook that maps the Invoice object into EN 16931 XML yourself" [dev.to](https://dev.to/thor_4e886628f33fca7fa8df/stripe-invoice-json-en-16931-bt-germany-2026-cda).
- **Goals:** keep the invoice design he already has; ship one endpoint change; get a file that "actually pass[es] veraPDF and Mustang" [dev.to (GH #11)](https://dev.to/pdfik/factur-x-zugferd-from-html-how-to-embed-en-16931-xml-in-a-pdfa-3-and-actually-pass-verapdf-and-pgn); never think about the topic again.
- **Fears:** a customer's accounts-payable system rejecting invoices, so cash comes in late; an auditor finding invalid XML; owning a standards codebase (schematron updates, profile versions) forever; "working with a million libraries not updated in ages" (GH #1).
- **Where he is:** dev.to, Stack Overflow, GitHub issues on `factur-x`/`mustang`, the Stripe docs and App Marketplace, Hacker News, and German dev communities (heise forums, r/de_EDV). He searches for "ZUGFeRD API", "XRechnung Stripe", "Factur-X node" and "PDF/A-3 embed XML".
- **His words:** "e-invoice", "E-Rechnung", "ZUGFeRD", "EN 16931", "PDF/A-3", "validator says invalid", "Stripe doesn't support it", "we just need a valid file".
- **Budget authority:** can put $50–$200/mo on a company card without approval. Above roughly $300/mo he needs sign-off from finance.

**Persona B: "Priya, the app builder"** (a multiplier channel)
- **Snapshot:** indie or small-team developer who builds invoicing plugins and apps for WooCommerce, Shopify, Shopware, Odoo or Stripe Apps, and sells them to thousands of merchants.
- **Situation:** her merchants keep asking "is your app ready for e-invoicing?" She wants a reliable backend rather than maintaining PDF/A plus XML herself.
- **Goals:** a white-label, per-document API with predictable cost so she can resell at a margin.
- **Fears:** your API going down during *her* merchants' month-end; per-document costs eating her margin; you becoming her competitor.
- **Her words:** "backend", "per document", "rate limits", "SLA", "white-label".

**Secondary buyer, not the wedge: "the accessible-documents requester."** An engineer at a govtech, edtech or healthcare vendor whose public-sector customer needs PDF/UA output for ADA Title II. The deadlines have moved to **Apr 2027** (≥50k population) and **Apr 2028** (smaller), and a May 2026 NFB lawsuit is trying to restore the earlier dates [search summary of DOJ IFR coverage](https://www.siteimprove.com/blog/ada-title-ii-document-accessibility/). A good second segment for the same engine in 2027. Do not lead with it.

### 1.2 Anti-persona: do not serve
- **"Full-stack e-invoicing outsourcer":** an enterprise that wants Peppol/PA transmission, ERP integration, archiving contracts and a named account manager. They generate support load and legal questions. Send them to Billit, Pagero or a PA.
- **Freelancers or small businesses who "just need to send one invoice":** free PAs (Indy, Tiime, Pennylane) and free generators already serve them. They churn in month 1.
- **Remediation of existing PDFs** (governments with 10,000 legacy PDFs to fix). That is a services and OCR problem (PDFix, Equidox), not generation.
- **Anyone expecting you to decide the tax treatment** (VAT categories, reverse charge). You validate structure; they own the tax logic. Put this in the ToS.

### 1.3 JTBD and forces of progress
**JTBD:** *"When a customer, our finance team or a legal deadline tells us our invoices must be e-invoices, I want to turn the invoice HTML/data we already produce into a validated hybrid e-invoice with one API call, so I can ship compliance this sprint and never maintain PDF/XML standards code."*
- Functional: produce a valid file that also looks right to a human reader.
- Emotional: "not my problem any more"; no 2 a.m. validator errors.
- Social: be the engineer who closed the compliance ticket quickly, not the one who delayed invoicing.

| Force | Content |
|---|---|
| **Push** | Hard dates: DE issuing obligation **1 Jan 2027 for sellers >€800k turnover, 1 Jan 2028 for everyone** [wavect](https://wavect.io/blog/stripe-billing-e-invoicing-2027/); FR receive since 1 Sep 2026, issue from Sep 2026 (large) and Sep 2027 (SME). Stripe won't do it natively. Chromium PDFs cannot be PDF/A or PDF/UA (GH #10). |
| **Pull** | One endpoint; keeps the current design; a validation report as proof; PDF/UA included, so the same call covers accessibility requests; MCP tool for agents. |
| **Anxiety** | "Will a tiny vendor be around in 3 years?" "Is my invoice data (PII, amounts) safe?" "Is the file *really* valid for my customer's AP system?" "Will I get locked in?" |
| **Habit** | "Python `factur-x` + WeasyPrint is free. I'll hack it over a weekend." A Stripe App (Billit) is click-to-install. Free XMLBridge exists. |

**Psychology note:** the habit force is strong because the DIY path looks free and bounded ("a focused day or two", GH #14). You win when the validation failures start: 2–10 dev days at €600–800/day (**€1.2k–8k**, estimate) plus ongoing maintenance. Market against the *second* week of DIY, not the first.

### 1.4 Buying triggers (ranked)
1. **A customer's AP department rejects a PDF invoice or demands ZUGFeRD/XRechnung.** Structural; likely the most common trigger in DE from Jan 2027. Evidence: the DE rules make a PDF insufficient from 2027/28 [wavect](https://wavect.io/blog/stripe-billing-e-invoicing-2027/). *(no buyer quote retrieved; interview to confirm)*
2. **"Stripe doesn't do it" moment:** "Stripe Invoicing still does not emit EN 16931 … Stripe's own docs tell you to use a Marketplace app or a webhook." [dev.to](https://dev.to/thor_4e886628f33fca7fa8df/stripe-invoice-json-en-16931-bt-germany-2026-cda)
3. **DIY validation failure:** the title "…how to embed EN 16931 XML in a PDF/A-3 and **actually pass** veraPDF and Mustang" (GH #11) implies earlier attempts that failed. "I decided I was done with hacky fixes" (GH #1).
4. **Deadline calendar:** finance or the accountant sends the Jan-2027 memo in Q4 2026. The window is **now through Q1 2028**.
5. **Serverless Chromium pain** (a general HTML-to-PDF trigger): "PDF generation is the heaviest thing you can ask headless Chrome to do…" (GH #2).
6. **Public-sector procurement asks for accessible (PDF/UA) documents** ahead of the Apr 2027 ADA Title II deadline.

### 1.5 Top objections and how to defuse them
| Objection | Defuse |
|---|---|
| "There's a free API / open-source lib." | Publish a **public validator comparison**: run free outputs through veraPDF + KoSIT/EN 16931 schematron and show the results. Offer "we validate every file, free tier 50 docs/mo". Validation is the product. |
| "You're tiny. What if you vanish?" | Open file formats (no lock-in), an exportable template, a self-host Docker licence as an escape hatch at a premium tier, a public status page and changelog. |
| "Is our invoice data stored?" | Default zero-retention (render and discard), EU region, a DPA template. Optional archive is paid and opt-in. |
| "Is it legally compliant?" | Don't claim "legally compliant". Say "**validated against EN 16931 and PDF/A-3b; report attached**". Tax treatment stays with the customer. |
| "France needs a PA anyway." | True. Position FR as "generate the file; hand it to your PA". **Lead with Germany**, where emailing a valid ZUGFeRD/XRechnung is itself compliant (no clearance platform). |

### 1.6 Willingness to pay
| Tier | Price | Reasoning |
|---|---|---|
| **Low** | **$19/mo** (~500 docs) | PDFShift from $9 (250 credits), DocRaptor $15 for 125 docs; commodity HTML→PDF anchors. Enough for small SaaS monthly invoice runs. |
| **Likely** | **$49–99/mo** (2k–10k docs, validation reports, PDF/UA) | Workaround cost: €1.2k–8k one-off DIY plus maintenance, so ~$100/mo pays back in under a year even before maintenance. Below the ~$300 approval threshold. DocRaptor's mid-tiers sit here and are called "one of the most expensive" (GH #8), so this price looks like good value next to Prince-grade output. |
| **Premium** | **$249–499/mo** (volume, archive, SLA, white-label for Persona B, self-host licence) | App builders reselling to thousands of merchants; DocRaptor plans run up to ~$1k/mo. |

Pricing shape: usage tiers with *document* counts, not credits by MB (avoid PDFShift's "extra credits over 5 MB" complaint, GH #9). Annual plans with 2 months free fit the budget-cycle psychology of B2B devs.

### 1.7 Retention engine: Strong
- **Natural recurrence:** invoices go out every month, indefinitely. This avoids ScreenshotOne's "one project, one month" churn (GH #13).
- **Switching costs:** the endpoint sits in the billing code path. Nobody touches working billing code, which is the strongest retention force in B2B dev tools.
- **Growing value:** standards drift (ZUGFeRD 2.3.x updates, XRechnung versions, the FR format "socle", Peppol BIS). Every schema update you absorb is a month the customer didn't spend on it. Send a changelog email: "**This month we absorbed XRechnung 3.0.x; you changed nothing.**" That makes invisible value visible.
- **Accumulated history (optional add-on):** a searchable archive of validated originals plus validation reports (DE GoBD and 8–10-year retention needs, per wavect). An archive is sticky by nature.
- **Churn risks:** (a) Stripe, Chargebee or Paddle ship native e-invoicing, which is the biggest threat for Persona A on Stripe (mitigate by serving non-Stripe and home-grown billing plus app builders); (b) the customer moves to a full AP/AR suite; (c) DIY once the in-house dev has time.
- **Month 12 = month 1?** Yes. The job ("produce valid invoices every month") never ends, and its difficulty *increases* with each new mandate (PL, BE, ViDA 2030).

### 1.8 Message test (customer's words)
1. "**Stripe won't make your ZUGFeRD invoices. One API call will**, validated against EN 16931, with the report attached."
2. "Your invoice HTML in. **A valid ZUGFeRD / Factur-X PDF/A-3 out.** It passes veraPDF, or we tell you why."
3. "Done with hacky PDF fixes? **E-Rechnung-ready PDFs by Friday**, and we keep up with the standards so you don't."

**Recurring-revenue path: YES, and the best of the four.**

---

## Niche 2 — Shopify accessibility "regression guard"
*(rescans on every app or theme change, attributes violations to their source, suggests code-level fixes, keeps an evidence trail; no overlay, no compliance claims)*

### 2.1 Primary persona cards

**Persona A: "Dana, the burned (or scared) store owner"**
- **Snapshot:** founder or ops lead of a US- and/or EU-selling Shopify store doing $20k–$300k/mo in apparel, beauty, home or food. Team of 2–15. 15–40 apps installed. A premium theme with custom edits by freelancers.
- **Situation:** she has received a demand letter, or a peer in her founder group has. Maybe she installed an overlay and learned that doesn't help: "We had a couple lawsuits with AccessiBe… a temporary solution" (GH #5). Plaintiff firms "specifically searched for the widget's script tag" (GH #11).
- **Goals:** stop being an easy target; know when something breaks; have *proof* she is acting in good faith; avoid a $5k–25k settlement again (typical demand-letter settlements, [accessible.org / wcagsafe via snippet](https://accessible.org/shopify-ada-compliance/)).
- **Fears:** being sued again, as the merchant "sued 3 times over 4 years" was (GH #1); paying for another tool that "doesn't work"; a developer bill she can't estimate; breaking her theme.
- **Where she is:** Shopify Community ("ADA accessibility lawsuits", "Legal Scam re ADA compliance/websites"), r/shopify, r/ecommerce, DTC Facebook groups, the Shopify App Store's accessibility category, podcasts (2X eCommerce, DTC Pod), and her Shopify agency.
- **Her words:** "ADA lawsuit", "demand letter", "shakedown", "scam", "am I compliant?", "my theme said it was accessible" (GH #2), "the widget didn't protect me", "what do I actually need to fix?"

**Persona B: "Marcus, the Shopify agency/freelancer"** (channel and multi-store buyer)
- **Snapshot:** runs a 2–15-person Shopify agency with 10–60 retainer clients.
- **Situation:** clients forward demand letters to him. He can't manually re-audit 40 stores after every app install.
- **Goals:** a multi-store dashboard, white-label reports, and a billable "accessibility care plan" line item ($100–300/client/mo, estimate).
- **Fears:** liability for claiming compliance on a client's behalf.

**EU variant:** an EU-selling merchant who heard about the EAA. The Carrefour France injunction (Jun 2026) and Swedish PTS e-commerce cases [search summary](https://www.levelaccess.com/compliance-overview/european-accessibility-act-eaa/) create fear, but EU enforcement is complaint-then-remedy, so urgency is lower than in the US. Needs an accessibility statement.

### 2.2 Anti-persona: do not serve
- **"Make me compliant / lawsuit-proof with one click."** They want a certificate or badge, will churn when told the truth, and are the FTC trap (accessiBe's $1M order). Filter them out with copy: "we don't sell a badge."
- **Stores mid-litigation** that need an expert witness, a VPAT or a manual audit report. Refer them to a partner auditor.
- **Tiny stores (<$5k/mo)** with a free theme and 3 apps: low risk, low budget, and they churn after the first scan.
- **Headless/Hydrogen enterprise builds:** your theme-patch model doesn't apply.

### 2.3 JTBD and forces
**JTBD:** *"When I get (or hear about) an ADA demand letter, or when I install a new app or push a theme update, I want to know immediately what broke accessibility and exactly how to fix it, with dated proof that I did, so I can stop being an easy target and sleep at night."*
- Functional: detect, attribute, fix, document.
- Emotional: from dread and helplessness ("it's a scam!") to control.
- Social: to their lawyer or insurer, "we have a documented, ongoing program"; to an agency, "we're responsible operators".

| Force | Content |
|---|---|
| **Push** | 3,117 federal suits in 2025 (+27%); Shopify ≈32% of ADA web suits ([accessible.org via snippet](https://accessible.org/shopify-ada-compliance/)); settlements of $5k–25k; plaintiffs use automated crawlers to flag "the same handful of WCAG failures". |
| **Pull** | "Tells me *which app* broke it"; fixes are real code, not a widget; an evidence ledger to hand to counsel; an auto-updated accessibility statement (needed for the EAA). |
| **Anxiety** | "Another tool that doesn't protect me?" "Will patches break my theme?" "Automated scans catch only ~30%" (GH #8). "Will it slow my store?" (no: it's server-side, no storefront JS, which is also a selling point). |
| **Habit** | The overlay already installed ("at least I have *something*"); free WAVE/Lighthouse; "I'll deal with it if I get sued"; "my theme is supposedly accessible". |

### 2.4 Buying triggers (ranked)
1. **Receiving a demand letter or lawsuit.** "receiving demand letters from lawyers quoting thousands of dollars to 'settle'" *(paraphrase, GH #3)*; "The moment you receive legal notice, your website's current state becomes evidence" ([testparty snippet](https://testparty.ai/blog/shopify-ada-lawsuit-defense-step-by-step-framework)). Highest intent, but a reactive buyer. They pay fast and are the most likely to churn after 3 months unless you show ongoing value.
2. **Being sued *despite* an overlay:** "We had a couple lawsuits with AccessiBe…" (GH #5); Bloomsybox sued "about six months" after subscribing to UserWay (GH #6). These buyers switch from an overlay.
3. **A peer being sued** (vicarious trigger) in a founder group, a podcast or the Shopify Community thread "ADA accessibility lawsuits". Proactive, calmer, better long-term customers.
4. **Theme redesign or migration** (new OS 2.0 theme, replatform). "Relying on a Shopify theme that was represented as accessible" *(paraphrase, GH #2)*.
5. **App update or new install silently breaks things.** "store owners often discover problems only after receiving an ADA demand letter" (GH #9); "an update might change the app's code in a way that breaks keyboard navigation" *(paraphrase, [testparty](https://testparty.ai/blog/third-party-shopify-apps-accessibility-risks))*.
6. **EU expansion or an EAA news spike** (Carrefour injunction, Jun 2026).
7. **Insurance or legal review:** a cyber/EPLI insurer questionnaire, or counsel recommending "documentation". *(hypothesis; interview to confirm)*

### 2.5 Top objections and defusals
| Objection | Defuse |
|---|---|
| "Does this make me compliant / stop lawsuits?" | Honest answer, and a differentiator: "No tool can. We find and fix what automation can find (~30–40%), catch regressions the day they happen, and give you a dated record of good-faith effort. For the rest, here's a partner manual audit." Honesty is the trust moat after overlays. |
| "I already pay for an overlay." | "Plaintiff firms search for widget script tags" (GH #11). Offer a guided overlay-removal checklist. |
| "Will it break my theme?" | Fixes are **diffs you approve**, applied to a duplicate theme first, with one-click rollback. |
| "I can't read code." | Plain-English issue cards ("Your search icon has no label: screen readers say 'button'"), a one-click fix for common patterns (alt text, labels) and "send to my developer" export. |
| "$79/mo is a lot." | "One demand letter settles for $5k–25k. Remediation projects cost $4k–30k (GH #13). That's 5+ years of the plan." |

### 2.6 Willingness to pay
| Tier | Price | Reasoning |
|---|---|---|
| **Low** | **$29/mo** (single store, weekly scan plus on-change scan, statement) | At the top of the overlay/app band ($12–49); an easy impulse install. |
| **Likely** | **$79/mo** (daily and on-change scans, app attribution, fix suggestions, evidence ledger) | Cost of the problem: $5k–25k settlement plus lawyer fees. Cost of the workaround: ad-hoc developer at $75–150/hr × 5–20 hrs after each incident. Leaves a wide gap below TestParty at $599+. |
| **Premium** | **$149–199/mo** (full-site crawl, PDF evidence pack, priority, EAA statement in several languages); **agency $299+/mo for 10 stores** | Recently sued merchants and agencies reselling a care plan. |

### 2.7 Retention engine: Medium-strong
- **Natural recurrence:** real. Stores install or update apps and edit themes constantly, and plaintiff crawlers never stop. The risk never goes away, which fits a "guard" subscription.
- **Accumulated history:** the dated evidence ledger becomes more valuable every month ("24 months of monitoring records"). Cancelling means losing your paper trail. This is the key switching cost. Make the ledger exportable only as a full PDF on paid plans, but never hold data hostage.
- **Churn risks (retention psychology):**
  - **"Invisible value":** when nothing breaks, the owner sees no value. Fix: a monthly **"What changed on your store"** email (e.g. "3 apps updated, 1 introduced 4 new issues, fixed in 1 click; you've had 0 unresolved critical issues for 97 days").
  - **"Job done" after the backlog is fixed:** reposition from "fix list" to "guard". Put the on-change scan front and centre.
  - **Post-lawsuit relief:** reactive buyers relax after settlement. Lock in with an annual plan offered at the moment of highest fear (the first dashboard after a letter).
- **Month 12 vs month 1:** month 1 is the backlog fix (big visible value). By month 12 the value comes from regressions caught plus the evidence file. Weaker emotionally, stronger defensively. Expect monthly churn of ~3–5% (estimate) unless the "changed" digest works.

### 2.8 Message test
1. "**A new app just broke your store's accessibility. We'll tell you which one**, and give you the fix, before a demand letter does."
2. "**Got an ADA demand letter?** Stop guessing. See exactly what's broken, fix it in your theme code, and keep a dated record of every fix."
3. "**No widget. No 'compliance' badge.** Real code fixes and a paper trail for your Shopify store."

**Recurring-revenue path: YES.** The risk is permanent and the store keeps changing. Retention depends on making "nothing happened" feel like value.

---

## Niche 3 — French B2C e-reporting aggregator for multi-channel sellers (via a partner PA)

### 3.1 What the new evidence changes
- **Micro-entrepreneurs:** e-reporting is a **summary every two months** ("this month, I sold X euros to individuals", *paraphrase*), starts 1 Sep 2027, and carries €250 per missing transmission (capped at €15k/yr). **13 PAs offer free plans** to TPEs and auto-entrepreneurs, and "pour un auto-entrepreneur aux besoins simples, la conformité à la réforme ne coûte donc rien" (for a self-employed person with simple needs, compliance costs nothing). [search summary citing quelle-pa.fr, tiime, portail-autoentrepreneur](https://www.quelle-pa.fr/articles/guides/meilleure-pa-auto-entrepreneur)
- **Marketplace sales:** some flows are carried by the marketplace under "deemed supplier" rules (IOSS/EU cases), which reduces what a seller must aggregate from Amazon or Etsy in some cases. [hr-associes](https://www.hr-associes.fr/blog/facturation-electronique-e-commerce-e-invoicing-e-reporting), [ma-facture-electronique](https://ma-facture-electronique.org/cas-dusage/marketplaces/)
- **Conclusion:** the long tail of micro sellers is **not a paying market**. Only **VAT-registered SMEs with real multi-channel complexity** remain, and their accountant (expert-comptable) usually decides for them.

### 3.2 Primary persona (the one remaining payer)
**"Claire, the multi-channel SME e-merchant"**
- **Snapshot:** French SARL/SAS, €300k–€5M turnover, VAT-registered (not franchise en base), selling on Shopify or WooCommerce/PrestaShop plus Amazon plus 1–2 others (Etsy, Cdiscount, ManoMano), and taking payments through Stripe, PayPal and Alma. 3–20 staff; no in-house accountant.
- **Situation:** her expert-comptable says: "à partir de septembre 2027 il faudra transmettre l'e-reporting via une PA; il me faut des données propres par canal et par taux de TVA" (from September 2027 you'll have to submit e-reporting through a PA; I need clean data by channel and VAT rate). *(Illustrative, not a retrieved quote.)* Consultants say plainly that "your approved platform will need to aggregate orders from Shopify, Stripe, PayPal…" (GH #6), and "Many Shopify e-merchants think the platform will 'manage' e-reporting automatically, but this is false." (GH #4)
- **Goals:** not have to think about it; have her accountant stop chasing exports; no fines.
- **Fears:** fines, tax audits ("exposing merchants to tax assessments during audits", GH #8), a tool that doesn't match her accountant's figures.
- **Her words:** "e-reporting", "plateforme agréée", "PA", "facturation électronique 2027", "mon expert-comptable", "export des ventes", "rapprochement Stripe/PayPal" (Stripe/PayPal reconciliation).
- **Where she is:** French Shopify Community threads, Facebook groups for e-merchants, her accountant, and the PA comparison sites that are ranking now (quelle-pa.fr, comparatif-facture-electronique.fr).

### 3.3 Anti-persona
- **Every micro-entrepreneur with simple needs** (Etsy, Vinted, crafts). The free PAs already serve them; they will not pay €19/mo.
- **Large merchants with ERPs** (Cegid, SAP), whose PA is chosen by their DAF (finance director).
- **Merchants wanting tax advice.** Liability and support load, in French.

### 3.4 JTBD and forces
**JTBD:** *"When my accountant tells me e-reporting starts in September 2027, I want all my sales channels to flow into the approved platform automatically and reconciled, so I stay compliant without spending month-end on exports."*

| Force | Content |
|---|---|
| Push | A legal deadline (Sep 2027), fines, and multi-source data mess. |
| Pull | Aggregation across channels, reconciliation by VAT rate, "your accountant sees it too". |
| Anxiety | "Will this match my accountant's figures?" "Is a foreign, non-French-speaking vendor trustworthy for tax data?" "What if the PA partner changes its API or pricing?" |
| Habit (very strong) | **Their accountant's own PA or suite (Pennylane, Tiime) does it for free or as part of an existing fee**; the Shopify-native apps (Agrafe, Regulo) exist; "I'll wait until 2027". |

### 3.5 Buying triggers
1. The accountant's memo or engagement-letter update (H1 2027). *Structural; accountant-led.*
2. The Sep 2027 deadline window (Jun–Oct 2027 spike).
3. The first PA onboarding failure: "Shopify by default does not produce this data in the correct format" (GH #5).
4. A switch to a new PA or accountant.
*Few first-person trigger quotes were retrieved; buyer anger isn't visible yet (as gap-hunter also noted).*

### 3.6 Top objections
- "My accountant/PA already does it for free." **This objection is decisive, and there is no good answer for most sellers.**
- "You're not French and you don't have a PA." Only partially fixable (partner branding, French UI and docs).
- "What if the numbers are wrong?" Liability. Needs an accountant-review workflow.

### 3.7 Willingness to pay
| Tier | Price | Reasoning |
|---|---|---|
| Low | **€0** | 13 free PAs; accountants bundle it. |
| Likely | **€19–39/mo** | Agrafe $22/$57 (Shopify-only) anchors the price; only multi-channel SMEs with ≥3 sources pay. |
| Premium | **€79/mo+** (or accountant multi-client plans) | Accountant firms handling 50+ e-merchant clients. That is a sales-led channel that needs French-language relationship selling, which is **not passive**. |

### 3.8 Retention engine: strong in theory, weak in practice
- The need is legally recurring (every reporting period, for ever). Once connected, switching is annoying. So retention of *installed* customers would be good.
- **But:** (a) acquisition is a **one-time wave** around Sep 2027; (b) free PAs and accounting suites keep adding connectors (Pennylane already has Shopify/Stripe connectors, *assumption to verify*), so the job is gradually absorbed into free bundles; (c) it depends on a PA partner's API and margin; (d) French-language support is required for a tax-sensitive product.
- **Month 12 = month 1?** Functionally yes. Commercially, the product becomes a commodity feature.

### 3.9 Message test (FR, for completeness)
1. "Shopify + Amazon + Stripe + PayPal : **votre e-reporting 2027 envoyé automatiquement** à votre plateforme agréée." ("Shopify + Amazon + Stripe + PayPal: your 2027 e-reporting sent automatically to your approved platform.")
2. "Votre expert-comptable vous réclame vos exports ? **Toutes vos ventes, rapprochées par taux de TVA, chaque mois.**" ("Is your accountant chasing you for exports? All your sales, reconciled by VAT rate, every month.")
3. "Non, Shopify ne fera pas votre e-reporting. **Nous, si.**" ("No, Shopify won't do your e-reporting. We will.")

**Recurring-revenue path: technically yes (legal recurrence). Poor fit for this founder: free incumbents, a one-off acquisition spike, a language barrier and partner dependency. Verdict: DROP as a standalone.** Keep it as a possible 2027 add-on to Niche 1: sell the rendering or validation layer *to* the PAs and app builders serving these merchants, rather than to the merchants directly.

---

## Niche 4 — Shopify app: which products AI assistants recommend, tied to AI-referral revenue

### 4.1 Primary persona
**"Marco, the DTC growth lead watching Google fade"**
- **Snapshot:** founder or head of growth at a Shopify DTC brand doing $50k–$1M/mo (supplements, skincare, outdoor gear, pet). Organic Google traffic down 15–40% YoY. He sees `chatgpt.com` in his referrers.
- **Situation:** his board, investors or co-founder asks "what's our AI strategy?" As a CMO put it, "Every CMO we know has been under excruciating pressure… to crack this nut" (GH #4). He read Shopify's data: AI sessions up 8x, orders up ~13x YoY, and AI-referred shoppers convert ~50% better [shopify.com enterprise blog via snippet](https://www.shopify.com/enterprise/blog/ai-search-insights).
- **Goals:** know whether ChatGPT or Perplexity recommend *his* products for "best X for Y", what they say, which competitors win, and whether it makes money.
- **Fears:** paying for a dashboard of noise: "If you use three different tools and give them the same prompts, you get three different answers" (GH #1); tools where "the referral traffic they promised still rounds to zero" (GH #14).
- **His words:** "show up in ChatGPT", "AI traffic", "AEO/GEO", "is ChatGPT recommending us?", "share of voice", "what's the ROI?"

### 4.2 Anti-persona
- **Stores under ~$20k/mo:** AI referral volume is statistically tiny. Datma's panel had ChatGPT at **0.11% of sessions** in Aug 2025 ([Shopify Community thread via snippet](https://community.shopify.com/t/emerging-traffic-source-chatgpt-sessions-across-shopify-stores/574374)). They see nothing and churn in month 2.
- **"Get me ranked in ChatGPT" buyers** expecting guaranteed placement. Expectation mismatch.
- **Agencies wanting white-label:** the copycat trap (GH §1.6).

### 4.3 JTBD and forces
**JTBD:** *"When my Google traffic drops and everyone says shoppers now ask ChatGPT, I want to see whether AI assistants recommend my products and whether that turns into orders, so I can decide what to fix and justify the effort to my team."*

| Force | Content |
|---|---|
| Push | Falling organic traffic; board pressure; AI orders up 13x. |
| Pull | Product-level visibility plus revenue in one Shopify screen. |
| Anxiety | Noisy, non-deterministic results ("benchmark, not source of truth", GH #3); another metric with no action attached. |
| Habit (strong and cheap) | **Shopify Admin already shows ChatGPT-referred orders**; GA4 referral reports; **Visibly AI at $9.90/mo**, TrackGPT, ShelfSight, Mento, IndexGPT, Finseo; manually asking ChatGPT. |

### 4.4 Buying triggers
1. **Noticing `chatgpt.com` in referrers or an order attributed to ChatGPT** ("Is ChatGPT sending you Shopify traffic? Here's how to check" is a whole article genre: [get-ryze](https://www.get-ryze.ai/blog/how-to-know-if-chatgpt-is-sending-you-shopify-traffic)).
2. **Google organic drop** in the monthly report.
3. **Board or investor question** (GH #4).
4. **Shopify's own data announcements** ([ecomrevolution](https://ecomrevolution.substack.com/p/shopify-report-ai-traffic)).
5. **A competitor appearing in a ChatGPT answer** the founder tried himself.

### 4.5 Top objections
- "Shopify already shows me ChatGPT orders." Hard to defuse. The only answer is *why* (which prompts, which product, which competitor) plus *what to change*.
- "The results change every time." Show confidence bands and repeated sampling (an honest differentiator, but ShelfSight already markets "no invented percentages").
- "Visibly is $9.90." A price war you can't win on features that are easy to copy.

### 4.6 Willingness to pay
| Tier | Price | Reasoning |
|---|---|---|
| Low | **$9.90/mo** | Visibly AI Pro anchors the Shopify App Store price at the bottom. |
| Likely | **$29–49/mo** | Otterly $29 (brand-level); many merchants treat this as an "experiment" budget. |
| Premium | **$99–149/mo** | $250k+/mo brands with meaningful AI revenue. LLM query costs rise with prompts × engines × samples, so margin is squeezed at the low end. |

### 4.7 Retention engine: Weak-medium
- **Recurrence:** the results change weekly, so there is a *curiosity* loop, but no obligation (unlike a mandate) and no accumulated switching cost beyond trend history.
- **Churn drivers:** "job done" once the owner has seen the picture; invisible or zero ROI for most stores ("rounds to zero"); skepticism about noise; Shopify, Semrush and Ahrefs bundling; LLM API costs.
- **Month 12 = month 1?** Only if the tool drives *actions* (product-copy or feed fixes) with measured uplift. That is a bigger, riskier product in a market with 6+ direct Shopify competitors.

### 4.8 Message test
1. "**Is ChatGPT recommending your products or your competitor's?** See it by product, weekly, with the orders it drove."
2. "Your Google traffic is down. **Find out what ChatGPT says when shoppers ask for 'the best [your category]'.**"
3. "Stop screenshotting ChatGPT answers. **Real prompts, sampled daily, and the revenue behind them.**"

**Recurring-revenue path: weak.** It is subscription-shaped, but the need isn't mandatory, value is invisible for most stores, competitors start at $9.90 and Shopify covers the attribution half natively. **Verdict: DROP or park.** If kept at all, make it a free lead-magnet feature, not the business.

---

## Cross-niche psychology

- **The two keepers share one buyer truth: "prove it to a third party."** For Jonas, the third party is the customer's AP system and the auditor; for Dana, it is the plaintiff's lawyer and the regulator. Both products should output **a validation or evidence artefact** with every run, not just "done". That artefact is both the retention hook and the trust signal after the overlay and "compliance-illusion" era.
- **Honesty as positioning:** both categories were poisoned by overclaiming (overlays; "fully compliant PDF" vendors). "Validated, report attached; here is what we *don't* cover" converts skeptical technical buyers.
- **Mandates beat curiosity.** Niches 1–2 are pushed by deadlines and lawsuits (push force > habit). Niche 4 depends on curiosity and loses to cheap habit. Niche 3 is mandate-pushed, but the habit alternative is *free*.

## Validation next step: switch-interview script (8–10 per kept niche)
1. "Take me back to the day you first realised you needed [e-invoices / to deal with accessibility]. What happened? Who told you?"
2. "What did you try first? What did it cost in time and money? Why wasn't it enough?"
3. "What almost stopped you from paying for a tool?"
4. "What would make you cancel after 6 months?"
5. "If this disappeared tomorrow, what would you do instead?"
Recruit Niche 1 interviewees from GitHub issue authors on `akretion/factur-x`, `ZUGFeRD/mustangproject` and dev.to authors of Factur-X posts. Recruit Niche 2 interviewees from Shopify Community ADA-thread posters and Shopify agencies.

## Sources (new in this stage; others via 02-gap-hunter.md)
- https://wavect.io/blog/stripe-billing-e-invoicing-2027/
- https://dev.to/thor_4e886628f33fca7fa8df/stripe-invoice-json-en-16931-bt-germany-2026-cda
- https://dev.to/ismail183/generate-a-factur-x-pdf-invoice-in-python-free-no-library-needed-2n3o
- https://apify.com/josefbednar/factur-x-zugferd-e-invoice
- https://thelawin.dev/
- https://github.com/akretion/factur-x
- https://www.siteimprove.com/blog/ada-title-ii-document-accessibility/
- https://accessible.org/shopify-ada-compliance/
- https://testparty.ai/blog/shopify-ada-lawsuit-defense-step-by-step-framework
- https://testparty.ai/blog/third-party-shopify-apps-accessibility-risks
- https://www.levelaccess.com/compliance-overview/european-accessibility-act-eaa/
- https://blog.tiime.fr/plateforme-agreee-facturation-electronique-gratuite
- https://www.quelle-pa.fr/articles/guides/meilleure-pa-auto-entrepreneur
- https://www.hr-associes.fr/blog/facturation-electronique-e-commerce-e-invoicing-e-reporting
- https://ma-facture-electronique.org/cas-dusage/marketplaces/
- https://apps.shopify.com/visibly-ai
- https://gettrackgpt.com/shopify
- https://apps.shopify.com/shelfsight
- https://www.finseo.ai/chatgpt-shopping-visibility
- https://www.shopify.com/enterprise/blog/ai-search-insights
- https://community.shopify.com/t/emerging-traffic-source-chatgpt-sessions-across-shopify-stores/574374
- https://www.get-ryze.ai/blog/how-to-know-if-chatgpt-is-sending-you-shopify-traffic
