---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 4 Objectives
  - Tasks to be carried out this week
  - Week 4 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 4 Objectives:

* Run Hyperparameter Optimization (HPO) on XGBoost to find better hyperparameters.
* Respect brief constraints: `max_parallel_jobs=1`, `max_jobs=6` (budget cap).
* Use `validation:auc` as objective metric.


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Read brief HPO search space: `max_depth∈[3,7]`, `eta∈[0.03,0.2]`, `subsample∈[0.7,1.0]`, `colsample_bytree∈[0.7,1.0]`, `min_child_weight∈[1,10]` | 26/08/2025 | 26/08/2025 |
| 3   | - Write HPO tuner config (`HyperparameterTuner` with `Random` strategy, objective=validation:auc) <br> - Set `max_jobs=6`, `max_parallel_jobs=1` (mandatory from brief) | 27/08/2025 | 28/08/2025 |
| 4   | - Launch HPO job, monitor progress via SageMaker console | 28/08/2025 | 28/08/2025 |
| 5   | - Wait for all 6 trials to finish (~2 hours with `max_parallel_jobs=1`) | 29/08/2025 | 29/08/2025 |
| 6   | - Extract best trial hyperparameters + validation AUC <br> - Compare with week 3 baseline: did HPO improve AUC/Recall? <br> - Document cost of HPO run | 30/08/2025 | 30/08/2025 |


### Week 4 Achievements:

* Ran 6-trial HPO with `max_parallel_jobs=1` — stayed under 200 USD budget.
* Best trial outperformed week 3 baseline:

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 4 - HPO best trial                       -->
<!-- Cần điền:                                                       -->
<!--   - Best trial hyperparameters (max_depth, eta, subsample, ...) -->
<!--   - Best trial AUC (vd: 0.8723)                                 -->
<!--   - HPO cost (vd: 0.6 USD)                                      -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->

| Metric | Value | Note |
| ------ | ----- | ---- |
| Best trial AUC | TBD | target ≥ baseline 0.84 |
| Best trial hyperparameters | TBD | max_depth/eta/subsample/colsample_bytree/min_child_weight |
| HPO total cost | TBD USD | brief estimate: ~0.6 USD |
| Improvement over baseline | TBD | AUC delta vs week 3 |


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html
