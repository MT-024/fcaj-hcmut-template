---
title: "Prerequiste"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Before any SageMaker resource is created, three things must be in place: the right region, a budget guardrail, and a tagging convention. Doing these first is what made it possible to stay inside the 200 USD cap for the entire 8-week build.

#### 1. Pick and lock the region

All resources for this project live in **`ap-southeast-1`**. Picking one region avoids cross-region data transfer and keeps every IAM, networking, and storage decision consistent.

![Region us-east-1 ... actually ap-southeast-1 selected for the project](/fcaj-hcmut-template/images/5-Workshop/AWS-01-selected-region.png)

> The screenshot shows the region selector; on my account the value is `ap-southeast-1` (Singapore), not `us-east-1`. The image filename reflects the original capture naming but the project itself is single-region `ap-southeast-1` end to end.

#### 2. Set up AWS Budgets before any billable resource

A Budget alarm with three thresholds (50 %, 80 %, 100 %) is the safety net that lets you experiment without overspending.

![AWS Budget overview with the project's monthly budget set below 200 USD](/fcaj-hcmut-template/images/5-Workshop/AWS-02-budget-overview.png)

When the projected or actual spend crosses a threshold, an SNS notification is sent. This is the single signal that proves the cost discipline was real, not just claimed.

#### 3. Tag every billable resource

Every SageMaker job, endpoint, model, and bucket that belongs to the project is tagged with:

```
Project = heart-risk-mlops
Stage   = {dev | hpo | endpoint | drift | cleanup}
```

Tags make the `cleanup.py` script at the end of week 8 safe: it deletes only resources with `Project=heart-risk-mlops` and never touches anything tagged otherwise.

#### 4. Create two IAM roles

Two execution roles are wired up once and reused throughout the build:

- **SageMaker execution role** — `sagemaker:full-access` on the project bucket and project endpoint, with `iam:PassRole` limited to itself. Used by Processing, Training, HPO, the Model Registry, and Pipeline.
- **Lambda execution role** — only `sagemaker:InvokeEndpoint` on the project endpoint ARN, plus `logs:*` for CloudWatch. **No S3 read, no training-job permissions.**

The split means a misbehaving API call cannot accidentally train a new model or read the dataset.

#### 5. Local tooling

- AWS CLI v2 configured against the project account (`aws sts get-caller-identity` returns the expected account ID).
- Python 3.12 with `boto3`, `sagemaker` SDK, `pandas`, `scikit-learn`, `xgboost`.
- A project notebook folder storing Processing / Training / Pipeline scripts and the `cleanup.py` skeleton.

#### What this gives you

With these five prerequisites in place, week 2 can start provisioning the actual pipeline without making any architectural decision about cost discipline — that decision has already been made and wired into Budgets and tags.
