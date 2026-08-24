# Indicator Framework

The twelve indicators tracked by the system, grouped by the question each one adjudicates.

Numeric thresholds are intentionally omitted. What follows is the **structure**: what each indicator observes, what distinguishes its three states, and which layer's authority it carries.

---

## Layer I — Determinative variables

*Question: should this exposure exist at all?*
*Authority: only this layer can trigger a reduction in exposure.*

### 1. Hyperscaler capital-expenditure guidance
- **Observes:** quarterly capex guidance from the major North American cloud providers — the funding source for the entire chain.
- **Green:** guidance maintained or raised. **Amber:** growth decelerating, or visible investor pushback on spend. **Red:** any single provider explicitly cuts full-year guidance.
- **Why it is Layer I:** every downstream indicator in the system is ultimately a derivative of this number. If it turns, nothing further down matters.
- **Attached forward reading:** depreciation-life assumptions for AI server hardware, read from quarterly filings. A shortening of assumed useful life compresses earnings and free cash flow before it compresses capex — it appears in footnotes earlier than in any order data.

### 2. Interim-results delivery by chain leaders
- **Observes:** whether reported results beat or merely meet, at the leading listed names in each segment.
- **Green:** delivery exceeds expectation on operating metrics. **Amber:** in line, or beat driven by non-operating items. **Red:** clear miss attributable to demand, not to one-off items.
- **Symmetry rule:** one-off items (currency effects, debt-extinguishment charges, equity revaluation gains) are stripped out in **both** directions. A beat inflated by a one-off is not a beat; a miss caused by one is not a miss.

---

## Layer II — Demand realization

*Question: is the revenue actually arriving?*
*Authority: informs the position band; cannot trigger a reduction on its own.*

### 3. AI monetization and the revenue gap
- **Observes:** the gap between end-user AI revenue run-rates and the capital being spent to serve them; unit economics direction.
- **Green:** monetization accelerating, gap narrowing. **Amber:** revenue growing but the gap and cash burn remain large. **Red:** monetization stalls, enterprise adoption recedes.

### 4. Order visibility at the optical-module layer
- **Observes:** how far forward production schedules are booked; presence or absence of deferrals and cancellations; ramp progress on the next speed generation.
- **Green:** visibility extending into later years, incremental orders continuing. **Amber:** visibility flat. **Red:** orders pushed out or cut.

### 5. Memory pricing cycle
- **Observes:** contract price direction and slope; supplier capacity commentary; whether price increases are spreading into adjacent components.
- **Green:** pricing firm with extending visibility. **Amber:** slope decelerating. **Red:** pricing rolls over.
- **Distinction enforced:** *level* and *slope* are tracked separately. Prices can be high and still be decelerating; those are different signals.

### 6. Substitution pace of the competing interconnect architecture
- **Observes:** adoption rate of the packaging approach that would displace the incumbent pluggable module.
- **Green (for incumbent exposure):** substitution remains incremental and slow. **Amber:** adoption accelerating. **Red:** large-scale displacement arrives ahead of schedule.
- **Note:** this indicator is inverted relative to the others — slow adoption is the favorable state.

### 7. Upstream bottleneck tightness
- **Observes:** capacity conditions in the scarce inputs — specialty laser chips, advanced packaging, substrate materials.
- **Green:** upstream persistently tight, which is a strong demand signal. **Amber:** localized easing. **Red:** clear loosening, which reads as demand softening.
- **Inversion note:** tightness is bullish here. A supply-chain indicator that improves is, in this position, bad news.

---

## Layer III — Macro and liquidity gates

*Question: is the discount-rate environment hostile?*
*Authority: red state pauses additions and prompts a cash-reserve check. It never triggers a reduction.*

### 8. Central-bank path and rates
- **Observes:** policy path, long-end yields, and the market-implied probability distribution across upcoming meetings.
- **Green:** easing bias or benign path. **Amber:** ambiguous, data-dependent. **Red:** hawkish repricing underway.

### 9. Energy prices and the geopolitical inflation channel
- **Observes:** crude prices as the transmission channel from geopolitical events into the inflation path and therefore into indicator 8.
- **Green:** prices below the level that would reopen the inflation debate. **Amber:** elevated or unstable. **Red:** sustained spike.

---

## Layer IV — Fragility amplifiers

*Question: will the ride be violent?*
*Authority: same as Layer III. Red states here describe volatility, not value.*

### 10. Positioning crowdedness
- **Observes:** market-wide margin balances, turnover intensity relative to fund size, and the shape of price and volume together.
- **Green:** positioning normalized. **Amber:** elevated. **Red:** extreme crowding.
- **Pattern recognized from post-mortem:** a parabolic price advance combined with a step-change in turnover is treated as a late-stage form, entered as evidence under this indicator rather than acted on independently.

### 11. Export controls and the geopolitical tail
- **Observes:** the state of export-control and entity-listing regimes affecting the chain.
- **Green:** stable. **Amber:** restrictions under discussion or narrowly scoped. **Red:** escalation to substantive export control.
- **Directional asymmetry, made explicit:** the two exposures held react in **opposite** directions to the same escalation. One is priced off global capex; the other is priced off substitution urgency. Holding both is a partial self-hedge on this axis — not diversification, and not a mistake, but two wagers whose winning conditions differ.

### 12. Compute-rental pricing and the scarcity narrative
- **Observes:** rental rates for accelerator capacity; whether large holders of compute begin leasing it out; whether any capex guidance starts carrying a rental-revenue softening item.
- **Green:** scarcity intact. **Amber:** one of the three deteriorating. **Red:** any two of the three deteriorating.
- **Why it exists:** this is the valuation anchor. If compute stops being scarce, the multiple loses its justification independently of whether current orders are healthy.

---

## Cross-cutting rules

**Attribution before evidence.** A cross-market price move is classified by driver before it is entered as evidence. The same move can be confirming or contradicting depending on whether it was driven by demand or by policy-substitution expectations.

**Collinearity caveat.** Indicators 1, 2, 4 and 7 plausibly measure one underlying variable. An "all green" reading across them may represent a single signal counted several times. A correlation audit of the state time series is outstanding — see the limitations section of the main README.

**Update cadence.** Every indicator is refreshed on the scheduled bi-weekly sweep. Individual indicators are refreshed off-cycle when the catalyst calendar or a tripwire names them. The weekly summary records readings but is barred from changing any state.
