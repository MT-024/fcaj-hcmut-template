---
title: "Tổng quan workshop"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Workshop này đề cập tới những gì

Workshop này là nhật ký xây dựng một pipeline MLOps end-to-end cho **bài toán phân loại nhị phân nguy cơ đau tim** trong một AWS account duy nhất, một region duy nhất. Pipeline nhận một tập dữ liệu 7.000 dòng, huấn luyện các mô hình ứng viên, đăng ký các mô hình đạt chuẩn vào SageMaker Model Registry, triển khai một real-time endpoint, wrap endpoint bằng Lambda và API Gateway, giám sát data drift và dọn dẹp tài nguyên chạy liên tục khi kết thúc.

Năm trang con ánh xạ tám tuần thực tập:

- **5.2 Chuẩn bị** — AWS account, khóa region, IAM role, AWS Budgets và quy tắc tag.
- **5.3 Tiền xử lý dữ liệu (Tuần 2)** — layout dữ liệu S3, SageMaker Processing Job, chia train/validation/test, preprocessor artifacts.
- **5.4 Huấn luyện, HPO, endpoint và API (Tuần 3–6)** — Logistic Regression vs XGBoost, HPO, Model Registry, real-time endpoint, Lambda, API Gateway, Data Capture.
- **5.5 Phát hiện drift và CloudWatch alarm (Tuần 7)** — custom Processing Job, lịch EventBridge, CloudWatch metrics, alarm.
- **5.6 Pipeline và cleanup (Tuần 8)** — SageMaker Pipeline, quality gate, test failure có chủ đích, script `cleanup.py`.

#### Kiến trúc pipeline (dạng text)

```
Amazon S3 (raw data)
        ↓
SageMaker Processing Job        ← preprocessing + schema check
        ↓
train / validation / test / preprocessor artifacts
        ↓
Logistic Regression + XGBoost (+ HPO)
        ↓
Managed Evaluation (ROC-AUC, F1, recall, precision, confusion matrix)
        ↓
Quality Gate (ConditionStep)
        ↓
SageMaker Model Registry         ← PendingManualApproval
        ↓
Manual Approval
        ↓
Real-Time Endpoint (ml.m5.large) ← Data Capture 100%
        ↓
AWS Lambda                        ← least-privilege IAM
        ↓
Amazon API Gateway (GET /health, POST /predict)
        ↓
Client

Đường drift chạy song song:
Data Capture (S3)
        ↓
EventBridge rule theo giờ
        ↓
Custom Processing Job (PSI / KL divergence)
        ↓
CloudWatch metrics (namespace Custom/HeartRisk)
        ↓
CloudWatch Alarm → ALARM khi có drift

SageMaker Pipeline:
Processing → Training → Evaluation → Condition → Register / Fail
```

Đây là **luồng logic**, không phải sơ đồ kiến trúc vẽ tay. Sơ đồ kiến trúc tham chiếu nằm ở đầu trang index của workshop.

#### Nguyên tắc kỷ luật chi phí xuyên suốt workshop

- Một region `ap-southeast-1` — không có cross-region transfer.
- Không dùng instance type GPU.
- Không dùng NAT Gateway.
- Spot Training ở mọi nơi hỗ trợ.
- Real-time endpoint giới hạn trong các cửa sổ demo, dọn bởi `cleanup.py`.
- Lifecycle rule trên S3 cho logs và artifact tạm.
- Mọi resource tính phí được gắn tag `Project=heart-risk-mlops`.
- AWS Budgets alarm ở các mốc 50 / 80 / 100 %.

Đây là những lựa chọn giữ dự án trong cap 200 USD trong khi vẫn chạy được pipeline end-to-end.