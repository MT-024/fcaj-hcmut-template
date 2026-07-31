---
title: "Tuần 4"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
includeInReport: true
reportType: worklog
reportTableColumns:
  - Thứ
  - Công việc
  - Ngày hoàn thành
reportHeadings:
  - Mục tiêu tuần 4
  - Công việc cần làm trong tuần
  - Thành quả tuần 4
---


### Mục tiêu tuần 4:

* Chạy Hyperparameter Optimization (HPO) trên XGBoost để tìm hyperparameter tốt hơn.
* Tuân thủ ràng buộc brief: `max_parallel_jobs=1`, `max_jobs=6` (budget cap).
* Dùng `validation:auc` làm objective metric.


### Công việc cần làm trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành |
| --- | --------- | ------------ | --------------- |
| 2   | - Đọc HPO search space trong brief: `max_depth∈[3,7]`, `eta∈[0.03,0.2]`, `subsample∈[0.7,1.0]`, `colsample_bytree∈[0.7,1.0]`, `min_child_weight∈[1,10]` | 22/06/2026 | 22/06/2026 |
| 3   | - Viết HPO tuner config (`HyperparameterTuner` strategy `Random`, objective=validation:auc) <br> - Set `max_jobs=6`, `max_parallel_jobs=1` (bắt buộc theo brief) | 23/06/2026 | 24/06/2026 |
| 4   | - Chạy HPO job, monitor progress qua SageMaker console | 24/06/2026 | 24/06/2026 |
| 5   | - Đợi cả 6 trials chạy xong (~2 giờ với `max_parallel_jobs=1`) | 25/06/2026 | 25/06/2026 |
| 6   | - Lấy best trial hyperparameters + validation AUC <br> - So với baseline tuần 3: HPO có cải thiện AUC/Recall? <br> - Document cost của lần chạy HPO | 26/06/2026 | 26/06/2026 |


### Thành quả tuần 4:

* Đã chạy HPO 6 trials với `max_parallel_jobs=1` — giữ dưới budget 200 USD.
* Best trial vượt baseline tuần 3:

<!-- ============================================================== -->
<!-- SAGE_MAKER TODO: Tuần 4 - HPO best trial                      -->
<!-- Cần điền:                                                       -->
<!--   - Best trial hyperparameters (max_depth, eta, subsample, ...)  -->
<!--   - Best trial AUC (vd: 0.8723)                                 -->
<!--   - HPO cost (vd: 0.6 USD)                                      -->
<!-- Sau khi điền, xóa comment block này.                            -->
<!-- ============================================================== -->

| Metric | Value | Note |
| ------ | ----- | ---- |
| Best trial AUC | TBD | target ≥ baseline 0.84 |
| Best trial hyperparameters | TBD | max_depth/eta/subsample/colsample_bytree/min_child_weight |
| HPO total cost | TBD USD | ước tính brief: ~0.6 USD |
| Cải thiện so baseline | TBD | AUC delta so với tuần 3 |


### Tài liệu tham khảo:

* https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html