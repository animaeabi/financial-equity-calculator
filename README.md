# Financial Equity Calculator

A research-backed calculator that answers one question with intellectual honesty:

> **Should I buy this house, or invest the same money in the stock market?**

**[→ Open the calculator](https://animaeabi.github.io/financial-equity-calculator/)**

*Tagline: Buy vs. Invest, honestly.*

## What this is

The popular FinTwit answer ("renting + investing always wins") is structurally wrong, and the popular parent-generation answer ("renting is throwing money away") is structurally wrong too. The honest answer is a **two-dimensional regime** — buy and invest each dominate inside well-defined zones of `(length-of-stay × price-to-rent ratio)`, with discipline and state tax as strong modifiers.

This calculator surfaces that regime visually instead of collapsing it to a single dollar number.

## What's inside

- **Buyer vs. Renter setup** — the first screen is split into two plain paths. Buyer owns the home; Renter owns stocks. The renter's starting portfolio is automatically matched to the buyer's day-1 cash.
- **2D regime map** as the verdict's headline. The map is the answer; the dollar number is the caption. A brand-tinted "YOU" dot shows where your specific scenario sits relative to the buy/invest crossover.
- **Crossover sentence** — the single most useful line: *"Buy takes over at year 9 — your 5-year stay is 4 years short."*
- **Trajectory chart** with explicit endpoint captions — *"Started with $80k down + closing → $312k home equity at year 10"* vs *"Started with the same $80k in stocks + $620/mo → $234k portfolio after tax."* Same starting capital, two paths, side by side.
- **Discipline archetype** — three Tier-1 cards (Automated / Mixed / Spender) replace the silent default everyone gets wrong. Based on Goodman-Mayer 2018 + Dalbar QAIB realized-investor-return data.
- **Explicit monthly contribution override** — if "rent − own × discipline" doesn't match how you actually budget, set a flat `$/mo to stocks` directly. The simulator uses your number instead.
- **Flip-bar** — two click-to-preview counterfactuals show exactly what would flip your verdict.
- **Simple vs Advanced** — Simple mode strips the verdict down to map + headline + footnote and hides all the levers; Advanced surfaces the drawer, sensitivity strip, stress test, and full assumptions accordion.
- **Refusal-to-verdict** — for sub-3-year stays, DTI > 43%, or thin post-purchase reserves, the model redirects instead of producing a confident dollar number.
- **Monte Carlo** at N=2,000 trials with log-normal sampling, t-copula tail dependence, single-home idiosyncratic σ, and a jump-diffusion stock tail.
- **Tax model**: §121 with partial-exclusion gates, IRC §163(h)(3) avg-balance MID, SALT cap with state-income-tax stacking, §1250 unrecapture for converted rentals, AMT-lite for post-2026 sunset, tax-advantaged-account shelter on the renter side.

## Methodology, in one paragraph

The decision-relevant comparison is *buy* vs *rent + invest the difference*, never *buy vs free housing*. The owner's "cost" excludes mortgage principal (it's savings, not consumption — Himmelberg-Mayer-Sinai 2005 user cost). The investor's portfolio includes a tax-advantaged-account shelter and pays LTCG + NIIT + state at exit. The buyer's exit subtracts selling costs, §121-eligible gain, and depreciation recapture if converted. Headline number equals the detailed exit number — no bait and switch.

Default scenario at honest assumptions: MC P(buy wins) ≈ 50%, median delta ≈ $0 — a genuine coin flip at the median, matching Beracha-Johnson 2012 empirical for 8-year rolling windows.

## How to run

Open `index.html` in any modern browser. No build, no server, no accounts, no lead capture, no analytics. One Google Fonts call (Inter); all math runs client-side. State persists via URL query string (shareable). Theme and Simple/Advanced mode persist via `localStorage`.

## What it is not

- Not financial advice. Estimates only. Talk to a fee-only fiduciary planner and a CPA before a decision this size.
- Not a forecast. The Monte Carlo bands are wide for a reason — the future is uncertain and the model says so.
- Not exhaustive. Doesn't see your job stability, partner alignment, climate-risk repricing, or whether the rental stock in your school district actually exists.

## Key sources

- Beracha & Johnson (2012), *Real Estate Economics* 40(2) — empirical buy-vs-rent across 8 US metros, 1978-2009
- Himmelberg, Mayer & Sinai (2005), *JEP* 19(4) — user cost framework
- Flavin & Yamashita (2002), *AER* — leverage and housing equity beta
- Goetzmann (1993), Goetzmann & Spiegel (1995) — single-home idiosyncratic risk
- Goodman & Mayer (2018), *JEP* 32(1) — forced savings, realized renter discipline
- Poterba (1984), *QJE* 99(4) — tax subsidies to owner-occupied housing
- Cocco, Campbell & Maenhout (2005), *RFS* — life-cycle portfolio choice with housing
- Sinai & Souleles (2005), *QJE* — owner-occupied housing as a rent-risk hedge
- Dalbar QAIB (annual) — realized investor returns vs index
- IRC §121, §163(h)(3), §1250, §1411 — primary tax authority

## License

MIT — see `LICENSE`.
