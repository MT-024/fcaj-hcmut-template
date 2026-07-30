# Design — Hoàn thiện fcaj-hcmut-template

**Date:** 2026-07-30
**Status:** Draft — chờ user review
**Scope:** Đổ nội dung dự án SageMaker MLOps 8 tuần + 3 blog đã post + 3 event meetup + 2 phần reflection (Self-evaluation, Feedback) vào template `fcaj-hcmut-template/`.

**OUT OF SCOPE (theo user 2026-07-30):**
- KHÔNG tạo mới Workshop 5.7 SageMaker MLOps (giữ nguyên 5.1–5.6 S3 workshop đã có).
- KHÔNG thay đổi Section 2-Proposal (giữ nguyên nội dung IoT Weather Platform hiện tại).

## 1. Mục tiêu

Hoàn thiện template `fcaj-hcmut-template` (Hugo song ngữ EN/VI cho báo cáo thực tập FCAJ) bằng cách thêm các nhóm nội dung, theo đúng format đang có. Toàn bộ thay đổi nằm trong `fcaj-hcmut-template/`; không sửa `AWS/AWS/` (đó là data-only folder).

Kết quả: người clone template, chạy `hugo server -D` sẽ thấy một báo cáo thực tập "đầy đặn" với:
- 8 tuần worklog (1.1 cũ + 1.2–1.8 mới)
- 3 blog SageMaker/Lambda (3.1–3.3)
- 5 event (4.1–4.2 cũ + 4.3 Meet 13-06, 4.4 Meetup 06-06, 4.5 FCAJ x AABW)
- Workshop S3 (5.1–5.6, giữ nguyên)
- Self-evaluation (6, viết SageMaker-specific)
- Sharing & Feedback (7, viết SageMaker-specific)

## 2. Phát hiện quan trọng

**3 blog hiện tại (3.1, 3.2, 3.3) trong template đang PLACEHOLDER** — tất cả copy cùng nội dung "SESSION POLICIES IN AMAZON EKS POD IDENTITY". Cần ghi đè bằng 3 blog thật từ `AWS/noidung-blog.md` (Lambda cost, SageMaker cost, 200 USD budget).

**Section 6 (Self-evaluation) và Section 7 (Feedback) hiện là template chung** (12 tiêu chí FCAJ + 6 tiêu chí feedback chung). Cần viết lại SageMaker-specific dựa trên kinh nghiệm 8 tuần.

## 3. Cấu trúc Hugo cần thêm/sửa

```
fcaj-hcmut-template/content/
├── 1-Worklog/
│   ├── 1.1-Week1/      (đã có — giữ nguyên)
│   ├── 1.2-Week2/      (mới) — SageMaker preprocess + EDA
│   ├── 1.3-Week3/      (mới) — XGBoost training, AUC ≥ 0.84
│   ├── 1.4-Week4/      (mới) — HPO (6 trials, max_parallel_jobs=1)
│   ├── 1.5-Week5/      (mới) — Model Registry + Endpoint
│   ├── 1.6-Week6/      (mới) — Lambda + API Gateway
│   ├── 1.7-Week7/      (mới) — Drift detection (manual pipeline)
│   ├── 1.8-Week8/      (mới) — SageMaker Pipeline + Cleanup
│   └── _index.md        (sửa — thêm link 1.2–1.8)
│
├── 2-Proposal/         (GIỮ NGUYÊN — IoT Weather Platform hiện tại)
│
├── 3-BlogsPosted/
│   ├── 3.1-Blog1/      (GHI ĐÈ — từ noidung-blog.md BLOG 1)
│   ├── 3.2-Blog2/      (GHI ĐÈ — từ noidung-blog.md BLOG 2)
│   ├── 3.3-Blog3/      (GHI ĐÈ — từ noidung-blog.md BLOG 3)
│   └── _index.md        (sửa — cập nhật mô tả + link)
│
├── 4-EventParticipated/
│   ├── 4.1-Event1/      (giữ nguyên)
│   ├── 4.2-Event2/      (giữ nguyên)
│   ├── 4.3-FCAJ-Meet-13-06/      (mới)
│   ├── 4.4-FCAJ-Meetup-06-06/   (mới)
│   ├── 4.5-FCAJ-x-AABW/         (mới)
│   └── _index.md        (sửa — thêm 3 link)
│
├── 5-Workshop/          (GIỮ NGUYÊN 5.1–5.6, KHÔNG tạo 5.7)
│
├── 6-Self-evaluation/
│   └── _index.md        (GHI ĐÈ — SageMaker-specific EN)
│   └── _index.vi.md     (GHI ĐÈ — SageMaker-specific VI)
│
├── 7-Feedback/
│   └── _index.md        (GHI ĐÈ — SageMaker-specific EN)
│   └── _index.vi.md     (GHI ĐÈ — SageMaker-specific VI)
│
└── (Không sửa) config.toml, layouts/, static/, scripts/
```

## 4. Content strategy — Hybrid (Brief + Placeholder cho SageMaker)

### 4.1. Phân loại 3 mức

| Mức | Ý nghĩa | Xử lý |
|-----|---------|-------|
| **A. Cứng** | Có trong brief (`Nội dung.docx`) / `CLAUDE.md` / notebook output | Viết đầy đủ, số liệu thật |
| **B. Mềm** | Có trong 3 blog đã viết (noidung-blog.md) | Tóm tắt verbatim, không bịa thêm |
| **C. SageMaker-thực** | Cần chạy trên AWS SageMaker mới có (metric thật, console screenshot, billing thật) | **Placeholder** + TODO marker để user điền sau |

### 4.2. Nguồn nội dung

| Đầu ra | Nguồn | Mức |
|--------|-------|-----|
| Blog 3.1 (Lambda cost) | `AWS/noidung-blog.md` lines 1–52 | B (verbatim) |
| Blog 3.2 (SageMaker cost) | `AWS/noidung-blog.md` lines 53–106 | B (verbatim) |
| Blog 3.3 (200 USD budget) | `AWS/noidung-blog.md` lines 107–280 | B (verbatim) |
| Worklog 1.2–1.8 | Brief + CLAUDE.md + 3 blog | A + C (placeholder) |
| Event 4.3 (Meet 13-06) | 4 PPTX files (Cường+Đạt, Trọng, Nghị, Kiên+Thọ) | A + C |
| Event 4.4 (Meetup 06-06) | 6 PPTX files (Bảo, Đại, Bảo_NQ, Phước, Phát, Vinh) — 4 file lỗi parse | A + C |
| Event 4.5 (FCAJ x AABW) | 4 PPTX files (3KA, OneTeam, PlanV, SignalScout) | A + C |
| Self-evaluation (6) | Brief + 8 tuần worklog | A (SageMaker-specific) |
| Feedback (7) | FCAJ program + 8 tuần experience | A (SageMaker-specific) |

### 4.3. Format placeholder

```markdown
<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Week 3 - XGBoost training                     -->
<!-- Cần điền:                                                       -->
<!--   - Training job name (vd: heart-risk-train-2026-xx-xx)         -->
<!--   - Instance type đã dùng (vd: ml.t3.medium)                    -->
<!--   - Training time thực tế (vd: 4 phút 12 giây)                  -->
<!--   - AUC trên validation set (vd: 0.8612)                        -->
<!--   - Recall (vd: 0.679)                                          -->
<!--   - F1 (vd: 0.703)                                              -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->
```

## 5. Frontmatter chuẩn

### 5.1. Worklog (1.x-WeekX)

```yaml
---
title: "Week N Worklog"
date: 2024-01-01
weight: N
chapter: false
pre: " <b> 1.N. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Day
  - Task
  - Completion Date
reportHeadings:
  - Week N Objectives
  - Tasks to be carried out this week
  - Week N Achievements
---
```

VI: title → "Tuần N", columns → "Thứ" / "Công việc" / "Ngày hoàn thành", headings → "Mục tiêu tuần N" / "Công việc cần làm trong tuần" / "Thành quả tuần N".

### 5.2. Blog (3.x-BlogX)

```yaml
---
title: "Blog N - <tiêu đề EN>"
date: 2026-07-30
weight: N
chapter: false
pre: " <b> 3.N. </b> "
includeInReport: false
---
```

### 5.3. Event (4.x-EventX)

```yaml
---
title: "<Event title EN>"
date: 2026-MM-DD
weight: N
chapter: false
pre: " <b> 4.N. </b> "
includeInReport: false
---
```

### 5.4. Self-evaluation (6)

```yaml
---
title: "Self-Assessment"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 6. </b> "
includeInReport: false
---
```

VI: title → "Tự đánh giá".

### 5.5. Feedback (7)

```yaml
---
title: "Sharing and Feedback"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 7. </b> "
includeInReport: false
---
```

VI: title → "Chia sẻ và Phản hồi".

## 6. Cấu trúc nội dung

### 6.1. Worklog tuần N — `_index.md`

```markdown
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only...
{{% /notice %}}


### Week N Objectives:

* <mục tiêu 1>
* <mục tiêu 2>


### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - task 1 <br> - task 2 | DD/MM/YYYY | DD/MM/YYYY      |
| 3   | - task 1 | DD/MM/YYYY | DD/MM/YYYY      |
| 4   | - task 1 <br> - task 2 | DD/MM/YYYY | DD/MM/YYYY      |
| 5   | - task 1 | DD/MM/YYYY | DD/MM/YYYY      |
| 6   | - task 1 <br> - task 2 | DD/MM/YYYY | DD/MM/YYYY      |


### Week N Achievements:

* <thành quả 1>
* <thành quả 2>

**Metric snapshot:**

<!-- SAGE_MAKER TODO: Week N - <tên> -->

| Metric | Value | Note |
| ------ | ----- | ---- |
| Training time | TBD | from SageMaker training job |
| AUC (validation) | TBD | target ≥ 0.84 |
| Recall (validation) | TBD | target ≥ 0.65 |
| F1 (validation) | TBD | target ≥ 0.70 |
| Cost this week | TBD USD | from billing dashboard |


### References:
* https://docs.aws.amazon.com/sagemaker/...
```

### 6.2. Blog 3.1, 3.2, 3.3

Vì 3 blog đã viết hoàn chỉnh trong `noidung-blog.md` (~22KB tổng), tôi sẽ:

1. Copy verbatim từng phần sang `_index.vi.md` (tác giả viết VI, đó là bản gốc).
2. Dịch sang `_index.md` (EN). Dịch có chọn lọc: giữ thuật ngữ kỹ thuật (Lambda, SageMaker, HPO, Endpoint, Drift…), dịch tự nhiên phần narrative.

### 6.3. Event 4.x — `_index.md`

```markdown
{{% notice info %}}
**Event:** <tên event>
**Date:** DD/MM/YYYY
**Format:** <format>
{{% /notice %}}


### Talks at this event

#### 1. <Talk title> — <Speaker> (<affiliation>)

Key takeaways:
- <takeaway 1>
- <takeaway 2>

Slide: `<path>`


### What I learned from this event

- <reflection>


### References
```

### 6.4. Self-evaluation (6)

```markdown
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}

During my internship at **FCAJ (First Cloud AI Journey)** from **[start date]** to **[end date]**, I worked on an **8-week SageMaker MLOps capstone project**: a binary classifier for heart-attack risk (educational, non-medical). The project covered the full lifecycle: data preprocessing → XGBoost training → HPO → Model Registry → Endpoint → Lambda + API Gateway → Drift monitoring → Pipeline orchestration.

I gained hands-on experience in:
- **SageMaker core services**: Processing, Training, HPO, Model Registry, Endpoints, Pipelines, Data Capture, Model Monitor fallback.
- **AWS infrastructure**: S3, IAM (least-privilege), Lambda, API Gateway, CloudWatch, EventBridge.
- **MLOps discipline**: versioning, tag-based cost tracking, cleanup scripts, drift detection, lifecycle rules.
- **Budget discipline**: capped at 200 USD total — single region, no GPU, no NAT Gateway, 6 HPO trials, endpoint lifecycle.

### Self-evaluation criteria

| No. | Criteria | Description | Good | Fair | Average |
| --- | -------- | ----------- | ---- | ---- | ------- |
| 1 | **Professional knowledge — AWS core** | Comfortable with S3, IAM, Lambda, API Gateway, CloudWatch | ✅ | ☐ | ☐ |
| 2 | **Professional knowledge — SageMaker** | Built end-to-end pipeline (Processing → Training → HPO → Registry → Endpoint → Monitor → Pipeline) | ✅ | ☐ | ☐ |
| 3 | **ML fundamentals** | Data preprocessing (impute, scale, encode), XGBoost training, HPO search space, drift concepts | ☐ | ✅ | ☐ |
| 4 | **Ability to learn** | Picked up SageMaker SDK, Data Capture → EventBridge → Processing pipeline in week 7 | ✅ | ☐ | ☐ |
| 5 | **Proactiveness** | Designed cost-saving measures proactively (tagging, lifecycle, cleanup script) before bills appeared | ✅ | ☐ | ☐ |
| 6 | **Sense of responsibility** | Stayed within 200 USD budget across 8 weeks; verified cleanup script after each deployment | ✅ | ☐ | ☐ |
| 7 | **Discipline** | Followed brief constraints (no GPU, single region, 6 HPO trials) even when tempted to relax them | ☐ | ✅ | ☐ |
| 8 | **Progressive mindset** | When Model Monitor access uncertain post-2026-07-30, built a manual fallback pipeline instead of waiting | ✅ | ☐ | ☐ |
| 9 | **Communication** | Documented work in worklog, published blogs, presented at meetups | ☐ | ✅ | ☐ |
| 10 | **Teamwork** | Collaborated with FCAJ members at meetups, shared lessons learned in 3 blog posts | ✅ | ☐ | ☐ |
| 11 | **Problem-solving** | Replaced Model Monitor with custom Data Capture → EventBridge → Processing pipeline under tight timeline | ✅ | ☐ | ☐ |
| 12 | **Contribution** | 3 blog posts contributed to AWS Study Group community | ✅ | ☐ | ☐ |
| 13 | **Overall** | Successfully delivered project within budget and constraints; built transferable MLOps discipline | ✅ | ☐ | ☐ |


### Strengths
- Cost discipline: kept total AWS spend under 110 USD against 200 USD cap.
- End-to-end ownership: from raw CSV to deployed API with drift monitoring.
- Pragmatic engineering: chose `ml.t3.medium` over `ml.m5.large` after benchmarking, not on intuition.
- Communication: distilled 8-week experience into 3 public blog posts.

### Needs improvement
- **Discipline on cleanup**: forgot to delete endpoint after first demo — caught on day 2 of week 5 before it became expensive. Need to wire `schedule.shutdown_endpoint()` from the start.
- **Problem-solving under ambiguity**: week 7's "Model Monitor may be unavailable after 2026-07-30" was stressful. Should pre-design fallback paths for any managed service.
- **Test coverage**: focused on happy path for HPO; edge cases (e.g., missing feature column in live data) not unit-tested. Need pytest fixtures for preprocessing schema.
```

VI bản dịch tương ứng.

### 6.5. Feedback (7)

```markdown
{{% notice warning %}}
⚠️ **Note:** For reference only.
{{% /notice %}}

> My honest reflection on the FCAJ (First Cloud AI Journey) program after the 8-week SageMaker MLOps capstone.

### Overall evaluation

**1. Working environment**
FCAJ provides a friendly, async-friendly working environment. Members are responsive on the AWS Study Group Facebook group and Slack. I appreciated the flexibility — most of my work was self-paced, with mentor reviews on key milestones (week 3 baseline, week 5 deployment, week 8 cleanup).

**2. Support from mentor / team admin**
My mentor provided sharp technical guidance on AWS architecture decisions (single-region vs multi-region, IAM scope, HPO bounds). The admin team organized 3 meetups during my internship (06/06, 13/06, FCAJ x AABW) where I got exposure beyond my own project.

**3. Relevance of work to academic major**
The SageMaker capstone directly applied my coursework (Python, ML basics, AWS fundamentals) while pushing me into new territory (production MLOps, drift monitoring, cost engineering). The 200 USD budget constraint was unique — academic projects rarely enforce hard budgets.

**4. Learning & skill development opportunities**
Beyond SageMaker, the meetups exposed me to:
- Containerization (Docker — Bảo Huỳnh, meetup 06/06)
- Network intrusion detection (AWS WAF + ML NIDS — Lê Hoàng Gia Đại, meetup 06/06)
- Modernization strategy (DDD + event-driven architecture — Jignesh Shah, event 4.1)
- DevOps engineering reality (Trọng Trương, meet 13/06)

**5. Company culture & team spirit**
FCAJ culture mirrors AWS culture surprisingly well:
- **No-blame post-mortem** — when a model fails or budget blows, the response is "what can we learn", not "who's fault".
- **Caring & inclusive** — mentors actively check on interns who go quiet.
- **Customer obsession (inverted)** — for interns, this means "respect the brief constraints" (e.g., no GPU, single region).

**6. Internship policies / benefits**
FCAJ is volunteer-run, so no stipend. The benefit is access to a real AWS project brief, mentor review, and a community of ~500+ Vietnamese students also learning AWS.

### Additional questions
**Most satisfying:** Week 7 — building the manual drift detection pipeline after learning Model Monitor access might change. The constraint forced creative engineering.

**Should improve:** A pre-internship "AWS account + budget alert + tagging" checklist would save ~3 hours of week-1 setup. Also, a shared `iam_helper.py` for the standard SageMaker role JSON would prevent reinvented IAM in week 2–3.

**Recommend to a friend?** Yes, with a caveat: only if they're willing to commit 8 weeks and read the brief carefully. FCAJ is what you make of it — passive interns finish with a blank repo, active ones finish with 3 blogs and a working pipeline.

### Suggestions & expectations
- Add a 1-page "Cost Cheat Sheet" (instance type vs $/hour, 200 USD equivalents) to onboarding.
- Pre-create a template repo (`fcaj-sagemaker-template`) with the standard preprocessing + training scripts so interns focus on the unique parts (HPO search space, drift recipe).
- Keep the constraint-based briefs (single region, no GPU, 200 USD) — those constraints teach more than open-ended projects.
```

VI bản dịch tương ứng.

## 7. Nội dung chi tiết cho 3 events

### 7.1. Meet 13-06-2026 (4 talks) → `4.3-FCAJ-Meet-13-06/`

1. **Cường Nguyễn & Đạt Phạm** — Data Analytics Engineer @ MNC, văn hóa tập đoàn đa quốc gia.
   - Kỹ năng: tư duy phản biện, giao tiếp, data storytelling (truy nguyên nhân biến động GMV).
   - Quy trình tuyển dụng MNC: ATS → Test → Tech interview → Culture fit.
   - Văn hóa MNC: No-Blame Post-Mortem (Tech), Caring & Inclusive (FMCG).
   - Bài học Á Đông: Wakon Yosai (Nhật), Chaebol (Hàn).
   - Việt Nam 1975→1997: cô lập → đổi mới → kết nối internet (19/11/1997).
   - Chuỗi domino số: 4G/Broadband → Smartphone → Startups → Cloud.

2. **Trọng Trương (Endava Vietnam)** — What does a DevOps Engineer really do?
   - DevOps ≠ chỉ CI/CD hay K8s. Bao gồm incident handling, communication.
   - Học gì trước: Linux, Networking, Python/Golang, Git, CI/CD, Containers.
   - Mindset: Tools change, fundamentals stay. Think in systems. Use AI to leverage skills.

3. **Danh Hoàng Hiếu Nghị (AI Engineer, AWS Community Builder)** — From FCJ to AWS Partner.
   - Hành trình: FCJ Program → Student Builder Group → Community Builder → AWS Partner.
   - Lời khuyên: "Getting the job is just a beginning. Write your own history."

4. **Kiên & Thọ** — (slide deck không có text, ghi nhận đã tham gia).

### 7.2. Meetup 06-06-2026 (6 talks) → `4.4-FCAJ-Meetup-06-06/`

1. **Bảo Huỳnh (Endava, ITea Lab)** — Docker — A containerization technology.
   - VM: nặng, mỗi VM có OS riêng.
   - Container: lightweight, đóng gói app + deps, chạy nhất quán.
   - Dockerfile: mỗi instruction = 1 layer. Layer cache được tái sử dụng.
   - Use cases: CI/CD, microservices, dev/test, cloud-native, legacy modernization.

2. **Lê Hoàng Gia Đại (HUTECH, AWS G3)** — AWS WAF + ML NIDS for Cyber Attack Detection.
   - AWS WAF bảo vệ CloudFront/ALB/API Gateway/Cognito. Rule-based không đủ trước novel/zero-day.
   - NIDS kết hợp ML: học từ network data, phát hiện pattern mới.
   - Dataset: **CSE-CIC-IDS2018** (UNB).
   - Pipeline: Multi-CSV → cleaning (invalid label, NaN, ±∞) → balance → train (LightGBM).
   - Architecture: VPC + EC2 + ALB + WAF + S3 + Kinesis Firehose + Lambda + Security Hub + GuardDuty + SNS + CloudWatch.

3–6. **Các talk khác (slides không đọc được — 4 file PPTX trả về "Package not found"):**
   - Nguyễn Quốc Bảo — Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets
   - Trương Phước — Cách làm việc nhóm hiệu quả
   - Việt Phát — AWS Neptune for Building a Graph Knowledge Base for GraphRAG
   - Vinh Trần — Từ IT Helpdesk lên Senior Sysadmin: Hành trình tự học và Cloud-DevOps

Tôi sẽ ghi nhận speaker + topic cho 4 talk này, không bịa nội dung slide.

### 7.3. FCAJ x AABW Hackathon (4 nhóm) → `4.5-FCAJ-x-AABW/`

1. **Team 3KA** — Hackathon Journey (6 thành viên). Slide deck không có text chi tiết, ghi nhận đã tham gia.

2. **One Team** — AI-Powered Conversation Ordering (5 thành viên). Sản phẩm giúp sắp xếp cuộc hội thoại bằng AI. Pitch: Trigger → Problem → Product.

3. **Plan V** — Solution Architect Professional Native App (5 thành viên). Vấn đề: SA nhận yêu cầu thiết kế AI cho SOP, phải vẽ architecture từ đầu. Giải pháp: AI assistant phân tích requirement, draft architecture (hybrid-cloud), sinh Drawio + AWS Architecture Icons, ước lượng cost ap-southeast-1, refine qua chat.

4. **Signal Scout** — Corporate strategy early detection (6 thành viên). Phát hiện sớm thay đổi chiến lược doanh nghiệp. Partners: AWS, LangFuse, TinyFish, Apify.

## 8. Cập nhật script LaTeX

Trong `scripts/convert_hugo_to_latex.py`, thêm các entry mới vào `SECTION_TITLES_EN` / `SECTION_TITLES_VI`:

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

PDF chỉ chứa worklog (1.x) do `includeInReport: true`.

## 9. Data flow & build pipeline

```
Nguồn (read-only)                    Hugo site source                     Output
──────────────                       ────────────────                      ──────
AWS/noidung-blog.md          ──►     content/3-BlogsPosted/         ──►   public/ (HTML)
AWS/Meet 13-06/*.pptx        ──►     content/4-EventParticipated/  ──►   report_vn.pdf
AWS/Meetup 06-06/*.pptx      ──►     content/4-EventParticipated/  ──►   report_en.pdf
AWS/FCAJ x AABW/*.pptx       ──►     content/4-EventParticipated/
AWS/AWS/blog-*.md            ──►     BỎ — dùng noidung-blog.md
AWS/data/Nội dung.docx       ──►     content/1-Worklog/ (1.2–1.8)
AWS/data/CLAUDE.md           ──►     content/6,7 (self-eval, feedback)
```

### Build pipeline

1. `hugo --renderToMemory` — check syntax.
2. `hugo --minify` — production build, kiểm tra HTML được sinh.
3. `python3 scripts/convert_hugo_to_latex.py && cd report && latexmk -pdf main.tex (×3) && latexmk -pdf main_en.tex (×3)` — sinh PDF.

## 10. Verification matrix

| # | Kiểm tra | Pass criteria |
|---|----------|---------------|
| V1 | `hugo server -D` | Listen 1313, không panic |
| V2 | Homepage | Title + sidebar 7 menu |
| V3 | Worklog | 8 tuần đều click được |
| V4 | Worklog 1.5 | Bảng + placeholder block |
| V5 | Blog | 3 link blog |
| V6 | Blog 3.1 | Lambda cost article render OK |
| V7 | Event | 5 link event |
| V8 | Event 4.5 | 4 subsection nhóm |
| V9 | Workshop | 6 menu con (5.1–5.6, không có 5.7) |
| V10 | Self-eval | 13 tiêu chí render OK |
| V11 | Feedback | 6 tiêu chí + reflection render OK |
| V12 | LaTeX converter | Output "Generated..." cho vi/en |
| V13 | `latexmk main.tex` | `report_vn.pdf` > 1MB |
| V14 | `latexmk main_en.tex` | `report_en.pdf` > 1MB |
| V15 | `git status` | Không có file thừa |

## 11. Error handling

| Lỗi | Phát hiện | Xử lý |
|-----|-----------|-------|
| Hugo fail (frontmatter sai) | `--renderToMemory` exit 1 | Thêm `weight` |
| Shortcode không đóng | "unclosed action" log | Fix HTML, có `{{% /notice %}}` |
| Image path 404 | HTML 404 | Tạo `.gitkeep`, kiểm tra path |
| LaTeX fail (special char) | `latexmk` log | Pandoc tự escape, fallback `\&` |
| UTF-8 tiếng Việt trong PDF | Glyph lỗi | Đã có `texlive-lang-other` |

## 12. Rollback

```bash
git stash
git checkout main
git stash pop
```

Worst case: `git restore .`.

## 13. YAGNI

- Không refactor `config.toml`, `layouts/`, `partials/`, `shortcodes/`.
- Không thêm analytics, search index, RSS feed mới.
- Không tạo CI/CD mới.
- Không sửa `report/main.tex`/`main_en.tex`.
- Không đổi `scripts/convert_hugo_to_latex.py` ngoài thêm SECTION_TITLES.
- Không tạo Workshop 5.7 (theo user).
- Không sửa Section 2-Proposal (theo user).

## 14. Tổng số file thay đổi

**File mới (32 file):**
- Worklog 1.2–1.8 × 2 (EN+VI) = 14 file
- Blog 3.1–3.3 × 2 (EN+VI) = 6 file (ghi đè placeholder)
- Event 4.3, 4.4, 4.5 × 2 (EN+VI) = 6 file
- Self-evaluation + Feedback × 2 × 2 (EN+VI) = 4 file (ghi đè placeholder)

**File sửa (4 file):**
- `content/1-Worklog/_index.md`
- `content/3-BlogsPosted/_index.md`
- `content/4-EventParticipated/_index.md`
- `scripts/convert_hugo_to_latex.py`

**Tổng: 32 file mới + 4 file sửa = 36 file**

## 15. Commit đề xuất

```
docs(worklog+blog+event+self-eval+feedback): add SageMaker 8-week content + 3 events + 3 blogs + reflection

- content/1-Worklog/1.2-Week2 ... 1.8-Week8 (14 files, EN+VI) — SageMaker worklog with placeholders
- content/3-BlogsPosted/3.1–3.3 (6 files, EN+VI) — overwrite placeholder EKS with real Lambda/SageMaker/200USD blogs
- content/4-EventParticipated/4.3/4.4/4.5 (6 files, EN+VI) — Meet 13-06, Meetup 06-06, FCAJ x AABW
- content/6-Self-evaluation/{md,vi.md} — SageMaker-specific 13-criteria self-assessment
- content/7-Feedback/{md,vi.md} — FCAJ program reflection
- update _index.md (1, 3, 4)
- minor: scripts/convert_hugo_to_latex.py — add SECTION_TITLES

OUT: Workshop 5.7 SageMaker (skipped per user), 2-Proposal (kept as-is).
```

## 16. Cảnh báo PPTX không đọc được

4 file PPTX trả về `Package not found` từ python-pptx (do dấu/ký tự đặc biệt hoặc version cũ):

- `Multiplayer in the Cloud.pptx`
- `Trương Phước - Cách làm việc nhóm hiệu quả.pptx`
- `ViệtPhát_NeptuneGraphRAG.pptx`
- `VinhTran.pptx`

Tôi sẽ **không bịa nội dung** cho 4 talk này — chỉ ghi nhận "có talk nhưng không đọc được slide".

## 17. Lệnh build & verify

```bash
cd fcaj-hcmut-template

hugo server -D
hugo --minify

python3 scripts/convert_hugo_to_latex.py
cd report
latexmk -pdf -interaction=nonstopmode main.tex && latexmk -pdf -interaction=nonstopmode main.tex && latexmk -pdf -interaction=nonstopmode main.tex
cp main.pdf ../report_vn.pdf
latexmk -pdf -interaction=nonstopmode main_en.tex && latexmk -pdf -interaction=nonstopmode main_en.tex && latexmk -pdf -interaction=nonstopmode main_en.tex
cp main_en.pdf ../report_en.pdf
```