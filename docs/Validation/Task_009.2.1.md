# CTS Alpha Validation — Task 009.2.1

## Finding 009.2.1-S01 — Structure Alignment Candidate Edge

### Observation

Structure Alignment 在两个不同的验证框架中表现出一致的方向性区分：

1. Exploratory Validation V1
2. Non-overlapping Controlled Validation V2

在 Non-overlapping Validation V2 中，控制：

- Market State = RANGE
- Direction = SHORT

得到：

| Structure Alignment | N | Mean +12 ATR | Median +12 ATR | Positive % |
|---|---:|---:|---:|---:|
| ALIGNED | 47 | +0.757 | +0.601 | 59.57% |
| CONFLICT | 30 | -0.516 | -0.771 | 30.00% |

Mean difference:

+1.273 ATR

Median difference:

+1.372 ATR

### Statistical Note

The result is exploratory evidence only.

The current sample is non-overlapping in the forward observation window, but
non-overlap does not guarantee full statistical independence because adjacent
samples are temporally contiguous and may share market-regime characteristics.

A two-sided Mann-Whitney U test on the current RANGE + SHORT sample produced
p ≈ 0.0135.

This result is considered supportive evidence, not final proof.

### Interpretation

Structure Alignment is currently classified as:

STRONG CANDIDATE FEATURE

It is NOT classified as:

PROVEN EDGE

### Decision

DO NOT MODIFY Structure Engine.

Continue collecting validation evidence.

Required next validation:

- Larger controlled sample
- Multiple market states
- Multiple directions
- Stability across +1/+3/+6/+12 bar horizons
- Out-of-sample / walk-forward validation

### Status

Candidate Edge:
    ACTIVE

Engine Modification:
    NO

Confidence:
    EXPLORATORY