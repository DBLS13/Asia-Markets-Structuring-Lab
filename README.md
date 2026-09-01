# Asia Markets Structuring Lab
## Dual Currency Investment & Equity-Linked Reverse Convertible

Replicates how a HK Global Markets structuring / sales desk turns a
client objective into a term sheet, prices the embedded option, and
measures dealer hedge slippage.

### Products
- **6-Month USD/CNH Dual Currency Investment** — structured FX deposit
  embedding a short put. Enhanced yield = put premium.
- **6-Month HSCEI Equity-Linked Note** — structured deposit embedding a
  short equity put. Coupon above risk-free = put premium, not free yield.

### What's Inside
- Black-Scholes / Garman-Kohlhagen pricing engine with put-call parity
  verification
- Terminal payoff diagrams and break-even analysis
- Dealer greeks scaled to notional, spot × vol MTM P&L heatmaps
- Discrete delta-hedge simulation (daily / weekly / monthly, 0 / 5 /
  10 bps transaction costs including HK stamp duty)
- Client suitability matrix and 50 bp funding/XVA footnote
- Two-page structuring memo in `/docs`

### Stack
Python, NumPy, Pandas, SciPy, Plotly

### Market Data
Frozen snapshot: 30 Jun 2026 (Q2 quarter-end). See
`data/market_snapshot_20260630.csv`.

### Limitations
Flat vol (no smile/skew), no issuer credit/CVA, no barriers, no
stochastic rates. See notebook Cell 13 for full table with effort
estimates.
