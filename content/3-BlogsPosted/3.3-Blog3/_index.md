---
title: "Blog 3 - Running SageMaker MLOps on 200 USD: 13 decisions to not blow the budget"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
includeInReport: false
---
{{% notice warning %}}
⚠️ **Note:** For reference only. Do not copy verbatim.
{{% /notice %}}

# BLOG 3
## Running SageMaker MLOps on 200 USD: 13 decisions to not blow the budget

**Disclaimer:** This post shares experience from a student capstone project on AWS SageMaker (binary classifier for heart-attack risk prediction — educational purposes). All cost figures are **estimates for a learning environment** at the time I worked on it, and may have changed. This post is **not** financial advice or an official AWS cost-optimization guide. Please verify with the AWS Pricing Calculator before applying to a real project.

---

### INTRODUCTION: 200 USD sounds like a lot — until you calculate

When I first read the brief, the figure **200 USD** looked like a generous budget. Then I did a quick calculation:

- 8 weeks × 2 sessions/week × 4 hours/session = **64 hours** of AWS work
- An `ml.t2.medium` endpoint running 24/7: ~**35 USD/month**
- An HPO job with 6 trials × ~20 min/trial on `ml.m5.large`: ~**3 USD/run**
- S3 storage over 8 weeks (data + logs + artifacts): ~**5–8 USD**
- CloudWatch, Lambda, API Gateway, Data Capture: ~**5–10 USD** trickling in

Summed up, it seems "still plenty". But AWS costs don't scale linearly with effort — they scale with **carelessness**.

- An endpoint left running all week instead of being shut down after demo.
- An S3 bucket without a lifecycle rule.
- An HPO job running 30 trials instead of 6.

Each small mistake compounds into a surprise bill.

I learned that:

> **"On the cloud, 'shutting down after use' is a more important skill than 'running it right'."**

Here are 13 specific decisions that helped me reach the finish line with a 200 USD budget cap without straining the bill.

---

### DECISIONS 1–4: COMPUTE

**Decision #1: Choose `ml.t3.medium` as the default instance**

Instead of `ml.m5.large`, I tried training XGBoost with ~7,000 rows on `ml.t3.medium`.

Result:
- Training in ~4 minutes
- AUC reached 0.86

→ For small datasets (<100K rows), `t3.medium` is fully sufficient.

**Lesson:** Don't upgrade instances just "to be safe". Benchmark first.

**Cost:**
- `ml.t3.medium`: ~$0.05/hour
- `ml.m5.large`: ~$0.134/hour (~2.7× more)

**Decision #2: NO GPU**

The project doesn't need GPU. An idle `ml.g4dn.xlarge` notebook alone costs ~$0.736/hour.

**Lesson:** GPU is only worth it when the model or dataset is large enough.

**Decision #3: Single region only**

I chose `ap-southeast-1` (Singapore). Not for performance reasons, but to avoid duplicating IAM Role, S3 Bucket, Security Group...

**Lesson:** Student projects don't need multi-region.

**Decision #4: Shut down the endpoint right after demo**

This is the most money-saving decision.

- Endpoint 24/7 for 8 weeks: ~35 USD
- Only ON during demos: ~5 USD
- **Saves ~30 USD** (15% of total budget).

**Lesson:** An endpoint's existence itself is billable, even with zero requests.

---

### DECISIONS 5–8: DATA & STORAGE

**Decision #5: Use S3 Lifecycle Rule**

Logs, artifacts, and models auto-delete after 30 days.

**Lesson:** Lifecycle is nearly free but saves significant storage. Saves ~3–5 USD.

**Decision #6: Preprocess the dataset only once**

Instead of re-preprocessing every training, I saved the processed dataset to `processed/`. Subsequent runs just read it.

**Saves:** ~1–2 USD.

**Decision #7: Don't retrain for very small improvements**

AUC from 0.850 to 0.855 may not bring real-world value.

**Lesson:** "Good enough" is also a technical decision. Saves ~3–5 USD.

**Decision #8: Tag every resource**

Every resource is tagged:
- `Project`
- `Owner`
- `Environment`
- `AutoDelete=true`

When the project ends, just run the cleanup script.

**Lesson:** No tags = easy to forget resources and keep paying.

---

### DECISIONS 9–11: PIPELINE & TRAINING

**Decision #9:** Limit HPO: `max_parallel_jobs = 1`, `max_jobs = 6`. Total HPO ~0.6 USD.

**Decision #10:** Run the cleanup script weekly. It deletes old endpoints, cleans CloudWatch Logs, removes unnecessary resources.

**Lesson:** CloudWatch doesn't auto-clean logs.

**Decision #11:** Don't use a Notebook Instance for training. Only use SageMaker Training Job. The instance auto-terminates when training finishes.

**Lesson:** Notebooks and Training Jobs have completely different billing models.

---

### DECISIONS 12–13: IAM & NETWORK

**Decision #12:** Never use `AdministratorAccess`. IAM grants only what's needed.

**Lesson:** Least Privilege is safer AND avoids costly mistakes.

**Decision #13:** No NAT Gateway. NAT Gateway can cost ~30 USD/month.

Replace with:
- Gateway Endpoint
- Interface Endpoint
- Public Internet (when appropriate)

**Lesson:** Don't enable NAT Gateway if you don't really need it.

---

### COST SUMMARY (ESTIMATES)

| Item | Not optimized | Optimized |
| ---- | ------------- | --------- |
| Endpoint | 35 USD | 5 USD |
| S3 | 10 USD | 5 USD |
| HPO | 3 USD | 0.6 USD |
| Training | 10 USD | 3 USD |
| NAT Gateway | 30 USD | 0 USD |
| CloudWatch | 5 USD | 1 USD |
| Misc | 10 USD | 2 USD |
| **Total** | **~113 USD** | **~26.6 USD** |

In reality, my project ran **80–110 USD** for the full 8 weeks, still with plenty of buffer under the 200 USD cap.

---

### CHECKLIST BEFORE CREATING A RESOURCE

- [ ] Is this resource really needed?
- [ ] Can I use `t3.medium` instead of `m5`?
- [ ] Will the resource auto-delete?
- [ ] Have I tagged `Project` and `AutoDelete`?
- [ ] Is the IAM scope right?
- [ ] Does the Endpoint/Notebook have a shutdown schedule?
- [ ] Does the S3 bucket have a Lifecycle Rule?
- [ ] If I forget to shut it down, what's the weekly cost?

---

### CONCLUSION

I used to think using AWS was simple: build, then clean up. After this project, I realized:

> **You have to think about cleanup from the design stage.**

A budget cap isn't a limit. It forces you to design a better pipeline:
- With lifecycle
- With tagging
- With cleanup
- With clear IAM

These things are still useful even if the budget grows 10×.

If you're working on an AWS project with a limited budget, **don't see it as a disadvantage**. See it as an opportunity to learn how to design systems correctly from the start.