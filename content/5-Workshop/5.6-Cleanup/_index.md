---
title: "SageMaker Pipeline and cleanup (Week 8)"
date: 2026-06-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Week 8 ties the build together as a **SageMaker Pipeline** and then runs `cleanup.py` to remove always-on resources.

#### 5.6.1 The Pipeline definition

The Pipeline wires up the steps from weeks 2–5 as a single graph:

```
PreprocessData → TrainModel → EvaluateModel → CheckModelQuality
                                            ├── Pass → RegisterModel
                                            └── Fail → MetricThresholdFailed
```

![SageMaker Pipeline graph shows the 4 steps wired together](/images/5-Workshop/W8-01-pipeline-graph.png)

The Pipeline runs end-to-end with a single `pipeline.start()`. Passing the quality gate (ROC-AUC ≥ 0.84, F1 ≥ 0.70, recall ≥ 0.65) lands a new version in the Model Registry at `PendingManualApproval`. The Pipeline does **not** deploy the model — humans still approve.

#### 5.6.2 Happy-path execution

![Pipeline execution completes successfully](/images/5-Workshop/W8-02-pipeline-success.png)

A successful run creates Model Package Version 3 — the third Approved entry visible in the Model Registry screenshot from week 5.

#### 5.6.3 Intentional failure as a quality-gate test

To prove that the quality gate actually rejects bad models, a second Pipeline execution is run with `AucThreshold` parameter overridden to **0.99** — far above the actual ROC-AUC of the trained model.

![Pipeline execution with the threshold override fails at the ConditionStep](/images/5-Workshop/W8-05-pipeline-failure.png)

Preprocessing, training, and evaluation all complete. The `ConditionStep` returns `false`. The Pipeline transitions to `FailStep` and **no new Model Package is registered**. This is the expected behavior: a model that does not pass the project's quality bar cannot reach the registry, and therefore cannot reach the API.

#### 5.6.4 Cleanup script

`cleanup.py` is the project's safety net. It deletes only resources tagged with `Project=heart-risk-mlops` and skips anything tagged otherwise. Concretely it removes:

- Real-time endpoint `heart-risk-endpoint` and its endpoint configuration.
- The deployed inference model.
- The CloudWatch alarm under `Custom/HeartRisk`.
- Lambda function `heart-risk-api`.
- API Gateway routes for `/health` and `/predict`.
- CloudWatch log groups for Lambda and Processing Jobs that are no longer needed.

It deliberately **keeps**:

- The S3 bucket and its contents (raw, processed, artifacts, baseline, reports, drift).
- The Model Registry versions (so that re-deploying is one click).
- The Pipeline definition (so that `pipeline.start()` works from week 9 onwards).

```python
# cleanup.py — outline
import boto3

PROJECT_TAG = {"Key": "Project", "Value": "heart-risk-mlops"}

def tagged(tag_list, project_tag=PROJECT_TAG):
    return any(t["Key"] == project_tag["Key"] and t["Value"] == project_tag["Value"]
               for t in tag_list or [])

def cleanup_endpoint(sm_client):
    # delete endpoint, endpoint-config, and matching model
    ...

def cleanup_lambda(lambda_client, apigw_client, logs_client):
    # delete Lambda function, API Gateway, log groups
    ...

if __name__ == "__main__":
    cleanup_endpoint(boto3.client("sagemaker"))
    cleanup_lambda(boto3.client("lambda"), boto3.client("apigateway"),
                   boto3.client("logs"))
    print("Cleanup complete.")
```

The script was the final run of week 8 and is what made it possible to land the 8-week project inside the 200 USD budget — every always-on billable resource is now tagged and removable with one command.

#### Final state after cleanup

- `Project=heart-risk-mlops` resources that are no longer running: nothing.
- `Project=heart-risk-mlops` resources that are kept for reproducibility: S3 bucket, Model Registry versions, Pipeline definition, CloudWatch logs.
- Cost after cleanup: only S3 storage and Model Registry line items, well under 5 USD/month.
