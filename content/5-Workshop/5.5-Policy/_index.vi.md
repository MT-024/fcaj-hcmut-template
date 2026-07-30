---
title: "Phát hiện drift và CloudWatch alarm (Tuần 7)"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Tuần 7 bản dựng nhặt các file Data Capture mà tuần 6 đã ghi và trả lời một câu hỏi: *traffic thực đã drift khỏi phân phối training chưa?* Một custom SageMaker Processing Job đảm nhận việc này vì official Model Monitor schedule không xuất ra metric theo feature đúng lúc trong quá trình test.

#### 5.5.1 Drift Processing Job

Custom Processing Job chạy theo giờ, được trigger bởi một EventBridge rule trên prefix Data Capture.

![Custom Processing Job chạy để phát hiện drift](/images/5-Workshop/W7-01a-custom-processing-job.png)

Script:

1. Đọc các file Data Capture JSONL mới nhất từ S3.
2. Load thống kê `baseline/` do Processing Job tuần 2 tạo ra.
3. Với numeric feature, tính **standardized mean shift** giữa baseline và current; flag drift nếu `|shift| > 0,5`.
4. Với categorical feature, tính **total variation distance** giữa phân phối baseline và current; flag drift nếu `TVD > 0,20`.
5. Ghi một drift report (`reports/drift/<run-id>.json`) và publish hai CloudWatch metric trong namespace **`Custom/HeartRisk`**:
   - `DriftDetected` — 0 hoặc 1.
   - `DataQualityViolationCount` — số feature bị drift.

> Các threshold này là rule PoC minh họa, không phải chuẩn thống kê lâm sàng hay production. Chúng được tài liệu hóa tại đây để bạn đọc biết đã kiểm tra gì và tại sao chọn giá trị đó.

#### 5.5.2 Drift report — ví dụ một run

![Drift report cho thấy 6/20 feature bị drift](/images/5-Workshop/W7-02-drift-report.png)

Run ví dụ cho thấy:

- Baseline rows: 4.900
- Current rows: 7.000
- Features checked: 20
- Violations: 6
- Drift detected: true

Sáu feature bị drift: `age`, `resting_bp`, `cholesterol`, `bmi`, `smoking_status`, `stress_level`. Cùng sáu feature này xuất hiện trong danh sách feature chi tiết (không hiển thị ở trên — chuyển vào phụ lục trong report).

#### 5.5.3 CloudWatch metrics

![DriftDetected và DataQualityViolationCount được publish trong Custom/HeartRisk](/images/5-Workshop/W7-04-custom-metrics.png)

Giá trị hiển thị:

- `DriftDetected = 1`
- `DataQualityViolationCount = 6`
- `Namespace = Custom/HeartRisk`

Hai metric này là hợp đồng mà alarm được dựng trên.

#### 5.5.4 CloudWatch Alarm

![CloudWatch Alarm chuyển sang ALARM khi DriftDetected đạt 1](/images/5-Workshop/W7-05-custom-alarm.png)

Alarm dùng:

- Statistic: **Maximum**
- Threshold: **1**
- Comparison: **GreaterThanOrEqualToThreshold**
- Missing data: **ignore** (custom Processing Job chỉ publish khi hoàn thành, nên missing data là bình thường)

Khi `DriftDetected = 1` được publish, alarm chuyển sang `ALARM` và một thông báo chạy qua SNS topic đã tạo ở mục 5.2.

#### Điều này mang lại cho dự án

- Một chuỗi cảnh báo chạy được trong budget 200 USD: Data Capture → EventBridge rule theo giờ → custom Processing Job → CloudWatch metrics → CloudWatch alarm → SNS.
- Không cần managed Model Monitor cho phạm vi này, giữ chi phí có thể dự đoán được.
- Namespace metric và threshold alarm tạo thành hợp đồng rõ ràng cho khái niệm "drift detected" — dễ mở rộng khi managed Model Monitor trở nên khả dụng.

#### Chi phí

Mỗi run theo giờ của Processing Job tốn vài cent (một `ml.m5.large` chạy ~2 phút). Custom CloudWatch metric cũng vài cent mỗi metric một tháng. Tổng dưới 5 USD cho cả 8 tuần.