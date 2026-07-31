---
title: "Data preprocessing (Week 2)"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

In week 2 the goal is to take the raw 7,000-row CSV sitting in S3 and turn it into a clean `train / validation / test` set with a fitted preprocessor — all on managed infrastructure, with no local notebook state.

The processing step is expressed as a SageMaker **Processing Job** that reads `raw/heart_attack_risk.csv` from the project bucket, validates the schema, imputes missing values, encodes the categorical features, splits 70/15/15 with stratification, fits the preprocessor **only on the train split**, and writes all outputs back to S3.

#### Processing Job completed

![SageMaker Processing Job completes successfully](/fcaj-hcmut-template/images/5-Workshop/W2-01-processing-completed.png)

The status `Completed` is the proof that preprocessing ran on managed infrastructure, not in a local Jupyter kernel. The script that ran inside the container is the same script that later gets attached to the Pipeline's `ProcessingStep`.

#### Data quality report

![Processing Job log with rows / columns / split counts / missing count](/fcaj-hcmut-template/images/5-Workshop/W2-02-processing-log.png)

Key numbers printed at the end of the job:

- Rows: 7,000
- Columns: 22
- Train: 4,900
- Validation: 1,050
- Test: 1,050
- Positive rate: 0.42 (kept across all three splits)
- Processed features: 36 (after one-hot encoding)
- Missing after processing: 0
- Preprocessor fit scope: `train_only`

The fact that the positive rate is preserved across splits is evidence that stratification is working. The fact that the preprocessor is fit only on `train` is the safeguard against data leakage from validation/test into the model.

#### Processed S3 layout

![S3 bucket with the processed splits and artifacts organized by prefix](/fcaj-hcmut-template/images/5-Workshop/W2-03-processed-s3.png)

After the job finishes, the bucket has this layout:

```
s3://heart-risk-mlops/
    raw/
        heart_attack_risk.csv
    processed/
        train/train.csv
        validation/validation.csv
        test/test.csv
        raw_split/                ← stratification check artifact
    artifacts/
        preprocessor/             ← fitted sklearn ColumnTransformer
    baseline/                     ← stats used by drift detection
    reports/                      ← evaluation metrics, drift reports
    drift/                        ← custom Processing Job input/output
```

This layout is what every later step (Training, HPO, Evaluation, Pipeline, Drift) depends on. Changing the prefix later means re-running the Processing Job, so getting the layout right in week 2 saves rework in weeks 3–7.

#### What this enables

After week 2 the project has the deterministic inputs every later step needs:

- Three CSV files in fixed locations.
- A fitted preprocessor artifact that any new inference request can reuse.
- A baseline statistics file used by the drift Processing Job in week 7.

Cost impact: this Processing Job runs once for ~10 minutes on a `ml.m5.large`, well under 1 USD.
