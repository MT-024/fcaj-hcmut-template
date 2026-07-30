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