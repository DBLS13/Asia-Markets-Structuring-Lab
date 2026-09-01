# Indicative 6-Month USD/CNH DCI and 6-Month HSCEI ELN

## Pricing, Risks & Hedge Residual

**Prepared by:** Dylan Bruce Lim Sam  
**Date:** June 2026 (Q2 quarter-end snapshot)  
**Status:** Indicative — not a term sheet, not investment advice

* * *

## 1. Product Summaries

### 1.1 USD/CNH Dual Currency Investment (DCI)

| Term | Detail |
| --- | --- |
| Observation date | 30 June 2026 |
| Underlying | USD/CNH (offshore RMB) |
| Deposit currency | USD |
| Notional | USD 1,000,000 |
| Tenor | 6 months |
| Strike | 7.2450 (ATM spot) |
| USD 6M rate | 4.05% |
| CNH 6M rate | 1.95% |
| USD/CNH 6M ATM vol | 5.50% |
| **Indicative coupon** | **see notebook output** (vs 4.05% USD deposit) |
| Yield pickup | Annualised put premium |

**Decomposition.** The client deposits USD and simultaneously sells a  
USD put / CNH call to the bank. If USD/CNH fixes below the strike at  
maturity, the bank exercises: principal is returned in CNH at the strike  
rate. The "enhanced yield" is the annualised Garman-Kohlhagen put premium.
**Break-even.** The client is worse off than a plain USD deposit if  
USD/CNH falls below the level where FX conversion loss exceeds the  
coupon pickup. Exact level computed in the notebook.
**Suitability.** A HK corporate with natural CNH payables (e.g. mainland  
supplier invoices). The conversion risk is tolerable because the CNH  
would have been purchased anyway. The DCI is a yield-enhanced hedging  
tool, not a speculative instrument.

### 1.2 HSCEI Equity-Linked Note (ELN)

| Term | Detail |
| --- | --- |
| Observation date | 30 June 2026 |
| Underlying | HSCEI (Hang Seng China Enterprises Index) |
| Deposit currency | HKD |
| Notional | HKD 1,000,000 |
| Tenor | 6 months |
| Strike | 6,556.50 (90% of spot) |
| HKD 6M rate | 3.85% |
| HSCEI dividend yield | 3.40% |
| HSCEI 6M ATM vol | 24.80% |
| **Indicative coupon** | **see notebook output** (vs 3.85% HKD deposit) |
| Yield pickup | Annualised put premium |

**Decomposition.** The client deposits HKD and sells a 90%-strike put  
on HSCEI to the bank. The coupon above risk-free is the future value  
of the put premium. If HSCEI closes below the strike at maturity, the  
client suffers a loss proportional to the decline below the strike —  
equivalent to owning the equity from that level down.
**Three scenarios:**
| Scenario | HSCEI at Maturity | Outcome |
| --- | --- | --- |
| Rally (+15%) | ~8,378 | Principal + full coupon |
| Unchanged | 7,285 | Principal + full coupon |
| Sell-off (−20%) | ~5,828 | Coupon received, but equity loss below strike |

**Suitability.** Private-bank client with moderate risk appetite who  
holds a bullish or range-bound view on HK/China equities and wants  
above-deposit yield. The client must be able to explain in one sentence  
what happens if HSCEI drops 25%.

* * *

## 2. Dealer Risk — Greeks Snapshot (at Inception)

The dealer is **long** the embedded options (put in both cases) and  
delta-hedges.
| Greek | DCI (per USD 1M) | ELN (per HKD 1M) | Interpretation |
| --- | --- | --- | --- |
| Delta | Negative USD eq. | Negative HKD eq. | Short underlying exposure |
| Gamma | Positive | Positive | P&L convexity; helps in moves |
| Vega (1 vol pt) | Positive | Positive | Long vol; profits if vol rises |
| Theta (per day) | Negative | Negative | Time decay works against dealer |

_Exact scaled values are computed in the notebook for the 30 Jun 2026 snapshot._
**Spot × Vol scenario grid.** A heatmap of dealer MTM P&L is computed  
3 months into the trade across ±10% spot moves and ±3 vol-point shifts  
(DCI) and ±20% spot / ±8 vol-point shifts (ELN). The ELN grid shows  
much larger P&L variance due to higher vol and wider moneyness range.

* * *

## 3. Hedge Slippage — Discrete Delta-Hedging Simulation

The notebook simulates 10,000 GBM paths for the ELN underlying and  
measures the dealer's residual P&L after delta-hedging the embedded  
put at different frequencies.
| Rebalancing | Transaction Cost | Expected Effect |
| --- | --- | --- |
| Daily | 0 bps | Baseline — tightest hedge |
| Weekly | 0 bps | ~2–3× daily std dev |
| Monthly | 0 bps | ~4–5× daily std dev |
| Daily | 5 bps | Slightly worse than 0-cost daily |
| Daily | 13 bps (HK stamp) | Materially worse; tail risk visible |

**Key insight:** The standard deviation of hedge error grows with the  
square root of the rebalancing interval (a textbook result that the  
simulation confirms). HK stamp duty (10 bps per side) materially  
erodes the premium collected, particularly in high-gamma regimes near  
the strike as expiry approaches.
**What this means for the desk:** The put premium must cover not just  
expected payoff but also the expected hedge slippage and transaction  
costs. In practice, the desk marks a bid-ask spread on vol (selling  
the option at a higher implied vol than the mid) to capture this.

* * *

## 4. Funding Footnote — Issuer Spread

A simplified 50 bp flat issuer spread is applied to illustrate how  
funding costs reduce the coupon passed to the client:
| Product | Effect of 50 bp Spread |
| --- | --- |
| DCI | Total coupon reduced by ~50 bps p.a. |
| ELN | Total coupon reduced by ~50 bps p.a. |

In a real trade, the spread is determined by the issuer's CDS curve  
and internal treasury transfer pricing. The Product & XVA desk owns  
this calculation. The 50 bp flat add-on here shows the direction and  
magnitude of the adjustment without pretending to replicate a full  
XVA framework.

* * *

## 5. Model Limitations

| What is missing | Why it matters |
| --- | --- |
| Volatility smile (SABR / local-vol) | Misprices OTM puts; real coupons differ |
| Issuer credit / CVA | Real notes embed issuer default risk |
| Stochastic rates | Second-order for 6M tenor but material for longer |
| Barriers / autocall | Many real ELNs use knock-in puts |
| Correlation (worst-of) | Multi-asset ELNs need joint simulation |

These are omitted deliberately to keep the decomposition transparent.  
Each omission is documented in the notebook with an estimate of the  
effort required to add it.

* * *

_Market snapshot: 30 Jun 2026 (Q2 quarter-end). Values sourced from_  
_public feeds and cross-checked against typical ranges. For production_  
_use, replace with Bloomberg closing data._
_This memo accompanies the Python notebook in_ `/notebooks` _and the_  
_frozen market snapshot in_ `/data/market_snapshot_20260630.csv`_. The_  
_four key charts (DCI payoff, DCI spot×vol grid, ELN payoff, ELN hedge_  
_error distribution) are exported to_ `/outputs`_._
