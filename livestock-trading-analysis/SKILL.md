---
name: livestock-trading-analysis
description: >
  Comprehensive livestock trading analysis for Australian cattle operations. Use this skill
  whenever the user mentions saleyards, cattle prices, MLA reports, AuctionsPlus, PTIC heifers,
  sell-buy analysis, cost of carry, weaner steers, grow-out scenarios, livestock P&L, or
  balance sheets for farming entities. Also trigger for questions about:
  - Parsing MLA prime/store reports (CSV) from any saleyard
  - Comparing AuctionsPlus price discovery data with physical saleyard prices
  - Analysing buy-sell arbitrage for any livestock category
  - Modelling grow-out profitability at different target weights and sell prices
  - Calculating PTIC premiums and cow+calf unit values
  - Building hold-period profit curves with cost of carry sensitivity
  - Generating livestock P&L statements and balance sheets
  - Interpreting livestock agent account sale invoices
  Even if the user asks only a simple question about cattle prices, use this skill to ensure
  correct methodology, terminology, and framing are applied.
---

# Livestock Trading Analysis Skill

This skill encodes the methodology, terminology, and analytical frameworks for
analysing Australian cattle trading operations — from parsing raw MLA saleyard reports
through to full P&L and balance sheet construction.

---

## Core Concepts and Terminology

### Price Units
- Prices quoted in **c/kg live weight (LW)** unless stated as dressed weight (DW)
- To convert: gross sale $ = weight (kg) × rate (c/kg) ÷ 100
- e.g. 300kg × 500c/kg = $1,500 gross

### Standard Cost Structure (validated from real Australian agent invoices)

**Selling costs (vendor, through saleyards/agent):**
- Agent commission: **4% of gross** (common modelling error: using 2% — always confirm with agent)
- Cartage: varies by load size and distance — $30–65/head typical range
- Yard dues: ~$45/lot or ~$5/head
- Government levy: ~$45/lot (GST exempt)
- Total selling cost: approximately 5.5–6% of gross plus fixed per-head charges
- Net formula: `gross_sale × (1 − commission_rate) − cartage − yard_dues − levy`

**Buying costs (buyer):**
- No buyer's commission on direct paddock sales — buyer pays cartage only
- Saleyard purchases may include buyer's premium (confirm with agent)
- Cartage: $50–65/head depending on distance and truck load
- Vaccination/induction: $5–15/head
- GST: livestock sales are **GST-free supply** — $0 GST on animals; GST on fees is claimable

### GST Treatment
- Livestock sales between registered parties: GST-free (zero-rated)
- Agent fees, cartage, yard dues: GST applies (claimable as input credits)
- Government levy: GST exempt
- Recipient Created Tax Invoices (RCTI): agent creates the tax invoice on vendor's behalf
- Always separate GST paid on fees as a recoverable asset on the balance sheet

---

## MLA Saleyard Report Analysis

### CSV Structure
MLA prime/store reports have this column order (0-indexed):
`Category, WeightRange, Prefix, Muscle, Fat, HeadCount, WeekDelta, Min, Max, Avg, ...`

Key parsing notes:
- Skip rows starting with `"`, `Data`, `Please`, `Saleyard`, `Report`, `Comparison`, `Total`, `Commentary`, `Market`
- Data rows start after the `Category,` header row
- `NC` = Not Current (no change), `NQ` = Not Quoted (not traded that week)
- Avg c/kg is column index 9
- Weighted average across muscle/fat grades: `sum(avg × head) / sum(head)`

### Category Naming Convention
Format: `[Species] [WeightRange] [Prefix] M[Muscle]F[Fat]`
- Species: Vealer Steer/Heifer, Yearling Steer/Heifer, Grown Steer/Heifer, Cows, Bulls
- Weight ranges: 0-200, 200-280, 280-330, 330+, 330-400, 400+, 400-500, 500-600, 600-750
- Prefix: Restocker (C2), Feeder (C3), Processor (various)
- Muscle grades: B, C; Fat grades: 1, 2, 3L, 3H, 4, 5

### Multi-Sale Trend Analysis
When multiple MLA reports are available, always build a trend table showing:
- Avg c/kg per sale date per key category
- Week-on-week delta (use the `WeekDelta` column or calculate)
- Yarding numbers (from Total Yarding line) — supply context drives prices
- Market commentary summary (look for destocking events, seasonal shifts, buyer sentiment)

### Physical Saleyards vs AuctionsPlus National
Physical saleyards typically trade **below** AuctionsPlus national averages for light calves
and heifers — commonly 50–100c/kg lower. This is structural, not a data error:
- AuctionsPlus aggregates assessed, well-presented lots nationally
- Physical yards include the full quality range including tail-end stock
- For breeding/PTIC stock: AuctionsPlus is the primary benchmark
- For store and processor categories: local saleyard reports are more relevant

---

## Grow-Out Profitability Analysis

### The Core Model
For any buy-grow-sell scenario:

```
buy_gross    = buy_wt × buy_rate / 100          # $/head
buy_agent    = buy_gross × agent_rate            # if through saleyards
carry        = days × carry_per_hd_per_day       # internal farm cost
entry        = vacc + transport                   # per head
total_in     = buy_gross + buy_agent + carry + entry

sell_gross   = sell_wt × sell_rate / 100
net_sell     = sell_gross × (1 − agent_sell) − transport_sell

profit       = net_sell − total_in
```

### Cost of Carry
**Critical distinction** — always clarify with user which framing applies:

1. **Fixed farm overhead** (capacity-based): e.g. $X/week for a Y-animal farm
   - Per head rate = fixed_cost / animals_on_farm
   - Does NOT scale with animal count — fewer animals = higher per-head cost
   - Carry rate changes at each phase as animal numbers change

2. **Agistment rate** (per-head market rate): e.g. $5–10/head/week
   - Scales directly with animal count
   - Use this when the farm incurs zero cost without these specific animals

3. **Zero carry** (cash profit only): for pure livestock trading profit analysis
   - Always present as a separate line, not blended into cost of goods

**Standard presentation format:**
```
Cash profit     = total_cash_in − actual_cash_out   ← real money in pocket
Cost of carry   = weeks × rate                       ← internal farm overhead
Economic profit = cash profit − cost of carry        ← full economic view
```
Present both. Let the user decide which reflects their situation.

### Hold-Period Analysis
Model profit at every sell day from purchase to maximum practical hold:
- Use the actual sell rate for each weight band the animal passes through as it grows
- Identify: optimal sell day, second-best window, break-even day, loss trough
- Rate band changes create step-function jumps in profit — sell timing relative to these is critical
- The dominant risk on long holds is usually a rate cliff (dropping into a lower-valued category)
  rather than carry cost alone

### Weight Band Rate Cliffs
Categories where a weight crossover causes a significant rate DROP:
- Grown Heifer 0-540 C3 Feeder: typically much lower than Yearling Heifer rates
- Grown Steer 400-500 Feeder: lower rate than Yearling Steer 400+
- The grown feeder bands are known traps for over-held animals
- Always map the rate curve across all weight bands before recommending a sell point

---

## PTIC Heifer Valuation

### What PTIC Means
**Pregnancy Tested In Calf** — heifer or cow confirmed pregnant by vet ultrasound.
The PTIC designation adds value because the buyer acquires both the dam and the unborn calf.

### Valuation Framework (build-up method)
```
base_heifer_value  = weight × current_livestock_rate_for_that_weight_band
calf_value         = (weaner_steer_net + weaner_heifer_net) / 2 × calving_rate_adj
gross_intrinsic    = base_heifer_value + calf_value
less: carry_to_calving = days_to_calving × carry_rate_per_head
less: entry_costs (vacc, transport, agent)
= break_even_buy_price
```

### PTIC Premium Behaviour
The PTIC premium (above unjoined livestock value) varies by weight and market conditions:
- Light PTIC (370–470kg): smaller absolute premium — market benchmarks against weaner value
- Heavy PTIC (550–600kg): larger absolute premium — reflects heavier dam value plus calf
- The **percentage** premium shrinks as weight increases
- Always benchmark against actual comparable SOLD lots from AuctionsPlus, not averages
- A market average PTIC price includes a wide quality range — compare like-for-like

**Common modelling error:** applying a flat $/head PTIC premium across all weight bands.
The premium must be validated against current comparable sales at the relevant weight.

### Exit Strategy Comparison
Always run all three exits for PTIC heifer trades:
1. **Sell as PTIC** (before calving, after preg test confirms)
2. **Sell heifer + wean calf** (sell dam post-calving, sell calf at weaning weight)
3. **Sell as cow + calf unit** (sell together at or after calving)

Key decision drivers:
- Carry rate is the dominant variable — high carry strongly favours early exit
- Fixed overhead at low animal numbers makes holding expensive per head
- The break-even carry rate for holding to C+C vs selling PTIC can be calculated precisely

---

## Sell-Buy Arbitrage Analysis

When a producer sells one class of animal and buys another, analyse three dimensions:

1. **Asset value swap**: What were the sold animals worth at current market rates?
   What are the bought animals worth? Net asset gain/loss on day 1.

2. **Sell-side execution**: Did the vendor achieve better or worse than the local saleyard rate?
   Private sales often achieve 50–150c/kg above equivalent saleyard rate.
   Always compare net proceeds against what the same animals would have made through the yards.

3. **Buy-side execution**: Did the buyer pay above, at, or below AuctionsPlus comparables?
   Use real sold lot data — not just published averages — to assess.
   Match on weight, age, fat score, breed, and location.

**Day-1 wealth change:**
```
wealth_change = (new_asset_market_value − cost_to_buy)
              + (net_sell_proceeds − prior_asset_market_value)
```
Note: the bull (or any retained animal) should be excluded from the sell proceeds
and carried separately as a retained asset at market or cost value.

---

## P&L and Balance Sheet Construction

See `references/livestock-pl-template.md` for the full template with worked structure.

### P&L Structure (livestock trading entity)
```
REVENUE
  Gross sale proceeds (ex GST)

COST OF LIVESTOCK SOLD
  Purchase price (allocated to animals sold, at cost)
  Death losses (at cost basis — derecognise the animal)

GROSS PROFIT

SELLING EXPENSES
  Agent commission, cartage, yard dues, government levy

NET PROFIT (CASH, before carry)

OTHER GAINS
  Unrealised appreciation on retained livestock (mark to market)

NOTE: Cost of carry shown separately, not in cost of goods
```

### Balance Sheet Structure
```
ASSETS
  Livestock (lower of cost or NRV)
    On-hand animals at cost basis
    Retained animals at market value where materially different
  Tax assets
    GST input credits receivable

LIABILITIES
  (typically nil for small equity-funded operations)

NET EQUITY
  Cash deployed net of sale proceeds
  Retained profit from completed trades
  Unrealised gains
```

### Key Accounting Notes
- Livestock held at **lower of cost or net realisable value (NRV)**
- Death losses: remove the cost basis; no GST adjustment required
- GST on purchase: show as a tax asset (receivable), not a cost
- GST on fees: also claimable — separate from the livestock GST
- Flag any multi-entity structure (different buying vs selling entities) for accountant review

---

## AuctionsPlus Price Discovery

### What the Tool Does
AuctionsPlus Price Discovery filters recent sold lots by species, breed, category, and
distance from a location. Returns individual lot data: weight, age, fat score, c/kg LW,
c/kg DW, $/head, sold date, status (SOLD / PASSED IN).

### Location Filter Behaviour
- Published weekly averages = **national** (all eastern states, no geographic filter)
- Price Discovery = **filtered by distance** from the specified location
- The state filter on the interactive dashboard allows NSW/QLD/VIC/Angus-only views
- Physical saleyards trade below AuctionsPlus national — use local saleyard data for
  store/processor benchmarks, AuctionsPlus for breeding/PTIC benchmarks

### Parsing Pasted Price Discovery Data
AuctionsPlus data copied from the browser contains duplicated blocks. Parse by:
- Extracting: Lot number, head count, location, avg/min/max weight, age, fat score,
  $/head, c/kg LW, sold date, SOLD or PASSED IN status
- Deduplicating on lot number
- Excluding PASSED IN lots from market averages — show as reserve price context only
- Grouping by type (PTIC, weaned, NSM, C+C) and weight band for comparable analysis

### When Price Discovery Returns No Results
Too specific a filter for the 30-day window — not that no market exists.
Broaden sequentially: remove fat score → widen age range → increase distance radius.

---

## Bull Valuation

A bull has two distinct value floors:
1. **Processor price**: weight × current bulls processor rate (absolute minimum)
2. **Working sire market**: significant premium for proven fertility, known genetics, appropriate age

Factors that increase sire value above processor floor:
- Documented joining record (seasons × cows joined)
- Known stud genetics (registered sire, named stud)
- Age — 2–4yo bulls command highest sire premiums; older bulls depreciate
- BCS and soundness (BBSE result)
- Breed and frame matching the buyer's cow herd

**Opportunity cost framing for on-farm bulls:**
If the bull is already on farm and his carry is absorbed in the fixed overhead, the
incremental cost of using him is only: BBSE ($150–$250) + pregnancy testing ($35–$50/head).
The return is the PTIC premium across all joined animals. ROI is almost always very high.

---

## Standard Assumptions (Australian cattle, general)

| Parameter | Typical range | Notes |
|---|---|---|
| Agent commission (sell) | 4–5% of gross | Confirm with agent — 2% is an underestimate |
| Transport (sell) | $5/head | Fixed per-head component |
| Cartage (buy) | $30–65/head | Varies by distance and load size |
| Vaccination | $5–15/head | On-farm cost |
| BBSE (bull) | $150–$250 | Vet farm visit |
| PTIC preg test | $35–$50/head | Vet ultrasound scan |
| Growth rate (post-wean, pasture) | 0.7–0.9 kg/day | 0.8kg/day standard NSW pasture |
| Growth rate (pre-calving, heifer) | 0.3–0.4 kg/day | Pregnancy reduces intake efficiency |
| Calving rate (first-calf heifer) | 85–93% | Use 88–90% for conservative modelling |
| Calving rate (mature cow) | 90–95% | |
| Calf sex split | 50/50 steer/heifer | Standard assumption |

---

## Reference Files

- `references/livestock-pl-template.md` — Generic P&L and balance sheet template for livestock trades
- `references/cost-structures.md` — Agent fee structures and GST treatment guide