---
title: "Drift detection and CloudWatch alarm (Week 7)"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

In week 7 the build picks up the Data Capture files written by week 6 and answers one question: *has the live traffic drifted away from the training distribution?* A custom SageMaker Processing Job does the work because the official Model Monitor schedule did not surface the needed feature-level metrics in time during testing.

#### 5.5.1 The drift Processing Job

The custom Processing Job runs hourly, triggered by an EventBridge rule on the Data Capture prefix.

![Custom Processing Job runs to detect drift](/fcaj-hcmut-template/images/5-Workshop/W7-01a-custom-processing-job.png)

The script:

1. Reads the latest Data Capture JSONL files from S3.
2. Loads the `baseline/` statistics produced by the week-2 Processing Job.
3. For numeric features, computes **standardized mean shift** between baseline and current; flag drift if `|shift| > 0.5`.
4. For categorical features, computes **total variation distance** between baseline and current distributions; flag drift if `TVD > 0.20`.
5. Writes a drift report (`reports/drift/<run-id>.json`) and publishes two CloudWatch metrics under namespace **`Custom/HeartRisk`**:
   - `DriftDetected` — 0 or 1.
   - `DataQualityViolationCount` — count of features that drifted.

> These thresholds are illustrative PoC rules, not a clinical or production statistical standard. They are documented here so future readers know what was checked and why these specific values were chosen.

#### 5.5.2 Drift report — example run

![Drift report shows 6 of 20 features drifted](/fcaj-hcmut-template/images/5-Workshop/W7-02-drift-report.png)

The example run shows:

- Baseline rows: 4,900
- Current rows: 7,000
- Features checked: 20
- Violations: 6
- Drift detected: true

The six drifted features: `age`, `resting_bp`, `cholesterol`, `bmi`, `smoking_status`, `stress_level`. The same six appear in the detailed feature list (not shown above — moved to appendix in the report).

#### 5.5.3 CloudWatch metrics

![DriftDetected and DataQualityViolationCount published under Custom/HeartRisk](/fcaj-hcmut-template/images/5-Workshop/W7-04-custom-metrics.png)

Values visible:

- `DriftDetected = 1`
- `DataQualityViolationCount = 6`
- `Namespace = Custom/HeartRisk`

These two metrics are the contract the alarm is built on.

#### 5.5.4 CloudWatch Alarm

![CloudWatch Alarm transitions to ALARM when DriftDetected reaches 1](/fcaj-hcmut-template/images/5-Workshop/W7-05-custom-alarm.png)

The alarm uses:

- Statistic: **Maximum**
- Threshold: **1**
- Comparison: **GreaterThanOrEqualToThreshold**
- Missing data: **ignore** (the custom Processing Job only publishes on completion, so missing data is normal)

When `DriftDetected = 1` is published, the alarm transitions to `ALARM` and a notification fires through the SNS topic created in 5.2.

#### What this gives the project

- A working alert chain on a 200 USD budget: Data Capture → EventBridge hourly rule → custom Processing Job → CloudWatch metrics → CloudWatch alarm → SNS.
- No managed Model Monitor required for this scope, which keeps the cost predictable.
- The metrics namespace and the alarm threshold form a clear contract for what "drift detected" means — easy to extend later when managed Model Monitor becomes available.

#### Cost

Each hourly run of the Processing Job costs a few cents (a `ml.m5.large` for ~2 minutes). The CloudWatch custom metrics are also a few cents each per month. Total is well under 5 USD for the full 8 weeks.
