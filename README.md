# Tiered Monitoring & Decision System for Sector Exposure

A personal decision-support system for equity exposure to the AI-hardware supply chain — specifically **optical communications** and **semiconductor capital equipment** in the China A-share market.

The system does not pick stocks and does not generate trades. It converts a stream of noisy inputs into a small set of discrete signal states, maps those states to pre-defined actions, and forces every judgment to be made against criteria written down *before* the evidence arrived.

**Status:** in production. Current version **v3.1** (October 2026), with every change dated and versioned since v2.0 (July 2026).
Position sizes, entry prices, and returns are deliberately excluded from this document.
This repository documents the system's design. The system itself runs as a versioned document inside an agent workspace; no source code is published here.

**What changed in v3.x:** the system was rebuilt from a one-directional risk-control system into a **two-directional decision system**. Every rule that can reduce exposure now has a mirror rule that can restore it, written in advance and judged by the same standard. See [Section 3](#3-two-directional-rules-v30) and the [October 2026 case note](#case-note-october-2026).

---

## The problem it was built to solve

Three failure modes, all of them mine before the system existed:

1. **High-frequency, low-quality decisions.** Watching an intraday screen produces the *feeling* of diligence and the *substance* of noise trading. Decision quality is not a function of decision frequency.
2. **Confusing volatility risk with value risk.** A crowded-positioning warning and a demand-collapse warning feel identical in the moment. They call for opposite responses. Without a rule that separates them in advance, the loud one wins.
3. **Retroactive reinterpretation.** Absent a falsification criterion written down beforehand, any adverse outcome can be re-explained. A thesis that cannot be wrong is not a thesis.

v3.0 added a fourth, discovered in production:

4. **A system that can only sell.** If every pre-committed trigger points down, the system will protect capital in a crash and then hand over its position at the low, because nothing in it is allowed to buy. Controlling the size of losses is not the same as having a high hit rate.

The system's job is to make the first mode rarer, the second structurally impossible, the third visible when it happens, and the fourth symmetric.

---

## Architecture

### 1. Indicator taxonomy — eleven indicators and two sub-factors, by question adjudicated

Indicators are grouped not by subject matter but by **what question they adjudicate**, and each carries an explicit authority level:

| Layer | Question it answers | Authority | Example indicators |
|---|---|---|---|
| **L0 Determinative** | Should this exposure exist at all? | Red → reduction review (both sleeves) | Hyperscaler capex guidance; results delivery by chain leaders |
| **L0.5 Sleeve-specific determinative** | Should the equipment sleeve exist? | Red → reduction review (equipment sleeve only) | Domestic compute-chip and equipment chain |
| **L1 Demand validation** | Is the revenue actually arriving? | Informs the band; cannot reduce on its own | Advanced packaging; memory pricing cycle; domestic model monetization |
| **L2 Macro gate** | Is the discount-rate environment hostile? | Red → pause additions, check cash. Never reduces | Central-bank path, with two sub-factors (yen carry; energy-inflation pass-through) |
| **L3 Crowding gate** | How leveraged and crowded is the trade? | Same as L2 | Margin balances, turnover, ETF premium, breadth |
| **L4 Tail and anchor** | Could the story break from outside? | Escalation to a defined level → reduction review | Export-control state; compute-rental scarcity |

Each indicator carries a green / amber / red state, an explicit threshold definition, and a dated reading log with sources. One background indicator (overseas AI monetization) is tracked but mapped to no action. Full definitions: [`docs/indicator-framework.md`](docs/indicator-framework.md).

**Definition and state are stored separately.** Indicator entries hold only definitions, triggers and history; the current state lives in the version header and the latest check. This was fixed after several entries were found to show stale states months old because definition and state had been written in the same place.

### 2. Signal → action mapping (the runbook)

The single most important design decision in the system:

> **Only determinative layers are authorized to trigger exposure reduction.**
> Macro and crowding gates, when red, trigger *pause additions and verify cash reserves* — never a reduction.

L0 answers "should I be here." L2–L3 answer "how rough will it be." Conflating the two is the most expensive error available to a retail participant, and the runbook makes it unavailable rather than merely discouraged.

Every state combination resolves to exactly one action. There is no cell in the table that reads "use judgment."

**Compound-trigger dual track.** When a pre-written stress scenario is realized (for example, a macro shock coinciding with a technical break in the comparable basket), the system does not sell at market. It first asks how far away the next fundamental checkpoint is. If it is within about two weeks, two tracks are armed in parallel: an *information track* (if the checkpoint disappoints, execute the next day; if it beats, cancel) and a *technical floor* (a closing break of a named level executes immediately; an intraday break does not count).

### 3. Two-directional rules (v3.0)

v3.0 rewrote the system's philosophy into five principles:

1. **Hit rate first; payoff serves hit rate.** The objective moved from "control drawdown" to "hit rate × expected value". During a decline the legal actions went from two (cut / hold) to three (cut / hold / buy). A buy is legitimate only when a pre-committed rule fires, exactly as a cut is.
2. **Symmetry.** Every reduction trigger has a mirror re-entry trigger. The craft used to judge a valid break (closing basis, high-volume reclaim voids the break, a second break is not forgiven) applies unchanged to judging a valid recovery. Rebounds after a cut and further declines after a buy are both recorded, in two separate ledgers, and neither may be deleted.
3. **Same evidence, upgraded output.** "Determinative layer all green + crowding layer showing panic exhaustion" used to output only "do not reduce". It now outputs "highest-probability buying window".
4. **Hit rate decides whether; payoff decides when and how much.** Fundamentals are the source of hit rate; the technical layer is the source of timing. The macro gate freezes left-side scaling-in, but not right-side confirmation: a hostile rate path compresses the multiple, it does not cancel a confirmed earnings beat.
5. **No rule changes under fire.** Rules are revised only in writing and when calm. Any buy outside the three rules below is classed as an improvised trade, the same offence as catching a falling knife without a rule.

The three re-entry rules:

| Rule | Fires when (all conditions) | Action | Limit and exit |
|---|---|---|---|
| **B1 Information-triggered** | A fundamental checkpoint beats (chain leader results or guidance) **and** the first session after it does not make a new low | Review restoring to the midpoint of the pre-trigger band | Checkpoint later falsified → freeze, handle under cut rules |
| **B2 Right-side confirmation** | A key level is **reclaimed on a closing, high-volume basis** **and** no determinative indicator is red | Review restoring; may proceed under a red macro gate, capped one tier lower | Closing break again within three sessions → "false reclaim", restored portion exits |
| **B3 Left-side scaling** | Determinative layer all green **and** a defined panic-exhaustion pattern **and** macro gate not red | Scale in by pre-set steps (one step per further 8–10% decline) up to the top of the current band | Entire rule frozen while the macro gate is red |

Priority B1 > B2 > B3. Only one net direction may be under review at a time; conflicts are settled by the determinative layer. The operator still pulls every trigger.

### 4. Operating cadence — four tiers

| Tier | Trigger | Scope |
|---|---|---|
| **Scheduled** | Every 14–15 days | Full sweep of all indicators plus the timing layer; refresh position band |
| **Event-driven** | Dates on a published catalyst calendar (earnings, central-bank decisions, listings) | Deep check on the 1–2 affected indicators only |
| **Exception-driven** | Four pre-defined tripwires (single-day drawdown threshold; scarcity-anchor deterioration; export-control escalation; off-cycle capex revision) | Targeted check on the triggered indicator |
| **Weekly summary** | Friday close | Records price/volume facts and marginal changes only. **Explicitly cannot change any signal state.** |

The calendar is published months ahead. Event-driven checks are hard nodes: they run even if a scheduled sweep happened two days earlier, because the two answer different questions — "what did this event do to these indicators" versus "what drifted across all indicators."

### 5. Position band — three-step lookup

1. **Tier.** Signal-state combinations resolve mechanically to one of five tiers (from "small addition window" down to "reduction review"), each with a percentage **band**, not a number, under a hard ceiling on total exposure.
2. **Split.** Each tier's band is split into a sub-band per exposure, weighted by **pricing purity**: the exposure whose price is set by institutional/fundamental flow carries a higher base weight than the one set by leverage and retail sentiment, because the latter's price signal is a worse estimator of the thing it is supposed to estimate.
3. **Position within the band.** A technical-confirmation layer (turnover, own-volume behaviour, price stabilisation and breadth) moves position *within* the sub-band and can never breach it — it governs *when* to act, never *whether* to. For the optical sleeve, whose pricing anchor is North American, a US comparable-basket stabilisation factor must also pass before position can move up.

Since v3.0, moving down a tier goes through the cut rules and moving up goes through B1–B3. The upper tiers stopped being decorative.

---

## Design principles

These are the transferable part. The indicator list is domain-specific; these are not.

**Separation of adjudication authority.** Each layer is scoped to one question and cannot rule on another layer's question. This is what prevents a volatility signal from being read as a value signal.

**Pre-registered falsification criteria.** For each thesis, the specific observation that would refute it is written into the system while the thesis is still favored. Example structure: *"if [leading order indicator] continues to decline AND [inventory turnover] deteriorates, the industry-side thesis is refuted"* — recorded as a named trigger line, not as an afterthought.

**Stage-gate renewal instead of open-ended holding.** Long-horizon exposure is not held "until the thesis plays out." It is renewed at explicit gates (each reporting period), with pass/fail criteria defined before the gate. Failing a gate compresses the position; it is a thesis decision, not a stop-loss.

**Attribution layering.** A price move is not evidence until its driver is classified. A worked example from the log: a sharp rally in the US optical-component complex was driven by *import-substitution policy expectations*, which is a **negative** read-through for China-listed suppliers, not a positive one. Same direction, opposite meaning. A rule now requires driver classification before any cross-market move is entered as evidence.

**Time-frame stop ≠ thesis invalidation.** The system distinguishes exiting because the thesis broke from exiting because personal holding-period tolerance ran out. Both are legitimate. Only the first is allowed to update the thesis. The rule is written as: *adjudication belongs to the indicator, not to patience.*

**Two ledgers, kept apart.** The discipline ledger records whether a rule was followed; the price ledger records what happened next. A cut followed by a rebound is a premium paid, not proof the rule was wrong. Merging the two ledgers is how rules get abandoned after one bad outcome.

**Calendar effects adjust weight, never action.** Seasonal patterns (pre-holiday de-risking, quarter-end liquidity, spring rally) are graded A/B/C by evidence quality and are only allowed to up-weight or down-weight how a crowding or timing reading is interpreted. They never change a signal threshold and never trigger a position change. One commonly cited pattern failed statistical testing and is recorded as grade C: never used.

**Confidence tiering on external inputs.** Research produced outside the system — including by other AI tools — is admitted as *framework* but never as *evidence*. Numeric claims from such sources are tagged pending-verification and cannot move a signal state.

**Severity grading for policy events.** Export-control news is graded before it is acted on: procurement-level restrictions are noise-grade and move nothing; commercial import bans or entity listings, once in force, are treated as the red state.

---

## Data quality controls

The system treats its own errors as defects to be fixed at the process level, not incidents to be corrected and forgotten. Two worked examples, both from the log:

**Static-estimate contamination.** A moving-average threshold was estimated once and reused for a week while the underlying series moved. This produced a falsely negative condition check for six consecutive sessions. Fix: the correction was applied *and* a permanent rule was written — *moving-average conditions must be recomputed from the current full series; reuse of a prior estimate is prohibited.*

**Intraday snapshot mistaken for a close.** A mid-session quote was logged as a closing value, understating a decline by several percentage points. Fix: a permanent timing rule — *US closing data may only be pulled on the following morning local time; intraday snapshots are provisional and must be revised.*

**Source fallback chain.** No single data-source outage may block a check. Retrieval degrades in a fixed order — financial-terminal API → market-data API → multi-source media cross-check by a sub-agent → operator screenshots — and every reading is tagged with the tier actually used. When tiers conflict, the more official source wins (exchange or company filing > data vendor > media), and the conflict is logged.

Other standing controls: every reading carries a source tag and retrieval date; corporate-action adjustments (splits, ex-rights) are declared and pre/post series are never concatenated; readings supplied by the operator from a live terminal are accepted directly, because the system's own retrieval lags the terminal by hours to days and re-checking them produces false "unverified" flags rather than accuracy; results are logged without same-day price reactions, so a good number on a down day is not misremembered as a bad one.

---

## Case note: October 2026

A compressed account, without position sizes, of the period that produced v3.0.

- **Before:** the macro gate had been red since a September rate hike, and a stress scenario ("macro shock plus a break in the US comparable basket") had been written down weeks in advance.
- **Scenario realized:** the US optical basket broke down while a strong fundamental report was ignored by the market. The system did not sell at market. It armed the dual track: the third-quarter reports, two weeks away, as the information track, and a named closing level as the technical floor.
- **Execution:** the technical floor broke on a closing basis and the reduction was executed. The next session rebounded sharply. Both facts went into separate ledgers: rule followed; premium paid at the low.
- **Checkpoint:** the chain leader then reported results well above expectations, with next-year demand converted from forecasts into firm orders. Under v2.x rules, nothing in the system could act on that.
- **Reform:** that gap — correct risk control, no legal way back in — is what v3.0 closed. The first live test of B1 was armed the same night.

The system protected capital through the decline, which was its design. It also showed that a rule set with only one direction systematically gives up position at the worst point, which was its flaw.

---

## Technical implementation

- **Orchestration:** an AI agent in workspace mode, holding the system state as a single versioned Markdown document, with web search for news and filings.
- **Data sources:** a Chinese financial terminal API for A-share pricing and financial statements; Yahoo Finance for a US comparables basket (index, hyperscalers, accelerator, optical, networking, power/cooling and memory names); a corporate-registry API for ownership structure.
- **State store:** plain Markdown under version discipline — every change dated, versioned, and attributed to a trigger. Deprecated content is struck through rather than deleted, so superseded judgments remain auditable. Periodic compaction passes archive dated logs into summaries, with full pre-compaction backups kept.
- **Human boundary:** the system produces readings, signal states, and a band. The operator executes. The division is explicit in the document: *the system reduces the frequency of low-quality decisions; it does not make the high-quality ones.*

---

## Known limitations

Listed because a monitoring system that does not monitor itself is decoration.

1. **Indicator collinearity is untested.** Several determinative and demand indicators plausibly measure the same underlying variable (is demand real). If so, "all green" may represent one signal counted several times, overstating evidential strength. A correlation audit of the signal-state time series is outstanding. The results indicator is also shared by both exposures, so on that axis they are fully correlated and provide no diversification.
2. **No control arm.** There is no recorded benchmark — for example, a static allocation held unchanged over the same window. Without it, the system's marginal contribution over doing nothing cannot be measured, and a negative result would be the most valuable finding available.
3. **Execution and outcome are not closed-loop.** The system records recommended bands but not realized fills or returns, so "correct call, poor execution" cannot be separated from "incorrect call."
4. **The re-entry rules have no track record.** B1–B3 entered production in late October 2026. Their thresholds are reasoned, not tested.
5. **One lamp sets the ceiling.** In a hiking cycle the macro gate alone pins the optical sleeve near the bottom of its band. This is deliberate conservatism, recorded so the operator knows the ceiling is set by a single indicator.
6. **One threshold was revised while unmet.** A volume-cooling threshold was replaced with an adaptive two-track definition at a time when the original condition had not been satisfied. The rationale was sound and the revision did not loosen the current-state judgment, but the pattern needs a guardrail: any threshold revision must record whether the condition was met at the time, with a cooling-off period when it was not.
7. **Maintenance cost scales with complexity.** The full cadence approaches a part-time workload. A complex system generates its own labor, and that labor can masquerade as research. Downgrading the light tiers under time pressure is a designed-in option, not a failure.

---

## What this project is actually about

The instrument is equities. The subject is **decision architecture under uncertainty with noisy, partially adversarial inputs** — tiered severity, runbook-mapped actions in both directions, scheduled versus event-driven versus exception-driven triggers, data lineage and confidence tiering, post-incident controls written back as permanent constraints, and pre-registered kill criteria.

Those components are domain-independent.
