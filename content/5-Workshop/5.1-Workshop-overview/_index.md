---
title: "Workshop overview"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### What this workshop covers

This workshop documents the build of a single AWS account, single region, end-to-end MLOps pipeline for **binary classification of heart-attack risk**. The pipeline ingests a 7,000-row dataset, trains candidate models, registers passing ones in SageMaker Model Registry, deploys a real-time endpoint, wraps it with Lambda and API Gateway, monitors for data drift, and tears down always-on resources on cleanup.

The five sub-pages map to the eight-week internship:

- **5.2 Prerequisite** — AWS account, region lock, IAM roles, AWS Budgets, and tag discipline.
- **5.3 Data preprocessing (Week 2)** — S3 data layout, SageMaker Processing Job, train/validation/test split, preprocessor artifacts.
- **5.4 Training, HPO, endpoint, and API (Weeks 3–6)** — Logistic Regression vs XGBoost, HPO, Model Registry, real-time endpoint, Lambda, API Gateway, Data Capture.
- **5.5 Drift detection and CloudWatch alarm (Week 7)** — custom Processing Job, EventBridge schedule, CloudWatch metrics, alarm.
- **5.6 Pipeline and cleanup (Week 8)** — SageMaker Pipeline, quality gate, intentional-failure test, `cleanup.py` script.

#### Pipeline architecture (text view)

```
Amazon S3 (raw data)
        ↓
SageMaker Processing Job        ← preprocessing + schema check
        ↓
train / validation / test / preprocessor artifacts
        ↓
Logistic Regression + XGBoost (+ HPO)
        ↓
Managed Evaluation (ROC-AUC, F1, recall, precision, confusion matrix)
        ↓
Quality Gate (ConditionStep)
        ↓
SageMaker Model Registry         ← PendingManualApproval
        ↓
Manual Approval
        ↓
Real-Time Endpoint (ml.m5.large) ← Data Capture 100%
        ↓
AWS Lambda                        ← least-privilege IAM
        ↓
Amazon API Gateway (GET /health, POST /predict)
        ↓
Client

Parallel drift path:
Data Capture (S3)
        ↓
EventBridge hourly rule
        ↓
Custom Processing Job (PSI / KL divergence)
        ↓
CloudWatch metrics (namespace Custom/HeartRisk)
        ↓
CloudWatch Alarm → ALARM on drift

SageMaker Pipeline:
Processing → Training → Evaluation → Condition → Register / Fail
```

This is the **logical flow**, not a hand-drawn architecture diagram. The actual reference architecture is shown at the top of this workshop index page.

#### Cost discipline principles used throughout

- Single region `ap-southeast-1` — no cross-region transfer.
- No GPU instance types.
- No NAT Gateway.
- Spot Training wherever supported.
- Real-time endpoint scoped to demo windows, torn down by `cleanup.py`.
- S3 lifecycle rules on logs and ephemeral artifacts.
- Every billable resource tagged `Project=heart-risk-mlops`.
- AWS Budgets alarm at 50 / 80 / 100 % thresholds.

These choices are what kept the project inside the 200 USD cap while still running an end-to-end pipeline.
