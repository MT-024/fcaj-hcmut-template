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