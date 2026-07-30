---
title: "Sharing and Feedback"
date: 2026-07-30
weight: 7
chapter: false
pre: " <b> 7. </b> "
includeInReport: false
---

> Here, I share my personal opinions about my experience in the First Cloud AI Journey program through the lens of my 8-week SageMaker MLOps capstone. This feedback is intended to help the FCAJ team improve any shortcomings for future cohorts.

### Overall Evaluation

**1. Working Environment**
The FCAJ working environment is supportive and pragmatic. I could always ping the mentor on Slack when a pipeline step stalled (Data Capture misconfiguration, drift baseline statistics job), and got unblocked within minutes. The weekly review meetings kept me accountable to the 200 USD budget cap and to declaring trade-offs explicitly. The AWS community channels gave me a place to ask architecture questions beyond my mentor's bandwidth. For future cohorts, it would help to have a shared lab notebook (or pinned Slack channel) where interns post a 1-line daily log, so peers can spot blockers earlier.

**2. Relevance of Work to Academic Major**
The capstone sits exactly where my coursework left off: I had learned XGBoost, preprocessing, and basic cloud concepts in class, but had never stitched them into a single end-to-end MLOps pipeline on AWS. SageMaker Processing → Training → HPO → Model Registry → Serverless Endpoint → Data Capture → Drift Detection was the missing integration layer. The dataset (heart-attack-risk prediction, 7000 rows) was small enough to stay inside the 200 USD cap but realistic enough that data-quality choices mattered — class balance, missing-value handling, schema validation. This is closer to a real production ML setup than any course assignment I've done.

**3. Learning & Skill Development Opportunities**
Concrete skills I picked up beyond the syllabus:
- **SageMaker MLOps pipeline as code**: ProcessingStep / TrainingStep / HPOStep / RegisterModelStep chained via Kubeflow, executed end-to-end with one `pipeline.start()`.
- **Cost engineering**: Spot Training, Serverless Inference, lifecycle rules on logs/artifacts, `max_parallel_jobs=1` for HPO — and the discipline to **delete the endpoint between demos** to avoid the ~35 USD/month idle bill.
- **Drift detection without managed Model Monitor**: Data Capture → S3 → EventBridge (1-hour rule) → Processing Job → PSI/KL divergence → CloudWatch custom metrics → SNS alarm. Building this by hand forced me to understand each hop.
- **API hardening**: Lambda with least-privilege IAM scope (only `sagemaker:InvokeEndpoint` on the endpoint ARN), API Gateway with AWS_PROXY integration, fixed disclaimer on every response to keep the demo honest.
- **Technical writing**: 3 blog posts on AWS Vietnam community channels, each peer-reviewed by my mentor before publication.

**5. Company Culture & Team Spirit**
The "No-Blame Post-Mortem" mindset I heard from the MNC speaker at the FCAJ Meet on 13/06/2026 is the same one the FCAJ team practices. When my HPO job failed on the third trial because I'd over-specified the search space, the discussion was *"what does the failure tell us about the search space"* rather than *"why didn't you check first"*. That culture made it safe to take the kinds of cost-risky decisions (real-time endpoint? GPU instance?) and then walk them back when the numbers didn't add up. The hackathon team rooms in the FCAJ x AABW week showed the same vibe: cross-team help instead of internal competition.

**6. Internship Policies / Benefits**
The 200 USD AWS budget was the right level of constraint — enough to actually run a real pipeline end-to-end, tight enough that I had to think about cost on every step. The Slack channel for budget alerts and the brief's hard constraints (single region `ap-southeast-1`, no GPU, no NAT Gateway) saved me from over-spending decisions before they happened. One thing I would have valued: a **pre-built AWS Budgets + SNS guardrail template** (CloudFormation or CDK) wired up in week 1, so I could spend weeks 2–3 on the pipeline instead of re-inventing budget alarms.

---

### Additional Questions

- **What did you find most satisfying during your internship?**
  Designing the drift-detection pipeline as a fallback to Model Monitor — specifically, the moment the CloudWatch alarm actually fired when I sent drifted traffic to the endpoint. Watching the alert chain I had built by hand (Data Capture → EventBridge → Processing Job → PSI → CloudWatch → SNS) light up end-to-end is the most satisfying technical moment of the 8 weeks. Second-most satisfying: hitting week 8 and seeing the total bill under 200 USD after the cleanup script ran.

- **What do you think the company should improve for future interns?**
  Three concrete things:
  1. **Ship a pre-built cost guardrail template in week 1**, so interns don't reinvent AWS Budgets + SNS wiring. The brief should also include a sample `cleanup.py` so the last week isn't spent rediscovering which resources to delete.
  2. **Add a dedicated "production-readiness" week** (or stretch goal). Endpoint security (VPC, IAM scope tightening), observability (CloudWatch alarms on the right metrics, not all metrics), and cost dashboard for the mentor to review. These got crammed into my last 2 weeks.
  3. **Front-load one blog-post milestone to week 4**, not the final week. Writing forces synthesis, and writing earlier would have caught gaps in my pipeline assumptions.

- **If recommending to a friend, would you suggest they intern here? Why or why not?**
  Yes — but with context. Recommend FCAJ if the friend wants **end-to-end ownership** of a real ML system under a real budget constraint, and if they're willing to write about what they learn (the blog cadence is non-negotiable, not optional). Don't recommend if the friend wants to be spoon-fed a tutorial — the program expects you to read AWS docs, debug your own jobs, and ask the mentor targeted questions, not "what do I do next".

---

### Suggestions & Expectations

- **Suggestions to improve the internship experience**:
  - Provide a sample `ap-southeast-1` CloudFormation/CDK template in week 1 for the AWS account scaffolding (S3 bucket + IAM role + lifecycle rule + Budget alarm + SNS topic). It would cut roughly 2 days of trial-and-error.
  - Pair interns by topic interest in week 1 — even a 30-minute "buddy" pairing helps when the mentor is offline.
  - Make the slide-deck library (from past meetups) searchable by topic. Several of my meetup slide decks wouldn't parse via python-pptx, and I had to reconstruct takeaways from notes.
  - Encourage (or require) blog publication **before** week 8. Writing under deadline at the end reduces depth.

- **Would you like to continue this program in the future?**
  Yes — I would join a follow-on cohort that focuses on **production hardening** (multi-AZ failover, IaC refactor, canary deployment for new model versions). The 8-week capstone gave me a working MLOps pipeline; the next layer of skills is making it survive real-world traffic.

- **Any other comments (free sharing)**:
  The biggest thing I underestimated was **how much of MLOps is unglamorous plumbing**: IAM scope, lifecycle rules, log retention, endpoint cleanup, drift baseline statistics jobs that take 20 minutes to run synchronously. The capstone brief was honest about this, but I'd flag it to future interns: the satisfaction comes from the system working as a whole, not from any one component. And if you only have time to read one AWS doc end-to-end, read the **SageMaker Pipelines developer guide** — it ties everything (Processing, Training, HPO, Registry, Deploy) into one mental model.

  Thanks to the FCAJ mentors and the AWS Vietnam community for the 8-week ride — and to the FCAJ x AABW hackathon team for showing that a one-week build can ship a real product.
