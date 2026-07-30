---
title: "Training, HPO, endpoint, and API (Weeks 3-6)"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Weeks 3 through 6 build the part of the pipeline that actually trains models, picks one, deploys it behind a REST API, and starts saving inference traffic for monitoring. The four sub-sections here map roughly one-to-one with each week.

#### 5.4.1 Baseline training (Week 3): Logistic Regression vs XGBoost

Two candidate models are trained as SageMaker Training Jobs. Both run on a `ml.m5.large` and finish in under five minutes each.

![Logistic Regression validation metrics — ROC-AUC 0.863949, F1 0.747583, recall 0.789116, precision 0.710204](/images/5-Workshop/W3-02-lr-metrics.png)

![XGBoost default validation metrics — ROC-AUC 0.854283, F1 0.749749, recall 0.845805, precision 0.673285](/images/5-Workshop/W3-04-xgb-metrics.png)

Logistic Regression wins on the primary metric (ROC-AUC). XGBoost wins on recall but loses on precision; per the project's selection rule, ROC-AUC decides.

#### 5.4.2 HPO on XGBoost (Week 4)

HPO uses 3 trials with `max_parallel_jobs=1` to keep the cost under control. The objective is `validation:auc`. Best trial reaches validation ROC-AUC ~ 0.860982 — still below Logistic Regression. Logistic Regression is locked in as the deployment candidate.

#### 5.4.3 Model Registry and real-time endpoint (Week 5)

![Model Registry with versions 1 and 2 Approved and version 3 PendingManualApproval](/images/5-Workshop/W5-01-model-versions.png)

Versions 1 and 2 reflect earlier manual deploys. Version 3 is what the Pipeline will produce; it sits at `PendingManualApproval` to keep humans in the loop.

![Real-time endpoint heart-risk-endpoint in InService state](/images/5-Workshop/W6-01a-endpoint-inservice.png)

A single `ml.m5.large` instance is enough for demo traffic. The instance is created only for the demo window and torn down by `cleanup.py` afterwards.

![Confusion matrix and ROC-AUC/F1/recall/precision on the test split for the chosen Logistic Regression model](/images/5-Workshop/W5-02-evaluation-metrics-and-confusion-matrix.png)

Test metrics confirm what the validation ROC-AUC suggested: ROC-AUC 0.885515, F1 0.768903, recall 0.818594 — comfortably above the project's quality gate (ROC-AUC ≥ 0.84, F1 ≥ 0.70, recall ≥ 0.65).

#### 5.4.4 Lambda, API Gateway, and Data Capture (Week 6)

The endpoint is wrapped by a Lambda function that validates the request body and calls `sagemaker:InvokeEndpoint`. The Lambda role is scoped to **only** that endpoint ARN, with no S3 or SageMaker permissions.

![API Gateway routes — GET /health and POST /predict](/images/5-Workshop/W6-07-api-routes.png)

Three HTTP tests prove the API surface:

- `GET /health` → `200 OK` (sanity check).
- `POST /predict` with valid payload → `200 OK` with `prediction`, `risk_probability`, `threshold`, `model_type`, and disclaimer.

![POST /predict happy path returns 200 with prediction and risk_probability](/images/5-Workshop/W6-09-predict-200.png)

- `POST /predict` with a missing field → `400 Bad Request`. The Lambda validator catches this before it ever reaches SageMaker.

![POST /predict returns 400 when a required field is missing](/images/5-Workshop/W6-10-predict-400.png)

Data Capture is enabled with sampling rate 100 % on both input and output. Each captured record becomes one JSONL line in S3 containing both the request payload and the response — the raw material for the drift job in week 7.

#### Cost check at the end of week 6

The endpoint is the only always-on resource. Estimated monthly bill for the endpoint if left running is ~35 USD — which is why the project rule is "endpoint up only during demo windows". The next section adds drift detection that runs hourly, but it is implemented as a Processing Job (per-run cost), not a managed schedule.
