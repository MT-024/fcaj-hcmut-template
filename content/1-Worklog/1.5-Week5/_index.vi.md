---
title: "Tuần 5"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportHeadings:
  - Mục tiêu tuần 5
  - Công việc cần làm trong tuần
  - Thành quả tuần 5
---
{{% notice warning %}}
⚠️ **Lưu ý:** Chỉ mang tính tham khảo.
{{% /notice %}}


### Mục tiêu tuần 5:

* Register best HPO model trong SageMaker Model Registry.
* Deploy real-time Endpoint để inference.
* Bật Data Capture làm input cho drift monitoring (tuần 7).


### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Tạo Model Package Group `heart-attack-risk-models` trong SageMaker <br> - Register best HPO model làm Model Package v1 | 29/06/2026 | 29/06/2026 |
| 3   | - Viết inference script `inference.py` (load preprocessor + XGBoost) <br> - Đóng gói `model.tar.gz` gồm inference.py + xgb_model.json + preprocessor.joblib | 30/06/2026 | 01/07/2026 |
| 4   | - Deploy Endpoint `heart-risk-endpoint` trên `ml.t2.medium` (hoặc `ml.t3.medium`) <br> - Bật Data Capture (sampling_percentage=100, capture_options=[Input, Output]) | 01/07/2026 | 02/07/2026 |
| 5   | - Test endpoint với sample patients từ `processed/test_processed.csv` <br> - Verify response có disclaimer `"Educational demonstration only; not a medical diagnosis."` | 02/07/2026 | 03/07/2026 |
| 6   | - Cleanup: xóa endpoint sau demo (kỷ luật chi phí — endpoint 24/7 ≈ 35 USD/tháng) <br> - Document endpoint lifecycle: create-on-demo, delete-after-demo | 03/07/2026 | 03/07/2026 |


### Thành quả tuần 5:

* Register Model Package v1 trong Model Registry.
* Deploy Endpoint đầu tiên chạy được với Data Capture bật.
* Kỷ luật chi phí: endpoint lifecycle script (create → predict → delete).

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Tuần 5 - Endpoint + Registry                  -->
<!-- Cần điền:                                                       -->
<!--   - Endpoint ARN                                                -->
<!--   - Model Package ARN                                           -->
<!--   - First demo prediction (sample input + output)                -->
<!--   - Cost tuần 5                                                 -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->


### Tài liệu tham khảo:

* https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html
* https://docs.aws.amazon.com/sagemaker/latest/dg/model-data-plane.html