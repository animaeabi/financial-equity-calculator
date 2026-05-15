# CLAUDE.md

Agent orientation for the Financial Equity Calculator. Read this first.

## What this repo is

A single-file static web app (`index.html`, ~4500 lines) that compares buying a home vs. renting + investing the difference. The whole app — HTML, CSS, JS — lives in that one file. There is no build step, no server, no test suite, no package manager.

Deployed via GitHub Pages at https://animaeabi.github.io/financial-equity-calculator/. Pushing to `main` rebuilds Pages automatically.

## File layout inside `index.html`

| Line range (approx.) | Contents |
| --- | --- |
| 1–40 | `<head>`, theme/mode boot script (must run before paint) |
| 40–1300 | CSS: design tokens → components → verdict styles → archetype → hierarchy polish → mobile polish (480/400/360px) |
| 1210–2080 | HTML body: header → intro → tier-1 inputs → disclaimer/banners → verdict region → meta grid → CTA → P/R gauge → sensitivity → drawer → charts → assumptions accordion → footer |
| 2130–2200 | `DEFAULTS` object — the canonical state schema. Adding a key here auto-enables URL persistence. |
| 2200–2700 | `simulate(inp)` — deterministic year-by-year sim. Returns `{ yearly[], finalDelta, upfront, breakEven, ... }`. All math in **real (today's) dollars**. |
| 2750–2950 | Monte Carlo: `simulateMonteCarlo()`, log-normal sampling, t-copula, jump-diffusion stock tail, idiosyncratic σ. |
| 2970+ | `state` + URL persistence (`loadFromURL`, `syncURL`). Any key in `DEFAULTS` round-trips through the URL automatically. |
| 3000–3700 | Render functions: `render()` orchestrates, plus `renderVerdict`, `renderSensitivity`, `renderAffordability`, `renderRegimeMap`, etc. |
| 3700–3900 | `renderChart()` — the trajectory SVG. Also writes the endpoint caption into `#chart-endpoints`. |
| 4000+ | Init / event wiring (`bindNumber`, `bindSlider`, `bindChips`, archetype cards, state preset, theme/mode toggles). |

## Conventions and gotchas

### Money is real, not nominal
The whole simulator runs in **real (inflation-adjusted) dollars**. Nominal mortgage cash flows are computed internally then deflated by the year midpoint (search for `deflator = Math.pow(1 + inflAnnual, year - 0.5)`). Don't introduce nominal-vs-real ambiguity in user-facing copy unless explicitly working with the nominal display toggle.

### Methodology is opinionated
The model uses Himmelberg-Mayer-Sinai 2005 *user cost*: owner mortgage **principal is savings, not consumption**. So the renter's "gap to invest" is `userCost - rent`, not `total housing payment - rent`. If you change this, you also change the empirical calibration — flag it loudly.

### Renter monthly contribution: two paths
1. Auto (default): `monthlyGap × disciplineRate` where `monthlyGap = max(0, userCost - rent)`. Discipline comes from the archetype cards (Automated=90, Mixed=50, Spender=20).
2. Override: if `state.renterMonthlyContribution > 0`, the simulator uses that flat real $/mo instead, bypassing gap × discipline.

Look at `index.html` around the `monthlyDiffReal` assignment to see the branch.

### Versioned visual layers
The verdict region has historical layers: `.verdict` (v3, hidden via CSS), `.verdict-v5`, `.v6-card`. Some legacy IDs are kept on hidden `<span>` elements at the bottom of the verdict section because old render functions still reference them — don't delete those without auditing `render()`.

### Simple vs Advanced mode
`body.simple-mode` is the default. It hides several **sections** (`#drawer`, `#pr-gauge-card`, `#sensitivity-card`, `#stress-test-card`, `#assumptions-acc`, `footer.method`) **and** several **internal verdict pieces** (`.v5-pills`, `.v5-battle`, `.v6-flip-bar`, `.verdict-meta-detail`). When adding new content, decide intentionally whether it shows in Simple, Advanced, or both. CSS rules for this live in the "Hierarchy polish" block.

### Mobile breakpoints
Layered media queries at 480 / 400 / 360 px in the "Mobile polish" block. Plus existing breakpoints at 540, 640, 720, 760, 1120. When tightening for mobile, **layer breakpoints** (don't fight inheritance) and keep tap targets ≥ 40px high.

### URL state
Any key added to `DEFAULTS` is auto-persisted to the URL on change and auto-restored on load. No need to touch `syncURL` / `loadFromURL` for new fields, but the value must be a primitive (number / boolean / string).

## How to add a new input (recipe)

1. Add `keyName: defaultValue` to `DEFAULTS`.
2. Add the HTML field (input / select / chips).
3. Wire it in the init function. For numeric inputs, the simplest path is `bindNumber('elementId', 'keyName')`. For inputs where "empty" must mean "off", write a custom listener (see `renterMonthlyContribution` for the pattern).
4. Read `inp.keyName` in `simulate()` and use it.
5. (Optional) Mirror to a display element with `id="keyName-display"` for live readouts.

## Working on this repo

- **No build, no tests.** Verify changes by opening `index.html` in a browser (or use `python3 -m http.server` from the repo root).
- **Don't add a build step or dependencies without asking.** The single-file constraint is intentional: shareable, auditable, no supply chain.
- **The README is user-facing.** Update it when adding visible features. This file (CLAUDE.md) is agent-facing — update it when architecture or conventions change.
- **Branch convention**: `claude/<short-description>`. Squash-merge into `main` via PR.
- **GitHub MCP only**: this repo is scoped to `animaeabi/financial-equity-calculator`. No `gh` CLI available — use the GitHub MCP tools for PRs, comments, merges.

## Recent change log (high-leverage context)

- **Mobile polish (#1)**: layered breakpoints at 480/400/360 — header, regime map, verdict cards, drawer tabs, chart text.
- **Verdict hierarchy restructure (#1)**: added "Your answer" section title, promoted the trajectory chart with explicit endpoint captions, removed redundant `.v5-hedge` band, made Simple mode actually simplify the verdict (hides pills/battle/flip/meta inside it), promoted the archetype picker into a brand-bordered callout.
- **Renter override (#2)**: explicit `$/mo to stocks` field inside the archetype callout. Overrides the gap × discipline auto-calc. Endpoint caption reports which method is in use.

When these get out of date, prune or rewrite — don't let it become a changelog.
