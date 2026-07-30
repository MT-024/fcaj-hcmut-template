---
title: "Week 7 Worklog"
date: 2026-07-13
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 7 Objectives
  - Tasks to be carried out this week
  - Week 7 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 7 Objectives:

* Build drift detection pipeline without depending on SageMaker Model Monitor.
* Brief note: Model Monitor access may change after 2026-07-30 → design fallback (Data Capture → S3 → EventBridge → Processing Job → CloudWatch custom metrics).
* Generate drift data using the recipe from brief week 7.


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Read brief: drift-generation recipe (shift distribution on certain features) <br> - Write `generate_drift.py` to produce drifted batch (5–10% of features perturbed) | 07/13/2026 | 07/13/2026 |
| 3   | - Confirm Data Capture is ON (week 5 setup) → S3 path `s3://.../capture/` | 07/14/2026 | 07/14/2026 |
| 4   | - Create EventBridge rule: every 1 hour → trigger Processing Job <br> - Processing Job reads captured JSON, computes PSI/KL divergence vs baseline | 07/15/2026 | 07/16/2026 |
| 5   | - Push custom metrics to CloudWatch: `feature_drift_psi`, `prediction_drift_psi` <br> - Set CloudWatch alarm: PSI > 0.2 → SNS alert | 07/16/2026 | 07/17/2026 |
| 6   | - Run drift generation, send drifted traffic to endpoint, verify alarm fires <br> - Document the manual pipeline (since Model Monitor may not be available) | 07/17/2026 | 07/17/2026 |


### Week 7 Achievements:

* Drift detection pipeline working without Model Monitor.
* CloudWatch alarm fires when PSI > 0.2.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 7 - Drift detection                      -->
<!-- Cần điền:                                                       -->
<!--   - PSI value on drifted batch                                  -->
<!--   - CloudWatch alarm ARN                                        -->
<!--   - Drift recipe actually used (which features shifted by how %)-->
<!--   - Cost week 7                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html
* https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rules.html
