# CTS Alpha Validation --- Task 009.2.2

## Controlled Statistical Analysis --- 2000-bar Non-overlapping Dataset

### Dataset

-   Source: `pine-logs-CTS (4).csv`
-   Samples: 166
-   Non-overlap spacing: 12 bars
-   Forward horizon: +1 / +3 / +6 / +12 bars
-   Market states: RANGE 82, TRANSITION 31, PULLBACK 26, TRENDING 23,
    BREAKOUT 4
-   Directions: SHORT 83, LONG 51, NEUTRAL 32

### Validation Infrastructure

  Check               Result

------------------- --------

  Sample count        PASS
  Anchor spacing      PASS
  Non-overlap rule    PASS
  Controlled labels   PASS
  Forward horizon     PASS

The validation infrastructure is considered frozen after Commit 0041.2.

------------------------------------------------------------------------

## Finding 009.2.2-S01 --- Structure Alignment

### Controlled comparison

Control variables:

-   Market State = RANGE
-   Direction = SHORT

Sample:

- ALIGNED: N = 47

- CONFLICT: N = 30

  Horizon     ALIGNED Mean ATR   CONFLICT Mean ATR   Difference

  --------- ------------------ ------------------- ------------

  +1                    +0.180              +0.031       +0.149
  +3                    +0.032              -0.175       +0.207
  +6                    +0.573              -0.082       +0.655
  +12                   +0.757              -0.516       +1.273

At +12 bars:

-   ALIGNED median: +0.601 ATR
-   CONFLICT median: -0.771 ATR
-   ALIGNED positive rate: 59.6%
-   CONFLICT positive rate: 30.0%
-   Mann--Whitney U two-sided p ≈ 0.0135

### Interpretation

The full-sample result supports the hypothesis that Structure Alignment
contains directional information after controlling for Market State and
Direction.

However, statistical significance on the pooled sample is not sufficient
for deployment because the observations are time-ordered and market
regimes are not identically distributed.

------------------------------------------------------------------------

## Finding 009.2.2-S02 --- Temporal Stability Failure

The RANGE + SHORT sample was split chronologically into three
approximately equal groups.

--------------------------------------------------------------------------------

  Period   Date Range     ALIGNED N     ALIGNED  CONFLICT N    CONFLICT Ordering
                                           Mean                    Mean 

-------- ------------ ----------- ----------- ----------- ----------- ----------

  A        2025-09-25 →          16      -0.279          10      +0.648 REVERSED
           2025-12-20                                                   

  B        2025-12-24 →          13      +0.893          12      -0.818 EXPECTED
           2026-04-05                                                   

  C        2026-05-19 →          18      +1.579           8      -1.519 EXPECTED

           2026-08-05                                                   
  --------------------------------------------------------------------------------

### Conclusion

The Structure Alignment effect is NOT temporally stable across the full
observation period.

The first period produces the opposite ordering:

ALIGNED \< CONFLICT

while the later two periods produce:

ALIGNED \> CONFLICT

Therefore the previous classification of Structure Alignment as a
"Strong Candidate Feature" must be downgraded.

### Current classification

**CANDIDATE --- TEMPORALLY UNSTABLE**

Not:

-   Proven Edge
-   Stable Edge
-   Production Feature

### Engineering decision

**DO NOT MODIFY Structure Engine.**

Do not change:

-   Structure formulas
-   Structure thresholds
-   Structure weights
-   Structure Gate thresholds
-   Direction Engine weights
-   Execution Guard logic

------------------------------------------------------------------------

## Finding 009.2.2-R01 --- Risk

Risk remains inconclusive.

The current validation does not establish a stable monotonic
relationship between Risk State and future directional return after
market-state control.

**Decision: HOLD.**

------------------------------------------------------------------------

## Finding 009.2.2-E01 --- Entry

Entry state does not currently demonstrate a stable independent
predictive relationship after controlled stratification.

**Decision: HOLD.**

------------------------------------------------------------------------

## Finding 009.2.2-X01 --- Execution Guard

APPROVE remains too rare for reliable statistical evaluation.

The current dataset contains only a small number of APPROVE
observations. This is insufficient to estimate live execution quality.

**Decision: HOLD.**

------------------------------------------------------------------------

# Validation Decision

Task 009.2.2 is **PARTIAL PASS**:

-   Controlled statistical framework: PASS
-   Pooled Structure Alignment separation: SUPPORTIVE
-   Temporal stability: FAIL
-   Production Edge qualification: FAIL

The next stage must therefore focus on **Edge Stability / Regime
Conditioning**, not Engine optimization.

## Next Task

### Task 009.3 --- Edge Stability + Regime Conditioning

Objectives:

1.  Determine whether the Structure Alignment effect is
    regime-dependent.
2.  Compare results by calendar period and Market State.
3.  Test whether the effect survives when the candidate rule is frozen
    before the evaluation period.
4.  Separate discovery-period evidence from out-of-sample evidence.
5.  Establish a minimum evidence threshold before any Engine
    modification is permitted.

**No trading logic changes until Task 009.3 is completed.**