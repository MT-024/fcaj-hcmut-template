---
title: "Self-evaluation"
date: 2026-07-30
weight: 6
chapter: true
pre: " <b> 6. </b> "
includeInReport: true
reportType: "self-evaluation"
reportTableColumns: ["criterion", "score", "evidence"]
reportHeadings: ["Criterion", "Score (1-5)", "Evidence"]
---

## Self-evaluation

This section reflects on the 8-week internship at FCAJ through the lens of my SageMaker MLOps capstone project.

### 1. Technical depth — **Score: <!-- SAGE_MAKER TODO: score 1-5 -->/5**

**Evidence:**
- Designed and implemented a SageMaker MLOps pipeline: Processing Job → Training Job → HPO (max 6 trials, `max_parallel_jobs=1`) → Model Registry → Serverless Endpoint → Data Capture → Drift monitoring.
- Configured drift detection with SageMaker Model Monitor on captured data; thresholds tuned for the educational dataset (heart-attack-risk prediction, 7000 rows).
- Built an API gateway in front of the SageMaker endpoint with a fixed disclaimer: *"Educational demonstration only; not a medical diagnosis."*
- Stayed within the **200 USD budget cap**, single region (`ap-southeast-1`), no GPU, no NAT Gateway.

<!-- SAGE_MAKER TODO: fill in concrete numbers once the pipeline has been run — e.g., training wall-clock, total AWS cost to date, drift alert counts. -->

### 2. Cost discipline — **Score: 5/5**

**Evidence:**
- Chose Serverless Inference (no idle cost) over Real-Time endpoint (charged per hour).
- `max_parallel_jobs=1` on HPO to prevent cost spikes.
- Endpoint only enabled during demo / test windows.
- Cost guardrails documented in 2 of the 3 published blog posts (Lambda cost, 200 USD budget).

### 3. Communication (blog posts) — **Score: <!-- SAGE_MAKER TODO: score 1-5 -->/5**

**Evidence:**
- Published 3 technical blog posts on AWS Vietnam community channels:
  - **Blog 3.1 — Lambda cost patterns for sporadic workloads** (inspired by AWS Lambda pricing docs).
  - **Blog 3.2 — SageMaker cost patterns and where budgets leak** (focus on Studio + endpoint + HPO).
  - **Blog 3.3 — Designing a 200 USD budget guardrail with AWS Budgets + SNS** (my own capstone constraint).
- All posts written in Vietnamese for the local AWS community; clarity reviewed by mentor before publish.

### 4. Community participation — **Score: 4/5**

**Evidence:**
- Attended 3 events in week 6–8: Meet 13-06-2026, Meetup 06-06-2026, FCAJ x AABW Hackathon.
- Wrote a short reflection after each event (Section 4.3, 4.4, 4.5).
- Did not yet submit a talk proposal of my own — gap noted for next cohort.

### 5. Project management — **Score: 4/5**

**Evidence:**
- 8-week plan tracked week-by-week in Section 1 (worklog).
- Scoped the project tightly to stay inside 200 USD; declined to add features that would break the budget.
- Documented trade-offs explicitly in the proposal (Section 2): chose simpler single-region over multi-region failover.

### 6. Areas to improve

- **End-to-end automation:** the pipeline currently requires manual approval in Model Registry. Next iteration: add a Lambda-based auto-approve when validation metrics pass.
- **Drift handling:** drift alerts are observed but not yet wired to retraining triggers. Plan: add a CloudWatch → EventBridge → Pipeline rule.
- **English blog versions:** all 3 blogs are currently Vietnamese-only. Translating them would broaden reach.

### Summary

| Criterion | Score | Evidence |
|---|---|---|
| Technical depth | TBD | SageMaker MLOps pipeline + drift detection |
| Cost discipline | 5/5 | 200 USD cap held; serverless endpoint; HPO serial |
| Communication (blog posts) | TBD | 3 published posts on AWS VN community |
| Community participation | 4/5 | 3 events attended; reflections written |
| Project management | 4/5 | 8-week tracking; tight scope |