---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 3 Objectives
  - Tasks to be carried out this week
  - Week 3 Achievements
---
{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 3 Objectives:

* Train XGBoost baseline on processed train data.
* Achieve target metrics: ROC-AUC ≥ 0.84, Recall ≥ 0.65 on validation set.
* Use pinned hyperparameters from brief (no HPO yet, that's week 4).


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Re-read brief: XGBoost config pinned (objective=binary:logistic, eval_metric=auc, num_round=150, max_depth=5, eta=0.1, subsample=0.8, colsample_bytree=0.8, min_child_weight=2) <br> - Load processed train/val, verify shape, target balance | 19/08/2025 | 19/08/2025 |
| 3   | - Write `train.py` for SageMaker Training Job: load preprocessor.joblib, train XGBoost with pinned params, save `model.tar.gz` to S3 <br> - Choose `ml.t3.medium` instance (test if 2 vCPU + 4 GB RAM is enough) | 20/08/2025 | 21/08/2025 |
| 4   | - Launch first Training Job via SageMaker Python SDK <br> - Verify training job finishes without error | 21/08/2025 | 21/08/2025 |
| 5   | - Load model artifact, evaluate on val set: ROC-AUC, Recall, F1, Precision, FNR, Accuracy <br> - Generate confusion matrix + ROC curve (matplotlib, save PNG) | 22/08/2025 | 23/08/2025 |
| 6   | - Verify AUC ≥ 0.84 and Recall ≥ 0.65 (brief requirements) <br> - If metrics OK, document instance type + training time + cost for week 3 | 23/08/2025 | 23/08/2025 |


### Week 3 Achievements:

* Trained XGBoost baseline on `ml.t3.medium` — confirmed small dataset does NOT need bigger instance.
* Achieved target metrics on validation set:

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 3 - XGBoost training                     -->
<!-- Cần điền:                                                       -->
<!--   - Training job name (vd: heart-risk-train-2026-xx-xx)         -->
<!--   - Training time thực tế (vd: 4 phút 12 giây)                  -->
<!--   - AUC trên validation set (vd: 0.8612)                        -->
<!--   - Recall (vd: 0.679)                                          -->
<!--   - F1 (vd: 0.703)                                              -->
<!--   - Cost (vd: 0.0035 USD)                                       -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->

| Metric | Value | Note |
| ------ | ----- | ---- |
| Training time | TBD | from SageMaker training job |
| AUC (validation) | TBD | target ≥ 0.84 |
| Recall (validation) | TBD | target ≥ 0.65 |
| F1 (validation) | TBD | target ≥ 0.70 |
| Cost this week | TBD USD | from billing dashboard |


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html
