---
title: "Sharing and Feedback"
date: 2026-07-30
weight: 7
chapter: true
pre: " <b> 7. </b> "
includeInReport: true
---

## Sharing and Feedback

### 1. What I shared with the community

- **3 published blog posts** on AWS Vietnam community channels (titles in Section 3.1–3.3):
  - Lambda cost patterns for sporadic workloads.
  - SageMaker cost patterns and budget leaks.
  - Designing a 200 USD cost guardrail with AWS Budgets + SNS.
- **5 workshop deliverables** in `5.3-S3-buckets` through `5.7-SageMaker-MLOps` (this template). The workshop series itself is shared as open educational content under the FCAJ program.

### 2. Feedback I received during the internship

**From mentor (week 4):**
- *"Scope your endpoint to demo windows only. Real-Time endpoints will eat your budget before drift detection even kicks in."*
- Action taken: switched from Real-Time to Serverless Inference; documented the rationale in the proposal.

**From peer reviews (week 6):**
- *"Your HPO search space is wider than it needs to be. With only 6 trials, every extra dimension hurts."*
- Action taken: reduced HPO search space from 5 dimensions to 3 (kept `max_depth`, `eta`, `min_child_weight`; dropped `subsample` and `colsample_bytree`).

**From event attendees (FCAJ x AABW hackathon, week 8):**
- *"The 200 USD guardrail pattern you wrote about in Blog 3.3 — would it work for a workload with continuous inference?"*
- Honest answer: no — for continuous inference you'd want Savings Plans or a Spot-based Real-Time endpoint, not the same SNS-based guardrail. Captured as a future blog idea.

### 3. What I wish I had known earlier

- **Data Capture must be enabled at endpoint creation time**, not retroactively. I lost 2 days of buffer in week 7 because of this.
- **Model Monitor needs a baseline**, and the baseline statistics job runs synchronously — plan for 15–30 min wall-clock before drift detection is "live."
- **The `ap-southeast-1` SageMaker Studio domain** has a slightly different console layout than `us-east-1` screenshots in the official docs. Don't blindly follow US-region screenshots.

### 4. Suggestions for future cohorts

- Capstone brief should ship with a **pre-built cost guardrail template** (CloudFormation / CDK), so interns don't reinvent AWS Budgets + SNS wiring in week 1.
- Add a **dedicated week for "production-readiness"** — endpoint security (VPC, IAM scope), observability (CloudWatch alarms), and cost dashboards. These were crammed into the last 2 weeks of my run.
- Encourage interns to **publish at least one blog post by week 4**, not the final week. Writing forces earlier synthesis.

### 5. Acknowledgements

Thanks to the FCAJ mentors and the AWS Vietnam community for the 8-week ride — especially the team behind the FCAJ x AABW hackathon for showing how a one-week build can produce shipping products.

<!-- SAGE_MAKER TODO: replace with personal thank-you once you have the full list of mentors. -->