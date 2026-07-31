---
title: "Tuần 7"
date: 2026-07-13
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportHeadings:
  - Mục tiêu tuần 7
  - Công việc cần làm trong tuần
  - Thành quả tuần 7
---

### Mục tiêu tuần 7:

* Xây pipeline phát hiện drift không phụ thuộc SageMaker Model Monitor.
* Ghi chú brief: quyền truy cập Model Monitor có thể thay đổi sau 2026-07-30 → thiết kế fallback (Data Capture → S3 → EventBridge → Processing Job → CloudWatch custom metrics).
* Sinh dữ liệu drift theo recipe tuần 7 của brief.


### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Đọc brief: drift-generation recipe (shift distribution trên một số feature) <br> - Viết `generate_drift.py` sinh batch drifted (5–10% feature bị perturb) | 13/07/2026 | 13/07/2026 |
| 3   | - Confirm Data Capture đang BẬT (setup tuần 5) → S3 path `s3://.../capture/` | 14/07/2026 | 14/07/2026 |
| 4   | - Tạo EventBridge rule: mỗi 1 giờ → trigger Processing Job <br> - Processing Job đọc captured JSON, tính PSI/KL divergence so với baseline | 15/07/2026 | 16/07/2026 |
| 5   | - Đẩy custom metric lên CloudWatch: `feature_drift_psi`, `prediction_drift_psi` <br> - Set CloudWatch alarm: PSI > 0.2 → SNS alert | 16/07/2026 | 17/07/2026 |
| 6   | - Chạy drift generation, gửi drifted traffic tới endpoint, verify alarm fire <br> - Document pipeline thủ công (vì Model Monitor có thể không available) | 17/07/2026 | 17/07/2026 |


### Thành quả tuần 7:

* Pipeline phát hiện drift chạy được mà không cần Model Monitor.
* CloudWatch alarm fire khi PSI > 0.2.

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Tuần 7 - Drift detection                      -->
<!-- Cần điền:                                                       -->
<!--   - PSI value trên drifted batch                                -->
<!--   - CloudWatch alarm ARN                                        -->
<!--   - Drift recipe thực sự dùng (feature nào shift bao nhiêu %)    -->
<!--   - Cost tuần 7                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### Tài liệu tham khảo:

* https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html
* https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rules.html