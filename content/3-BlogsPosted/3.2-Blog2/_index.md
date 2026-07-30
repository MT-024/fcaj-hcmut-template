---
title: "Blog 2 - Amazon SageMaker: AWS's AI/ML and how to optimize without burning money"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
includeInReport: false
---


# BLOG 2
## Amazon SageMaker: AWS's AI/ML and how to optimize without burning money

Hi everyone,

In today's AI era, not only large corporations but also Startups want to integrate Machine Learning into their products. However, setting up traditional ML infrastructure with EC2 GPUs, installing CUDA, managing Jupyter Notebooks, and scaling inference is usually a nightmare for DevOps.

Amazon SageMaker was born to solve that problem — it's not just a service, but a platform that includes every tool to build, train, and deploy any ML model.

"Powerful" yes, but if you don't deeply understand it, your AWS bill at the end of the month will be "hard to swallow". Below are my practical experiences to master SageMaker.

### 1. Understanding SageMaker architecture correctly (Not just Notebook!)

Many newcomers think SageMaker is just a JupyterLab on Cloud. In reality, SageMaker is an ecosystem with 3 main pillars:

- **SageMaker Studio:** All-in-one IDE for Data Scientists to explore data and build models.
- **Training:** Job management mechanism, lets you run on powerful server clusters (P4d, G5) without worrying about infrastructure.
- **Inference (Hosting):** Deploy models as Endpoints (real-time) or Batch Transform (async).

👉 **Advice:** Always separate SageMaker Studio (for dev/test) and Endpoint (for production). Studio should only be used for experimentation with small datasets. When training for real, use Jobs.

### 2. "Tricks" to optimize cost when Training models

Training a Deep Learning model can cost thousands of USD if you leave an EC2 instance running 24/7.

💡 **1. Use "Managed Spot Training"**

When creating a Training Job on SageMaker, enable Managed Spot Training.

- **Mechanism:** SageMaker uses excess EC2 capacity (Spot Instances) to run training at up to 70% cheaper than On-Demand.
- **Risk:** Spot can be reclaimed at any time. But SageMaker is smart: it auto-saves checkpoints and resumes training from the last position when new capacity becomes available. "Cheap and safe".

💡 **2. Warm Start & Hyperparameter Tuning**

Don't train from scratch every time.

- Use pre-trained models on S3 (e.g., ResNet, BERT) and finetune on your dataset. SageMaker supports Incremental Training.
- Use Automatic Model Tuning (HPO) but set `MaxParallelJobs` and `MaxNumberOfTrainingJobs` moderately. Don't let it run 500 jobs if you don't need to!

💡 **3. Choose the right Instance for Training**

Instance type selection is critical:

- **CPU (M5/C5):** Only for XGBoost, LightGBM, or traditional algorithms (Linear Learner).
- **GPU (G4dn/G5/P4d):** For Deep Learning.
- **G4dn (T4):** Cheap, good for Inference and medium Training.
- **P4d (A100):** "Supercomputer", only for huge datasets and big budget.

💡 **Tip:** For Distributed training, more GPU doesn't always mean faster. Sometimes 2×8 GPU machines run slower than 1×8 GPU due to data transfer (Bandwidth bottleneck). Always test with small jobs first.

### 3. "Tricks" to optimize cost during Deployment (Inference)

Deploying a model as a real-time Endpoint is the biggest cost driver because it runs 24/7.

💡 **1. Auto Scaling based on RAM / CPU**

Don't keep your Endpoint at maximum instance count. Configure Target Tracking Scaling to scale out when CPU > 50% and scale in when traffic is low (e.g., at night).

Especially, set `MinInstanceCount = 0` for Staging environments to auto-shut down when not used (Serverless inference).

💡 **2. Serverless Inference (SageMaker Serverless)**

Recently launched — Lambda's "sibling" for ML.

- You don't manage EC2 anymore. SageMaker auto-scales from 0 to max concurrency.
- **Fit:** Low-frequency APIs, no ultra-low latency requirement.
- **Not fit:** Systems with steady traffic > 100 req/min (then EC2 is cheaper).

💡 **3. Model Optimization**

Techniques to reduce file size and speed up inference — few people do this:

- **Quantization:** Convert weights from FP32 to INT8. PyTorch and TensorRT support this. Reduces size by 4× and doubles inference speed with near-zero accuracy loss.
- **SageMaker Neo:** For edge devices or CPU-optimized deployment, use SageMaker Neo to compile the model into highly optimized code.

### 4. Don't forget MLOps: CI/CD for AI

SageMaker integrates very well with SageMaker Pipelines (based on Kubeflow). This is how you turn ML into a real software application:

1. Code commit to Git (CodeCommit/GitHub).
2. Pipeline auto-triggers to run Unit Tests on code, train model on new data.
3. Model evaluation (Model Registry) — if Accuracy is higher than the old version, auto-deploy to Staging.
4. A/B Testing with Production Variants (Canary deployment) before pushing all traffic to the new version.

### 5. Conclusion: Is SageMaker worth the money?

Answer: YES, if you know how to use it. Building your own GPU cluster on EC2 and installing Kubernetes to scale is expensive and extremely time-consuming. SageMaker frees you from all that "dry infrastructure" layer.

**Final advice:**

Don't see SageMaker as a "laptop". See it as an "AI Factory". Use Spot for Training, Serverless for Staging, and Auto Scaling + Neo for Production. Then you'll have a powerful AI pipeline at very reasonable cost.

---

**FB post:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2227364341361859/