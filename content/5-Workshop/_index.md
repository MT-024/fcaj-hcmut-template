---
title: "Workshop"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
includeInReport: false
---

# Heart-Attack-Risk Prediction — End-to-End MLOps Workshop

#### Overview

This workshop is the build log of my **SageMaker MLOps capstone** for the FCAJ internship. It walks through every step from a raw CSV in S3 to a deployed, monitored REST API on AWS, with a budget cap of **200 USD**, a single region (`ap-southeast-1`), no GPU instances, and no NAT Gateway.

Rather than a hands-on lab for the reader, this section documents **how I built the system**: what was provisioned, what each step did, what the artifacts looked like, and where the cost discipline showed up. Each sub-page corresponds to a phase of the build, in the same order as the 8-week internship.

![Heart-Attack-Risk Prediction Architecture](/images/2-Proposal/aws-flow.jpg)

#### Content

1. [Workshop overview](5.1-Workshop-overview/)
2. [Prerequisite — AWS account, IAM, Budgets](5.2-Prerequiste/)
3. [Data preprocessing on SageMaker (Week 2)](5.3-S3-vpc/)
4. [Training, HPO, endpoint, and API (Weeks 3–6)](5.4-S3-onprem/)
5. [Drift detection and CloudWatch alarm (Week 7)](5.5-Policy/)
6. [SageMaker Pipeline and cleanup (Week 8)](5.6-Cleanup/)
