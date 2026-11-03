# Indicator Framework

The indicators tracked by the system (v3.1), grouped by the question each one adjudicates and the authority it carries.

Numeric thresholds are intentionally omitted. What follows is the **structure**: what each indicator observes, what distinguishes its three states, and what a red state is allowed to do.

**Current structure:** eleven primary indicators and two sub-factors. Number 9 is permanently retired and not reused: the former energy/geopolitics indicator was downgraded to a sub-factor of indicator 8 after data showed the direct oil-shock channel to the sector had faded (see indicator 8). Earlier versions tracked twelve indicators; in September 2026 the semantics were realigned and several former indicators were folded into others, as noted below.

---

## L0 — Determinative variables

*Question: should this exposure exist at all?*
*Authority: red triggers a reduction review. Only determinative layers can do this.*

### 1. Overseas compute build-out (hyperscaler capex)
- **Observes:** quarterly capex guidance from the major North American cloud providers and one large database/cloud vendor — the funding source for the entire chain.
- **Green:** guidance maintained or raised. **Amber:** growth decelerating, or visible investor pushback on spend. **Red:** any single provider explicitly cuts full-year guidance.
- **Why it is L0:** every downstream indicator is ultimately a derivative of this number. If it turns, nothing further down matters.
- **Folded in (September 2026):** optical-module order visibility; the substitution pace of the competing interconnect architecture (tracked as a forward risk item, where slow substitution is the favourable state); and laser-chip supply tightness (where tightness is bullish, so an "improving" supply reading is bad news).
- **Attached forward readings:** depreciation-life assumptions for AI server hardware, read from quarterly filings, which compress earnings and free cash flow before they compress capex. Since October 2026, a second watch item: whether the market has begun to price capex spenders differently according to whether their free cash flow is positive or negative.

### 2. Results delivery by chain leaders
- **Observes:** whether reported results beat or merely meet, at the leading listed names in both exposures, plus the capex of large domestic internet platforms as a mirror reading.
- **Green:** delivery exceeds expectation on operating metrics. **Amber:** in line, or beat driven by non-operating items. **Red:** clear miss attributable to demand, not to one-off items.
- **Symmetry rule:** one-off items (currency effects, debt-extinguishment charges, equity revaluation gains) are stripped out in **both** directions. A beat inflated by a one-off is not a beat; a miss caused by one is not a miss.
- **Structural fact, stated explicitly:** this indicator is shared by both exposures. On this axis the two are fully correlated and provide no diversification.

## L0.5 — Sleeve-specific determinative

*Question: should the equipment sleeve exist?*
*Authority: red triggers a reduction review of the equipment sleeve only.*

### 4. Domestic compute chips and the equipment chain
- **Observes:** delivery by domestic accelerator and memory makers; localisation rate of equipment and materials; contract liabilities and inventory at core equipment makers; warnings about circular financing.
- **Green:** localisation progressing, contract liabilities healthy. **Amber:** a reporting gate passed weakly or is incomplete. **Red:** contract liabilities falling for two consecutive quarters together with abnormal inventory build.
- **Why it was added:** before September 2026 the equipment sleeve had only a manual gate-renewal rhythm and no hard trigger. This line turns its falsification criterion into a panel rule.

## L1 — Demand validation

*Question: is the revenue actually arriving?*
*Authority: informs the position band; cannot trigger a reduction on its own.*

### 5. Advanced packaging
- **Observes:** advanced-packaging capacity and utilisation, globally and in the domestic memory chain.
- **Green:** persistently tight. **Amber:** localised easing. **Red:** clear loosening, read as demand softening.

### 6. Memory pricing cycle
- **Observes:** contract price direction and slope for DRAM, NAND and high-bandwidth memory; sell-out status; supplier capacity plans.
- **Green:** pricing firm with extending visibility. **Amber:** slope decelerating or large expansion announced. **Red:** pricing rolls over.
- **Distinction enforced:** *level* and *slope* are tracked separately. Prices can be high and still be decelerating.
- **Two-directional role:** for the equipment sleeve, rising memory prices are demand validation (positive). For the optical sleeve, they are cost inflation at the customer, which can crowd out other spending (potentially negative).

### 7. Domestic AI model monetization
- **Observes:** commercial traction of domestic model developers, token-call volumes, and the pull on domestic inference chips.
- **Green / Amber / Red:** accelerating / growing but unproven / stalling. Treated mainly as background.

## Background (no action mapping)

### 3. Overseas AI monetization and the revenue gap
- **Observes:** the gap between end-user AI revenue run-rates and the capital spent to serve them; cash burn at the leading labs.
- Tracked for context only. It maps to no action, because it does not correspond directly to either exposure.

## L2 — Macro gate

*Question: is the discount-rate environment hostile?*
*Authority: red pauses additions and prompts a cash-reserve check. It never triggers a reduction. Under v3.0 it freezes left-side scaling-in (B3) but not right-side confirmation (B2).*

### 8. Central-bank path and rates
- **Observes:** policy path, inflation prints, long-end yields, and the market-implied distribution across upcoming meetings. Since October 2026, whether officials begin attributing inflation to AI investment.
- **Green:** easing bias or benign path. **Amber:** ambiguous, data-dependent. **Red:** hawkish repricing underway.
- **Sub-factor 8-B, yen carry:** the funding-currency channel through which another central bank's tightening can force deleveraging in global growth assets.
- **Sub-factor 8-O, energy-inflation pass-through:** formerly indicator 9. Red only when transmission is evidenced (a sustained price spike together with a core-inflation response, or a central bank explicitly naming second-round effects), not on the price spike alone.
- **Tension, recorded so the operator knows:** this single gate currently sets the ceiling of the optical sleeve. In a hiking cycle the conditions to turn it amber are hard to meet, so the sleeve may stay near the bottom of its band for a long time. This is intended conservatism; the rule is not changed.

## L3 — Crowding gate

*Question: how leveraged and crowded is the trade?*
*Authority: same as L2. Red states here describe volatility, not value.*

### 10. Leverage, turnover and crowding
- **Observes:** market-wide margin balances, turnover intensity relative to fund size, ETF premium/discount, breadth, and the equipment ETF's own volume and unit flows.
- **Green:** positioning normalized. **Amber:** elevated. **Red:** extreme crowding with signs of distribution.
- **Weighting:** heavier for the equipment sleeve, whose price behaves as a leverage-and-sentiment amplifier, than for the optical sleeve, which is priced mainly by institutional fundamentals.
- **Pattern recognized from post-mortem:** a parabolic price advance combined with a step-change in turnover is treated as a late-stage form, entered as evidence here rather than acted on independently. "Strong results, sold on the day" is also logged here, never under indicator 2.

## L4 — Tail and anchor

*Question: could the story break from outside?*

### 11. US–China technology competition
- **Observes:** entity listings, procurement bans, import bans on components, tariffs and negotiated arrangements.
- **Green:** stable. **Amber:** restrictions under discussion or narrowly scoped. **Red:** escalation to substantive export control or a commercial import ban in force.
- **Severity grading:** procurement-level restrictions are noise-grade and move nothing. A commercial import ban or entity listing, once in force, is treated as red and triggers a reduction review.
- **Directional asymmetry:** the two exposures react in **opposite** directions to the same escalation. One is priced off global capex; the other off substitution urgency. Holding both is a partial self-hedge on this axis.

### 12. Compute-rental pricing and the scarcity anchor
- **Observes:** rental rates for accelerator capacity; whether large holders of compute begin leasing it out; whether any capex guidance starts carrying a rental-revenue softening item.
- **Green:** all three intact. **Amber:** one of the three deteriorating. **Red:** any two deteriorating; red triggers a reduction review.
- **Why it exists:** this is the valuation anchor. If compute stops being scarce, the multiple loses its justification independently of whether current orders are healthy.

---

## Cross-cutting rules

**Attribution before evidence.** A cross-market price move is classified by driver before it is entered as evidence. The same move can be confirming or contradicting depending on whether it was driven by demand or by policy-substitution expectations.

**US comparable-basket factor (optical sleeve only).** The optical sleeve's revenue is North American, so its position within the band also requires the US basket to stabilise: index and accelerator leader back above their 20-day averages, optical names no longer making new lows, and no large high-volume single-day drop. The equipment sleeve, priced off domestic drivers, is exempt.

**Collinearity caveat.** Indicators 1 and 2, and the readings folded into 1, plausibly measure one underlying variable. An "all green" reading across them may represent a single signal counted several times. A correlation audit of the state time series is outstanding — see the limitations section of the main README.

**Update cadence.** Every indicator is refreshed on the scheduled bi-weekly sweep. Individual indicators are refreshed off-cycle when the catalyst calendar or a tripwire names them. The weekly summary records readings but is barred from changing any state.
