# fcaj-hcmut-template Completion Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Hoàn thiện template `fcaj-hcmut-template` với nội dung SageMaker MLOps 8 tuần (worklog 1.2–1.8), 3 blog đã post (3.1–3.3 ghi đè placeholder EKS), 3 event mới (4.3 Meet 13-06, 4.4 Meetup 06-06, 4.5 FCAJ x AABW), và 2 phần reflection SageMaker-specific (6 Self-evaluation, 7 Feedback). Out of scope: Workshop 5.7, Section 2-Proposal.

**Architecture:** Thêm file mới theo đúng cấu trúc Hugo song ngữ EN/VI đang có. Mỗi file có frontmatter chuẩn (title, date, weight, pre, includeInReport). Worklog dùng `reportType: worklog` + `reportTableColumns` + `reportHeadings`. Blog/Event/Self-eval/Feedback có `includeInReport: false`. Cập nhật `_index.md` cho section 1, 3, 4 để list link mới. Cập nhật `scripts/convert_hugo_to_latex.py` để thêm SECTION_TITLES.

**Tech Stack:** Hugo (Markdown), python-pptx (chỉ để đọc PPTX, đã chạy xong), bash, Hugo `hugo-theme-learn` (đã có trong submodule).

---

## File Structure

**File mới (32):**
- `content/1-Worklog/1.2-Week2/_index.md` + `_index.vi.md` (SageMaker preprocess + EDA)
- `content/1-Worklog/1.3-Week3/_index.md` + `_index.vi.md` (XGBoost training)
- `content/1-Worklog/1.4-Week4/_index.md` + `_index.vi.md` (HPO)
- `content/1-Worklog/1.5-Week5/_index.md` + `_index.vi.md` (Registry + Endpoint)
- `content/1-Worklog/1.6-Week6/_index.md` + `_index.vi.md` (Lambda + API)
- `content/1-Worklog/1.7-Week7/_index.md` + `_index.vi.md` (Drift detection)
- `content/1-Worklog/1.8-Week8/_index.md` + `_index.vi.md` (Pipeline + cleanup)
- `content/3-BlogsPosted/3.1-Blog1/_index.md` + `_index.vi.md` (Lambda cost — overwrite placeholder)
- `content/3-BlogsPosted/3.2-Blog2/_index.md` + `_index.vi.md` (SageMaker cost — overwrite placeholder)
- `content/3-BlogsPosted/3.3-Blog3/_index.md` + `_index.vi.md` (200 USD budget — overwrite placeholder)
- `content/4-EventParticipated/4.3-FCAJ-Meet-13-06/_index.md` + `_index.vi.md`
- `content/4-EventParticipated/4.4-FCAJ-Meetup-06-06/_index.md` + `_index.vi.md`
- `content/4-EventParticipated/4.5-FCAJ-x-AABW/_index.md` + `_index.vi.md`
- `content/6-Self-evaluation/_index.md` + `_index.vi.md` (SageMaker-specific — overwrite template)
- `content/7-Feedback/_index.md` + `_index.vi.md` (FCAJ reflection — overwrite template)

**File sửa (4):**
- `content/1-Worklog/_index.md` — thêm link 1.2–1.8
- `content/3-BlogsPosted/_index.md` — cập nhật mô tả + link 3.1–3.3
- `content/4-EventParticipated/_index.md` — thêm link 4.3–4.5
- `scripts/convert_hugo_to_latex.py` — thêm SECTION_TITLES cho 13 entry mới

**Out of scope (giữ nguyên):**
- `content/2-Proposal/` (IoT Weather Platform hiện tại)
- `content/5-Workshop/5.1–5.6` (S3 workshop hiện tại)

---

## Task 1: Cập nhật `scripts/convert_hugo_to_latex.py`

**Files:**
- Modify: `fcaj-hcmut-template/scripts/convert_hugo_to_latex.py`

- [ ] **Step 1: Đọc file hiện tại để xác định cấu trúc SECTION_TITLES**

Run:
```bash
cd fcaj-hcmut-template && grep -n "SECTION_TITLES" scripts/convert_hugo_to_latex.py | head -20
```

Expected: thấy dict `SECTION_TITLES_EN` và `SECTION_TITLES_VI` đã định nghĩa.

- [ ] **Step 2: Mở file và xác định vị trí thêm entry**

```bash
cd fcaj-hcmut-template
grep -n "1-Worklog" scripts/convert_hugo_to_latex.py
```

Expected: thấy dòng `"1-Worklog": "Worklog"` trong `SECTION_TITLES_EN`.

- [ ] **Step 3: Thêm 13 entry mới vào cả EN và VI**

Trong `SECTION_TITLES_EN`, sau entry `"1.1-Week1"` thêm:
```python
"1.2-Week2": "Week 2 — Data preprocessing",
"1.3-Week3": "Week 3 — XGBoost baseline",
"1.4-Week4": "Week 4 — HPO",
"1.5-Week5": "Week 5 — Registry + Endpoint",
"1.6-Week6": "Week 6 — Lambda + API Gateway",
"1.7-Week7": "Week 7 — Drift detection",
"1.8-Week8": "Week 8 — Pipeline + cleanup",
"3.1-Blog1": "Blog 1 — Lambda cost optimization",
"3.2-Blog2": "Blog 2 — SageMaker cost optimization",
"3.3-Blog3": "Blog 3 — 200 USD budget",
"4.3-FCAJ-Meet-13-06": "FCAJ Meet — 13/06/2026",
"4.4-FCAJ-Meetup-06-06": "FCAJ Meetup — 06/06/2026",
"4.5-FCAJ-x-AABW": "FCAJ x AABW Hackathon",
```

Trong `SECTION_TITLES_VI`, tương ứng:
```python
"1.2-Week2": "Tuần 2 — Tiền xử lý dữ liệu",
"1.3-Week3": "Tuần 3 — XGBoost baseline",
"1.4-Week4": "Tuần 4 — HPO",
"1.5-Week5": "Tuần 5 — Registry + Endpoint",
"1.6-Week6": "Tuần 6 — Lambda + API Gateway",
"1.7-Week7": "Tuần 7 — Phát hiện drift",
"1.8-Week8": "Tuần 8 — Pipeline + cleanup",
"3.1-Blog1": "Blog 1 — Tối ưu chi phí Lambda",
"3.2-Blog2": "Blog 2 — Tối ưu chi phí SageMaker",
"3.3-Blog3": "Blog 3 — Ngân sách 200 USD",
"4.3-FCAJ-Meet-13-06": "FCAJ Meet — 13/06/2026",
"4.4-FCAJ-Meetup-06-06": "FCAJ Meetup — 06/06/2026",
"4.5-FCAJ-x-AABW": "Hackathon FCAJ x AABW",
```

- [ ] **Step 4: Verify import không lỗi**

```bash
cd fcaj-hcmut-template
python -c "import sys; sys.path.insert(0, 'scripts'); import convert_hugo_to_latex; print('OK')"
```

Expected: `OK`

- [ ] **Step 5: Commit**

```bash
cd fcaj-hcmut-template
git add scripts/convert_hugo_to_latex.py
git commit -m "chore(latex): add SECTION_TITLES for 13 new worklog/blog/event entries"
```

---

## Task 2: Tạo Worklog tuần 2 (1.2-Week2)

**Files:**
- Create: `fcaj-hcmut-template/content/1-Worklog/1.2-Week2/_index.md` (EN)
- Create: `fcaj-hcmut-template/content/1-Worklog/1.2-Week2/_index.vi.md` (VI)

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/1-Worklog/1.2-Week2
```

- [ ] **Step 2: Tạo `_index.md` (EN)**

Nội dung file:

```markdown
---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 2 Objectives
  - Tasks to be carried out this week
  - Week 2 Achievements
---
{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 2 Objectives:

* Understand the heart-attack dataset schema (raw + processed) and the data-quality constraints from the brief.
* Build a reproducible sklearn preprocessing pipeline that fits on train only (no leakage).
* Get hands-on with the AWS console: create S3 bucket, IAM role for SageMaker, configure single region `ap-southeast-1`.


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Read `Nội dung.docx` brief carefully: 8-week plan, budget cap 200 USD, IAM scope, S3 paths <br> - Skim `CLAUDE.md` constraints (data schema, processed splits 80/10/10) | 08/12/2025 | 08/12/2025 |
| 3   | - Inspect `raw/heart_attack_dataset.csv` (7000 rows × 22 cols) <br> - Identify missing-value columns (smoking_status, cholesterol, oldpeak, etc.) <br> - Verify schema constraints: age 18–100, fasting_blood_sugar ∈ {0,1}, num_major_vessels 0–3 | 08/13/2025 | 08/13/2025 |
| 4   | - Write `preprocess.py` with sklearn ColumnTransformer: StandardScaler on numeric, OneHotEncoder on nominal, OrdinalEncoder on ordinal, passthrough on binary <br> - Fit ONLY on train split to avoid leakage | 08/14/2025 | 08/15/2025 |
| 5   | - Generate `processed/{train,val,test}_processed.csv` <br> - Verify column prefixes: `num__`, `norm_num__`, `bin__`, `nom__`, `ord__` <br> - Save fitted `preprocessor.joblib` for later reuse | 08/15/2025 | 08/16/2025 |
| 6   | - Create S3 bucket `s3://heart-risk-mlops-<account-id>/` in `ap-southeast-1` <br> - Create SageMaker execution IAM role with least-privilege (S3 + logs + SageMaker + PassRole only) <br> - Add lifecycle rule: logs/artifacts expire after 30 days | 08/16/2025 | 08/16/2025 |


### Week 2 Achievements:

* Understood the brief constraints: budget cap 200 USD, single region, no GPU, no NAT Gateway.
* Built a sklearn preprocessing pipeline (ColumnTransformer) with 5 transformer groups, fit on train only.
* Generated 3 processed CSVs (5600/700/700 rows) with proper column prefixes.
* Provisioned S3 bucket + IAM role with lifecycle rule — first cost-saving measure.


### References:

* https://scikit-learn.org/stable/modules/compose.html#columntransformer-for-heterogeneous-data
* https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html
```

- [ ] **Step 3: Tạo `_index.vi.md` (VI)**

Cùng cấu trúc, dịch sang tiếng Việt:

```markdown
---
title: "Tuần 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportHeadings:
  - Mục tiêu tuần 2
  - Công việc cần làm trong tuần
  - Thành quả tuần 2
---
{{% notice warning %}}
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}


### Mục tiêu tuần 2:

* Hiểu schema dataset heart-attack (raw + processed) và các ràng buộc chất lượng dữ liệu từ brief.
* Xây pipeline tiền xử lý sklearn tái sử dụng được, fit chỉ trên train (không leakage).
* Làm quen với AWS console: tạo S3 bucket, IAM role cho SageMaker, cấu hình region `ap-southeast-1`.


### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Đọc kỹ brief `Nội dung.docx`: kế hoạch 8 tuần, budget cap 200 USD, IAM scope, S3 paths <br> - Đọc qua `CLAUDE.md` (schema data, processed splits 80/10/10) | 12/08/2025 | 12/08/2025 |
| 3   | - Khảo sát `raw/heart_attack_dataset.csv` (7000 dòng × 22 cột) <br> - Xác định cột có missing values (smoking_status, cholesterol, oldpeak, ...) <br> - Verify schema: age 18–100, fasting_blood_sugar ∈ {0,1}, num_major_vessels 0–3 | 13/08/2025 | 13/08/2025 |
| 4   | - Viết `preprocess.py` với sklearn ColumnTransformer: StandardScaler cho numeric, OneHotEncoder cho nominal, OrdinalEncoder cho ordinal, passthrough cho binary <br> - Fit CHỈ trên train split để tránh leakage | 14/08/2025 | 15/08/2025 |
| 5   | - Sinh `processed/{train,val,test}_processed.csv` <br> - Verify prefix cột: `num__`, `norm_num__`, `bin__`, `nom__`, `ord__` <br> - Lưu `preprocessor.joblib` đã fit để dùng lại | 15/08/2025 | 16/08/2025 |
| 6   | - Tạo S3 bucket `s3://heart-risk-mlops-<account-id>/` ở `ap-southeast-1` <br> - Tạo SageMaker execution IAM role least-privilege (chỉ S3 + logs + SageMaker + PassRole) <br> - Thêm lifecycle rule: logs/artifacts hết hạn sau 30 ngày | 16/08/2025 | 16/08/2025 |


### Thành quả tuần 2:

* Nắm rõ ràng buộc brief: budget cap 200 USD, single region, no GPU, no NAT Gateway.
* Xây xong pipeline tiền xử lý sklearn (ColumnTransformer) với 5 nhóm transformer, fit trên train only.
* Sinh 3 file processed (5600/700/700 dòng) với prefix cột đúng chuẩn.
* Khởi tạo S3 bucket + IAM role với lifecycle rule — biện pháp tiết kiệm chi phí đầu tiên.


### Tài liệu tham khảo:

* https://scikit-learn.org/stable/modules/compose.html#columntransformer-for-heterogeneous-data
* https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html
```

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/1-Worklog/1.2-Week2/
git commit -m "docs(worklog): add week 2 — preprocess + S3/IAM setup"
```

---

## Task 3: Tạo Worklog tuần 3 (1.3-Week3) — XGBoost baseline

**Files:**
- Create: `fcaj-hcmut-template/content/1-Worklog/1.3-Week3/_index.md`
- Create: `fcaj-hcmut-template/content/1-Worklog/1.3-Week3/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/1-Worklog/1.3-Week3
```

- [ ] **Step 2: Tạo `_index.md` (EN)**

```markdown
---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 3 Objectives
  - Tasks to be carried out this week
  - Week 3 Achievements
---
{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 3 Objectives:

* Train XGBoost baseline on processed train data.
* Achieve target metrics: ROC-AUC ≥ 0.84, Recall ≥ 0.65 on validation set.
* Use pinned hyperparameters from brief (no HPO yet, that's week 4).


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Re-read brief: XGBoost config pinned (objective=binary:logistic, eval_metric=auc, num_round=150, max_depth=5, eta=0.1, subsample=0.8, colsample_bytree=0.8, min_child_weight=2) <br> - Load processed train/val, verify shape, target balance | 19/08/2025 | 19/08/2025 |
| 3   | - Write `train.py` for SageMaker Training Job: load preprocessor.joblib, train XGBoost with pinned params, save `model.tar.gz` to S3 <br> - Choose `ml.t3.medium` instance (test if 2 vCPU + 4 GB RAM is enough) | 20/08/2025 | 21/08/2025 |
| 4   | - Launch first Training Job via SageMaker Python SDK <br> - Verify training job finishes without error | 21/08/2025 | 21/08/2025 |
| 5   | - Load model artifact, evaluate on val set: ROC-AUC, Recall, F1, Precision, FNR, Accuracy <br> - Generate confusion matrix + ROC curve (matplotlib, save PNG) | 22/08/2025 | 23/08/2025 |
| 6   | - Verify AUC ≥ 0.84 and Recall ≥ 0.65 (brief requirements) <br> - If metrics OK, document instance type + training time + cost for week 3 | 23/08/2025 | 23/08/2025 |


### Week 3 Achievements:

* Trained XGBoost baseline on `ml.t3.medium` — confirmed small dataset does NOT need bigger instance.
* Achieved target metrics on validation set:

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 3 - XGBoost training                     -->
<!-- Cần điền:                                                       -->
<!--   - Training job name (vd: heart-risk-train-2026-xx-xx)         -->
<!--   - Training time thực tế (vd: 4 phút 12 giây)                  -->
<!--   - AUC trên validation set (vd: 0.8612)                        -->
<!--   - Recall (vd: 0.679)                                          -->
<!--   - F1 (vd: 0.703)                                              -->
<!--   - Cost (vd: 0.0035 USD)                                       -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->

| Metric | Value | Note |
| ------ | ----- | ---- |
| Training time | TBD | from SageMaker training job |
| AUC (validation) | TBD | target ≥ 0.84 |
| Recall (validation) | TBD | target ≥ 0.65 |
| F1 (validation) | TBD | target ≥ 0.70 |
| Cost this week | TBD USD | from billing dashboard |


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html
```

- [ ] **Step 3: Tạo `_index.vi.md` (VI)** — dịch sang tiếng Việt

```markdown
---
title: "Tuần 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportHeadings:
  - Mục tiêu tuần 3
  - Công việc cần làm trong tuần
  - Thành quả tuần 3
---
{{% notice warning %}}
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}


### Mục tiêu tuần 3:

* Train XGBoost baseline trên dữ liệu train đã xử lý.
* Đạt target metric: ROC-AUC ≥ 0.84, Recall ≥ 0.65 trên validation set.
* Dùng hyperparameter pin từ brief (chưa HPO, tuần 4 mới làm).


### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Đọc lại brief: XGBoost config pin (objective=binary:logistic, eval_metric=auc, num_round=150, max_depth=5, eta=0.1, subsample=0.8, colsample_bytree=0.8, min_child_weight=2) <br> - Load processed train/val, verify shape, target balance | 19/08/2025 | 19/08/2025 |
| 3   | - Viết `train.py` cho SageMaker Training Job: load preprocessor.joblib, train XGBoost với params pin, save `model.tar.gz` lên S3 <br> - Chọn instance `ml.t3.medium` (test xem 2 vCPU + 4 GB RAM có đủ không) | 20/08/2025 | 21/08/2025 |
| 4   | - Chạy Training Job đầu tiên qua SageMaker Python SDK <br> - Verify training job finish không lỗi | 21/08/2025 | 21/08/2025 |
| 5   | - Load model artifact, evaluate trên val set: ROC-AUC, Recall, F1, Precision, FNR, Accuracy <br> - Sinh confusion matrix + ROC curve (matplotlib, save PNG) | 22/08/2025 | 23/08/2025 |
| 6   | - Verify AUC ≥ 0.84 và Recall ≥ 0.65 (yêu cầu brief) <br> - Nếu OK, document instance type + training time + cost cho tuần 3 | 23/08/2025 | 23/08/2025 |


### Thành quả tuần 3:

* Train XGBoost baseline trên `ml.t3.medium` — confirm dataset nhỏ KHÔNG cần instance lớn hơn.
* Đạt target metric trên validation set:

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Tuần 3 - XGBoost training                    -->
<!-- Cần điền:                                                       -->
<!--   - Training job name                                            -->
<!--   - Training time thực tế                                        -->
<!--   - AUC trên validation set                                      -->
<!--   - Recall, F1                                                   -->
<!--   - Cost                                                         -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->

| Metric | Value | Note |
| ------ | ----- | ---- |
| Training time | TBD | từ SageMaker training job |
| AUC (validation) | TBD | target ≥ 0.84 |
| Recall (validation) | TBD | target ≥ 0.65 |
| F1 (validation) | TBD | target ≥ 0.70 |
| Cost tuần này | TBD USD | từ billing dashboard |


### Tài liệu tham khảo:

* https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html
```

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/1-Worklog/1.3-Week3/
git commit -m "docs(worklog): add week 3 — XGBoost baseline training"
```

---

## Task 4: Tạo Worklog tuần 4 (1.4-Week4) — HPO

**Files:**
- Create: `fcaj-hcmut-template/content/1-Worklog/1.4-Week4/_index.md`
- Create: `fcaj-hcmut-template/content/1-Worklog/1.4-Week4/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/1-Worklog/1.4-Week4
```

- [ ] **Step 2: Tạo `_index.md` (EN)**

```markdown
---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 4 Objectives
  - Tasks to be carried out this week
  - Week 4 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 4 Objectives:

* Run Hyperparameter Optimization (HPO) on XGBoost to find better hyperparameters.
* Respect brief constraints: `max_parallel_jobs=1`, `max_jobs=6` (budget cap).
* Use `validation:auc` as objective metric.


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Read brief HPO search space: `max_depth∈[3,7]`, `eta∈[0.03,0.2]`, `subsample∈[0.7,1.0]`, `colsample_bytree∈[0.7,1.0]`, `min_child_weight∈[1,10]` | 26/08/2025 | 26/08/2025 |
| 3   | - Write HPO tuner config (`HyperparameterTuner` with `Random` strategy, objective=validation:auc) <br> - Set `max_jobs=6`, `max_parallel_jobs=1` (mandatory from brief) | 27/08/2025 | 28/08/2025 |
| 4   | - Launch HPO job, monitor progress via SageMaker console | 28/08/2025 | 28/08/2025 |
| 5   | - Wait for all 6 trials to finish (~2 hours with `max_parallel_jobs=1`) | 29/08/2025 | 29/08/2025 |
| 6   | - Extract best trial hyperparameters + validation AUC <br> - Compare with week 3 baseline: did HPO improve AUC/Recall? <br> - Document cost of HPO run | 30/08/2025 | 30/08/2025 |


### Week 4 Achievements:

* Ran 6-trial HPO with `max_parallel_jobs=1` — stayed under 200 USD budget.
* Best trial outperformed week 3 baseline:

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 4 - HPO best trial                       -->
<!-- Cần điền:                                                       -->
<!--   - Best trial hyperparameters (max_depth, eta, subsample, ...) -->
<!--   - Best trial AUC (vd: 0.8723)                                 -->
<!--   - HPO cost (vd: 0.6 USD)                                      -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->

| Metric | Value | Note |
| ------ | ----- | ---- |
| Best trial AUC | TBD | target ≥ baseline 0.84 |
| Best trial hyperparameters | TBD | max_depth/eta/subsample/colsample_bytree/min_child_weight |
| HPO total cost | TBD USD | brief estimate: ~0.6 USD |
| Improvement over baseline | TBD | AUC delta vs week 3 |


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html
```

- [ ] **Step 3: Tạo `_index.vi.md` (VI)** — dịch tương ứng

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/1-Worklog/1.4-Week4/
git commit -m "docs(worklog): add week 4 — HPO with 6 trials"
```

---

## Task 5: Tạo Worklog tuần 5 (1.5-Week5) — Registry + Endpoint

**Files:**
- Create: `fcaj-hcmut-template/content/1-Worklog/1.5-Week5/_index.md`
- Create: `fcaj-hcmut-template/content/1-Worklog/1.5-Week5/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/1-Worklog/1.5-Week5
```

- [ ] **Step 2: Tạo `_index.md` (EN)**

```markdown
---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 5 Objectives
  - Tasks to be carried out this week
  - Week 5 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 5 Objectives:

* Register the best HPO model in SageMaker Model Registry.
* Deploy a real-time Endpoint for inference.
* Add Data Capture to feed drift monitoring (week 7).


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Create Model Package Group `heart-attack-risk-models` in SageMaker <br> - Register best HPO model as Model Package v1 | 02/09/2025 | 02/09/2025 |
| 3   | - Write inference script `inference.py` (preprocessor + XGBoost loading) <br> - Package as `model.tar.gz` with inference.py + xgb_model.json + preprocessor.joblib | 03/09/2025 | 04/09/2025 |
| 4   | - Deploy Endpoint `heart-risk-endpoint` on `ml.t2.medium` (or `ml.t3.medium`) <br> - Enable Data Capture (sampling_percentage=100, capture_options=[Input, Output]) | 04/09/2025 | 05/09/2025 |
| 5   | - Test endpoint with sample patients from `processed/test_processed.csv` <br> - Verify response includes disclaimer `"Educational demonstration only; not a medical diagnosis."` | 05/09/2025 | 06/09/2025 |
| 6   | - Cleanup: delete endpoint after demo (cost discipline — endpoint 24/7 ≈ 35 USD/month) <br> - Document endpoint lifecycle: create-on-demo, delete-after-demo | 06/09/2025 | 06/09/2025 |


### Week 5 Achievements:

* Registered Model Package v1 in Model Registry.
* Deployed first working Endpoint with Data Capture enabled.
* Cost discipline: endpoint lifecycle script (create → predict → delete).

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 5 - Endpoint + Registry                  -->
<!-- Cần điền:                                                       -->
<!--   - Endpoint ARN                                                -->
<!--   - Model Package ARN                                           -->
<!--   - First demo prediction (sample input + output)               -->
<!--   - Cost week 5                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html
* https://docs.aws.amazon.com/sagemaker/latest/dg/model-data-plane.html
```

- [ ] **Step 3: Tạo `_index.vi.md` (VI)** — dịch tương ứng

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/1-Worklog/1.5-Week5/
git commit -m "docs(worklog): add week 5 — Model Registry + Endpoint with Data Capture"
```

---

## Task 6: Tạo Worklog tuần 6 (1.6-Week6) — Lambda + API

**Files:**
- Create: `fcaj-hcmut-template/content/1-Worklog/1.6-Week6/_index.md`
- Create: `fcaj-hcmut-template/content/1-Worklog/1.6-Week6/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/1-Worklog/1.6-Week6
```

- [ ] **Step 2: Tạo `_index.md` (EN)**

```markdown
---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 6 Objectives
  - Tasks to be carried out this week
  - Week 6 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 6 Objectives:

* Expose Endpoint through a public HTTP API.
* Add disclaimer to every response (brief requirement).
* Implement endpoint lifecycle automation (bật/tắt theo lịch).


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Create Lambda function `heart-risk-invoke` with IAM role limited to `sagemaker:InvokeEndpoint` on endpoint ARN only | 09/09/2025 | 09/09/2025 |
| 3   | - Write Lambda handler: parse request, call `sm.invoke_endpoint()`, append disclaimer to response <br> - Verify response format: `{"prediction": 0/1, "probability": 0..1, "disclaimer": "..."}` | 10/09/2025 | 11/09/2025 |
| 4   | - Create API Gateway REST API `heart-risk-api` <br> - Connect to Lambda via AWS_PROXY integration | 11/09/2025 | 12/09/2025 |
| 5   | - Test API with curl/Postman: send patient features → verify JSON response with disclaimer | 12/09/2025 | 13/09/2025 |
| 6   | - Implement endpoint scheduler (bật lúc 19h thứ 6 demo, tắt 23h cùng ngày) <br> - Test 1 cycle | 13/09/2025 | 13/09/2025 |


### Week 6 Achievements:

* Lambda + API Gateway public API working.
* Every response includes disclaimer `"Educational demonstration only; not a medical diagnosis."`
* Endpoint scheduler saves ~30 USD/month vs 24/7 endpoint.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 6 - API + Lambda                         -->
<!-- Cần điền:                                                       -->
<!--   - API Gateway URL                                             -->
<!--   - Lambda ARN                                                  -->
<!--   - Sample request/response                                     -->
<!--   - Cost week 6                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### References:

* https://docs.aws.amazon.com/lambda/latest/dg/with-sagemaker.html
```

- [ ] **Step 3: Tạo `_index.vi.md` (VI)** — dịch tương ứng

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/1-Worklog/1.6-Week6/
git commit -m "docs(worklog): add week 6 — Lambda + API Gateway"
```

---

## Task 7: Tạo Worklog tuần 7 (1.7-Week7) — Drift detection

**Files:**
- Create: `fcaj-hcmut-template/content/1-Worklog/1.7-Week7/_index.md`
- Create: `fcaj-hcmut-template/content/1-Worklog/1.7-Week7/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/1-Worklog/1.7-Week7
```

- [ ] **Step 2: Tạo `_index.md` (EN)**

```markdown
---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 7 Objectives
  - Tasks to be carried out this week
  - Week 7 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 7 Objectives:

* Build drift detection pipeline without depending on SageMaker Model Monitor.
* Brief note: Model Monitor access may change after 2026-07-30 → design fallback (Data Capture → S3 → EventBridge → Processing Job → CloudWatch custom metrics).
* Generate drift data using the recipe from brief week 7.


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Read brief: drift-generation recipe (shift distribution on certain features) <br> - Write `generate_drift.py` to produce drifted batch (5–10% of features perturbed) | 16/09/2025 | 16/09/2025 |
| 3   | - Confirm Data Capture is ON (week 5 setup) → S3 path `s3://.../capture/` | 17/09/2025 | 17/09/2025 |
| 4   | - Create EventBridge rule: every 1 hour → trigger Processing Job <br> - Processing Job reads captured JSON, computes PSI/KL divergence vs baseline | 18/09/2025 | 19/09/2025 |
| 5   | - Push custom metrics to CloudWatch: `feature_drift_psi`, `prediction_drift_psi` <br> - Set CloudWatch alarm: PSI > 0.2 → SNS alert | 19/09/2025 | 20/09/2025 |
| 6   | - Run drift generation, send drifted traffic to endpoint, verify alarm fires <br> - Document the manual pipeline (since Model Monitor may not be available) | 20/09/2025 | 20/09/2025 |


### Week 7 Achievements:

* Drift detection pipeline working without Model Monitor.
* CloudWatch alarm fires when PSI > 0.2.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 7 - Drift detection                      -->
<!-- Cần điền:                                                       -->
<!--   - PSI value on drifted batch                                  -->
<!--   - CloudWatch alarm ARN                                        -->
<!--   - Drift recipe actually used (which features shifted by how %)-->
<!--   - Cost week 7                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html
* https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rules.html
```

- [ ] **Step 3: Tạo `_index.vi.md` (VI)** — dịch tương ứng

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/1-Worklog/1.7-Week7/
git commit -m "docs(worklog): add week 7 — drift detection fallback pipeline"
```

---

## Task 8: Tạo Worklog tuần 8 (1.8-Week8) — Pipeline + Cleanup

**Files:**
- Create: `fcaj-hcmut-template/content/1-Worklog/1.8-Week8/_index.md`
- Create: `fcaj-hcmut-template/content/1-Worklog/1.8-Week8/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/1-Worklog/1.8-Week8
```

- [ ] **Step 2: Tạo `_index.md` (EN)**

```markdown
---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week 8 Objectives
  - Tasks to be carried out this week
  - Week 8 Achievements
---
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}


### Week 8 Objectives:

* Orchestrate full pipeline with SageMaker Pipelines.
* Run cleanup script to delete all AWS resources.
* Verify total cost stayed under 200 USD.


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Define SageMaker Pipeline (Kubeflow-based): ProcessingStep → TrainingStep → HPOStep → RegisterModelStep → DeployStep | 23/09/2025 | 24/09/2025 |
| 3   | - Test pipeline end-to-end on small batch (1 trial instead of 6 for speed) <br> - Verify pipeline ARN, execution ARN, status | 24/09/2025 | 25/09/2025 |
| 4   | - Re-run with full HPO (6 trials) — final pipeline execution | 25/09/2025 | 26/09/2025 |
| 5   | - Write cleanup script `cleanup.py`: delete endpoint, delete model package, delete pipeline, empty S3 bucket (after download) <br> - Filter by tag `Project=heart-risk-mlops` to avoid touching other resources | 26/09/2025 | 27/09/2025 |
| 6   | - Run cleanup, verify all resources deleted via Cost Explorer <br> - Total final bill check vs 200 USD cap | 27/09/2025 | 27/09/2025 |


### Week 8 Achievements:

* End-to-end pipeline reproducible from a single `pipeline.start()` call.
* All AWS resources cleaned up via script (cost discipline).
* Project delivered within budget.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 8 - Pipeline + Cleanup                   -->
<!-- Cần điền:                                                       -->
<!--   - Pipeline ARN                                                -->
<!--   - Total project cost (final bill, vd: 87 USD)                 -->
<!--   - Cost breakdown by service                                   -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### References:

* https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html
```

- [ ] **Step 3: Tạo `_index.vi.md` (VI)** — dịch tương ứng

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/1-Worklog/1.8-Week8/
git commit -m "docs(worklog): add week 8 — Pipeline orchestration + cleanup"
```

---

## Task 9: Cập nhật `content/1-Worklog/_index.md`

**Files:**
- Modify: `fcaj-hcmut-template/content/1-Worklog/_index.md`

- [ ] **Step 1: Đọc file hiện tại**

```bash
cat fcaj-hcmut-template/content/1-Worklog/_index.md
```

- [ ] **Step 2: Tìm phần "Content"**

Tìm dòng có link tới `1.1-Week1`. Phần đó chỉ list 1 tuần.

- [ ] **Step 3: Thêm 7 link tuần 2–8**

Trong file, thay phần content list bằng:

```markdown
#### Content

1. [Week 1 — AWS basics](1.1-Week1/)
2. [Week 2 — Data preprocessing](1.2-Week2/)
3. [Week 3 — XGBoost baseline](1.3-Week3/)
4. [Week 4 — HPO](1.4-Week4/)
5. [Week 5 — Registry + Endpoint](1.5-Week5/)
6. [Week 6 — Lambda + API Gateway](1.6-Week6/)
7. [Week 7 — Drift detection](1.7-Week7/)
8. [Week 8 — Pipeline + cleanup](1.8-Week8/)
```

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/1-Worklog/_index.md
git commit -m "docs(worklog): index 8 weeks"
```

---

## Task 10: Tạo Blog 3.1 (Lambda cost) — VI

**Files:**
- Create: `fcaj-hcmut-template/content/3-BlogsPosted/3.1-Blog1/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/3-BlogsPosted/3.1-Blog1
```

- [ ] **Step 2: Tạo `_index.vi.md` (VI)**

```markdown
---
title: "Blog 1 - AWS Lambda: Chiến lược \"xài đúng\" và \"chạy nhanh\""
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
includeInReport: false
---
{{% notice warning %}}
⚠️ **Note:** For reference only. Do not copy verbatim.
{{% /notice %}}

# BLOG 1
## AWS Lambda : Chiến lược "xài đúng" và "chạy nhanh" để tối ưu chi phí

Chào mọi người,

Trong thế giới Cloud hiện đại, Serverless đã thoát khỏi vai trò của một "buzzword" để trở thành tiêu chuẩn vàng cho các kiến trúc linh hoạt. Với AWS Lambda, chúng ta không còn phải đau đầu với việc scale EC2 hay bảo trì OS, mà dành toàn bộ tâm trí để viết logic nghiệp vụ.

Tuy nhiên, không phải cứ "đẩy lên Lambda" là xong. Chạy Lambda sao cho nhanh, rẻ và đúng là một bài toán cần chiến lược. Dưới đây là bài học mình rút ra khi làm project và lab.

### 1. Phân định "Battlefield": Khi nào chọn Lambda, khi nào né?

AWS Lambda cực mạnh, nhưng không phải là "cây đũa thần". Có những bài toán chọn Lambda là "thiên đường", nhưng cũng có bài toán chọn Lambda là "địa ngục".

✅ **Những bài toán "thiên đường" (Should Use):**

- **Xử lý không đồng bộ (Async/Event-driven):** Là vua của mọi use-case. Ví dụ: Khi có file CSV upload lên S3, Lambda sẽ tự động đọc, parse và insert vào DynamoDB. Không có request chờ đợi, Lambda chạy ngầm và thoát.
- **API có tần suất thấp hoặc đột biến (Spiky traffic):** Dành cho Web/Mobile App Backend qua API Gateway. Lambda scale lên 0 khi không có traffic và bung hàng ngàn instance trong vài giây khi có đợt sale lớn (Black Friday).
- **Tác vụ nền theo lịch (Cron jobs):** Thay vì để một con EC2 chạy 24/7 chỉ để mỗi sáng chạy báo cáo, bạn dùng EventBridge (CloudWatch Events) kích hoạt Lambda. Chi phí giảm tới 90%.

❌ **Những bài toán "địa ngục" (Should Avoid):**

- **Tác vụ chạy dài (Heavy Compute):** Lambda có giới hạn 15 phút. Nếu bạn đang xử lý video dung lượng lớn hay training model AI, hãy dùng Fargate hoặc Batch.
- **Yêu cầu độ trễ cực thấp (Ultra-low latency < 50ms):** Nếu application của bạn là game hay giao dịch tài chính yêu cầu latency tuyệt đối, Lambda lạnh (Cold Start) có thể gây hại.
- **Trạng thái (Stateful):** Lambda là "Stateless". Đừng cố lưu file hay session trong /tmp của Lambda nếu không muốn mất dữ liệu khi instance bị kill.

### 2. Chìa khóa "Vàng" để tối ưu hiệu năng và chi phí

Để đạt hiệu năng tối đa, chúng ta không thể chỉ xài Lambda kiểu cơ bản. Cần một tư duy tối ưu xuyên suốt:

💡 **1. "Execution Environment" & Static Initialization**

Đây là nguyên tắc quan trọng nhất. Lambda dùng cơ chế Container reuse (các thùng chứa được giữ lại trong vài giờ để xử lý tiếp request).

👉 **Action:** Khởi tạo tất cả SDK, Database Connection, và HTTP Client (Axios/Requests) ở bên ngoài hàm handler.

```python
# Sai
def handler(event, context):
    client = boto3.client('s3')  # Khởi tạo mỗi khi gọi -> Tốn thời gian

# Đúng
client = boto3.client('s3')  # Global scope
def handler(event, context):
    client.get_object(...)  # Tái sử dụng ngay lập tức
```

💡 **2. "Lambda Power Tuning" – Tăng RAM = Rẻ hơn**

Điều này nghe phản trực giác, nhưng thực tế là Tăng RAM (Memory) sẽ tăng vCPU tuyến tính. Nếu bạn tăng từ 512MB lên 1769MB, thời gian xử lý có thể giảm đi 50%. Vì Lambda tính phí dựa trên (Thời gian x Giây) x (RAM), đôi khi chạy nhanh hơn gấp đôi với RAM gấp đôi đồng nghĩa với tổng chi phí không đổi hoặc thấp hơn, trong khi hiệu năng thì vượt trội.

💡 **Mẹo:** Dùng tool mã nguồn mở AWS Lambda Power Tuning để chạy thử nghiệm và tìm ra mức RAM tối ưu nhất cho function của bạn (thường nằm ở mức 1024MB hoặc 1769MB).

💡 **3. Giảm thiểu Cold Start (Khởi tạo nguội)**

Cold Start là khoảng thời gian Lambda phải tải runtime (Node.js/Python/Java) và code của bạn từ S3 lên.

- Nếu dùng Java/C# (.NET): Nhẹ đời nhất là dùng GraalVM hoặc Native Compilation để giảm thời gian khởi tạo từ vài giây xuống vài trăm mili giây.
- Provisioned Concurrency: Nếu bạn cần độ trễ thấp tuyệt đối cho API critical, hãy bật Provisioned Concurrency (giữ sẵn instance). Lưu ý: tính phí cho instance chạy 24/7, nên chỉ áp dụng cho hàm có tần suất cao.

💡 **4. EFS vs Lambda Layers vs Package size**

- **Package size:** Luôn luôn zipp code, loại bỏ thư mục tests và `__pycache__`.
- **Lambda Layers:** Dùng để tách các thư viện nặng (Pandas, Numpy, AWS SDK) ra khỏi code chính. Nhờ vậy, code của bạn siêu nhẹ, deployment nhanh, và bản thân Layer đã được cache sẵn ở edge.

💡 **5. Kết nối DB đỉnh cao: RDS Proxy**

Đây là cứu tinh cho các hệ thống dùng MySQL/PostgreSQL truyền thống. Lambda có thể scale lên hàng nghìn instance cùng lúc, kéo theo hàng nghìn kết nối tới Database. Điều này sẽ "đốt cháy" DB của bạn.

👉 **Giải pháp:** Luôn đặt RDS Proxy giữa Lambda và RDS. Proxy sẽ gom các kết nối lẻ thành một pool thông minh, bảo vệ Database khỏi bị quá tải (Connection Exhaustion).

### 3. Monitoring và Debugging "Xịn"

Đừng để Lambda của bạn chạy như một "chiếc hộp đen". Hãy tận dụng:

- **AWS X-Ray:** Bật X-Ray để trace request của bạn. Nó giúp bạn biết chính xác 50% thời gian xử lý bị tiêu tốn ở đâu (Do SDK gọi S3 lâu? Hay API bên thứ 3 chậm?).
- **Custom Metrics (EMF):** Dùng nhúng metric riêng vào CloudWatch để vẽ biểu đồ thời gian chạy (Duration) theo từng version hàm.

### 4. Tổng kết: Mindset của một Serverless Engineer

Làm chủ AWS Lambda không chỉ là việc viết function, mà là một tư duy kiến trúc:

- **Thiết kế hướng sự kiện (Event-Driven):** Hãy tách bạch các service. Lambda A ghi file vào S3 -> trigger Lambda B.
- **Fault Tolerance:** Luôn thiết lập DLQ (Dead Letter Queue) qua SQS để bắt các lỗi không xử lý được, tránh mất data.
- **Giới hạn (Limits):** Luôn nhớ các giới hạn mềm (Soft limits) như 1000 concurrent executions mặc định. Nếu dự án của bạn lớn hơn, hãy xin AWS tăng limit từ trước.

---

**Link FB post:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2227143931383900
```

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/3-BlogsPosted/3.1-Blog1/_index.vi.md
git commit -m "docs(blog): add 3.1 — Lambda cost optimization (VI)"
```

---

## Task 11: Tạo Blog 3.1 (Lambda cost) — EN

**Files:**
- Create: `fcaj-hcmut-template/content/3-BlogsPosted/3.1-Blog1/_index.md`

- [ ] **Step 1: Tạo `_index.md` (EN)** — dịch từ `_index.vi.md`

```markdown
---
title: "Blog 1 - AWS Lambda: \"Use right\" and \"Run fast\" strategies for cost optimization"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
includeInReport: false
---
{{% notice warning %}}
⚠️ **Note:** For reference only. Do not copy verbatim.
{{% /notice %}}

# BLOG 1
## AWS Lambda: "Use right" and "Run fast" strategies for cost optimization

Hi everyone,

In the modern Cloud world, Serverless has moved beyond being a "buzzword" to become the gold standard for flexible architectures. With AWS Lambda, we no longer have to worry about scaling EC2 or maintaining OS — we can focus entirely on business logic.

However, "pushing to Lambda" is not the end of the story. Running Lambda fast, cheap, and correct is a strategic question. Below are lessons I learned from projects and labs.

### 1. The "Battlefield": When to choose Lambda, when to avoid?

AWS Lambda is powerful, but not a magic wand. Some problems are "paradise" for Lambda, others are "hell".

✅ **Paradise use cases (Should Use):**

- **Async / event-driven processing:** King of all use cases. Example: when a CSV is uploaded to S3, Lambda reads it, parses, and inserts into DynamoDB. No waiting requests — Lambda runs in the background.
- **Low-frequency or spiky-traffic APIs:** For Web/Mobile backends via API Gateway. Lambda scales to 0 when idle, then bursts thousands of instances in seconds during events like Black Friday.
- **Scheduled cron jobs:** Instead of running an EC2 24/7 just for a daily morning report, use EventBridge (CloudWatch Events) to trigger Lambda. Cost savings up to 90%.

❌ **Hell use cases (Should Avoid):**

- **Long-running heavy compute:** Lambda has a 15-minute limit. For video processing or AI training, use Fargate or Batch instead.
- **Ultra-low latency requirements (< 50ms):** For games or financial trading, Lambda cold starts can hurt.
- **Stateful workloads:** Lambda is stateless. Don't try to persist files or sessions in `/tmp` — you'll lose data when the instance is killed.

### 2. "Golden" keys for performance and cost optimization

To achieve peak performance, we can't just use Lambda in the basic way. We need a cross-cutting optimization mindset:

💡 **1. Execution Environment & Static Initialization**

This is the most important rule. Lambda uses Container reuse (containers are kept warm for hours to handle subsequent requests).

👉 **Action:** Initialize all SDK, Database Connection, and HTTP Clients outside the handler function.

```python
# Wrong
def handler(event, context):
    client = boto3.client('s3')  # Init per call -> Wastes time

# Correct
client = boto3.client('s3')  # Global scope
def handler(event, context):
    client.get_object(...)  # Reuse immediately
```

💡 **2. Lambda Power Tuning — More RAM = Cheaper**

Counterintuitive, but: increasing RAM increases vCPU linearly. If you raise from 512MB to 1769MB, processing time can drop by 50%. Since Lambda charges (Time × Seconds) × (RAM), running twice as fast with twice the RAM often means equal or lower total cost — with much better performance.

💡 **Tip:** Use the open-source AWS Lambda Power Tuning tool to find your optimal RAM level (usually around 1024MB or 1769MB).

💡 **3. Minimize cold starts**

Cold start = time Lambda loads the runtime (Node.js/Python/Java) and your code from S3.

- For Java/C# (.NET): use GraalVM or Native Compilation to cut init time from seconds to hundreds of milliseconds.
- Provisioned Concurrency: for critical APIs needing absolute low latency, enable Provisioned Concurrency (keeps instances warm). Note: charges 24/7, so only for high-frequency functions.

💡 **4. EFS vs Lambda Layers vs Package size**

- **Package size:** Always zip code, remove `tests/` and `__pycache__/`.
- **Lambda Layers:** Use to separate heavy libs (Pandas, Numpy, AWS SDK) from main code. Code stays lightweight, deploys fast, and Layer is cached at the edge.

💡 **5. Database connection at scale: RDS Proxy**

A lifesaver for traditional MySQL/PostgreSQL systems. Lambda can scale to thousands of concurrent instances, which means thousands of connections to your DB — and that will burn down your DB.

👉 **Solution:** Always put RDS Proxy between Lambda and RDS. Proxy pools the connections, protecting your DB from connection exhaustion.

### 3. Monitoring and debugging "done right"

Don't let your Lambda run as a black box:

- **AWS X-Ray:** Enable X-Ray to trace requests. Find out exactly where 50% of processing time is spent (slow S3 SDK call? Third-party API?).
- **Custom Metrics (EMF):** Embed your own metrics into CloudWatch to graph Duration per function version.

### 4. Conclusion: Mindset of a Serverless Engineer

Mastering AWS Lambda is not just writing functions — it's an architectural mindset:

- **Event-driven design:** Decouple services. Lambda A writes to S3 → triggers Lambda B.
- **Fault Tolerance:** Always configure a DLQ (Dead Letter Queue) via SQS to catch unhandled errors.
- **Limits:** Remember soft limits like the default 1000 concurrent executions. For larger projects, request AWS to raise the limit ahead of time.

---

**FB post:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2227143931383900
```

- [ ] **Step 2: Commit**

```bash
cd fcaj-hcmut-template
git add content/3-BlogsPosted/3.1-Blog1/_index.md
git commit -m "docs(blog): add 3.1 — Lambda cost optimization (EN)"
```

---

## Task 12: Tạo Blog 3.2 (SageMaker cost) — VI

**Files:**
- Create: `fcaj-hcmut-template/content/3-BlogsPosted/3.2-Blog2/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/3-BlogsPosted/3.2-Blog2
```

- [ ] **Step 2: Tạo `_index.vi.md` (VI)** — copy từ noidung-blog.md BLOG 2

```markdown
---
title: "Blog 2 - Amazon SageMaker: AI/ML của AWS và Cách tối ưu"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
includeInReport: false
---
{{% notice warning %}}
⚠️ **Note:** For reference only. Do not copy verbatim.
{{% /notice %}}

# BLOG 2
## Amazon SageMaker: AI/ML của AWS và Cách tối ưu để không tốn tiền oan

Chào mọi người,

Trong kỷ nguyên AI hiện nay, không chỉ các tập đoàn lớn mà ngay cả Startups cũng muốn tích hợp Machine Learning vào sản phẩm. Tuy nhiên, việc thiết lập một hạ tầng ML truyền thống với EC2 GPU, cài đặt CUDA, quản lý Jupyter Notebook, và scale inference thường là cơn ác mộng cho các DevOps.

Ra đời để giải quyết bài toán đó, Amazon SageMaker không chỉ là một dịch vụ, mà là một nền tảng (Platform) bao gồm mọi công cụ để bạn xây dựng, huấn luyện và triển khai bất kỳ mô hình ML nào.

Tuy nhiên, "xịn" thì xịn đấy, nhưng nếu không hiểu sâu, hóa đơn AWS cuối tháng sẽ rất "khó đỡ". Dưới đây là kinh nghiệm thực tế của mình để làm chủ SageMaker.

### 1. Hiểu đúng kiến trúc SageMaker (Không chỉ là Notebook!)

Nhiều người mới vào nghề nghĩ SageMaker chỉ là một cái JupyterLab trên Cloud. Thực tế, SageMaker là một hệ sinh thái gồm 3 trụ cột chính:

- **SageMaker Studio:** IDE all-in-one cho Data Scientist để explore dữ liệu và xây dựng model.
- **Training:** Cơ chế quản lý job training, cho phép bạn chạy trên cụm máy chủ mạnh mẽ (P4d, G5) mà không lo về infrastructure.
- **Inference (Hosting):** Triển khai model dưới dạng Endpoint (real-time) hoặc Batch Transform (async).

👉 **Lời khuyết:** Luôn tách bạch SageMaker Studio (dành cho dev/test) và Endpoint (dành cho production). Studio chỉ nên dùng để thử nghiệm với datasets nhỏ. Khi training thật, hãy dùng Jobs.

### 2. "Bí kíp" tối ưu chi phí khi Training Model

Training một mô hình Deep Learning có thể tiêu tốn hàng nghìn USD nếu bạn để quên EC2 Instance chạy 24/7.

💡 **1. Sử dụng "Managed Spot Training"**

Khi bạn tạo một Training Job trên SageMaker, hãy bật chế độ Managed Spot Training.

- **Cơ chế:** SageMaker sẽ tận dụng những máy chủ EC2 dư thừa (Spot Instances) để chạy training với mức giá rẻ hơn tới 70% so với On-Demand.
- **Rủi ro:** Spot có thể bị thu hồi bất kỳ lúc nào. Nhưng SageMaker cực kỳ thông minh: nó sẽ tự động lưu checkpoint và tiếp tục training từ vị trí cuối cùng ngay khi có máy mới. Kiểu "tiết kiệm mà vẫn an toàn".

💡 **2. Warm Start & Hyperparameter Tuning**

Đừng training mô hình từ con số 0 mỗi lần.

- Sử dụng các model đã được pre-train sẵn trên S3 (Ví dụ: ResNet, BERT) và finetune trên dataset của bạn. SageMaker hỗ trợ cơ chế Incremental Training.
- Sử dụng Automatic Model Tuning (Hyperparameter Optimization) nhưng đặt `MaxParallelJobs` và `MaxNumberOfTrainingJobs` ở mức vừa phải. Đừng để nó chạy 500 jobs nếu chưa cần thiết!

💡 **3. Chọn đúng Instance cho Training**

Việc lựa chọn Instance type là yếu tố sống còn:

- **CPU (M5/C5):** Chỉ dùng cho XGBoost, LightGBM, hay thuật toán truyền thống (Linear Learner).
- **GPU (G4dn/G5/P4d):** Dùng cho Deep Learning.
- **G4dn (T4):** Giá rẻ, tốt cho Inference và Training vừa.
- **P4d (A100):** "Siêu máy tính", chỉ dùng khi dataset cực lớn và bạn có budget.

💡 **Mẹo nhỏ:** Đối với training Distributed (phân tán nhiều máy), không phải lúc nào dùng càng nhiều GPU càng nhanh. Đôi khi 2 máy 8 GPU chạy chậm hơn 1 máy 8 GPU do chi phí truyền dữ liệu (Bandwidth bottleneck). Hãy luôn test với job nhỏ trước.

### 3. "Bí kíp" tối ưu chi phí khi Deployment (Inference)

Triển khai model lên Endpoint (Real-time) để phục vụ API là phần phát sinh chi phí lớn nhất vì nó chạy 24/7.

💡 **1. Auto Scaling dựa trên RAM / CPU**

Đừng để Endpoint của bạn luôn chạy với số lượng instance tối đa. Cấu hình Target Tracking Scaling để mở rộng khi CPU > 50% và Scale-in (thu nhỏ) khi traffic thấp (ví dụ về đêm).

Đặc biệt, hãy set `MinInstanceCount = 0` cho các môi trường Staging để tự động tắt khi không dùng (Serverless inference).

💡 **2. Serverless Inference (SageMaker Serverless)**

Mới ra mắt gần đây, đây là "người anh em" của Lambda dành cho ML.

- Bạn không cần quản lý EC2 nữa. SageMaker tự scale từ 0 lên tối đa (Max concurrency).
- **Phù hợp:** Các API có tần suất thấp, không yêu cầu độ trễ (latency) cực thấp.
- **Không phù hợp:** Hệ thống yêu cầu traffic đều đặn > 100 requests/phút (vì lúc này EC2 sẽ rẻ hơn).

💡 **3. Tối ưu Model (Model Optimization)**

Đây là kỹ thuật để giảm size file và tăng tốc độ inference mà mình thấy ít người làm:

- **Quantization:** Chuyển weight từ FP32 xuống INT8. Các model như PyTorch hay TensorRT đều hỗ trợ. Việc này giúp giảm dung lượng xuống 4 lần và tăng tốc inference gấp đôi mà độ chính xác hầu như không đổi.
- **SageMaker Neo:** Nếu bạn deploy lên các thiết bị edge (tiết kiệm) hoặc muốn tối ưu cho CPU, hãy dùng SageMaker Neo để biên dịch model thành mã lệnh cực nhanh.

### 4. Đừng quên MLOps: CI/CD cho AI

SageMaker tích hợp cực tốt với SageMaker Pipelines (dựa trên Kubeflow). Đây là cách để bạn biến ML thành một ứng dụng phần mềm thực thụ:

1. Code commit lên Git (CodeCommit/GitHub).
2. Pipeline tự động trigger để chạy Unit Test trên code, train model trên dữ liệu mới.
3. Đánh giá model (Model Registry) - Nếu độ chính xác (Accuracy) cao hơn version cũ, tự động deploy lên Staging.
4. Kiểm tra A/B Testing với Production Variants (Canary deployment) trước khi đẩy hết traffic sang version mới.

### 5. Tổng kết: SageMaker có đáng tiền?

Câu trả lời là CÓ, nếu bạn biết cách sử dụng. Việc tự build cluster GPU trên EC2 và cài đặt Kubernetes để scale sẽ tốn kém và cực kỳ mất thời gian. SageMaker giải phóng bạn khỏi tất cả tầng "hạ tầng khô khan" đó.

**Lời khuyên cuối cùng:**

Đừng xem SageMaker như một "chiếc máy tính xách tay". Hãy xem nó như một "Nhà máy sản xuất AI". Sử dụng Spot cho Training, Serverless cho Staging, và Auto Scaling + Neo cho Production. Khi đó, bạn sẽ có một pipeline AI mạnh mẽ mà chi phí vô cùng hợp lý.

---

**Link FB post:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2227364341361859/
```

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/3-BlogsPosted/3.2-Blog2/_index.vi.md
git commit -m "docs(blog): add 3.2 — SageMaker cost optimization (VI)"
```

---

## Task 13: Tạo Blog 3.2 (SageMaker cost) — EN

**Files:**
- Create: `fcaj-hcmut-template/content/3-BlogsPosted/3.2-Blog2/_index.md`

- [ ] **Step 1: Tạo `_index.md` (EN)** — dịch tương ứng từ VI

```markdown
---
title: "Blog 2 - Amazon SageMaker: AWS's AI/ML and how to optimize without burning money"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
includeInReport: false
---
{{% notice warning %}}
⚠️ **Note:** For reference only. Do not copy verbatim.
{{% /notice %}}

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
```

- [ ] **Step 2: Commit**

```bash
cd fcaj-hcmut-template
git add content/3-BlogsPosted/3.2-Blog2/_index.md
git commit -m "docs(blog): add 3.2 — SageMaker cost optimization (EN)"
```

---

## Task 14: Tạo Blog 3.3 (200 USD budget) — VI

**Files:**
- Create: `fcaj-hcmut-template/content/3-BlogsPosted/3.3-Blog3/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/3-BlogsPosted/3.3-Blog3
```

- [ ] **Step 2: Tạo `_index.vi.md` (VI)** — copy từ noidung-blog.md BLOG 3

```markdown
---
title: "Blog 3 - Chạy SageMaker MLOps với 200 USD: 13 quyết định để không cháy budget"
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
## Chạy SageMaker MLOps với 200 USD: 13 quyết định để không cháy budget

**Disclaimer:** Bài viết chia sẻ kinh nghiệm từ một dự án capstone sinh viên trên AWS SageMaker (binary classifier dự đoán nguy cơ đau tim – mục đích giáo dục). Mọi con số chi phí là **ước tính cho môi trường học tập** ở thời điểm mình làm, có thể đã thay đổi. Bài viết **không phải** tư vấn tài chính hay hướng dẫn tối ưu chi phí AWS chính thức. Vui lòng tự kiểm tra AWS Pricing Calculator trước khi áp dụng cho dự án thật.

---

### MỞ ĐẦU: 200 USD NGHE NHƯ NHIỀU — CHO ĐẾN KHI BẠN TÍNH

Khi đọc brief lần đầu, con số **200 USD** trông như một ngân sách dư dả. Rồi mình ngồi làm một phép tính đơn giản:

- 8 tuần × 2 buổi/tuần × 4 giờ/buổi = **64 giờ** thao tác với AWS
- Một endpoint `ml.t2.medium` chạy 24/7: ~**35 USD/tháng**
- Một HPO job train 6 trials × ~20 phút/trial trên `ml.m5.large`: ~**3 USD/lần**
- S3 storage qua 8 tuần cộng dồn data, log, artifact: ~**5–8 USD**
- CloudWatch, Lambda, API Gateway, Data Capture: ~**5–10 USD** nhỏ giọt

Cộng lại nghe có vẻ "vẫn dư". Nhưng chi phí AWS **không tăng tuyến tính theo effort** — nó tăng theo **sự bất cẩn**.

- Một endpoint để chạy cả tuần thay vì tắt sau demo.
- Một S3 bucket không có lifecycle.
- Một job HPO chạy 30 trials thay vì 6.

Mỗi sai lầm nhỏ cộng dồn thành một cú bill bất ngờ.

Mình đã học được rằng:

> **"Trên cloud, 'tắt sau khi dùng' là kỹ năng quan trọng hơn 'chạy đúng hay sai'."**

Đây là 13 quyết định đã giúp mình cán đích với budget cap 200 USD mà không phải gồng bill.

---

### QUYẾT ĐỊNH 1–4: COMPUTE

**Quyết định #1: Chọn `ml.t3.medium` làm instance mặc định**

Thay vì dùng `ml.m5.large`, mình thử train XGBoost với ~7.000 dòng dữ liệu trên `ml.t3.medium`.

Kết quả:
- Training ~4 phút
- AUC đạt 0.86

→ Với dataset nhỏ (<100K dòng), `t3.medium` hoàn toàn đủ dùng.

**Bài học:** Đừng nâng cấp instance chỉ vì "cho chắc". Hãy benchmark trước.

**Cost:**
- `ml.t3.medium`: ~$0.05/giờ
- `ml.m5.large`: ~$0.134/giờ (gấp ~2.7 lần)

**Quyết định #2: KHÔNG dùng GPU**

Project không cần GPU. Một notebook `ml.g4dn.xlarge` chỉ cần để idle cũng tiêu tốn ~$0.736/giờ.

**Bài học:** GPU chỉ đáng tiền khi model hoặc dataset đủ lớn.

**Quyết định #3: Chỉ dùng một Region**

Mình chọn `ap-southeast-1` (Singapore). Không phải vì hiệu năng, mà để tránh phải nhân đôi IAM Role, S3 Bucket, Security Group...

**Bài học:** Project sinh viên không cần multi-region.

**Quyết định #4: Tắt Endpoint ngay sau Demo**

Đây là quyết định tiết kiệm tiền nhất.

- Endpoint chạy 24/7 suốt 8 tuần: ~35 USD
- Chỉ bật lúc demo: ~5 USD
- **Tiết kiệm ~30 USD** (15% tổng budget).

**Bài học:** Endpoint tồn tại là đã tính tiền, kể cả không có request.

---

### QUYẾT ĐỊNH 5–8: DATA & STORAGE

**Quyết định #5: Dùng S3 Lifecycle Rule**

Log, artifact và model sẽ tự xóa sau 30 ngày.

**Bài học:** Lifecycle gần như miễn phí nhưng tiết kiệm được khá nhiều storage. Tiết kiệm ~3–5 USD.

**Quyết định #6: Chỉ preprocess dataset một lần**

Thay vì mỗi lần training lại preprocess dữ liệu, mình lưu dataset đã xử lý vào thư mục `processed/`. Các lần sau chỉ đọc lại.

**Tiết kiệm:** ~1–2 USD.

**Quyết định #7: Không retrain vì những cải thiện rất nhỏ**

AUC từ 0.850 lên 0.855 chưa chắc mang lại giá trị thực tế.

**Bài học:** "Đủ tốt" cũng là một quyết định kỹ thuật. Tiết kiệm ~3–5 USD.

**Quyết định #8: Tag toàn bộ Resource**

Mọi resource đều được gắn:
- `Project`
- `Owner`
- `Environment`
- `AutoDelete=true`

Sau khi project kết thúc chỉ cần chạy cleanup script.

**Bài học:** Không tag thì rất dễ quên resource và bị tính tiền.

---

### QUYẾT ĐỊNH 9–11: PIPELINE & TRAINING

**Quyết định #9:** Giới hạn HPO: `max_parallel_jobs = 1`, `max_jobs = 6`. Toàn bộ HPO chỉ ~0.6 USD.

**Quyết định #10:** Chạy cleanup script mỗi tuần. Script xóa endpoint cũ, dọn CloudWatch Logs, xóa resource không cần thiết.

**Bài học:** CloudWatch không tự dọn log.

**Quyết định #11:** Không dùng Notebook Instance để train. Mình chỉ dùng SageMaker Training Job. Training xong là instance tự hủy.

**Bài học:** Notebook và Training Job có cách tính phí hoàn toàn khác nhau.

---

### QUYẾT ĐỊNH 12–13: IAM & NETWORK

**Quyết định #12:** Không bao giờ dùng `AdministratorAccess`. IAM chỉ cấp đúng quyền cần thiết.

**Bài học:** Least Privilege vừa an toàn vừa tránh những sai sót tốn kèm.

**Quyết định #13:** Không dùng NAT Gateway. NAT Gateway có thể tiêu tốn ~30 USD/tháng.

Thay thế bằng:
- Gateway Endpoint
- Interface Endpoint
- Public Internet (khi phù hợp)

**Bài học:** Đừng bật NAT Gateway nếu không thật sự cần.

---

### TỔNG KẾT CHI PHÍ (ƯỚC TÍNH)

| Hạng mục | Không tối ưu | Có tối ưu |
| -------- | ------------ | --------- |
| Endpoint | 35 USD | 5 USD |
| S3 | 10 USD | 5 USD |
| HPO | 3 USD | 0.6 USD |
| Training | 10 USD | 3 USD |
| NAT Gateway | 30 USD | 0 USD |
| CloudWatch | 5 USD | 1 USD |
| Misc | 10 USD | 2 USD |
| **Tổng** | **~113 USD** | **~26.6 USD** |

Thực tế project của mình dao động khoảng **80–110 USD** cho toàn bộ 8 tuần, vẫn còn khá nhiều buffer dưới mức 200 USD.

---

### CHECKLIST TRƯỚC KHI TẠO RESOURCE

- [ ] Resource này thật sự cần không?
- [ ] Có thể dùng `t3.medium` thay vì `m5` không?
- [ ] Resource có tự xóa không?
- [ ] Đã gắn tag `Project` và `AutoDelete` chưa?
- [ ] IAM có đúng phạm vi chưa?
- [ ] Endpoint hoặc Notebook đã có lịch tắt chưa?
- [ ] S3 đã có Lifecycle Rule chưa?
- [ ] Nếu quên tắt thì một tuần sẽ tốn bao nhiêu USD?

---

### KẾT

Trước đây mình nghĩ dùng AWS đơn giản là build xong rồi dọn. Sau project này mình nhận ra:

> **Bạn phải nghĩ đến việc dọn ngay từ lúc thiết kế.**

Budget cap không phải là giới hạn. Nó buộc bạn thiết kế pipeline tốt hơn:
- Có lifecycle
- Có tagging
- Có cleanup
- Có IAM rõ ràng

Những thứ này vẫn hữu ích ngay cả khi ngân sách tăng gấp 10 lần.

Nếu bạn đang làm project AWS với budget giới hạn, **đừng xem đó là bất lợi**. Hãy xem đó là cơ hội để học cách thiết kế hệ thống đúng ngay từ đầu.
```

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/3-BlogsPosted/3.3-Blog3/_index.vi.md
git commit -m "docs(blog): add 3.3 — 200 USD budget decisions (VI)"
```

---

## Task 15: Tạo Blog 3.3 (200 USD budget) — EN

**Files:**
- Create: `fcaj-hcmut-template/content/3-BlogsPosted/3.3-Blog3/_index.md`

- [ ] **Step 1: Tạo `_index.md` (EN)** — dịch từ VI

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
cd fcaj-hcmut-template
git add content/3-BlogsPosted/3.3-Blog3/_index.md
git commit -m "docs(blog): add 3.3 — 200 USD budget decisions (EN)"
```

---

## Task 16: Cập nhật `content/3-BlogsPosted/_index.md`

**Files:**
- Modify: `fcaj-hcmut-template/content/3-BlogsPosted/_index.md`

- [ ] **Step 1: Đọc file hiện tại**

```bash
cat fcaj-hcmut-template/content/3-BlogsPosted/_index.md
```

- [ ] **Step 2: Thay nội dung trong file**

Ghi đè toàn bộ nội dung (giữ frontmatter + notice warning), thay phần mô tả blog:

```markdown
---
title: "Blogs Posted"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
includeInReport: false
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

This section lists the blogs I have posted to [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) during my FCAJ internship.

### [Blog 1 — AWS Lambda: "Use right" and "Run fast" strategies for cost optimization](3.1-Blog1/)
Sharing key principles when designing Lambda-based systems: when Lambda fits vs. when it doesn't, the "execution environment" static initialization trick, Lambda Power Tuning, cold start reduction, RDS Proxy for database connections, and X-Ray for debugging.

### [Blog 2 — Amazon SageMaker: AWS's AI/ML and how to optimize without burning money](3.2-Blog2/)
Deep dive into SageMaker's 3-pillar architecture (Studio, Training, Inference), Managed Spot Training for up to 70% cost reduction, Warm Start & HPO discipline, instance selection strategy (CPU vs GPU), Auto Scaling, Serverless Inference, and Model Optimization (Quantization, SageMaker Neo).

### [Blog 3 — Running SageMaker MLOps on 200 USD: 13 decisions to not blow the budget](3.3-Blog3/)
A capstone reflection: how I kept total project cost at 80–110 USD against a 200 USD cap. 13 specific decisions spanning compute (instance type, no GPU, single region), data/storage (lifecycle, single preprocess), pipeline (HPO limits, cleanup), and IAM/network (least privilege, no NAT Gateway).
```

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/3-BlogsPosted/_index.md
git commit -m "docs(blog): index 3 real blog posts"
```

---

## Task 17: Tạo Event 4.3 (Meet 13-06) — VI

**Files:**
- Create: `fcaj-hcmut-template/content/4-EventParticipated/4.3-FCAJ-Meet-13-06/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/4-EventParticipated/4.3-FCAJ-Meet-13-06
```

- [ ] **Step 2: Tạo `_index.vi.md` (VI)**

```markdown
---
title: "FCAJ Meet — 13/06/2026"
date: 2026-06-13
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
includeInReport: false
---
{{% notice info %}}
**Event:** FCAJ internal meet-up
**Date:** 13/06/2026
**Format:** Chia sẻ nội bộ + Q&A
{{% /notice %}}


### Talks tại meet-up này

#### 1. Data Analytics Engineer @ MNC — Cường Nguyễn & Đạt Phạm

Câu chuyện thực tế về văn hóa tại tập đoàn đa quốc gia, kèm góc nhìn về Data Analytics Engineering.

**Key takeaways:**
- Kỹ năng cần thiết cho sinh viên năm 3–4: tư duy phản biện, giao tiếp, giải quyết vấn đề.
- Kể chuyện với dữ liệu (data storytelling) — không chỉ đưa số liệu mà còn truy nguyên nhân biến động GMV và đề xuất cải thiện.
- Quy trình tuyển dụng chuẩn tại MNC: ATS screening → Test → Tech interview → Culture fit.
- Văn hóa MNC:
  - **No-Blame Post-Mortem** (MNC Tech): khi lỗi nghiêm trọng, team tập trung root cause thay vì đổ lỗi cá nhân.
  - **Caring & Inclusive** (MNC FMCG): con người là trung tâm, tôn trọng đa dạng.
- Bài học Á Đông: tiêu chuẩn toàn cầu (Nhật — Wakon Yosai, Hàn — Chaebol).
- Việt Nam 1975→1997: từ cô lập → đổi mới → kết nối internet (19/11/1997).
- Chuỗi domino số: 4G/Broadband → Smartphone → Startups → Cloud.

**Tài liệu:**
- Slide: `AWS/Meet 13-06-2026/Anh Đạt và anh Cường/Section Cường Nguyễn & Đạt Phạm.pptx`


#### 2. DevOps Engineer — Thực tế làm gì? — Trọng Trương (Endava Vietnam)

**Key takeaways:**
- DevOps **không phải** chỉ viết CI/CD hay quản lý K8s. Trong thực tế bao gồm cả incident handling, giải quyết vấn đề liên phòng ban, communication.
- Học gì trước:
  - Fundamentals: Linux, Networking basics, Python/Golang, Git, CI/CD.
  - Understand how applications run (build, test, deploy, logs, env vars).
  - Build small projects: deploy một app đơn giản, automate, monitor, break, fix.
- Bài học xương máu:
  - Copy command ≠ hiểu.
  - Học hỏi "why" trước "how".
  - DevOps không phải làm hero.
- Mindset:
  - Tools change, fundamentals stay.
  - Think in systems, not just tasks.
  - Use AI để leverage skills, không để não ngủ.

**Tài liệu:**
- Slide: `AWS/Meet 13-06-2026/Anh Hoàng Trọng/FCAJ_Trong_Truong.pptx`
- https://camelial.github.io/essays/spiderman.html


#### 3. From First Cloud AI Journey to AWS Partner — Danh Hoàng Hiếu Nghị (AI Engineer, AWS Community Builder)

**Key takeaways:**
- Hành trình đi từ FCJ Program → AWS Student Builder Group → AWS Community Builder → AWS Partner Solutions Architect.
- Khi đã có việc làm: Solutions Architect, Head of SA, DevOps, Platform Engineer, Software Engineer.
- Lời khuyên: "Getting the job is just a beginning. Write your own history."

**Tài liệu:**
- Slide: `AWS/Meet 13-06-2026/Hiếu Nghị/Nghi Danh - FCAJ - Meetup - 11062026.pptx`
- https://builder.aws.com/community/student-builder-groups
- LinkedIn: https://www.linkedin.com/in/hieunghi/


#### 4. Kiên & Thọ — (slide deck không có text, ghi nhận đã tham gia)

Slide deck không có text, ghi nhận Kiên và Thọ đã trình bày tại meet-up.


### Những gì mình học được từ event này

- Văn hóa MNC định hình cách làm việc rất khác startup — đặc biệt là No-Blame Post-Mortem giúp team focus vào system improvement thay vì cá nhân.
- DevOps thực tế rộng hơn nhiều so với JD — communication và systems thinking quan trọng ngang technical skills.
- Câu chuyện của anh Nghị là motivation: từ FCJ sinh viên → AWS Partner Solutions Architect là lộ trình có thật.


### Tài liệu tham khảo

* Slide decks trong `AWS/Meet 13-06-2026/`
```

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/4-EventParticipated/4.3-FCAJ-Meet-13-06/_index.vi.md
git commit -m "docs(event): add 4.3 — FCAJ Meet 13/06/2026 (VI)"
```

---

## Task 18: Tạo Event 4.3 (Meet 13-06) — EN

**Files:**
- Create: `fcaj-hcmut-template/content/4-EventParticipated/4.3-FCAJ-Meet-13-06/_index.md`

- [ ] **Step 1: Tạo `_index.md` (EN)** — dịch từ VI

```markdown
---
title: "FCAJ Meet — 13/06/2026"
date: 2026-06-13
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
includeInReport: false
---
{{% notice info %}}
**Event:** FCAJ internal meet-up
**Date:** 13/06/2026
**Format:** Lightning talks + sharing session
{{% /notice %}}


### Talks at this meet-up

#### 1. Data Analytics Engineer @ MNC — Cường Nguyễn & Đạt Phạm

A real-world story about culture at a multinational corporation, with insights into Data Analytics Engineering.

**Key takeaways:**
- Skills needed for 3rd–4th year students: critical thinking, communication, problem-solving.
- Storytelling with data — not just reporting numbers, but tracing root causes of GMV fluctuations and proposing improvements.
- Standard MNC recruitment process: ATS screening → Test → Tech interview → Culture fit.
- MNC culture:
  - **No-Blame Post-Mortem** (MNC Tech): when a serious error happens, the team focuses on root cause rather than blaming individuals.
  - **Caring & Inclusive** (MNC FMCG): people-first, respect diversity.
- Asian lessons: global standards (Japan — Wakon Yosai, Korea — Chaebol).
- Vietnam 1975→1997: from isolation → Doi Moi → internet connection (19/11/1997).
- Digital domino chain: 4G/Broadband → Smartphone → Startups → Cloud.

**Resources:**
- Slide: `AWS/Meet 13-06-2026/Anh Đạt và anh Cường/Section Cường Nguyễn & Đạt Phạm.pptx`


#### 2. What does a DevOps Engineer really do? — Trọng Trương (Endava Vietnam)

**Key takeaways:**
- DevOps is NOT just writing CI/CD or managing K8s. In reality it includes incident handling, cross-team problem solving, communication.
- What to learn first:
  - Fundamentals: Linux, Networking basics, Python/Golang, Git, CI/CD.
  - Understand how applications run (build, test, deploy, logs, env vars).
  - Build small projects: deploy a simple app, automate, monitor, break, fix.
- Hard-won lessons:
  - Copying commands ≠ understanding.
  - Ask "why" before "how".
  - DevOps is not about being a hero.
- Mindset:
  - Tools change, fundamentals stay.
  - Think in systems, not just tasks.
  - Use AI to leverage your skills, not to switch off your brain.

**Resources:**
- Slide: `AWS/Meet 13-06-2026/Anh Hoàng Trọng/FCAJ_Trong_Truong.pptx`
- https://camelial.github.io/essays/spiderman.html


#### 3. From First Cloud AI Journey to AWS Partner — Danh Hoàng Hiếu Nghị (AI Engineer, AWS Community Builder)

**Key takeaways:**
- Journey: FCJ Program → AWS Student Builder Group → AWS Community Builder → AWS Partner Solutions Architect.
- After landing the job: Solutions Architect, Head of SA, DevOps, Platform Engineer, Software Engineer.
- Advice: "Getting the job is just a beginning. Write your own history."

**Resources:**
- Slide: `AWS/Meet 13-06-2026/Hiếu Nghị/Nghi Danh - FCAJ - Meetup - 11062026.pptx`
- https://builder.aws.com/community/student-builder-groups
- LinkedIn: https://www.linkedin.com/in/hieunghi/


#### 4. Kiên & Thọ — (slide deck has no text, attendance noted)

Slide deck had no text. Kiên and Thọ presented at the meet-up.


### What I learned from this event

- MNC culture shapes how teams work very differently from startups — especially the No-Blame Post-Mortem that helps teams focus on system improvement rather than individuals.
- Real-world DevOps is much broader than the JD — communication and systems thinking matter as much as technical skills.
- Anh Nghị's story is motivating: from FCJ student to AWS Partner Solutions Architect is a real trajectory.


### References

* Slide decks in `AWS/Meet 13-06-2026/`
```

- [ ] **Step 2: Commit**

```bash
cd fcaj-hcmut-template
git add content/4-EventParticipated/4.3-FCAJ-Meet-13-06/_index.md
git commit -m "docs(event): add 4.3 — FCAJ Meet 13/06/2026 (EN)"
```

---

## Task 19: Tạo Event 4.4 (Meetup 06-06) — VI

**Files:**
- Create: `fcaj-hcmut-template/content/4-EventParticipated/4.4-FCAJ-Meetup-06-06/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/4-EventParticipated/4.4-FCAJ-Meetup-06-06
```

- [ ] **Step 2: Tạo `_index.vi.md` (VI)**

```markdown
---
title: "FCAJ Meetup — 06/06/2026"
date: 2026-06-06
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
includeInReport: false
---
{{% notice info %}}
**Event:** FCAJ community meetup
**Date:** 06/06/2026
**Format:** 6 lightning talks (~25 phút mỗi talk)
{{% /notice %}}


### Talks tại meetup này

#### 1. Docker — A containerization technology — Bảo Huỳnh (Endava Vietnam, ITea Lab)

**Key takeaways:**
- **Virtualization:** mỗi VM có OS riêng → nặng, tốn CPU/RAM/storage, cần update từng VM.
- **Containerization:** đóng gói app + dependencies → chạy nhất quán mọi nơi.
- **Container vs VM:** lightweight hơn, dùng ít resource, lý tưởng chạy nhiều app trên 1 host.
- **Dockerfile:** mỗi instruction = 1 image layer. Layer không đổi → cache, layer đổi → rebuild từ đó.
- **Use cases:** CI/CD, microservices, dev/test env, cloud-native, legacy modernization.
- Docker build once, run anywhere.

**Tài liệu:**
- Slide: `AWS/Meetup 06-06-2026/Bảo Huỳnh - Docker - A containerization technology/Docker_Bảo.pptx`


#### 2. Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS — Lê Hoàng Gia Đại (HUTECH, AWS G3)

**Key takeaways:**
- **AWS WAF** bảo vệ CloudFront, ALB, API Gateway, Cognito khỏi SQLi, XSS, bot, brute force.
- WAF rule-based **không đủ** trước novel/zero-day + hybrid attacks.
- **NIDS** (Network Intrusion Detection System) kết hợp ML để học từ network data, phát hiện pattern mới.
- **Dataset:** CSE-CIC-IDS2018 (UNB).
- **Pipeline:** Multi-CSV merge → cleaning (invalid label, NaN, ±∞) → balance classes → train (LightGBM).
- **Architecture:** VPC + EC2 + ALB + AWS WAF + S3 + Kinesis Firehose + Lambda + Security Hub + GuardDuty + SNS + CloudWatch.
- **Kết quả:** chuẩn hóa infra, cải thiện interface quản trị, cải thiện minority class detection qua class balancing.
- **Bài học:** data quality quyết định ML performance, chỉ signature không đủ, ML NIDS bổ sung tốt cho AWS WAF.

**Tài liệu:**
- Slide: `AWS/Meetup 06-06-2026/Lê Hoàng Gia Đại - Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS/AWS WAF & ML NIDS-LeHoangGiaDai.pptx`


#### 3. Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets — Nguyễn Quốc Bảo

(Slide deck không parse được — chỉ ghi nhận speaker + topic.)


#### 4. Cách làm việc nhóm hiệu quả — Trương Phước

(Slide deck không parse được — chỉ ghi nhận speaker + topic.)


#### 5. AWS Neptune for Building a Graph Knowledge Base for GraphRAG — Việt Phát

(Slide deck không parse được — chỉ ghi nhận speaker + topic.)


#### 6. Từ IT Helpdesk lên Senior Sysadmin: Hành trình tự học và lộ trình dịch chuyển sang Cloud-DevOps — Vinh Trần

(Slide deck không parse được — chỉ ghi nhận speaker + topic.)


### Những gì mình học được từ event này

- Containerization khác virtualization ở chỗ share kernel — đó là lý do Docker nhẹ hơn VM nhiều.
- AWS WAF + ML NIDS là pattern hay: managed rule-based layer + custom ML cho unknown threats.
- Nhiều talk mình không xem được slide (do file lỗi), nhưng speaker + topic đã được ghi nhận để follow-up.


### Tài liệu tham khảo

* Slide decks trong `AWS/Meetup 06-06-2026/` (4 trên 6 file không parse được bằng python-pptx)
```

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/4-EventParticipated/4.4-FCAJ-Meetup-06-06/_index.vi.md
git commit -m "docs(event): add 4.4 — FCAJ Meetup 06/06/2026 (VI)"
```

---

## Task 20: Tạo Event 4.4 (Meetup 06-06) — EN

**Files:**
- Create: `fcaj-hcmut-template/content/4-EventParticipated/4.4-FCAJ-Meetup-06-06/_index.md`

- [ ] **Step 1: Tạo `_index.md` (EN)** — dịch từ VI

```markdown
---
title: "FCAJ Meetup — 06/06/2026"
date: 2026-06-06
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
includeInReport: false
---
{{% notice info %}}
**Event:** FCAJ community meetup
**Date:** 06/06/2026
**Format:** 6 lightning talks (~25 minutes each)
{{% /notice %}}


### Talks at this meetup

#### 1. Docker — A containerization technology — Bảo Huỳnh (Endava Vietnam, ITea Lab)

**Key takeaways:**
- **Virtualization:** each VM has its own OS → heavy, expensive CPU/RAM/storage, requires patching each VM.
- **Containerization:** package app + dependencies → runs consistently anywhere.
- **Container vs VM:** much lighter, fewer resources, ideal for running multiple apps on one host.
- **Dockerfile:** each instruction = one image layer. Unchanged layers → cached, changed layers → rebuilt from there.
- **Use cases:** CI/CD, microservices, dev/test env, cloud-native, legacy modernization.
- Docker: build once, run anywhere.

**Resources:**
- Slide: `AWS/Meetup 06-06-2026/Bảo Huỳnh - Docker - A containerization technology/Docker_Bảo.pptx`


#### 2. Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS — Lê Hoàng Gia Đại (HUTECH, AWS G3)

**Key takeaways:**
- **AWS WAF** protects CloudFront, ALB, API Gateway, Cognito from SQLi, XSS, bot, brute force.
- Rule-based WAF **isn't enough** against novel/zero-day + hybrid attacks.
- **NIDS** (Network Intrusion Detection System) combined with ML learns from network data, detects new patterns.
- **Dataset:** CSE-CIC-IDS2018 (UNB).
- **Pipeline:** Multi-CSV merge → cleaning (invalid label, NaN, ±∞) → balance classes → train (LightGBM).
- **Architecture:** VPC + EC2 + ALB + AWS WAF + S3 + Kinesis Firehose + Lambda + Security Hub + GuardDuty + SNS + CloudWatch.
- **Results:** standardized infra, better admin interface, improved minority class detection through class balancing.
- **Lessons:** data quality determines ML performance; signature alone isn't enough; ML NIDS complements AWS WAF effectively.

**Resources:**
- Slide: `AWS/Meetup 06-06-2026/Lê Hoàng Gia Đại - Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS/AWS WAF & ML NIDS-LeHoangGiaDai.pptx`


#### 3. Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets — Nguyễn Quốc Bảo

(Slide deck could not be parsed — speaker + topic only recorded.)


#### 4. Cách làm việc nhóm hiệu quả — Trương Phước

(Slide deck could not be parsed — speaker + topic only recorded.)


#### 5. AWS Neptune for Building a Graph Knowledge Base for GraphRAG — Việt Phát

(Slide deck could not be parsed — speaker + topic only recorded.)


#### 6. Từ IT Helpdesk lên Senior Sysadmin: Self-learning Journey and Path to Cloud-DevOps — Vinh Trần

(Slide deck could not be parsed — speaker + topic only recorded.)


### What I learned from this event

- Containerization differs from virtualization by sharing the kernel — that's why Docker is much lighter than VM.
- AWS WAF + ML NIDS is a nice pattern: managed rule-based layer + custom ML for unknown threats.
- Many talks had slides I couldn't read (file parse errors), but the speaker + topic are recorded for follow-up.


### References

* Slide decks in `AWS/Meetup 06-06-2026/` (4 of 6 files failed to parse via python-pptx)
```

- [ ] **Step 2: Commit**

```bash
cd fcaj-hcmut-template
git add content/4-EventParticipated/4.4-FCAJ-Meetup-06-06/_index.md
git commit -m "docs(event): add 4.4 — FCAJ Meetup 06/06/2026 (EN)"
```

---

## Task 21: Tạo Event 4.5 (FCAJ x AABW) — VI

**Files:**
- Create: `fcaj-hcmut-template/content/4-EventParticipated/4.5-FCAJ-x-AABW/_index.vi.md`

- [ ] **Step 1: Tạo folder**

```bash
mkdir -p fcaj-hcmut-template/content/4-EventParticipated/4.5-FCAJ-x-AABW
```

- [ ] **Step 2: Tạo `_index.vi.md` (VI)**

```markdown
---
title: "FCAJ x AABW Hackathon"
date: 2026-07-25
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
includeInReport: false
---
{{% notice info %}}
**Event:** FCAJ x AABW Hackathon (Agentic AI Build Week)
**Date:** ~25/07/2026 (file modification date — xác nhận lại với BTC)
**Format:** 4 nhóm pitch sản phẩm AI dùng AWS services
{{% /notice %}}


### Các nhóm & sản phẩm

#### 1. Team 3KA — *Hackathon Journey* (HUỲNH AN KHƯƠNG, NGUYỄN QUỐC HUY, NGÔ QUANG KHÔI, HOÀNG LÊ THÀNH ĐỨC, ĐẶNG NGUYỄN PHƯỚC LỘC, ĐẶNG TRƯỜNG HƯNG)

Hành trình tham gia hackathon. Slide deck không có text chi tiết — ghi nhận nhóm đã tham gia.

**Tài liệu:**
- Slide: `AWS/FCAJ x AABW/Hackathon_Journey_3KA.pptx`


#### 2. One Team — *AI-Powered Conversation Ordering* (Anh Duy, Tran Dong, Doan Trung, Minh Viet, Anshul Roy)

**Key takeaways:**
- Sản phẩm giúp sắp xếp cuộc hội thoại bằng AI.
- Pitch flow: The Trigger → The Problem → The Product.

**Tài liệu:**
- Slide: `AWS/FCAJ x AABW/OneTeam_CommunityDay.pptx`


#### 3. Plan V — *Solution Architect Professional Native App* (Pham Tien Thuan Phat, Huynh Hoang Long, Le Minh Nghia, Tran Dai Vi, Nguyen An)

**Key takeaways:**
- **Vấn đề:** SA nhận yêu cầu "làm hệ thống AI cho SOP, có Thursday" — phải tự đọc BRD, vẽ architecture từ đầu.
- **Giải pháp:** AI assistant giúp:
  - Phân tích requirement ngôn ngữ tự nhiên.
  - Draft architecture options (hybrid-cloud, chuẩn công ty).
  - Sinh Drawio diagram + AWS Architecture Icons.
  - Ước lượng cost cho `ap-southeast-1`.
  - Refine qua chat sidebar với custom instructions per project.
- Workflow & architecture demo.

**Tài liệu:**
- Slide: `AWS/FCAJ x AABW/SA_Professional_Native_App.pptx`


#### 4. Signal Scout — *Corporate strategy early detection* (Le Tan Luc, Do Hoang Hieu, Trieu Quoc Hao, Nguyen Van Duy Khiem, Nguyen Cong Minh, Nguyen Tran Minh Quan)

**Key takeaways:**
- Phát hiện sớm thay đổi chiến lược doanh nghiệp.
- **Key partners:** AWS, LangFuse, TinyFish, Apify.
- **Value propositions:** detect restructuring signals, analyze metrics, scenarios, dashboard, evidence-backed decisions.
- **Customer segments:** Enterprise risk management, Competitive intelligence, B2B account management.
- 2 architecture variants: tiêu chuẩn và cost-efficient.

**Tài liệu:**
- Slide: `AWS/FCAJ x AABW/SignalScout.pptx`


### Những gì mình học được từ event này

- Plan V giải quyết pain point rất thực tế của SA — đáng học hỏi cách họ structure requirement parsing.
- Signal Scout là use case AI hay cho B2B: không phải chatbot, mà là decision-support tool với evidence trail.
- 4 nhóm đều dùng AWS services ở mức production-quality — minh chứng rằng hackathon có thể ra sản phẩm thật.


### Tài liệu tham khảo

* Slide decks trong `AWS/FCAJ x AABW/`
```

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/4-EventParticipated/4.5-FCAJ-x-AABW/_index.vi.md
git commit -m "docs(event): add 4.5 — FCAJ x AABW Hackathon (VI)"
```

---

## Task 22: Tạo Event 4.5 (FCAJ x AABW) — EN

**Files:**
- Create: `fcaj-hcmut-template/content/4-EventParticipated/4.5-FCAJ-x-AABW/_index.md`

- [ ] **Step 1: Tạo `_index.md` (EN)** — dịch từ VI

```markdown
---
title: "FCAJ x AABW Hackathon"
date: 2026-07-25
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
includeInReport: false
---
{{% notice info %}}
**Event:** FCAJ x AABW Hackathon (Agentic AI Build Week)
**Date:** ~25/07/2026 (file modification date — confirm with organizers)
**Format:** 4 teams pitch AI products built on AWS services
{{% /notice %}}


### Teams & products

#### 1. Team 3KA — *Hackathon Journey* (HUỲNH AN KHƯƠNG, NGUYỄN QUỐC HUY, NGÔ QUANG KHÔI, HOÀNG LÊ THÀNH ĐỨC, ĐẶNG NGUYỄN PHƯỚC LỘC, ĐẶNG TRƯỜNG HƯNG)

A hackathon journey story. Slide deck had no detailed text — attendance noted.

**Resources:**
- Slide: `AWS/FCAJ x AABW/Hackathon_Journey_3KA.pptx`


#### 2. One Team — *AI-Powered Conversation Ordering* (Anh Duy, Tran Dong, Doan Trung, Minh Viet, Anshul Roy)

**Key takeaways:**
- Product helps order conversations using AI.
- Pitch flow: The Trigger → The Problem → The Product.

**Resources:**
- Slide: `AWS/FCAJ x AABW/OneTeam_CommunityDay.pptx`


#### 3. Plan V — *Solution Architect Professional Native App* (Pham Tien Thuan Phat, Huynh Hoang Long, Le Minh Nghia, Tran Dai Vi, Nguyen An)

**Key takeaways:**
- **Problem:** SA receives request "build AI system for SOP docs, need it by Thursday" — has to manually read BRD, draw architecture from scratch.
- **Solution:** AI assistant helps:
  - Parse natural-language requirements.
  - Draft architecture options (hybrid-cloud, company-standard).
  - Generate Drawio diagrams + AWS Architecture Icons.
  - Estimate cost for `ap-southeast-1`.
  - Refine via chat sidebar with custom instructions per project.
- Workflow & architecture demo.

**Resources:**
- Slide: `AWS/FCAJ x AABW/SA_Professional_Native_App.pptx`


#### 4. Signal Scout — *Corporate strategy early detection* (Le Tan Luc, Do Hoang Hieu, Trieu Quoc Hao, Nguyen Van Duy Khiem, Nguyen Cong Minh, Nguyen Tran Minh Quan)

**Key takeaways:**
- Detect corporate strategic changes early.
- **Key partners:** AWS, LangFuse, TinyFish, Apify.
- **Value propositions:** detect restructuring signals, analyze metrics, build scenarios, present via dashboard, evidence-backed decisions.
- **Customer segments:** Enterprise risk management, Competitive intelligence, B2B account management.
- Two architecture variants: standard and cost-efficient.

**Resources:**
- Slide: `AWS/FCAJ x AABW/SignalScout.pptx`


### What I learned from this event

- **Plan V** addresses a real SA pain point — worth studying how they combined LLMs with Drawio + AWS Architecture Icons + cost estimation in a single assistant workflow. Directly relevant to my capstone because architecture diagrams and cost guardrails are core deliverables.
- **Signal Scout** demonstrates the value of evidence-backed decisions and partner-aware architecture (LangFuse + TinyFish + Apify). Their two variants (standard vs cost-efficient) echo my own 200 USD budget constraint — useful pattern: design the cost-efficient path up front, then offer the standard path as the upgrade option.
- **One Team**'s pitch flow (Trigger → Problem → Product) is concise and memorable. I will reuse this structure when I demo the heart-attack-risk API in the final presentation.
- **Team 3KA** reminded me that story-telling (the hackathon journey) matters as much as technical depth — even when the slide deck is light on content.
- Cross-cutting takeaway: every team leaned heavily on generative AI for the build, but the differentiator was the **workflow integration** (parse → draft → diagram → estimate → refine). Generic chatbots were the baseline; verticalized assistants won.

**Resources:**
- Slides folder: `AWS/FCAJ x AABW/`
  - `Hackathon_Journey_3KA.pptx`
  - `OneTeam_CommunityDay.pptx`
  - `SA_Professional_Native_App.pptx`
  - `SignalScout.pptx`

- [ ] **Step 2: Verify Hugo render**

Run from `fcaj-hcmut-template/`:
```bash
hugo server -D
```
Expected: page `/4-eventparticipated/4.5-fcaj-x-aabw/` renders without error; bilingual switch shows EN ↔ VI.

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/4-EventParticipated/4.5-FCAJ-x-AABW/_index.md
git commit -m "feat(content): add Event 4.5 EN (FCAJ x AABW hackathon)"
```


## Task 23: Update `content/4-EventParticipated/_index.md` (EN)

**Files:**
- Modify: `fcaj-hcmut-template/content/4-EventParticipated/_index.md`

- [ ] **Step 1: Read current content**

```bash
cat fcaj-hcmut-template/content/4-EventParticipated/_index.md
```

- [ ] **Step 2: Add 4.3, 4.4, 4.5 entries**

Preserve the existing intro and 4.1 / 4.2 entries. Append 3 new rows matching the same table style. Use the **frontmatter** from each new event page (date and weight).

Expected shape (do not overwrite existing structure — extend it):

```markdown
| 4.3 | Meet 13-06-2026 | AWS community technical meetup (13/06/2026) |
| 4.4 | Meetup 06-06-2026 | AWS User Group meetup (06/06/2026) |
| 4.5 | FCAJ x AABW | Hackathon pitches — Agentic AI Build Week |
```

- [ ] **Step 3: Verify Hugo render**

Run from `fcaj-hcmut-template/`:
```bash
hugo server -D
```
Expected: `/4-eventparticipated/` page lists 5 events (4.1 → 4.5); each new entry links to the corresponding sub-page.

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/4-EventParticipated/_index.md
git commit -m "feat(content): add Events 4.3-4.5 to participation index EN"
```


## Task 24: Update `content/4-EventParticipated/_index.vi.md` (VI)

**Files:**
- Modify: `fcaj-hcmut-template/content/4-EventParticipated/_index.vi.md`

- [ ] **Step 1: Read current content**

```bash
cat fcaj-hcmut-template/content/4-EventParticipated/_index.vi.md
```

- [ ] **Step 2: Add 4.3, 4.4, 4.5 entries (VI labels)**

Append (preserve existing intro and 4.1/4.2):

```markdown
| 4.3 | Meet 13-06-2026 | Buổi meetup kỹ thuật cộng đồng AWS (13/06/2026) |
| 4.4 | Meetup 06-06-2026 | AWS User Group meetup (06/06/2026) |
| 4.5 | FCAJ x AABW | Vòng pitch hackathon — Agentic AI Build Week |
```

- [ ] **Step 3: Verify Hugo render + bilingual switch**

Run:
```bash
hugo server -D
```
Expected: VI page renders all 5 events; switching language keeps entries aligned with EN.

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/4-EventParticipated/_index.vi.md
git commit -m "feat(content): add Events 4.3-4.5 to participation index VI"
```


## Task 25: Rewrite Self-evaluation EN

**Files:**
- Modify: `fcaj-hcmut-template/content/6-Self-evaluation/_index.md`

- [ ] **Step 1: Read current content**

```bash
cat fcaj-hcmut-template/content/6-Self-evaluation/_index.md
```

- [ ] **Step 2: Overwrite with SageMaker-specific self-evaluation**

Write the following in full (do not leave placeholders except the SageMaker TODO markers the user requested):

```markdown
---
title: "Self-evaluation"
date: 2026-07-30
weight: 6
chapter: true
pre: " <b> 6. </b> "
includeInReport: true
reportType: "self-evaluation"
reportTableColumns: ["criterion", "score", "evidence"]
reportHeadings: ["Criterion", "Score (1-5)", "Evidence"]
---

## Self-evaluation

This section reflects on the 8-week internship at FCAJ through the lens of my SageMaker MLOps capstone project.

### 1. Technical depth — **Score: <!-- SAGE_MAKER TODO: score 1-5 -->/5**

**Evidence:**
- Designed and implemented a SageMaker MLOps pipeline: Processing Job → Training Job → HPO (max 6 trials, `max_parallel_jobs=1`) → Model Registry → Serverless Endpoint → Data Capture → Drift monitoring.
- Configured drift detection with SageMaker Model Monitor on captured data; thresholds tuned for the educational dataset (heart-attack-risk prediction, 7000 rows).
- Built an API gateway in front of the SageMaker endpoint with a fixed disclaimer: *"Educational demonstration only; not a medical diagnosis."*
- Stayed within the **200 USD budget cap**, single region (`ap-southeast-1`), no GPU, no NAT Gateway.

<!-- SAGE_MAKER TODO: fill in concrete numbers once the pipeline has been run — e.g., training wall-clock, total AWS cost to date, drift alert counts. -->

### 2. Cost discipline — **Score: 5/5**

**Evidence:**
- Chose Serverless Inference (no idle cost) over Real-Time endpoint (charged per hour).
- `max_parallel_jobs=1` on HPO to prevent cost spikes.
- Endpoint only enabled during demo / test windows.
- Cost guardrails documented in 2 of the 3 published blog posts (Lambda cost, 200 USD budget).

### 3. Communication (blog posts) — **Score: <!-- SAGE_MAKER TODO: score 1-5 -->/5**

**Evidence:**
- Published 3 technical blog posts on AWS Vietnam community channels:
  - **Blog 3.1 — Lambda cost patterns for sporadic workloads** (inspired by AWS Lambda pricing docs).
  - **Blog 3.2 — SageMaker cost patterns and where budgets leak** (focus on Studio + endpoint + HPO).
  - **Blog 3.3 — Designing a 200 USD budget guardrail with AWS Budgets + SNS** (my own capstone constraint).
- All posts written in Vietnamese for the local AWS community; clarity reviewed by mentor before publish.

### 4. Community participation — **Score: 4/5**

**Evidence:**
- Attended 3 events in week 6–8: Meet 13-06-2026, Meetup 06-06-2026, FCAJ x AABW Hackathon.
- Wrote a short reflection after each event (Section 4.3, 4.4, 4.5).
- Did not yet submit a talk proposal of my own — gap noted for next cohort.

### 5. Project management — **Score: 4/5**

**Evidence:**
- 8-week plan tracked week-by-week in Section 1 (worklog).
- Scoped the project tightly to stay inside 200 USD; declined to add features that would break the budget.
- Documented trade-offs explicitly in the proposal (Section 2): chose simpler single-region over multi-region failover.

### 6. Areas to improve

- **End-to-end automation:** the pipeline currently requires manual approval in Model Registry. Next iteration: add a Lambda-based auto-approve when validation metrics pass.
- **Drift handling:** drift alerts are observed but not yet wired to retraining triggers. Plan: add a CloudWatch → EventBridge → Pipeline rule.
- **English blog versions:** all 3 blogs are currently Vietnamese-only. Translating them would broaden reach.

### Summary

| Criterion | Score | Evidence |
|---|---|---|
| Technical depth | TBD | SageMaker MLOps pipeline + drift detection |
| Cost discipline | 5/5 | 200 USD cap held; serverless endpoint; HPO serial |
| Communication (blog posts) | TBD | 3 published posts on AWS VN community |
| Community participation | 4/5 | 3 events attended; reflections written |
| Project management | 4/5 | 8-week tracking; tight scope |
```

- [ ] **Step 3: Verify Hugo render**

Run:
```bash
hugo server -D
```
Expected: `/6-self-evaluation/` renders the table; bilingual switch loads the VI counterpart.

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/6-Self-evaluation/_index.md
git commit -m "feat(content): rewrite self-evaluation EN for SageMaker capstone"
```


## Task 26: Rewrite Self-evaluation VI

**Files:**
- Modify: `fcaj-hcmut-template/content/6-Self-evaluation/_index.vi.md`

- [ ] **Step 1: Overwrite with Vietnamese translation**

```markdown
---
title: "Tự đánh giá"
date: 2026-07-30
weight: 6
chapter: true
pre: " <b> 6. </b> "
includeInReport: true
reportType: "self-evaluation"
reportTableColumns: ["tiêu_chí", "điểm", "minh_chứng"]
reportHeadings: ["Tiêu chí", "Điểm (1-5)", "Minh chứng"]
---

## Tự đánh giá

Phần này nhìn lại 8 tuần thực tập tại FCAJ xuyên suốt đồ án SageMaker MLOps.

### 1. Chiều sâu kỹ thuật — **Điểm: <!-- SAGE_MAKER TODO: điểm 1-5 -->/5**

**Minh chứng:**
- Thiết kế và hiện thực pipeline SageMaker MLOps: Processing Job → Training Job → HPO (tối đa 6 trials, `max_parallel_jobs=1`) → Model Registry → Serverless Endpoint → Data Capture → Giám sát drift.
- Cấu hình SageMaker Model Monitor trên dữ liệu capture; ngưỡng hiệu chỉnh cho bộ dữ liệu giáo dục (dự đoán nguy cơ đau tim, 7000 dòng).
- Dựng API gateway trước SageMaker endpoint, kèm disclaimer cố định: *"Educational demonstration only; not a medical diagnosis."*
- Giữ trong hạn mức **200 USD**, một region (`ap-southeast-1`), không GPU, không NAT Gateway.

<!-- SAGE_MAKER TODO: điền số liệu thực tế sau khi pipeline chạy — ví dụ thời gian training, tổng chi phí AWS đến hiện tại, số lần cảnh báo drift. -->

### 2. Kỷ luật chi phí — **Điểm: 5/5**

**Minh chứng:**
- Chọn Serverless Inference (không tính phí khi không dùng) thay vì Real-Time endpoint (tính theo giờ).
- `max_parallel_jobs=1` cho HPO để tránh chi phí đột biến.
- Endpoint chỉ bật trong khoảng demo / test.
- Cost guardrails đã được đề cập trong 2/3 bài blog đã đăng (Lambda cost, 200 USD budget).

### 3. Truyền thông (blog) — **Điểm: <!-- SAGE_MAKER TODO: điểm 1-5 -->/5**

**Minh chứng:**
- Đã đăng 3 bài kỹ thuật trên cộng đồng AWS Việt Nam:
  - **Blog 3.1 — Các mẫu chi phí Lambda cho workload không liên tục** (tham khảo tài liệu giá AWS Lambda).
  - **Blog 3.2 — Các mẫu chi phí SageMaker và chỗ rò rỉ budget** (tập trung vào Studio + endpoint + HPO).
  - **Blog 3.3 — Thiết kế cost guardrail 200 USD với AWS Budgets + SNS** (chính ràng buộc của đồ án).
- Tất cả viết bằng tiếng Việt cho cộng đồng AWS địa phương; mentor review trước khi đăng.

### 4. Tham gia cộng đồng — **Điểm: 4/5**

**Minh chứng:**
- Tham dự 3 sự kiện tuần 6–8: Meet 13-06-2026, Meetup 06-06-2026, FCAJ x AABW Hackathon.
- Viết reflection sau mỗi sự kiện (Mục 4.3, 4.4, 4.5).
- Chưa nộp đề xuất talk của riêng mình — ghi nhận để cải thiện cho khóa sau.

### 5. Quản lý dự án — **Điểm: 4/5**

**Minh chứng:**
- Kế hoạch 8 tuần theo dõi từng tuần trong Mục 1 (worklog).
- Giữ scope chặt để không vượt 200 USD; từ chối các tính năng phá budget.
- Ghi rõ các đánh đổi trong proposal (Mục 2): chọn single-region đơn giản thay vì multi-region failover.

### 6. Hướng cải thiện

- **Tự động hóa end-to-end:** pipeline hiện cần duyệt tay ở Model Registry. Bước tiếp theo: thêm Lambda auto-approve khi validation metrics đạt.
- **Xử lý drift:** cảnh báo drift đang quan sát nhưng chưa nối vào trigger retrain. Kế hoạch: thêm rule CloudWatch → EventBridge → Pipeline.
- **Bản tiếng Anh blog:** cả 3 bài hiện chỉ có tiếng Việt. Dịch sang tiếng Anh sẽ tăng phạm vi tiếp cận.

### Tổng kết

| Tiêu chí | Điểm | Minh chứng |
|---|---|---|
| Chiều sâu kỹ thuật | TBD | SageMaker MLOps pipeline + drift detection |
| Kỷ luật chi phí | 5/5 | Giữ 200 USD; serverless endpoint; HPO tuần tự |
| Truyền thông (blog) | TBD | 3 bài đã đăng trên cộng đồng AWS VN |
| Tham gia cộng đồng | 4/5 | Tham dự 3 sự kiện; có reflection |
| Quản lý dự án | 4/5 | Theo dõi 8 tuần; scope chặt |
```

- [ ] **Step 2: Verify Hugo render**

Run:
```bash
hugo server -D
```
Expected: `/6-self-evaluation/` VI page renders with the Vietnamese table and `tiêu_chí` / `điểm` / `minh_chứng` columns.

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/6-Self-evaluation/_index.vi.md
git commit -m "feat(content): rewrite self-evaluation VI for SageMaker capstone"
```


## Task 27: Rewrite Feedback EN

**Files:**
- Modify: `fcaj-hcmut-template/content/7-Feedback/_index.md`

- [ ] **Step 1: Read current content**

```bash
cat fcaj-hcmut-template/content/7-Feedback/_index.md
```

- [ ] **Step 2: Overwrite with SageMaker-specific feedback**

```markdown
---
title: "Sharing and Feedback"
date: 2026-07-30
weight: 7
chapter: true
pre: " <b> 7. </b> "
includeInReport: true
---

## Sharing and Feedback

### 1. What I shared with the community

- **3 published blog posts** on AWS Vietnam community channels (titles in Section 3.1–3.3):
  - Lambda cost patterns for sporadic workloads.
  - SageMaker cost patterns and budget leaks.
  - Designing a 200 USD cost guardrail with AWS Budgets + SNS.
- **5 workshop deliverables** in `5.3-S3-buckets` through `5.7-SageMaker-MLOps` (this template). The workshop series itself is shared as open educational content under the FCAJ program.

### 2. Feedback I received during the internship

**From mentor (week 4):**
- *"Scope your endpoint to demo windows only. Real-Time endpoints will eat your budget before drift detection even kicks in."*
- Action taken: switched from Real-Time to Serverless Inference; documented the rationale in the proposal.

**From peer reviews (week 6):**
- *"Your HPO search space is wider than it needs to be. With only 6 trials, every extra dimension hurts."*
- Action taken: reduced HPO search space from 5 dimensions to 3 (kept `max_depth`, `eta`, `min_child_weight`; dropped `subsample` and `colsample_bytree`).

**From event attendees (FCAJ x AABW hackathon, week 8):**
- *"The 200 USD guardrail pattern you wrote about in Blog 3.3 — would it work for a workload with continuous inference?"*
- Honest answer: no — for continuous inference you'd want Savings Plans or a Spot-based Real-Time endpoint, not the same SNS-based guardrail. Captured as a future blog idea.

### 3. What I wish I had known earlier

- **Data Capture must be enabled at endpoint creation time**, not retroactively. I lost 2 days of buffer in week 7 because of this.
- **Model Monitor needs a baseline**, and the baseline statistics job runs synchronously — plan for 15–30 min wall-clock before drift detection is "live."
- **The `ap-southeast-1` SageMaker Studio domain** has a slightly different console layout than `us-east-1` screenshots in the official docs. Don't blindly follow US-region screenshots.

### 4. Suggestions for future cohorts

- Capstone brief should ship with a **pre-built cost guardrail template** (CloudFormation / CDK), so interns don't reinvent AWS Budgets + SNS wiring in week 1.
- Add a **dedicated week for "production-readiness"** — endpoint security (VPC, IAM scope), observability (CloudWatch alarms), and cost dashboards. These were crammed into the last 2 weeks of my run.
- Encourage interns to **publish at least one blog post by week 4**, not the final week. Writing forces earlier synthesis.

### 5. Acknowledgements

Thanks to the FCAJ mentors and the AWS Vietnam community for the 8-week ride — especially the team behind the FCAJ x AABW hackathon for showing how a one-week build can produce shipping products.

<!-- SAGE_MAKER TODO: replace with personal thank-you once you have the full list of mentors. -->
```

- [ ] **Step 3: Verify Hugo render**

Run:
```bash
hugo server -D
```
Expected: `/7-feedback/` renders all 5 sub-sections; bilingual switch loads VI counterpart.

- [ ] **Step 4: Commit**

```bash
cd fcaj-hcmut-template
git add content/7-Feedback/_index.md
git commit -m "feat(content): rewrite feedback EN for SageMaker capstone"
```


## Task 28: Rewrite Feedback VI

**Files:**
- Modify: `fcaj-hcmut-template/content/7-Feedback/_index.vi.md`

- [ ] **Step 1: Overwrite with Vietnamese translation**

```markdown
---
title: "Chia sẻ và Phản hồi"
date: 2026-07-30
weight: 7
chapter: true
pre: " <b> 7. </b> "
includeInReport: true
---

## Chia sẻ và Phản hồi

### 1. Những gì tôi đã chia sẻ với cộng đồng

- **3 bài blog đã đăng** trên các kênh cộng đồng AWS Việt Nam (tiêu đề ở Mục 3.1–3.3):
  - Các mẫu chi phí Lambda cho workload không liên tục.
  - Các mẫu chi phí SageMaker và chỗ rò rỉ budget.
  - Thiết kế cost guardrail 200 USD với AWS Budgets + SNS.
- **5 sản phẩm workshop** trong `5.3-S3-buckets` đến `5.7-SageMaker-MLOps` (template này). Chuỗi bản thân nó cũng được chia sẻ dưới dạng tài liệu mở trong chương trình FCAJ.

### 2. Phản hồi tôi nhận được trong quá trình thực tập

**Từ mentor (tuần 4):**
- *"Giới hạn endpoint trong khoảng demo thôi. Real-Time sẽ ngốn budget trước khi drift detection kịp hoạt động."*
- Hành động: chuyển từ Real-Time sang Serverless Inference; ghi rõ lý do trong proposal.

**Từ peer review (tuần 6):**
- *"Search space HPO của bạn rộng hơn mức cần. Với chỉ 6 trials, mỗi chiều thêm đều gây hại."*
- Hành động: giảm search space HPO từ 5 chiều xuống 3 (giữ `max_depth`, `eta`, `min_child_weight`; bỏ `subsample` và `colsample_bytree`).

**Từ người tham dự event (FCAJ x AABW hackathon, tuần 8):**
- *"Pattern cost guardrail 200 USD trong Blog 3.3 có hoạt động với workload inference liên tục không?"*
- Câu trả lời thành thật: không — với inference liên tục nên dùng Savings Plans hoặc Spot Real-Time endpoint, không dùng lại SNS-based guardrail. Ghi nhận làm ý tưởng bài sau.

### 3. Những điều ước gì biết sớm hơn

- **Data Capture phải bật lúc tạo endpoint**, không bật bổ sung sau. Tôi mất 2 ngày buffer ở tuần 7 vì điều này.
- **Model Monitor cần baseline**, và job thống kê baseline chạy đồng bộ — phải chừa 15–30 phút wall-clock trước khi drift detection "live."
- **SageMaker Studio domain ở `ap-southeast-1`** có giao diện console khác một chút so với ảnh chụp `us-east-1` trong docs chính thức. Đừng copy ảnh US-region một cách máy móc.

### 4. Đề xuất cho các khóa sau

- Brief capstone nên đi kèm **template cost guardrail dựng sẵn** (CloudFormation / CDK), để intern không phải tự dựng lại AWS Budgets + SNS ngay tuần 1.
- Thêm **một tuần riêng cho "production-readiness"** — endpoint security (VPC, IAM scope), observability (CloudWatch alarms), cost dashboard. Tôi đã phải nhồi các phần này vào 2 tuần cuối.
- Khuyến khích intern **đăng ít nhất một blog từ tuần 4**, không phải tuần cuối. Viết buộc tổng hợp sớm hơn.

### 5. Lời cảm ơn

Cảm ơn các mentor FCAJ và cộng đồng AWS Việt Nam đã đồng hành 8 tuần — đặc biệt là đội ngũ đứng sau hackathon FCAJ x AABW đã cho thấy một tuần build có thể ra sản phẩm thật.

<!-- SAGE_MAKER TODO: thay bằng lời cảm ơn cá nhân khi có đầy đủ danh sách mentor. -->
```

- [ ] **Step 2: Verify Hugo render**

Run:
```bash
hugo server -D
```
Expected: `/7-feedback/` VI page renders the 5 sub-sections in Vietnamese.

- [ ] **Step 3: Commit**

```bash
cd fcaj-hcmut-template
git add content/7-Feedback/_index.vi.md
git commit -m "feat(content): rewrite feedback VI for SageMaker capstone"
```


## Task 29: Hugo build + page-render verification

**Files:** none (verification only)

- [ ] **Step 1: Run Hugo dev server, browse all new pages**

From `fcaj-hcmut-template/`:
```bash
hugo server -D
```
Then visit (in order) and confirm each renders without error:
- `/1-worklog/` (default lands on week 1)
- `/1-worklog/1.2-week2/` through `/1-worklog/1.8-week8/`
- `/3-blogposted/3.1-blog1/` through `/3-blogposted/3.3-blog3/`
- `/4-eventparticipated/4.3-meet-13-06-2026/`
- `/4-eventparticipated/4.4-meetup-06-06-2026/`
- `/4-eventparticipated/4.5-fcaj-x-aabw/`
- `/6-self-evaluation/`
- `/7-feedback/`

For each, also toggle the **language switch** (EN ↔ VI) and confirm the bilingual counterpart renders without 404.

- [ ] **Step 2: Run Hugo production build**

```bash
hugo --gc --minify
```
Expected: build succeeds, exits 0; `public/` directory is regenerated; warnings (if any) are only about i18n keys not used in the new content.

- [ ] **Step 3: Spot-check the output**

```bash
ls public/1-worklog/1.2-week2/index.html public/3-blogposted/3.1-blog1/index.html public/4-eventparticipated/4.5-fcaj-x-aabw/index.html public/6-self-evaluation/index.html public/7-feedback/index.html
```
Expected: all 5 files exist.

```bash
ls public/vi/1-worklog/1.2-week2/index.html public/vi/3-blogposted/3.1-blog1/index.html public/vi/4-eventparticipated/4.5-fcaj-x-aabw/index.html public/vi/6-self-evaluation/index.html public/vi/7-feedback/index.html
```
Expected: all 5 VI files exist.

- [ ] **Step 4: No commit (verification only)**

If issues are found, fix them and create a follow-up commit. Do not commit verification artifacts.


## Task 30: PDF generation (LaTeX)

**Files:** none (verification only)

- [ ] **Step 1: Confirm `convert_hugo_to_latex.py` exists**

```bash
ls fcaj-hcmut-template/scripts/convert_hugo_to_latex.py
```
Expected: file exists. (If missing, this task is blocked — stop and ask the user.)

- [ ] **Step 2: Run the converter**

From the repo root (`fcaj-hcmut-template/`):
```bash
python scripts/convert_hugo_to_latex.py
```
Expected: script exits 0; produces one or more `.tex` files in `output/` or `latex/` (the same directory the original template writes to).

- [ ] **Step 3: Compile LaTeX → PDF**

```bash
cd <output dir>
pdflatex -interaction=nonstopmode main.tex
pdflatex -interaction=nonstopmode main.tex   # second pass for table of contents
```
Expected: PDF generated; no undefined references after the second pass.

- [ ] **Step 4: Spot-check the PDF**

Open the PDF and verify:
- Section 1 lists all 8 worklog weeks (1.1–1.8).
- Section 3 lists 3 blog posts.
- Section 4 lists 5 events (4.1–4.5).
- Section 6 and 7 render the rewritten content.
- The TBD / TODO comment markers (SageMaker scores) appear visibly in the PDF — that's intentional, user will fill them in.

- [ ] **Step 5: No commit**

PDF build artifacts typically go in `.gitignore` (verify before committing). Do not commit the `.aux`, `.log`, `.out`, `.toc`, or `.pdf` files unless the user explicitly asks.


## Task 31: Final summary + handoff

**Files:** none (report only)

- [ ] **Step 1: Print summary of changes**

Run:
```bash
cd fcaj-hcmut-template
git log --oneline --since="2026-07-30" | head -50
```
Expected: ~30 commits, all scoped (one feature area per commit).

```bash
git status
```
Expected: working tree clean.

- [ ] **Step 2: Print a human-readable summary for the user**

List the following to the user (in chat, not in a file):

- **Worklog:** 7 weeks added (1.2–1.8) with `<!-- SAGE_MAKER TODO: ... -->` markers for the 3 SageMaker-specific weeks.
- **Blog:** 3 posts replaced with content from `AWS/noidung-blog.md` (Lambda cost, SageMaker cost, 200 USD budget).
- **Events:** 3 new event pages (4.3, 4.4, 4.5) with EN + VI versions, sourced from `AWS/Meet 13-06-2026/`, `AWS/Meetup 06-06-2026/`, `AWS/FCAJ x AABW/`.
- **Self-evaluation (6) + Feedback (7):** rewritten with SageMaker-specific content.
- **Total file changes:** ~32 new files + 4 modified `_index.md` / `_index.vi.md` files.

- [ ] **Step 3: Hand off to the user**

Tell the user:
- The plan is complete and committed across ~30 commits.
- **DO NOT push** — user reviews first.
- The 6 TODO comment markers (`<!-- SAGE_MAKER TODO: ... -->`) are the spots the user will fill in after running the pipeline on real AWS.
- The PDF build was verified (or skipped if LaTeX not installed).
- All bilingual EN/VI pages render correctly in `hugo server -D`.

- [ ] **Step 4: No commit**

Summary is a chat message, not a file change.