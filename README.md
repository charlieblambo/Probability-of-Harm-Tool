# P(harm | defect) Risk Reference Table

An interactive reference table for medical device risk assessment. Calculates the 95% upper confidence bound on **P(harm | defect)** across combinations of sample sizes and autoclave cycles, assuming zero failures observed.

Built to support ISO 14971 risk management arguments for surgical devices with moisture ingress defects.

---

## Live Tool

👉 [https://charlieblambo.github.io/Probability-of-Harm-Tool](https://charlieblambo.github.io/Probability-of-Harm-Tool)

---

## What It Does

Given a defect occurrence probability and a maximum allowable risk, the table shows whether a given test configuration (N samples × cycles completed) is sufficient to statistically demonstrate that the probability of patient harm is below the required threshold.

Each cell shows the **95% UCB on P(harm | defect)**. Hover over any cell to see the resulting **P(loss of vision)** value.

Color coding:
- 🟢 **Green** — passes risk requirement
- 🟡 **Yellow** — marginal (within 10% of threshold)
- 🔴 **Red** — fails risk requirement

---

## Inputs

| Parameter | Description | Default |
|---|---|---|
| Visual Void Rate (%) | Rate of visually detectable voids in production | 1.38% |
| CT Confirmation Rate (%) | Fraction of visual voids confirmed as true voids by CT scan | 57% |
| Risk Requirement (%) | Maximum allowable P(loss of vision) | 0.50% |
| Design Life (cycles) | Number of autoclave cycles in the device's design life | 20 |
| Max Cycles (table) | Upper bound of the cycle axis displayed in the table | 24 |
| Max Samples (table) | Upper bound of the sample size axis displayed in the table | 10 |

All inputs update the table in real time.

---

## How It Works

### Defect Probability
```
P(defect) = Visual Void Rate × CT Confirmation Rate
```

### Maximum Allowable Conditional Harm Probability
```
P(harm | defect) < Risk Requirement / P(defect)
```

### Equivalent Samples
Partial cycle completion is accounted for by scaling to equivalent full-design-life units:
```
n_equiv = N samples × (cycles completed / design life)
```

### 95% Upper Confidence Bound (0 failures)
```
UCB = 1 − 0.05^(1 / n_equiv)
```

### Final Risk
```
P(loss of vision) = P(defect) × UCB
```

A test passes if `P(loss of vision) < Risk Requirement`.

---

## Statistical Basis

- **Method:** One-sided binomial confidence interval
- **Confidence level:** 95%
- **Assumption:** Zero failures observed across all samples
- **Any failure invalidates these bounds** and requires recalculation using the Clopper-Pearson exact method

---

## Limitations

- Valid only for **zero-failure** test outcomes
- Assumes failure probability scales **linearly** with cycle count (constant hazard rate)
- Does not account for multiple failure modes or conditional dependencies between samples
- Sample sizes below n=4 are unlikely to pass within a 20-cycle design life at the default parameters

---

## Context

Developed to support risk assessment for camera module defects in surgical devices, where gap voids allow moisture contact with PCBAs during autoclave sterilization, with potential for electrical failure and intraoperative loss of vision.
