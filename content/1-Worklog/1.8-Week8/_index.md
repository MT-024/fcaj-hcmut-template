---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 8 Objectives
  - Tasks to be carried out this week
  - Week 8 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 8 Objectives:

* Orchestrate full pipeline with SageMaker Pipelines.
* Run cleanup script to delete all AWS resources.
* Verify total cost stayed under 200 USD.


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Define SageMaker Pipeline (Kubeflow-based): ProcessingStep → TrainingStep → HPOStep → RegisterModelStep → DeployStep | 23/09/2025 | 24/09/2025 |
| 3   | - Test pipeline end-to-end on small batch (1 trial instead of 6 for speed) <br> - Verify pipeline ARN, execution ARN, status | 24/09/2025 | 25/09/2025 |
| 4   | - Re-run with full HPO (6 trials) — final pipeline execution | 25/09/2025 | 26/09/2025 |
| 5   | - Write cleanup script `cleanup.py`: delete endpoint, delete model package, delete pipeline, empty S3 bucket (after download) <br> - Filter by tag `Project=heart-risk-mlops` to avoid touching other resources | 26/09/2025 | 27/09/2025 |
| 6   | - Run cleanup, verify all resources deleted via Cost Explorer <br> - Total final bill check vs 200 USD cap | 27/09/2025 | 27/09/2025 |


### Week 8 Achievements:

* End-to-end pipeline reproducible from a single `pipeline.start()` call.
* All AWS resources cleaned up via script (cost discipline).
* Project delivered within budget.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 8 - Pipeline + Cleanup                   -->
<!-- Cần điền:                                                       -->
<!--   - Pipeline ARN                                                -->
<!--   - Total project cost (final bill, vd: 87 USD)                 -->
<!--   - Cost breakdown by service                                   -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html
