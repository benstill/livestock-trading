# Livestock Trading P&L and Balance Sheet — Generic Template

Use this template for any livestock trading scenario.
Fill in the variables from the user's actual transaction data and current market rates.

---

## P&L Template

### Inputs to Gather First
```
# Animals
N_PURCHASED  = ?    # total head purchased
N_SOLD       = ?    # head sold (may differ from purchased due to deaths, retentions)
N_RETAINED   = ?    # head retained on farm (bull, replacement heifers, etc.)
N_DEATHS     = ?    # deaths during holding period

BUY_WT       = ?    # average weight at purchase (kg)
BUY_RATE     = ?    # purchase rate (c/kg) — or use $/head directly
BUY_PX       = ?    # purchase price ($/head, ex GST)

SELL_WT      = ?    # average weight at sale (kg)
SELL_RATE    = ?    # sell rate (c/kg) from current market data

# Costs
COMMISSION   = 0.04 # agent commission rate (default 4% — confirm with agent)
CARTAGE_BUY  = ?    # $/head cartage on purchase
CARTAGE_SELL = ?    # $/head cartage on sale (or $/load)
VACC         = ?    # $/head vaccination/induction
YARD_DUES    = ?    # $/lot or $/head
GOVT_LEVY    = ?    # $/lot (GST exempt)

# Carry
CARRY_RATE   = ?    # $/week (fixed overhead or agistment rate)
WEEKS_HELD   = ?    # total weeks from purchase to final sale
N_ON_FARM    = ?    # animals sharing the carry cost (may change by phase)

# Special items (if PTIC or breeding stock)
PTIC_PREMIUM = ?    # $/head estimated PTIC premium (validate against recent AuctionsPlus data)
BBSE         = ?    # bull breeding soundness evaluation cost (one-off)
PREG_TEST    = ?    # $/head pregnancy testing cost
CALVING_RATE = ?    # expected calving rate (decimal, e.g. 0.90)
```

---

### Cash Costs (actual payments made)

```
ACTUAL CASH COSTS                              $/head    Total
  Purchase price (N × price, ex GST)          ______    ______
  Cartage — buying                             ______    ______
  Vaccination / induction                      ______    ______
  [PTIC/breeding only] BBSE                    ______    ______
  [PTIC/breeding only] Pregnancy testing       ______    ______
  Other entry costs                            ______    ______
  ─────────────────────────────────────────────────────────────
  TOTAL ACTUAL CASH OUT                                  ______
```

### Cash Receipts

```
CASH RECEIPTS                                  $/head    Total
  Sale of [N] animals @ [rate]c/kg            ______    ______
  Less: agent commission ([%] of gross)       ______    ______
  Less: cartage / yard dues / levy            ______    ______
  ─────────────────────────────────────────────────────────────
  [Repeat for each sale event / lot]
  ─────────────────────────────────────────────────────────────
  TOTAL CASH IN (net proceeds)                           ______

  [If retained animals] Retained asset value            ______
  (at market or sire value — memo item only)
```

### P&L Summary

```
  ═════════════════════════════════════════════════════════════
  CASH PROFIT (before carry)    = cash_in − cash_out    ______
  Cash ROI on outlay            = cash_profit / cash_out  ____%
  ═════════════════════════════════════════════════════════════

  COST OF CARRY (internal farm overhead — not a cash payment to third parties)
    Phase 1: [description, dates]  [wks] × $[rate]     ______
    Phase 2: [description, dates]  [wks] × $[rate]     ______
    Phase 3: [description, dates]  [wks] × $[rate]     ______
    ─────────────────────────────────────────────────────────
    Total cost of carry ([total weeks] weeks)           ______

  ═════════════════════════════════════════════════════════════
  ECONOMIC PROFIT = cash profit − carry                 ______
  Economic ROI on outlay                                  ____%
  Annualised economic ROI ([N] months)                    ____%
  ═════════════════════════════════════════════════════════════
```

### Multi-Phase Carry Breakdown

When animal numbers change across the holding period (e.g. dams sold at calving,
calves growing on), split carry into phases:

```
Phase 1: [Purchase date] → [first sale date]
  Animals on farm: [N]
  Duration: [weeks]
  Carry: [weeks] × [rate] = $[total]
  Per head: $[carry/N]/head

Phase 2: [After first sale] → [second sale date]
  Animals on farm: [N] (fewer — dams gone, calves present)
  Duration: [weeks]
  Carry: [weeks] × [rate] = $[total]
  Per head: $[carry/N]/head

[Add phases as needed]
```

---

## Balance Sheet Template

```
BALANCE SHEET — As at [date]

ASSETS

  Livestock (lower of cost or NRV)
    [Animals on hand] at cost basis              ______
    [Retained sire/bull] at market value         ______
    Total livestock                              ______

  Tax assets
    GST input credits receivable
      Purchase GST                               ______
      Fee GST (commission, cartage, etc.)        ______
      Total GST receivable                       ______

  TOTAL ASSETS                                  ══════

LIABILITIES
  Trade creditors (amounts owing to agents)     ______
  Borrowings                                    ______
  TOTAL LIABILITIES                             ══════

NET EQUITY (OWNER'S FUNDS)
  Cash deployed (purchases − sale proceeds)     ______
  Retained profit — completed trades            ______
  Unrealised gains — livestock appreciation     ______
  GST receivable                                ______
  TOTAL EQUITY                                  ══════

  ✓ CHECK: Total Assets = Total Liabilities + Total Equity
```

---

## Death Loss Treatment

When an animal dies during the holding period:

1. **P&L:** Expense the cost basis of the dead animal as a loss
   - `death_loss = cost_per_head × n_deaths`
   - Allocate the remaining purchase cost across survivors only
   - `cost_per_survivor = total_purchase / n_survivors`

2. **Balance sheet:** Derecognise the animal (remove from livestock asset value)

3. **GST:** No GST adjustment — the original input credit already claimed is not reversed
   (the animal was purchased, not sold; the death is a business loss)

4. **Insurance:** If livestock insurance was held, recognise any payout as income when received

---

## Sensitivity Analysis Format

For each key variable, show impact on economic profit:

```
Variable                    Base case    +10c/kg or +10%    Impact on profit
──────────────────────────────────────────────────────────────────────────────
Sell rate (c/kg)            [X]c         [X+10]c            +$[Y] per [N]hd
Buy rate (c/kg)             [X]c         [X+10]c            −$[Y] per [N]hd
Carry rate ($/week)         $[X]         $[X+10]            −$[Y] per week
PTIC premium ($/head)       $[X]         $[X+100]           +$[Y] total
Calving rate                [X]%         [X+5]%             +$[Y] per extra calf
Growth rate (kg/day)        [X]kg        [X+0.1]kg          ±[Y] days to target weight
```

Identify the **highest-leverage variable** — this is where market monitoring effort
should be concentrated.

---

## Worked Example — Simple Steer Grow-Out

```
Bought: 20 steers @ 250kg, 450c/kg = $1,125/head = $22,500 total
Entry:  $15/head vacc/transport = $300
Target: sell at 305kg @ 500c/kg

Days to target: (305-250) / 0.8kg/day = 69 days = 9.9 weeks

Carry (fixed $200/day, 20 animals): $10/head/day × 69 days = $690/head total
  OR carry (agistment $8/head/week): $8 × 9.9wks = $79/head

Sell gross: 305 × 5.00 = $1,525/head
Net sell:   $1,525 × 0.96 − $5 = $1,459/head  (4% commission, $5 transport)

WITH FIXED OVERHEAD CARRY:
  All-in per head: $1,125 + $15 + $690 = $1,830
  Profit/head: $1,459 − $1,830 = −$371  (loss — carry dominates)

WITH AGISTMENT CARRY:
  All-in per head: $1,125 + $15 + $79 = $1,219
  Profit/head: $1,459 − $1,219 = +$240  (profitable)

CASH PROFIT (no carry):
  Cash in:  $1,459 × 20 = $29,180
  Cash out: $22,800
  Cash profit: +$6,380  (28% cash ROI)
```

This illustrates why carry framing is critical — the same trade is −$371/head or +$240/head
depending on how carry is treated.