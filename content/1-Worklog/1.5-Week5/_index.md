---
title: "Week 5 Worklog"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 5 Objectives
  - Tasks to be carried out this week
  - Week 5 Achievements
---


### Week 5 Objectives:

* Register the best HPO model in SageMaker Model Registry.
* Deploy a real-time Endpoint for inference.
* Add Data Capture to feed drift monitoring (week 7).


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Create Model Package Group `heart-attack-risk-models` in SageMaker <br> - Register best HPO model as Model Package v1 | 06/29/2026 | 06/29/2026 |
| 3   | - Write inference script `inference.py` (preprocessor + XGBoost loading) <br> - Package as `model.tar.gz` with inference.py + xgb_model.json + preprocessor.joblib | 06/30/2026 | 07/01/2026 |
| 4   | - Deploy Endpoint `heart-risk-endpoint` on `ml.t2.medium` (or `ml.t3.medium`) <br> - Enable Data Capture (sampling_percentage=100, capture_options=[Input, Output]) | 07/01/2026 | 07/02/2026 |
| 5   | - Test endpoint with sample patients from `processed/test_processed.csv` <br> - Verify response includes disclaimer `"Educational demonstration only; not a medical diagnosis."` | 07/02/2026 | 07/03/2026 |
| 6   | - Cleanup: delete endpoint after demo (cost discipline — endpoint 24/7 ≈ 35 USD/month) <br> - Document endpoint lifecycle: create-on-demo, delete-after-demo | 07/03/2026 | 07/03/2026 |


### Week 5 Achievements:

* Registered Model Package v1 in Model Registry.
* Deployed first working Endpoint with Data Capture enabled.
* Cost discipline: endpoint lifecycle script (create → predict → delete).

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 5 - Endpoint + Registry                  -->
<!-- Cần điền:                                                       -->
<!--   - Endpoint ARN                                                -->
<!--   - Model Package ARN                                           -->
<!--   - First demo prediction (sample input + output)               -->
<!--   - Cost week 5                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html
* https://docs.aws.amazon.com/sagemaker/latest/dg/model-data-plane.html
