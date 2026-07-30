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