# HP Printing Franchise — FY27 Portfolio Investment Analysis

**An independent portfolio case study by [Chris Cannon](https://github.com/chrisacannon)**

A self-initiated financial modeling and strategy exercise evaluating two real, current investment levers for HP Inc.'s Printing franchise, built on HP's own public disclosures. This is a deeper, numbers-grounded companion to my [ink-portfolio-simulation](https://github.com/chrisacannon) site — that project is the conceptual/abstracted framework layer; this one is the actual financial modeling behind it, including the scenario toggles, sensitivity analysis, and structured portfolio comparison I'd be building day to day.

This was not a take-home assignment — it was built unprompted, from public information, as a way to demonstrate the work itself rather than just describe it. It also happens to be the specific business transition I worked on directly during my time at HP (the consumer printer business's shift toward a subscription model), so Option B is where the real domain credibility concentrates.

---

## The Case

HP's Printing franchise has incremental FY27 investment capacity. Two independent, real levers are evaluated on their own merits — this is a portfolio sequencing exercise, not a "pick one" decision:

- **Option A — New Printer Generation Launch.** A standard hardware-refresh/NPI lever. A new generation resets the aftermarket-ink-capture decay that comes with an aging installed base (older printers gradually leak ink spend to third-party/compatible cartridges).
- **Option B — Instant Ink Subscription Acceleration.** A business-model lever, not a hardware cycle. A targeted push converts an underpenetrated slice of the *existing* installed base onto the Instant Ink subscription, raising supplies capture rate without waiting on a refresh cycle.

HP's own FY2025 10-K names this exact tension directly: a decline in the installed base and usage driving Supplies revenue down, met by a stated push toward subscription and recurring revenue as the mitigation strategy. Option A and Option B are, almost verbatim, HP's own two stated levers against that headwind.

**⚠ A scope note that matters for how to read every number below:** Option A is modeled at full global Printing-segment scale (consumer + commercial, all product lines). Option B is scoped to the global consumer/home segment only — HP discloses no public consumer/commercial revenue split fine-grained enough to narrow Option A further. The two options should be read as independently fundable levers evaluated at different scopes, **not** as an apples-to-apples size comparison. Full reasoning — including three candidate fixes that were considered and rejected — is in [`docs/methodology.md`](docs/methodology.md) and called out directly in the model (`Cover_Assumptions`, `Portfolio_Comparison`) and the deck (slides 2–6, 9).

## What's in This Repo

| Path | Contents |
|---|---|
| `model/` | Full Excel workbook — assumptions, two scenario-driven NPV models, a Combined Scenario tab, two sensitivity grids, portfolio comparison |
| `deck/` | Companion deck — executive summary + full appendix detail |
| `docs/` | Write-up of key analytical decisions, bugs caught, and judgment calls (this is the most interesting part if you only have a few minutes) |

**Start here if you want the headline:** open the deck (`deck/HP_Print_Franchise_Portfolio.pptx`), slides 1–6.

**Start here if you want to see the work:** open the model (`model/HP_Print_Franchise_FY27_Portfolio_Investment_Analysis.xlsx`) and read `docs/methodology.md` alongside it.

## Headline Results

| | Option A: New Printer Generation | Option B: Instant Ink Acceleration |
|---|---|---|
| 5-yr NPV, Downside | -$55.9M | -$4.1M |
| 5-yr NPV, **Base** | **$272.6M** | **$55.2M** |
| 5-yr NPV, Upside | $672.7M | $251.2M |
| Scope | Full Printing segment (consumer + commercial) | Global consumer/home only |
| Sensitized on | Aftermarket recapture premium × NPI launch cost | Subscriber growth acceleration × CAC per incremental subscriber |

**Combined Scenario (Base): $375.1M** — Option A ($272.6M) + Option B ($55.2M) + a modeled point-of-sale "attach effect" ($47.3M): new-generation hardware buyers converting to Instant Ink at the point of setup, a real coordination mechanism that's genuinely absent from either option modeled alone. Shown as a directional, un-netted sum of independently-verified components — not an apples-to-apples addition, given the scope note above.

**Recommendation:** fund both, coordinated rather than run independently. Option A carries the larger base-case opportunity but a genuinely negative worst case, and its aftermarket edge resets rather than compounds as the refreshed fleet itself ages. Option B offers a materially safer downside and durable, non-decaying value once converted, but a smaller absolute ceiling on its own. The strongest finding isn't which one to prioritize — it's that designing the subscription push to run in tandem with the hardware launch, rather than as a disconnected campaign, captures real additional value (~$47.3M in this model) that neither option delivers funded alone. Full reasoning in the deck and the model's `Portfolio_Comparison` tab.

## Model Structure

- **`Cover_Assumptions`** — all sourced inputs, Downside/Base/Upside scenario tables for both options, shared structural decisions (5-yr horizon, 8.0% discount rate, do-nothing baseline methodology, the scope-limitation note)
- **`Option_A`** — revenue build (hardware + supplies, two-line margin structure), do-nothing baseline, NPI launch cost, full NPV chain with a live Downside/Base/Upside toggle, plus a 16×16 sensitivity grid (recapture premium × NPI cost)
- **`Option_B`** — subscription-share-of-Supplies mechanic, do-nothing baseline (organic subscriber growth), Invest Case with a CAC-per-incremental-subscriber cost toggle, full NPV chain, and a 7×7 sensitivity grid (growth acceleration × CAC) — built via Excel's native Data Table feature, since this option's economics are genuinely non-linear (confirmed before building, not assumed)
- **`Combined_Scenario`** — the point-of-sale attach effect between the two options, referencing both tabs' already-verified outputs rather than duplicating their mechanics
- **`Portfolio_Comparison`** — market attractiveness / right-to-win scoring (8 criteria across 2 dimensions), financial side-by-side, the scope-limitation callout, sequencing recommendation

**Modeling conventions used throughout:** blue text = hardcoded input, black text = same-sheet formula, green text = cross-sheet link, gray italic = source-citation note. Every hardcoded assumption carries a cell comment citing its source or flagging it explicitly as a reasoned judgment call rather than a hard data point.

## Sourcing & Methodology

Every assumption in the model is documented at the cell level (see the `Source/Basis` column on each tab, plus cell comments). At a glance:

- Market sizing, segment revenue, and margin trend: HP Inc.'s FY2025 10-K and quarterly earnings releases
- Instant Ink subscriber counts, growth trajectory, and value-per-conversion commentary: HP management disclosures (earnings calls, investor conferences), as reported
- Discount rate: midpoint of two independent third-party WACC estimates for HPQ specifically
- Several option-specific assumptions (e.g., the aftermarket recapture premium curve, Option B's subscription-share-of-Supplies split, the attach conversion rate) are explicitly flagged as **reasoned judgment calls**, not sourced figures — and documented as such rather than presented with false precision

See `docs/methodology.md` for a walkthrough of the more interesting judgment calls, including:
- A real formula bug caught and fixed before finalizing Option A's Supplies revenue line — subtracting figures built on inconsistent bases produced a nonsensical result, and what the fix reveals about validating a subtraction's inputs rather than just trusting its output
- Why a sensitivity grid was built with Excel's native Data Table feature for Option B (after confirming the underlying economics are genuinely non-linear) but with a faster closed-form approach for Option A (confirmed linear) — and a toggle-corruption bug that would have silently invalidated Option A's grid if left uncaught
- The project's strongest "judgment under ambiguity" story: a real scope mismatch between the two options, discovered late, with three candidate fixes each considered and correctly rejected before landing on the honest answer — naming the limitation plainly rather than patching around it

## A Direct Connection

Unlike the other projects in this portfolio, this one isn't built from public data alone — Option B is the specific business shift (the consumer printer business's move toward a subscription model) I worked on directly at HP. The model itself uses only public HP disclosures; no internal figures appear anywhere in this repo. Where the model's derived numbers (e.g., segment-level margin blends) diverge from what I know from direct experience, that's a difference in analytical altitude — public disclosures blend every tier, geography, and business model into one number, which is a different vantage point than program-level detail — not an error to quietly correct with anything non-public.

## Tools

Built in Microsoft Excel and PowerPoint. No macros, no add-ins — everything is native formulas, conditional formatting, and data validation, built to be fully auditable by opening the file and clicking on cells.

---

*This project uses only publicly available information about HP Inc. and its products. It is an independent analytical exercise and is not affiliated with, endorsed by, or representative of any non-public HP data, strategy, or decision-making.*
