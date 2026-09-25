# PMF Council: AI Agents for Finding a Passive-Income Niche

A team of 8 Claude Code subagents (7 specialists and a chair). Together they find a niche
product or service that:

- **people are actively searching for**,
- **people are already paying for**,
- has a **real gap** in what current competitors offer (so you customize rather than copy), and
- can become a **recurring-revenue machine** that runs mostly hands-off after the initial sweat equity.

## The council

| # | Agent | Persona | What they answer | Core skills |
|---|---|---|---|---|
| 1 | [`demand-scout`](.claude/agents/demand-scout.md) | Maya, Demand Scout | *Where are people searching AND paying right now?* | Keyword and trend signals, marketplace ranks, ad libraries, revenue proof, niching down |
| 2 | [`gap-hunter`](.claude/agents/gap-hunter.md) | Dev, Competitive Gap Hunter | *What are current providers failing to deliver?* | Competitor teardowns, 2–3★ review mining, Reddit/forum voice of customer, gap taxonomy, workaround detection |
| 3 | [`customer-psychologist`](.claude/agents/customer-psychologist.md) | Dr. Lena, Customer Psychologist | *Who exactly buys, why now, how much will they pay, and why would they stay?* | Jobs-to-be-Done, ICP and personas, buying triggers, willingness to pay, retention psychology |
| 4 | [`offer-architect`](.claude/agents/offer-architect.md) | Marcus, Offer & Pricing Architect | *What differentiated offer and recurring pricing fills the gap?* | Offer design, "only" positioning, subscription and retainer models, tiered pricing, unit economics (LTV, CAC, payback) |
| 5 | [`ops-automation-engineer`](.claude/agents/ops-automation-engineer.md) | Priya, Passive-Income Ops Engineer | *Can this truly run on fewer than 5–10 hrs/week? How?* | Value-chain mapping, no-code and AI automation, delegation and SOPs, suppliers and fulfillment, build vs. run costs, Passivity Score |
| 6 | [`growth-strategist`](.claude/agents/growth-strategist.md) | Jordan, Compounding Growth Strategist | *How do we get the first 10, 100 and 1,000 customers without being on a treadmill?* | Channel fit, SEO and programmatic content, marketplace leverage, referrals and partnerships, CAC by channel |
| 7 | [`risk-auditor`](.claude/agents/risk-auditor.md) | Victor, Devil's Advocate | *What kills this? How do we test it cheaply first?* | Saturation and AI-disruption checks, platform and legal risk, stress-tested economics, pre-mortem, kill criteria |
| 8 | [`pmf-chair`](.claude/agents/pmf-chair.md) | Helen, Council Chair | *Which one do we pick, and what do we do in the next 90 days?* | Synthesis, weighted scoring, resolving disagreements, a 90-day plan, the path to passive |

## How it flows

```
brief.md ──► 1 Demand Scout ──► top 5 niches
                                  │
                   2 Gap Hunter ──┴─► 3 Customer Psychologist
                                  │
      4 Offer Architect ◄─────────┼─────────► 6 Growth Strategist
                                  │
                    5 Ops/Automation Engineer
                                  │
                         7 Risk Auditor (red team)
                                  │
                        8 Chair ─► decision memo + 90-day plan
```

Every agent writes a report to `pmf/runs/<run-id>/`, and later agents build on earlier reports.
Everything must be backed by sources: URLs, verbatim customer quotes, and live competitor prices.

## How to use it

1. Fill in [`pmf/brief.md`](pmf/brief.md) with your budget, time, skills, markets and exclusions.
   The defaults work, but your unfair advantages make the results much better.
2. In Claude Code, from this repo, run:
   ```
   /pmf-hunt
   ```
   or with a focus:
   ```
   /pmf-hunt B2B productized services for local businesses
   ```
3. Read `pmf/runs/<run-id>/00-decision-memo.md` first, then dig into the specialist reports.

You can also call any agent on its own, e.g. *"use the gap-hunter agent on the niche 'bookkeeping for Airbnb hosts'"*.

## How winners are chosen

See [`pmf/scorecard.md`](pmf/scorecard.md). It's weighted toward recurring revenue, passivity and
gap strength rather than raw market size. It also has hard gates: a RED risk verdict, a Passivity Score below 4, or no
differentiated offer knocks a niche out.
