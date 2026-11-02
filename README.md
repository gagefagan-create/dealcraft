# Dealcraft ◈

**An acquisition strategy lab that connects finance, operations, and product decisions.**

[Open the hosted demo](https://dealcraft-lab.gagefagan24.chatgpt.site) (ChatGPT sign-in may be required).

A purchase multiple does not make an investment thesis. Dealcraft lets you underwrite a fictional vertical-software acquisition, change the operating plan, and see how customer behavior flows through earnings, cash, debt, and equity returns.

## What you can do

- Adjust 11 transaction, operating, and product assumptions with instant recalculation.
- Compare base, upside, and downside cases; save up to eight browser-local scenarios.
- Inspect a five-year operating model and debt schedule.
- Explore a 5×5 growth / exit-multiple IRR sensitivity grid.
- Run 1,000 reproducible Monte Carlo trials with percentile and hurdle-rate results.
- Export an investment committee memo as Markdown and the model as CSV.
- Read every material formula and limitation in the app.

## Run locally

No dependencies or build step. Requires Python 3 to serve ES modules, and Node 22+ for tests.

```sh
python3 -m http.server 8080 --directory dist
# Open http://localhost:8080
node --test
```

## Architecture

```text
dist/model.js  Pure financial engine, input bounds, seeded simulation
dist/app.js    UI, SVG chart, sensitivity grid, exports, local storage
dist/style.css Responsive visual system
test/         Node built-in test runner: accounting and financial invariants
```

The deterministic engine has no browser dependencies. The UI is a static ES module application; there is no backend, external financial-data dependency, or API key. Scenarios remain in localStorage. Google Fonts is the only external presentation resource, with system fallbacks.

### Financial conventions

All financial amounts are USD millions. Entry revenue is 100 and EBITDA is 24. Entry EV = 24 × entry multiple; entry debt = 24 × leverage. Sponsor equity funds EV plus a 2% transaction fee less debt. Exit occurs after five annual periods, with no interim distributions.

Annual revenue applies organic growth, retention, then pricing to prior revenue, plus annual cross-sell additions based on entry revenue and a three-year ramp. EBITDA is revenue × margin plus ramped cost synergies. Interest uses opening debt. Cash taxes are 25% of positive EBITDA less D&A (3% of revenue) and interest. Capex is 4% of revenue; incremental working capital is 10% of the change in revenue. Integration costs are split across years one and two. Positive free cash flow repays debt before cash accumulates.

Exit equity = max(0, exit EBITDA × exit multiple − debt + cash). MOIC = exit equity / initial sponsor equity; IRR = MOIC^(1/5) − 1.

Negative cash is a flagged funding gap, not a modeled financing source. Reported returns in such cases exclude additional sponsor capital and should not be treated as investable results.

### Simulation

A seeded LCG drives centered sums of three uniform draws. Independent shocks vary growth (±9 percentage points), exit multiple (±3×), and retention (±6 points), clamped to supported input ranges. Seed 42 is the default; re-running increments it. This is reproducible scenario exploration, not an empirically calibrated risk forecast.

## Quality checks

Nine engine tests cover sources/uses, annual cash and debt reconciliation, opening-debt interest, zero leverage, purchase/exit price effects, liquidity shortfalls, IRR identity, seeded simulation, sensitivity alignment, and invalid inputs. GitHub Actions runs the suite on pushes and pull requests. Browser QA covers scenario changes, persistence, exports, and responsive rendering.

## Interview walkthrough

1. Start with the base case: approximately 22% IRR and 2.70× MOIC.
2. Load downside and explain which assumptions undermine the thesis.
3. Separate price from operating execution using the sensitivity grid.
4. Explain why pricing and retention should be correlated in a more realistic model.
5. Open the cash schedule and trace earnings to debt repayment.
6. Export the memo and distinguish a return screen from a final investment decision.

**Banking:** transaction structure, cash conversion, leverage, multiple compression.

**Consulting:** synergy delivery, integration costs, scenario planning, accountable operating assumptions.

**Product:** willingness to pay, retention, cross-sell adoption, user-facing decision support.

**Engineering:** deterministic domain logic, financial invariants, reproducible randomness, dependency-free deployment, safe DOM rendering of stored data.

## Intentional limits and next steps

Fictional data; no real-company valuation claims. No purchase accounting, debt tranches, covenants, interest caps, tax loss carryforwards, interim distributions, or default recovery waterfall. Future extensions could add correlated shocks, explicit customer cohorts, multi-tranche debt, sponsor injections, and calibrated historical distributions. These are extensions, not implemented features.

Built with AI assistance. Review the model, modify assumptions, and understand the tradeoffs before presenting it as your work in an interview.

## License

MIT — see LICENSE.
