---
title: "Các bước chuẩn bị"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Trước khi bất kỳ resource SageMaker nào được tạo, cần có ba thứ: đúng region, một budget guardrail và một quy ước tag. Làm những việc này trước chính là điều giữ cho toàn bộ 8 tuần xây dựng nằm trong cap 200 USD.

#### 1. Chọn và khóa region

Toàn bộ resource của dự án nằm trong **`ap-southeast-1`**. Chọn một region giúp tránh cross-region data transfer và giữ mọi quyết định IAM, networking, storage nhất quán.

![Region ap-southeast-1 được chọn cho dự án](/fcaj-hcmut-template/images/5-Workshop/AWS-01-selected-region.png)

> Ảnh chụp region selector; trong tài khoản của em giá trị là `ap-southeast-1` (Singapore), không phải `us-east-1`. Tên file ảnh giữ theo cách đặt tên gốc lúc chụp nhưng bản thân dự án single-region `ap-southeast-1` từ đầu đến cuối.

#### 2. Dựng AWS Budgets trước khi tạo resource tính phí

Một Budget alarm với ba mốc (50 %, 80 %, 100 %) là lưới an toàn để em có thể thử nghiệm mà không sợ overspend.

![AWS Budget overview với monthly budget dưới 200 USD](/fcaj-hcmut-template/images/5-Workshop/AWS-02-budget-overview.png)

Khi chi phí dự kiến hoặc thực tế vượt một mốc, một thông báo SNS được gửi. Đây chính là tín hiệu duy nhất chứng minh kỷ luật chi phí là có thật, không phải chỉ nói.

#### 3. Gắn tag cho mọi resource tính phí

Mọi SageMaker job, endpoint, model và bucket thuộc dự án đều được gắn tag:

```
Project = heart-risk-mlops
Stage   = {dev | hpo | endpoint | drift | cleanup}
```

Tag giúp script `cleanup.py` ở cuối tuần 8 an toàn: nó chỉ xóa resource có `Project=heart-risk-mlops` và không động tới bất kỳ thứ gì tag khác.

#### 4. Tạo hai IAM role

Hai execution role được dựng một lần và tái sử dụng xuyên suốt:

- **SageMaker execution role** — `sagemaker:full-access` trên bucket và endpoint dự án, với `iam:PassRole` giới hạn cho chính nó. Được dùng bởi Processing, Training, HPO, Model Registry và Pipeline.
- **Lambda execution role** — chỉ `sagemaker:InvokeEndpoint` trên ARN endpoint dự án, cộng `logs:*` cho CloudWatch. **Không đọc S3, không có quyền training job.**

Sự tách biệt này đảm bảo một API call bất thường không thể vô tình train model mới hoặc đọc dataset.

#### 5. Local tooling

- AWS CLI v2 cấu hình cho account dự án (`aws sts get-caller-identity` trả về đúng account ID).
- Python 3.12 với `boto3`, `sagemaker` SDK, `pandas`, `scikit-learn`, `xgboost`.
- Một thư mục notebook dự án chứa các script Processing / Training / Pipeline và khung `cleanup.py`.

#### Những gì bạn có sau bước này

Với năm prerequisite trên, tuần 2 có thể bắt đầu provision pipeline thật mà không cần ra quyết định kiến trúc nào về kỷ luật chi phí — quyết định đó đã được đưa ra và wire vào Budgets cùng tag từ trước.