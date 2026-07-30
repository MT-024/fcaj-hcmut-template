---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 2 Objectives
  - Tasks to be carried out this week
  - Week 2 Achievements
---
{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 2 Objectives:

* Understand the heart-attack dataset schema (raw + processed) and the data-quality constraints from the brief.
* Build a reproducible sklearn preprocessing pipeline that fits on train only (no leakage).
* Get hands-on with the AWS console: create S3 bucket, IAM role for SageMaker, configure single region `ap-southeast-1`.


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Read `Nội dung.docx` brief carefully: 8-week plan, budget cap 200 USD, IAM scope, S3 paths <br> - Skim `CLAUDE.md` constraints (data schema, processed splits 80/10/10) | 08/12/2025 | 08/12/2025 |
| 3   | - Inspect `raw/heart_attack_dataset.csv` (7000 rows × 22 cols) <br> - Identify missing-value columns (smoking_status, cholesterol, oldpeak, etc.) <br> - Verify schema constraints: age 18–100, fasting_blood_sugar ∈ {0,1}, num_major_vessels 0–3 | 08/13/2025 | 08/13/2025 |
| 4   | - Write `preprocess.py` with sklearn ColumnTransformer: StandardScaler on numeric, OneHotEncoder on nominal, OrdinalEncoder on ordinal, passthrough on binary <br> - Fit ONLY on train split to avoid leakage | 08/14/2025 | 08/15/2025 |
| 5   | - Generate `processed/{train,val,test}_processed.csv` <br> - Verify column prefixes: `num__`, `norm_num__`, `bin__`, `nom__`, `ord__` <br> - Save fitted `preprocessor.joblib` for later reuse | 08/15/2025 | 08/16/2025 |
| 6   | - Create S3 bucket `s3://heart-risk-mlops-<account-id>/` in `ap-southeast-1` <br> - Create SageMaker execution IAM role with least-privilege (S3 + logs + SageMaker + PassRole only) <br> - Add lifecycle rule: logs/artifacts expire after 30 days | 08/16/2025 | 08/16/2025 |


### Week 2 Achievements:

* Understood the brief constraints: budget cap 200 USD, single region, no GPU, no NAT Gateway.
* Built a sklearn preprocessing pipeline (ColumnTransformer) with 5 transformer groups, fit on train only.
* Generated 3 processed CSVs (5600/700/700 rows) with proper column prefixes.
* Provisioned S3 bucket + IAM role with lifecycle rule — first cost-saving measure.


### References:

* https://scikit-learn.org/stable/modules/compose.html#columntransformer-for-heterogeneous-data
* https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html
