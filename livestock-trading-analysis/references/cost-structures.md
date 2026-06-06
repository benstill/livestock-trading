# Livestock Agent Cost Structures — Australian Cattle

Reference guide for agent fees, GST treatment, and invoice interpretation.
Figures are indicative based on validated Australian agent invoices.

---

## Selling Costs — Typical Saleyard / Agent Invoice Structure

| Item | Typical Rate | GST | Notes |
|---|---|---|---|
| Agent commission | **4% of gross** | Yes | Common error: modellers use 2% — actual is higher |
| Cartage (sell) | $30–$45/head | Yes | Varies by distance and load size |
| Yard dues | $40–$50/lot | Yes | Not per head — per lot/pen |
| Government levy | $40–$50/lot | **No — GST exempt** | Livestock Industry Levy |
| Weighbridge | Usually included in yard dues | | |

**Total selling cost (typical range):** 5.5–7% of gross sale value including fixed charges

**Net proceeds formula:**
```
gross_sale = weight_kg × rate_c_per_kg / 100
commission = gross_sale × commission_rate   (e.g. 0.04)
net = gross_sale − commission − cartage − yard_dues − levy
```

### Account Sale Invoice Structure
A standard agent Account Sale (RCTI) will show:

```
TOTAL SALE                    $XX,XXX.XX    GST $0.00    (livestock is GST-free)

TAX INVOICE (agent fees)
  Commission          $X,XXX    GST $XXX    Total $X,XXX
  [Total taxable fees]

TAX INVOICES HELD OR EXEMPT
  Cartage            $XXX       GST $XXX    Total $XXX
  Yard Dues          $XX        GST $XX     Total $XX
  Government Levy *  $XX        GST $0      Total $XX   (* = GST exempt)
  [Total]

Total Fees Payable                          $X,XXX
Net Proceeds                                $XX,XXX
```

---

## Buying Costs — Typical Invoice Structure

**Paddock sale (direct, agent-facilitated):**
- No buyer's commission — buyer pays cartage only
- Cartage: $500–$700/load, typically $50–$65/head for standard mob sizes

**Saleyard purchase:**
- Buyer's premium: varies by agent — confirm before bidding
- Cartage to property: separate, based on distance

**Livestock purchase invoice (RCTI) structure:**
```
Livestock (ex GST)        $XX,XXX    GST $X,XXX    Total $XX,XXX
Buyers Cartage 1          $XXX       GST $0         $XXX  (some carriers are exempt)
Buyers Cartage 2          $XXX       GST $XX        $XXX  (standard carrier with ABN)
Total Charges             $XXX       GST $XX        $X,XXX
Invoice Total                                       $XX,XXX
```

Note: two cartage lines on a single invoice usually indicate two trucks (different loads,
different carriers, or different GST registration status).

---

## GST Quick Reference

| Transaction | GST treatment | Buyer action |
|---|---|---|
| Livestock purchase (registered vendor) | GST applies — $0.10 per $ | Claim as input credit on BAS |
| Livestock sale (to registered buyer) | GST-free supply | No GST collected |
| Agent commission (as vendor) | 10% GST — claimable | Claim on BAS |
| Cartage (standard carrier) | 10% GST — claimable | Claim on BAS |
| Cartage (exempt carrier) | GST exempt | No claim available |
| Government levy | GST exempt | No claim available |
| Yard dues | 10% GST — claimable | Claim on BAS |
| BBSE / preg test (vet) | 10% GST — claimable | Claim on BAS |

**Key GST principle:** Livestock itself is GST-free between registered parties.
All the fees around the transaction (commission, transport, testing) attract normal GST.
The GST on fees is claimable as an input credit — show it as a separate asset on the
balance sheet, not as a cost.

### RCTI Explained
A Recipient Created Tax Invoice means the agent (not the vendor) creates the tax invoice.
This is standard practice in Australian livestock trading. The agent issues it on your behalf
so you do not need to issue a separate tax invoice for the livestock sale.

---

## Per-Head Cost Reference (indicative ranges)

| Item | Low | Mid | High | Notes |
|---|---|---|---|---|
| Agent commission | 3.5% | 4.0% | 5.0% | Confirm with agent |
| Cartage — sell | $25 | $35 | $50 | Per head, short-medium haul |
| Cartage — buy | $40 | $58 | $80 | Per head, varies by load |
| Yard dues | $4 | $5 | $7 | Per head equivalent |
| Government levy | $4 | $5 | $6 | Per head equivalent |
| Vaccination | $5 | $8 | $15 | Depends on protocols |
| BBSE (bull) | $150 | $200 | $300 | Farm visit fee |
| PTIC preg test | $35 | $42 | $55 | Per head scanned |

---

## Entity Structure Considerations

When buying and selling entities differ (e.g. one trust purchases, another trust sells):
- GST input credits belong to the entity that paid — they cannot be transferred
- Income from the sale must be reported in the selling entity
- Inter-entity transactions at non-arm's-length prices may have tax implications
- Document any intra-group stock transfers with a proper invoice at fair market value
- Always flag multi-entity structures for review by the operation's accountant

---

## Invoice Checklist — What to Verify on Receipt

1. ☐ Head count matches what was mustered / weighed
2. ☐ Average weight is consistent with weighbridge docket
3. ☐ Rate (c/kg) matches the agreed price or clearing sale hammer price
4. ☐ Commission rate is as per agent agreement (typically 4%)
5. ☐ GST on livestock is $0 (if both parties are GST-registered)
6. ☐ GST on fees is 10% of the fee amount
7. ☐ Government levy is GST-exempt (no GST on that line)
8. ☐ Net proceeds match the bank deposit received
9. ☐ Any discrepancy → contact agent before 7-day payment deadline