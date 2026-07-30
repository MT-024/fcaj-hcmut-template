---
title: "Proposal"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
includeInReport: false
---

# Heart-Attack-Risk Prediction on AWS SageMaker
## An End-to-End MLOps Pipeline for Binary Classification on a 200 USD Budget

### 1. Executive Summary
The Heart-Attack-Risk Prediction capstone is a self-contained MLOps system that takes a 7,000-row clinical-style dataset from raw CSV to a deployed, monitored REST API for binary classification. The project is built entirely on AWS SageMaker and surrounding managed services, runs in a single region (`ap-southeast-1`), and operates under a hard budget cap of 200 USD with no GPU and no NAT Gateway.

The pipeline is expressed as code: data preprocessing, model training, evaluation, conditional registration, and manual approval are wired through SageMaker Pipelines. The deployed endpoint is wrapped by a Lambda function and an API Gateway that exposes two routes — `GET /health` and `POST /predict`. Data Capture runs at 100 % sampling, and a custom Processing Job feeds drift metrics into CloudWatch and an alarm under the namespace `Custom/HeartRisk`.

The chosen deployment model is **Logistic Regression** (test ROC-AUC 0.885515, F1 0.768903, recall 0.818594), beating XGBoost with and without HPO on the primary metric. Disclaimer: predictions are a model output, not a medical diagnosis.

### 2. Problem Statement
#### What's the Problem?
Most student ML projects stop at "notebook trains a model". Production deployment, versioning, drift monitoring, and cost discipline are usually out of scope. The FCAJ capstone asks for the full lifecycle on AWS, but with the same constraints a real team would face: a tight budget, a single region, no GPU instances, and no hand-wavy "we'll fix it later" on cost.

A heart-attack-risk dataset (7,000 rows, 22 columns, target `heart_attack_risk`) is small enough to stay under 200 USD yet rich enough that data-quality decisions — class balance, missing-value handling, schema validation — actually matter. The deliverable is not a notebook; it is a pipeline that can be re-run end-to-end with a single `pipeline.start()`.

#### The Solution
The system uses Amazon S3 for data and artifact storage, SageMaker Processing for preprocessing, SageMaker Training and HPO for candidate models, SageMaker Model Registry with manual approval for versioning, and a SageMaker real-time endpoint for inference. Lambda + API Gateway expose the model as a REST API. A custom Processing Job — used as a fallback when official Model Monitor metrics did not surface in time — reads Data Capture output from S3, computes feature-level drift, and publishes `DriftDetected` and `DataQualityViolationCount` into CloudWatch under `Custom/HeartRisk`. A CloudWatch alarm fires when drift is detected.

#### Benefits and ROI
- **Reproducibility**: pipeline-as-code means a fresh `pipeline.start()` reproduces preprocessing, training, evaluation, and (conditional) registration.
- **Versioning & governance**: Model Registry + manual approval gate keep humans in the loop before any model is reachable from the API.
- **Observability**: Data Capture + custom drift Processing Job + CloudWatch alarm give a working alert chain on a 200 USD budget, without paying for SageMaker Model Monitor.
- **Cost discipline**: Spot Training, Serverless-compatible instance types, lifecycle rules on S3, a single endpoint instance scoped to demo windows, and a `cleanup.py` script that tears down always-on resources.
- **Educational value**: the same patterns (Pipeline as code, quality gate, drift fallback, IAM scoping) transfer directly to a production MLOps role.

### 3. Solution Architecture
The architecture below mirrors the project's actual deployed flow (S3 → Processing → Train/HPO → Registry → Endpoint → API, with a parallel drift path and a Pipeline orchestrator):

![Heart-Attack-Risk Prediction Architecture](/images/2-Proposal/aws-flow.jpg)

#### AWS Services Used
- **Amazon S3** — stores raw data, processed splits, baseline statistics, drift reports, and pipeline artifacts.
- **Amazon SageMaker Processing** — runs preprocessing and the custom drift-detection job.
- **Amazon SageMaker Training** — trains Logistic Regression and XGBoost candidates; supports Spot Training.
- **Amazon SageMaker HPO** — runs limited-trial tuning on XGBoost (3 trials, `max_parallel_jobs=1`) to control cost.
- **Amazon SageMaker Model Registry** — versions candidate models; Pipeline only registers models that pass the quality gate.
- **Amazon SageMaker Real-Time Endpoint** — single `ml.m5.large` instance with Data Capture enabled at 100 % for both input and output.
- **AWS Lambda** — thin wrapper that validates payloads and invokes the SageMaker endpoint; least-privilege IAM scope (`sagemaker:InvokeEndpoint` only on the project endpoint).
- **Amazon API Gateway** — exposes `GET /health` and `POST /predict` (AWS_PROXY integration).
- **Amazon CloudWatch** — namespace `Custom/HeartRisk` for `DriftDetected` and `DataQualityViolationCount`, plus an alarm that transitions to ALARM on drift.
- **AWS Budgets + SNS** — cost guardrail wired before any billable resource is created.

#### Component Design
- **Data layer**: S3 bucket holds `raw/`, `processed/{train,validation,test}/`, `artifacts/preprocessor/`, `baseline/`, `reports/`, and `drift/`.
- **Preprocessing**: a SageMaker Processing Job checks schema, imputes/encodes features, splits 70/15/15 with stratification, and writes preprocessor artifacts. The preprocessor is fit on train only.
- **Training**: Logistic Regression and XGBoost are trained; HPO tunes XGBoost with `validation:auc` as the objective metric.
- **Evaluation**: a managed evaluation step computes ROC-AUC, F1, recall, precision, accuracy, and the confusion matrix on the test split.
- **Quality gate**: a `ConditionStep` checks `roc_auc >= 0.84 AND f1 >= 0.70 AND recall >= 0.65`; passing models go to `RegisterModel`, failing ones go to `FailStep`.
- **Registry & approval**: Model Package versions land in `PendingManualApproval`. A human approves before the model is deployed.
- **Inference path**: API Gateway → Lambda (validate → invoke) → SageMaker endpoint → response (with `prediction`, `risk_probability`, `threshold`, `model_type`, disclaimer).
- **Drift path**: Data Capture (S3) → EventBridge hourly rule → custom Processing Job (standardized mean shift > 0.5 for numeric, total variation distance > 0.20 for categorical) → CloudWatch metrics → CloudWatch alarm.

### 4. Technical Implementation
#### Implementation Phases
The 8-week FCAJ internship maps roughly to one phase per week:
- **Week 1 — Onboarding + brief**: confirm budget cap (200 USD), region (`ap-southeast-1`), no-GPU/no-NAT constraints; set up AWS account, CLI, and Budgets alarm.
- **Week 2 — Data + S3/IAM**: define data layout on S3, build Processing Job for preprocessing, validate schema and splits.
- **Week 3 — Baseline training**: train Logistic Regression and XGBoost, compare validation metrics.
- **Week 4 — HPO**: tune XGBoost with a small, cost-controlled search (3 trials, `max_parallel_jobs=1`).
- **Week 5 — Model Registry + Endpoint**: wire evaluation, quality gate, manual approval, and the first real-time endpoint.
- **Week 6 — API layer**: Lambda + API Gateway, request validation, HTTP 200/400/502 handling, Data Capture on.
- **Week 7 — Drift detection**: custom Processing Job + EventBridge + CloudWatch metrics + alarm.
- **Week 8 — Pipeline + cleanup**: full `Processing → Training → Evaluation → Condition → Register/Fail` pipeline; `cleanup.py` to remove always-on resources.

#### Technical Requirements
- **Data**: 7,000 rows, 22 columns, 20 input features after dropping `patient_id` and target; 70/15/15 stratified split (4,900 / 1,050 / 1,050).
- **Models**: Logistic Regression (chosen) and XGBoost (with HPO). Logistic Regression selected on validation ROC-AUC: 0.863949 vs XGBoost default 0.854283 and XGBoost-after-HPO 0.860982.
- **Quality gate**: ROC-AUC ≥ 0.84, F1 ≥ 0.70, recall ≥ 0.65.
- **Drift detection rules**: numeric standardized mean shift > 0.5; categorical total variation distance > 0.20. Rules are illustrative for the PoC, not a clinical standard.
- **IAM**: SageMaker execution role scoped to project bucket + project endpoint; Lambda execution role scoped to `sagemaker:InvokeEndpoint` on `heart-risk-endpoint` only.
- **Cost controls**: Spot Training where supported, single endpoint instance, lifecycle rules on logs/artifacts, `cleanup.py` script with `Project=heart-risk-mlops` tag filter, AWS Budget alarm.

### 5. Timeline & Milestones
**Project Timeline**
- **Pre-internship**: read FCAJ materials, lock capstone brief, draft data and cost plan.
- **Internship (8 weeks, 01/06/2026 → 24/07/2026)**:
  - Week 1: Onboarding + brief.
  - Week 2: Preprocessing on S3 + IAM.
  - Week 3: XGBoost vs Logistic Regression baseline.
  - Week 4: HPO on XGBoost.
  - Week 5: Model Registry + endpoint.
  - Week 6: Lambda + API Gateway + Data Capture.
  - Week 7: Drift detection + CloudWatch alarm.
  - Week 8: SageMaker Pipeline + cleanup.
- **Post-internship**: archive artifacts, write up the report, present the demo.

### 6. Budget Estimation
**Infrastructure Costs (estimated, 200 USD cap, single region `ap-southeast-1`)**
- Amazon S3: storage for raw/processed/artifacts + lifecycle rules on logs → low single-digit USD over 8 weeks.
- SageMaker Processing: 2 jobs (preprocessing + drift), short-running → a few USD.
- SageMaker Training + HPO: 3 HPO trials + 2 baseline training jobs, Spot where possible → estimated under 30 USD.
- SageMaker Endpoint: single `ml.m5.large` instance scoped to demo windows (a few days total) → estimated under 60 USD.
- Lambda + API Gateway: request volume in the hundreds → estimated under 5 USD.
- CloudWatch metrics + alarm: namespace `Custom/HeartRisk`, 2 custom metrics → estimated under 5 USD.
- Data transfer within `ap-southeast-1`: negligible.

The largest cost driver is the endpoint, which is why it is scoped to demo windows and torn down by `cleanup.py`. No GPU instances and no NAT Gateway are used; Spot Training is used where supported.

> Note: AWS Billing and Cost Explorer have reporting latency. The actual end-of-project bill is reconciled against the AWS Budget alarm history rather than a single dashboard snapshot.

### 7. Risk Assessment
#### Risk Matrix
- **Endpoint idle cost**: medium impact, medium probability → mitigated by scoping the endpoint to demo windows and a `cleanup.py` script.
- **Drift false alarm (or missed drift)**: medium impact, medium probability → mitigated by a fixed hourly schedule and clearly defined rules; acknowledged as PoC rules, not clinical.
- **Cost overrun on HPO**: medium impact, low probability → mitigated by 3 trials max and `max_parallel_jobs=1`.
- **Pipeline failure blocks progress**: medium impact, low probability → mitigated by manual approval gate and a documented intentional-failure run.
- **IAM misconfiguration**: high impact, low probability → mitigated by two scoped roles (SageMaker vs Lambda) and least-privilege policies.

#### Mitigation Strategies
- **Cost**: AWS Budgets alarm at 50 / 80 / 100 %; tag every billable resource with `Project=heart-risk-mlops`; `cleanup.py` is part of the deliverable.
- **Drift**: documented thresholds and a custom Processing Job that is independent of managed Model Monitor availability.
- **Security**: Lambda role cannot read S3 or call `sagemaker:CreateTrainingJob`; SageMaker role cannot invoke arbitrary endpoints.

#### Contingency Plans
- If Model Monitor metrics appear later, swap the custom Processing Job for the managed schedule without changing the alarm contract.
- If endpoint cost becomes a concern, move the workload to Serverless Inference for low-traffic windows.

### 8. Expected Outcomes
#### Technical Improvements
- An end-to-end, reproducible MLOps pipeline that runs from a single `pipeline.start()`.
- A versioned model in SageMaker Model Registry with a manual approval gate.
- A deployed REST API (`GET /health`, `POST /predict`) backed by Lambda + API Gateway + SageMaker endpoint.
- A working drift alert chain: Data Capture → custom Processing Job → CloudWatch metrics → CloudWatch alarm.
- Test metrics: ROC-AUC 0.885515, F1 0.768903, recall 0.818594 — above the project's quality gate.

#### Long-term Value
- A reusable template for future FCAJ cohorts on cost-controlled SageMaker MLOps.
- Personal portfolio piece demonstrating pipeline-as-code, drift fallback design, and IAM scoping on a real budget.
- Foundation for a follow-on track on production hardening (multi-AZ failover, IaC refactor, canary deployment).

> Disclaimer: predictions from this system are a model output for educational purposes. They are not a medical diagnosis and must not replace consultation with a qualified clinician.
