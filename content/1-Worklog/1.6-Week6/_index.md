---
title: "Week 6 Worklog"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 6 Objectives
  - Tasks to be carried out this week
  - Week 6 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 6 Objectives:

* Expose Endpoint through a public HTTP API.
* Add disclaimer to every response (brief requirement).
* Implement endpoint lifecycle automation (bật/tắt theo lịch).


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Create Lambda function `heart-risk-invoke` with IAM role limited to `sagemaker:InvokeEndpoint` on endpoint ARN only | 07/06/2026 | 07/06/2026 |
| 3   | - Write Lambda handler: parse request, call `sm.invoke_endpoint()`, append disclaimer to response <br> - Verify response format: `{"prediction": 0/1, "probability": 0..1, "disclaimer": "..."}` | 07/07/2026 | 07/08/2026 |
| 4   | - Create API Gateway REST API `heart-risk-api` <br> - Connect to Lambda via AWS_PROXY integration | 07/08/2026 | 07/09/2026 |
| 5   | - Test API with curl/Postman: send patient features → verify JSON response with disclaimer | 07/09/2026 | 07/10/2026 |
| 6   | - Implement endpoint scheduler (turn on 19:00 Fri for demo, turn off 23:00 same day) <br> - Test 1 cycle | 07/10/2026 | 07/10/2026 |


### Week 6 Achievements:

* Lambda + API Gateway public API working.
* Every response includes disclaimer `"Educational demonstration only; not a medical diagnosis."`
* Endpoint scheduler saves ~30 USD/month vs 24/7 endpoint.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 6 - API + Lambda                         -->
<!-- Cần điền:                                                       -->
<!--   - API Gateway URL                                             -->
<!--   - Lambda ARN                                                  -->
<!--   - Sample request/response                                     -->
<!--   - Cost week 6                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### References:

* https://docs.aws.amazon.com/lambda/latest/dg/with-sagemaker.html
