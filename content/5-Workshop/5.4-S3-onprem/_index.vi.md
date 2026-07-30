---
title: "Huấn luyện, HPO, endpoint và API (Tuần 3–6)"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Tuần 3 đến tuần 6 dựng phần pipeline thực sự train model, chọn một model, triển khai sau một REST API và bắt đầu lưu traffic inference cho monitoring. Bốn tiểu mục ở đây ánh xạ gần một-một với từng tuần.

#### 5.4.1 Baseline training (Tuần 3): Logistic Regression vs XGBoost

Hai mô hình ứng viên được train dưới dạng SageMaker Training Job. Cả hai chạy trên `ml.m5.large` và hoàn thành trong dưới 5 phút mỗi cái.

![Validation metrics Logistic Regression — ROC-AUC 0,863949; F1 0,747583; recall 0,789116; precision 0,710204](/images/5-Workshop/W3-02-lr-metrics.png)

![Validation metrics XGBoost mặc định — ROC-AUC 0,854283; F1 0,749749; recall 0,845805; precision 0,673285](/images/5-Workshop/W3-04-xgb-metrics.png)

Logistic Regression thắng ở metric chính (ROC-AUC). XGBoost thắng ở recall nhưng thua ở precision; theo quy tắc chọn của dự án, ROC-AUC quyết định.

#### 5.4.2 HPO trên XGBoost (Tuần 4)

HPO chạy 3 trials với `max_parallel_jobs=1` để kiểm soát chi phí. Objective là `validation:auc`. Trial tốt nhất đạt validation ROC-AUC ~ 0,860982 — vẫn thua Logistic Regression. Logistic Regression được khóa làm deployment candidate.

#### 5.4.3 Model Registry và real-time endpoint (Tuần 5)

![Model Registry với version 1 và 2 Approved, version 3 PendingManualApproval](/images/5-Workshop/W5-01-model-versions.png)

Version 1 và 2 phản ánh các lần deploy thủ công trước. Version 3 là thứ Pipeline sẽ tạo; nó nằm ở `PendingManualApproval` để giữ con người trong vòng kiểm soát.

![Real-time endpoint heart-risk-endpoint ở trạng thái InService](/images/5-Workshop/W6-01a-endpoint-inservice.png)

Một instance `ml.m5.large` duy nhất đủ cho traffic demo. Instance chỉ được tạo trong cửa sổ demo và dọn bởi `cleanup.py` sau đó.

![Confusion matrix và ROC-AUC/F1/recall/precision trên tập test của Logistic Regression được chọn](/images/5-Workshop/W5-02-evaluation-metrics-and-confusion-matrix.png)

Metric trên tập test xác nhận điều mà validation ROC-AUC đã gợi ý: ROC-AUC 0,885515; F1 0,768903; recall 0,818594 — vượt thoải mái quality gate của dự án (ROC-AUC ≥ 0,84; F1 ≥ 0,70; recall ≥ 0,65).

#### 5.4.4 Lambda, API Gateway và Data Capture (Tuần 6)

Endpoint được wrap bởi một Lambda function validate request body và gọi `sagemaker:InvokeEndpoint`. Lambda role scope chỉ tới ARN endpoint đó, không có quyền S3 hay SageMaker.

![Routes API Gateway — GET /health và POST /predict](/images/5-Workshop/W6-07-api-routes.png)

Ba HTTP test chứng minh bề mặt API:

- `GET /health` → `200 OK` (sanity check).
- `POST /predict` với payload hợp lệ → `200 OK` kèm `prediction`, `risk_probability`, `threshold`, `model_type` và disclaimer.

![POST /predict happy path trả 200 với prediction và risk_probability](/images/5-Workshop/W6-09-predict-200.png)

- `POST /predict` với thiếu trường → `400 Bad Request`. Lambda validator chặn trước khi request tới SageMaker.

![POST /predict trả 400 khi thiếu trường bắt buộc](/images/5-Workshop/W6-10-predict-400.png)

Data Capture được bật với sampling rate 100 % cho cả input và output. Mỗi record capture trở thành một dòng JSONL trong S3 chứa cả request payload và response — nguyên liệu cho drift job tuần 7.

#### Kiểm tra chi phí cuối tuần 6

Endpoint là tài nguyên chạy liên tục duy nhất. Hóa đơn ước tính hàng tháng cho endpoint nếu để chạy liên tục là ~35 USD — vì vậy quy tắc dự án là "endpoint chỉ bật trong cửa sổ demo". Phần tiếp theo thêm phát hiện drift chạy theo giờ, nhưng được hiện thực dưới dạng Processing Job (chi phí theo lần chạy), không phải managed schedule.