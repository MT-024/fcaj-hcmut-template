---
title: "Blogs Posted"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
includeInReport: false
---


This section lists the blogs I have posted to [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) during my FCAJ internship.

### [Blog 1 — AWS Lambda: "Use right" and "Run fast" strategies for cost optimization](3.1-Blog1/)
Sharing key principles when designing Lambda-based systems: when Lambda fits vs. when it doesn't, the "execution environment" static initialization trick, Lambda Power Tuning, cold start reduction, RDS Proxy for database connections, and X-Ray for debugging.

### [Blog 2 — Amazon SageMaker: AWS's AI/ML and how to optimize without burning money](3.2-Blog2/)
Deep dive into SageMaker's 3-pillar architecture (Studio, Training, Inference), Managed Spot Training for up to 70% cost reduction, Warm Start & HPO discipline, instance selection strategy (CPU vs GPU), Auto Scaling, Serverless Inference, and Model Optimization (Quantization, SageMaker Neo).

### [Blog 3 — Running SageMaker MLOps on 200 USD: 13 decisions to not blow the budget](3.3-Blog3/)
A capstone reflection: how I kept total project cost at 80–110 USD against a 200 USD cap. 13 specific decisions spanning compute (instance type, no GPU, single region), data/storage (lifecycle, single preprocess), pipeline (HPO limits, cleanup), and IAM/network (least privilege, no NAT Gateway).