# Tiered Monitoring & Decision System for Sector Exposure

A personal decision-support system for equity exposure to the AI-hardware supply chain — specifically **optical communications** and **semiconductor capital equipment** in the China A-share market.

The system does not pick stocks and does not generate trades. It converts a stream of noisy inputs into a small set of discrete signal states, maps those states to pre-defined actions, and forces every judgment to be made against criteria written down *before* the evidence arrived.

**Status:** in production. 48 documented revisions since v2.0 (July 2026).
Position sizes, entry prices, and returns are deliberately excluded from this document.
This repository documents the system's design. The system itself runs as a versioned document inside an agent workspace; no source code is published here.

---

## The problem it was built to solve

Three failure modes, all of them mine before the system existed:

1. **High-frequency, low-quality decisions.** Watching an intraday screen produces the *feeling* of diligence and the *substance* of noise trading. Decision quality is not a function of decision frequency.
2. **Confusing volatility risk with value risk.** A crowded-positioning warning and a demand-collapse warning feel identical in the moment. They call for opposite responses. Without a rule that separates them in advance, the loud one wins.
3. **Retroactive reinterpretation.** Absent a falsification criterion written down beforehand, any adverse outcome can be re-explained. A thesis that cannot be wrong is not a thesis.

The system's job is to make the first mode rarer, the second mode structurally impossible, and the third mode visible when it happens.

---

## Architecture

### 1. Indicator taxonomy — four layers, twelve indicators

Indicators are grouped not by subject matter but by **what question they adjudicate**:

| Layer | Question it answers | Example indicators |
|---|---|---|
| **I. Determinative variables** | Should this exposure exist at all? | Hyperscaler capex guidance; interim-results delivery by chain leaders |
| **II. Demand realization** | Is the revenue actually arriving? | Order visibility; supply-chain tightness; pricing-cycle slope |
| **III. Macro & liquidity gates** | Is the discount rate environment hostile? | Central-bank path; energy/inflation channel; market-wide leverage |
| **IV. Fragility amplifiers** | Will the ride be violent? | Positioning crowdedness; geopolitical/export-control state; narrative-anchor integrity |

Each indicator carries a green / amber / red state, an explicit threshold definition, and a dated reading log with sources.

### 2. Signal → action mapping (the runbook)

The single most important design decision in the system:

> **Only Layer I is authorized to trigger exposure reduction.**
> Layers III and IV, when red, trigger *pause additions and verify cash reserves* — never a reduction.

Layer I answers "should I be here." Layers III–IV answer "how rough will it be." Conflating the two is the most expensive error available to a retail participant, and the runbook makes it unavailable rather than merely discouraged.

Every state combination resolves to exactly one action. There is no cell in the table that reads "use judgment."

### 3. Operating cadence — four tiers

| Tier | Trigger | Scope |
|---|---|---|
| **Scheduled** | Every 14–15 days | Full 12-indicator sweep; refresh position band |
| **Event-driven** | Dates on a published catalyst calendar (earnings, central-bank decisions, listings) | Deep check on the 1–2 affected indicators only |
| **Exception-driven** | Four pre-defined tripwires (single-day drawdown threshold; narrative-anchor deterioration; export-control escalation; off-cycle capex revision) | Targeted check on the triggered indicator |
| **Weekly summary** | Friday close | Records price/volume facts and marginal changes only. **Explicitly cannot change any signal state.** |

The calendar is published months ahead. Event-driven checks are hard nodes: they run even if a scheduled sweep happened two days earlier, because the two answer different questions — "what did this event do to these indicators" versus "what drifted across all indicators."

### 4. Position band — two-step lookup

Signal-state combinations resolve mechanically to a percentage **band**, not a number. A separate technical-confirmation layer moves position *within* the band and can never breach it — it governs *when* to act, never *whether* to.

Bands are split per exposure, weighted by **pricing purity**: the exposure whose price is set by institutional/fundamental flow carries a higher base weight than the one set by leverage and retail sentiment, because the latter's price signal is a worse estimator of the thing it is supposed to estimate.

---

## Design principles

These are the transferable part. The indicator list is domain-specific; these are not.

**Separation of adjudication authority.** Each layer is scoped to one question and cannot rule on another layer's question. This is what prevents a volatility signal from being read as a value signal.

**Pre-registered falsification criteria.** For each thesis, the specific observation that would refute it is written into the system while the thesis is still favored. Example structure: *"if [leading order indicator] continues to decline AND [inventory turnover] deteriorates, the industry-side thesis is refuted"* — recorded as a named trigger line, not as an afterthought.

**Stage-gate renewal instead of open-ended holding.** Long-horizon exposure is not held "until the thesis plays out." It is renewed at explicit gates (each reporting period), with pass/fail criteria defined before the gate. Failing a gate compresses the position; it is a thesis decision, not a stop-loss.

**Attribution layering.** A price move is not evidence until its driver is classified. A worked example from the log: a sharp rally in the US optical-component complex was driven by *import-substitution policy expectations*, which is a **negative** read-through for China-listed suppliers, not a positive one. Same direction, opposite meaning. A rule now requires driver classification before any cross-market move is entered as evidence.

**Time-frame stop ≠ thesis invalidation.** The system distinguishes exiting because the thesis broke from exiting because personal holding-period tolerance ran out. Both are legitimate. Only the first is allowed to update the thesis. The rule is written as: *adjudication belongs to the indicator, not to patience.*

**Confidence tiering on external inputs.** Research produced outside the system — including by other AI tools — is admitted as *framework* but never as *evidence*. Numeric claims from such sources are tagged pending-verification and cannot move a signal state.

---

## Data quality controls

The system treats its own errors as defects to be fixed at the process level, not incidents to be corrected and forgotten. Two worked examples, both from the log:

**Static-estimate contamination.** A moving-average threshold was estimated once and reused for a week while the underlying series moved. This produced a falsely negative condition check for six consecutive sessions. Fix: the correction was applied *and* a permanent rule was written — *moving-average conditions must be recomputed from the current full series; reuse of a prior estimate is prohibited.*

**Intraday snapshot mistaken for a close.** A mid-session quote was logged as a closing value, understating a decline by several percentage points. Fix: a permanent timing rule — *US closing data may only be pulled on the following morning local time; intraday snapshots are provisional and must be revised.*

Other standing controls: every reading carries a source tag and retrieval date; corporate-action adjustments (splits, ex-rights) are declared and pre/post series are never concatenated; readings supplied by the operator from a live terminal are accepted directly, because the system's own retrieval lags the terminal by hours to days and re-checking them produces false "unverified" flags rather than accuracy.

---

## Technical implementation

- **Orchestration:** an AI agent in workspace mode, holding the system state as a single versioned Markdown document.
- **Data sources:** a Chinese financial terminal API for A-share pricing and financial statements; Yahoo Finance for the US comparables basket; a corporate-registry API for ownership structure.
- **State store:** plain Markdown under version discipline — every change dated, versioned, and attributed to a trigger. Deprecated content is struck through rather than deleted, so superseded judgments remain auditable.
- **Human boundary:** the system produces readings, signal states, and a band. The operator executes. The division is explicit in the document: *the system reduces the frequency of low-quality decisions; it does not make the high-quality ones.*

---

## Known limitations

Listed because a monitoring system that does not monitor itself is decoration.

1. **Indicator collinearity is untested.** Several Layer I and Layer II indicators plausibly measure the same underlying variable (is demand real). If so, "all green" may represent one signal counted four times, overstating evidential strength. A correlation audit of the signal-state time series is outstanding.
2. **No control arm.** There is no recorded benchmark — for example, a static allocation held unchanged over the same window. Without it, the system's marginal contribution over doing nothing cannot be measured, and a negative result would be the most valuable finding available.
3. **Execution and outcome are not closed-loop.** The system records recommended bands but not realized fills or returns, so "correct call, poor execution" cannot be separated from "incorrect call."
4. **One threshold was revised while unmet.** A volume-cooling threshold was replaced with an adaptive two-track definition at a time when the original condition had not been satisfied. The stated rationale was sound (the absolute threshold did not scale with fund size and was contaminated by subscription flow), and the revision did not loosen the current-state judgment. It remains a pattern worth constraining: a proposed guardrail requires any threshold revision to record whether the condition was met at the time of revision, with a cooling-off period when it was not.
5. **Maintenance cost scales with complexity.** The full cadence approaches a part-time workload. A complex system generates its own labor, and that labor can masquerade as research. Downgrading the light tiers under time pressure is a designed-in option, not a failure.

---

## What this project is actually about

The instrument is equities. The subject is **decision architecture under uncertainty with noisy, partially adversarial inputs** — tiered severity, runbook-mapped actions, scheduled versus event-driven versus exception-driven triggers, data lineage and confidence tiering, post-incident controls written back as permanent constraints, and pre-registered kill criteria.

Those components are domain-independent.
